# Roomzy

Roomzy helps students judge rental apartments they found, by estimating rent with an ML model and scoring how well each apartment fits their own preferences.

## Rent

**Asking rent**:
The monthly rent stated in a listing the student entered.
_Avoid_: price, listing price

**Estimated rent**:
The monthly rent the model predicts for an apartment from its characteristics; never derived from the asking rent.
_Avoid_: predicted price, price prediction, valuation

## Suitability

**Preferences**:
The student's single profile of budget, target size, destination and desired amenities that a suitability score is measured against.
_Avoid_: settings, filters, profile

**Suitability score**:
A personal 0–10 measure of how well one apartment matches the student's preferences; not an objective quality rating.
_Avoid_: rating, apartment score, grade

**Score component**:
One criterion of the suitability score (affordability, size, location, amenities), each shown on its own.
_Avoid_: factor, sub-score

## Results

**Analysis**:
One snapshot of results (estimated rent, suitability score, location results) for one apartment at one point in time.
_Avoid_: evaluation, report, result

**Coverage**:
The geographic area the model was trained to support (Maribor, and Ljubljana if data allows); outside it no estimated rent is given.
_Avoid_: region, supported area
