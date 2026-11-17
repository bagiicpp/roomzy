# Roomzy — Working Context

Source of truth for both teammates and their Claudes. When this conflicts with `documentation/`, this file wins and `documentation/` gets updated.

## Team and ownership

Members: `bagiicpp`, `JovanObr`.

- ML is the focus of the project; ML work is split equally so each member owns features, a model and evaluation.
- Non-ML: `bagiicpp` owns mobile, `JovanObr` owns backend and CI.
- Work is planned in grilling sessions; each session produces a batch of GitHub Issues.

## Workflow

- One Issue → one branch (`<issue-number>-short-name`) → one PR into `main`.
- The other teammate reviews; CI must pass; squash merge.
- Issues are sized ½–2 days, with assignee, area label (`ml`, `mobile`, `backend`, `infra`), priority (`P1`–`P3`), acceptance criteria and `blocked by #N` links.
- No fixed deadlines; ~10-week project.

## Conventions

- Python: uv (`pyproject.toml` at root), ruff, pytest.
- ML: exploration notebooks in `ml/notebooks/` are personal and never imported. Anything the other person depends on (loading, splitting, features, training) lives in `ml/*.py`.
- Raw data stays out of Git (`ml/data/` is ignored); document how to obtain it.
- Mobile: Expo + Expo Router, React Native Reusables (shadcn-style) with NativeWind.
- Money as integer EUR cents; rent is EUR/month excluding utilities and deposits.

## Decision log

| # | Decision | Why |
| --- | --- | --- |
| 1 | Coverage: Maribor first, Ljubljana if data supports it. One model with a city feature vs one per city is decided after data exploration. | Team's city; Ljubljana has more data. |
| 2 | Dataset is chosen by a split data-source spike (ETN rentals vs alternatives) against shared criteria: rows in coverage, rent definition, features, licence, recency. | GURS reports ETN rental data is sparse; don't commit blind. |
| 3 | API contract (`backend/schemas.py` ↔ `mobile/src/lib/api.ts`) is written after the dataset is chosen. Changing it needs a PR approved by both. | Model inputs depend on the data. |
| 4 | ML split by feature family + model family on a shared, fixed train/test split and metric (MAE EUR/month vs median baseline). | Equal ML ownership; no one waits on the other. |
| 5 | `documentation/` is course-facing; CONTEXT.md is what we work from. | One source of truth. |

## Open questions

- Which dataset (decision 2).
- Who owns which feature/model family (after dataset choice).
- One model vs per-city model.
