# Roomzy: Entity-Relationship Model

Status: Draft for team review. This model describes the planned on-device SQLite database. It refines the initial data model in the [technical design](TECHNICAL-DESIGN.md) by turning its JSON columns (amenities, score components, location results, explanation) into separate entities.

Training data and model artifacts are not part of this model: they live in a separate, versioned dataset on the ML side, and the device only records which model and scoring version produced an analysis.

## Diagram

```mermaid
erDiagram
    PREFERENCES ||--o{ PREFERRED_AMENITY : wants
    AMENITY ||--o{ PREFERRED_AMENITY : "is wanted in"
    APARTMENT ||--o{ APARTMENT_AMENITY : has
    AMENITY ||--o{ APARTMENT_AMENITY : "is described in"
    APARTMENT ||--o| ANALYSIS : "has latest"
    PREFERENCES |o--o{ ANALYSIS : "is measured against"
    ANALYSIS ||--o{ SCORE_COMPONENT : "consists of"
    ANALYSIS ||--o{ NEARBY_SERVICE : "lists"
    ANALYSIS ||--o{ FEATURE_CONTRIBUTION : "is explained by"

    PREFERENCES {
        integer id PK
        integer revision "incremented on every save"
        integer budget_cents "nullable, EUR/month excl. utilities"
        real target_size_m2 "nullable"
        text destination_label "nullable"
        real destination_latitude "nullable"
        real destination_longitude "nullable"
        text updated_at
    }
    AMENITY {
        integer id PK
        text code UK "e.g. furnished, parking"
        text label
    }
    PREFERRED_AMENITY {
        integer preferences_id PK, FK
        integer amenity_id PK, FK
    }
    APARTMENT {
        integer id PK
        text title "nullable"
        text source_url "nullable, reference only"
        integer asking_rent_cents "EUR/month excl. utilities"
        real size_m2
        integer rooms
        text location_label
        real latitude
        real longitude
        text created_at
        text updated_at
    }
    APARTMENT_AMENITY {
        integer apartment_id PK, FK
        integer amenity_id PK, FK
        text availability "present | absent | unknown"
    }
    ANALYSIS {
        integer id PK
        integer apartment_id FK, UK
        integer preferences_id FK "nullable"
        integer preferences_revision "nullable"
        text apartment_updated_at "input snapshot time"
        integer estimated_rent_cents "nullable"
        real suitability_score "nullable, 0-10"
        real destination_distance_m "nullable, straight-line"
        text status "complete | partial | unavailable"
        text model_version "nullable"
        text scoring_version "nullable"
        text analyzed_at
    }
    SCORE_COMPONENT {
        integer analysis_id PK, FK
        text component PK "affordability | size | location | amenities"
        real score "nullable, 0-10"
        text status "available | unavailable"
    }
    NEARBY_SERVICE {
        integer id PK
        integer analysis_id FK
        text category "e.g. university, transport, grocery"
        text name "nullable"
        real latitude "nullable"
        real longitude "nullable"
        real distance_m "nullable, straight-line"
        text status "found | none_found | lookup_failed"
    }
    FEATURE_CONTRIBUTION {
        integer id PK
        integer analysis_id FK
        text feature
        real contribution "unit depends on explanation method"
        text scope "local | global"
    }
```

## Entities

| Entity | Meaning |
| --- | --- |
| **Preferences** | The student's single profile of budget, target size, destination and desired amenities. Every field can stay unset until entered. |
| **Amenity** | A fixed list of amenities the app knows about (for example furnished, parking). Shared by preferences and apartments, so both use the same codes. |
| **Preferred amenity** | Which amenities the student wants. |
| **Apartment** | An apartment the student entered manually, with its asking rent and characteristics. |
| **Apartment amenity** | Whether an apartment has a given amenity. `unknown` is stored explicitly so it is never confused with `absent`. |
| **Analysis** | The latest snapshot of results for one apartment: estimated rent, suitability score and location results. |
| **Score component** | One criterion of the suitability score, each shown on its own. |
| **Nearby service** | A service found (or looked for) near the apartment during location analysis. |
| **Feature contribution** | One line of the explanation of the estimated rent, labelled as either specific to this apartment (`local`) or model-wide (`global`). |

## Relationships and cardinalities

| Relationship | Cardinality | Rule |
| --- | --- | --- |
| Preferences – Amenity (via Preferred amenity) | M:N | A student can want several amenities; an amenity can be wanted or not. |
| Apartment – Amenity (via Apartment amenity) | M:N | Each row records availability as present, absent or unknown. |
| Apartment – Analysis | 1 : 0..1 | An apartment keeps at most its latest analysis (`apartment_id` is unique). A new analysis replaces the old one; deleting an apartment deletes its analysis. |
| Preferences – Analysis | 0..1 : 0..N | An analysis references the preferences it was scored against, or none if only a rent estimate was produced. |
| Analysis – Score component | 1 : 0..4 | One row per criterion; deleted with the analysis. |
| Analysis – Nearby service | 1 : 0..N | Deleted with the analysis. |
| Analysis – Feature contribution | 1 : 0..N | Deleted with the analysis. |

## Rules carried over from the design

- Money is stored as integer EUR cents; rent always means EUR/month excluding utilities and deposits.
- Missing values are `NULL`, never `0`. A service that could not be looked up (`lookup_failed`) is different from one confirmed not to exist (`none_found`).
- The asking rent is never an input to the estimated rent; the two are only compared after prediction.
- An analysis is outdated when `apartment_updated_at` differs from the apartment's `updated_at`, or `preferences_revision` differs from the current preferences revision.
- `model_version` and `scoring_version` record how a result was produced, so results from different versions are not compared as equal.
- Distances are straight-line and labelled as such, never presented as walking time.

## Open points

These depend on decisions still open in `CONTEXT.md`, so the model may change:

- Apartment fields and the amenity list depend on the dataset that is chosen.
- `Feature contribution` assumes the explanation can be expressed per feature; its unit and scope depend on the explanation method.
- The scoring components and their 0–10 normalization are not agreed yet.
