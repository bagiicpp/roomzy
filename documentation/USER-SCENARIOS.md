# Roomzy: User Scenarios

Status: Draft for team review. These scenarios specify planned behavior.

## Actors and scope

The primary actor is a student evaluating rental apartments. Supporting systems are the prediction backend and the selected location-data services. A team member prepares training data and trains the model outside the mobile application.

These key scenarios fulfill the assignment's scenario alternative to a use-case diagram. They assume manual entry and local storage, as proposed in the [project design](PROJECT-DESIGN.md).

## UC-01: Set preferences

**Goal:** Make apartment analysis reflect the student's needs.

**Precondition:** The application is open. No account is required.

1. The student opens Preferences from the home screen.
2. They enter a monthly rent budget, target apartment size, university or other destination, and desired amenities.
3. They save the preferences and return to the home screen.

**Result:** Preferences persist on the device and are used for subsequent analyses.

**Alternatives:** Invalid values receive field-level messages. If required preferences are absent, rent prediction remains available, but the application explains that a complete personalized score requires preferences. Editing preferences marks previously saved scores as needing recalculation.

## UC-02: Analyze an apartment

**Goal:** Compare an apartment's asking rent with a model estimate.

**Precondition:** The student knows the apartment's asking rent, size, room count, and location.

1. The student selects Analyze an apartment.
2. They enter the required details and any known amenities.
3. They confirm the location selected through search or a map pin.
4. They submit the form.
5. Roomzy validates the input and requests an analysis.
6. The result displays asking rent, predicted rent, the difference, available score components, and analysis status.

**Result:** The student can inspect, save, or edit the analyzed apartment.

**Alternatives:** Invalid fields are highlighted. An ambiguous address requires confirmation. Unsupported coverage, an unavailable model, or a network error produces an explanation and a retry or edit action. The form remains populated. Missing location services may produce a rent estimate with incomplete location analysis.

## UC-03: Inspect location and explanation

**Goal:** Understand the reasons behind the analysis.

**Precondition:** An analysis result or saved analysis is open.

1. The student opens the location section.
2. Roomzy shows the apartment, chosen destination, and available nearby services, with a corresponding text list.
3. The student returns to the result and opens the explanation section.
4. Roomzy describes the available model explanation and suitability components.

**Result:** The student can distinguish a rent estimate, personal suitability, and location information.

**Alternatives:** Missing data is labeled unavailable. Straight-line distance is not described as walking time. If the model only supports global feature importance, the interface labels it as model-wide information rather than a specific explanation of this apartment.

## UC-04: Save and manage apartments

**Goal:** Keep an apartment for later review.

**Precondition:** The student has entered an apartment; analysis may be complete or unavailable.

1. The student selects Save on the result screen, or Save without analysis after an analysis failure.
2. Roomzy stores the apartment details and any available analysis on the device.
3. The apartment appears in Saved apartments.
4. The student can reopen it, edit its details, request a new analysis, or delete it.

**Result:** The saved record survives an application restart. Editing analyzed property details marks the previous analysis as outdated.

**Alternatives:** A storage failure is shown explicitly; the application does not claim a successful save. Deletion requires confirmation and removes the locally stored apartment and its analysis.

## UC-05: Compare two apartments

**Goal:** Choose which apartment better matches the student's needs.

**Precondition:** At least two apartments have been saved.

1. The student opens Saved apartments and selects two entries.
2. They select Compare.
3. Roomzy presents asking and predicted rent, size, rooms, location measures, amenities, and suitability components side by side.
4. The student opens either apartment for more detail or returns to the saved list.

**Result:** Both apartments are compared using clearly labeled criteria.

**Alternatives:** With fewer than two saved apartments, the screen explains how to add another. Missing results remain unavailable. Scores based on old preferences or different model versions are labeled, with an option to recalculate before comparison.

## Supporting workflow: Prepare a model

A team member acquires an approved dataset, records its source and rent definition, cleans it, trains and evaluates candidate models, and exports the selected preprocessing pipeline and model. The backend loads the resulting artifact for inference. Dataset suitability and evaluation must be reviewed before presenting the output as a trained estimate.

The [technical design](TECHNICAL-DESIGN.md) describes this workflow. The [prototype](PROTOTYPE.md) maps the user scenarios to screens.
