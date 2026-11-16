# Roomzy: Project Design

Status: Draft for team review. All capabilities below are planned.

## Problem and intended users

Students searching for a rental apartment must compare asking prices, apartment characteristics, and locations across separate listings. A low asking price may come with a poor location, while a more expensive apartment may better match the student's needs. Listings alone do not provide a consistent comparison.

Roomzy is a mobile application that helps students evaluate apartments they have found. It estimates monthly rent from property characteristics and presents a separate, personalized suitability score. The application supports a decision; an estimate does not establish the correct market price or guarantee that a listing is legitimate.

The primary user is a student seeking an apartment near a university or another chosen destination. A secondary use is comparing several apartments before arranging viewings.

## Proposed first version

The first version follows one complete journey: enter an apartment, analyze it, inspect the result, save it, and compare it with another apartment.

The following scope choices are proposals, awaiting review:

- Users enter apartment details manually. A listing URL is an optional reference and is not fetched automatically.
- The first dataset covers one selected city or region. Predictions outside its supported coverage are unavailable.
- Rent is expressed in EUR per month, excluding utilities and deposits. The dataset must support this definition.
- Preferences and saved apartments remain on the device. Accounts and synchronization are outside the first version.
- The backend estimates rent and calculates analysis results. Model training runs separately from user requests.

## Main functionality

| Feature | Planned behavior | Completion criterion |
| --- | --- | --- |
| Apartment entry | Enter asking rent, size, room count, location, and available amenities. Optionally add a title and source URL. | Invalid or missing required values are identified before analysis. |
| Rent estimate | Predict monthly rent from supported property characteristics. | The result identifies its currency, rent definition, and model version, or explains why no estimate is available. |
| Personalized score | Score suitability from 0 to 10 using affordability, location, size, amenities, and explicit preferences. | Users can inspect the component scores; incomplete analysis is clearly labeled. |
| Location analysis | Show the apartment, the chosen destination, and available nearby services. | Distance labels identify the measurement method and unavailable data. |
| Save apartments | Store entered details and the latest analysis locally. | Saved apartments remain available after restarting the application and can be deleted. |
| Comparison | Compare two saved apartments using the same criteria. | Asking rent, estimated rent, characteristics, and score components appear side by side. |
| Explanation | Explain the estimate using the selected model's supported explanation method. | Model-wide importance is distinguished from an explanation of this specific prediction. |

## Prediction and suitability are separate

The rent estimate answers: "What monthly rent does the model predict for this property?" Asking rent is compared with that estimate after prediction; it must not be used as a model input when predicting the same rent.

The suitability score answers: "How well does this apartment match this student's preferences?" A low rent estimate does not automatically imply a high suitability score. The score must expose its criteria rather than appear to be an objective quality rating.

Preferences include a monthly rent budget excluding utilities, a target apartment size, a destination such as the user's university, and desired amenities. The initial scoring formula and criterion weights remain to be agreed upon.

## Quality requirements

- Preserve entered details if analysis fails, and allow retrying without re-entering the form.
- Keep saved information accessible without a network connection; fresh analysis requires connectivity.
- Validate requests on the server as well as in the mobile interface.
- Provide readable labels, sufficient contrast, accessible controls, and text alternatives to map information.
- Do not present missing data as zero, fabricate nearby services, or display a prediction when no trained model is available.
- Send only the inputs needed for analysis. Do not collect the user's live location for the proposed flow.

## Decisions needed before implementation

1. Confirm manual entry as the initial workflow.
2. Select the supported city or region and a usable rental dataset.
3. Confirm the rent definition, required property fields, and treatment of missing features.
4. Agree on the suitability formula and the explanation method.
5. Confirm local storage without accounts and select the initial mobile test platform.

See [user scenarios](USER-SCENARIOS.md), [wireframes](PROTOTYPE.md), and [technical design](TECHNICAL-DESIGN.md) for the corresponding behavior and structure.
