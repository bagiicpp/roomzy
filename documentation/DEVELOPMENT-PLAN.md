# Roomzy: Development Plan and Milestone Checklist

Status: Draft for team review. Owners, GitHub issue links, and later course deadlines have not been confirmed.

## Current evidence

The local repository contained a root README before this draft. These documentation files and README links have now been prepared locally. The user reports that the README has been pushed and a GitHub Project has been created.

Repository access, remote issues, project fields, and workflow runs have not been verified. This document is a plan, not evidence that those tasks are complete.

## Planning milestone

The assignment deadline is **20 November 2026 at 08:00**, using the course's stated time. The assistant reviews repository state at the deadline; later documentation does not replace timely completion.

| Requirement | Planned evidence | Draft status / remaining action |
| --- | --- | --- |
| Detailed project description and features | [Project design](PROJECT-DESIGN.md) | Draft ready for review; confirm scope. |
| Use-case diagram or key scenarios | [User scenarios](USER-SCENARIOS.md) | Draft ready for review. |
| Important screens and navigation | [Prototype](PROTOTYPE.md) | Text wireframes and navigation drafted; review whether visual mockups are needed. |
| Initial data model, architecture, technologies | [Technical design](TECHNICAL-DESIGN.md) | Draft ready; data source, providers, and scoring rules remain open. |
| Initial repository structure | Component folders and documentation | Documentation structure drafted; application structure still to establish. |
| Team and assistant added | Repository access settings | Confirm invitations have been accepted and access works. |
| README and documentation folder | Root README links and `documentation/` | Prepared locally; review and push before the deadline. |
| Initial assigned, prioritized issues | GitHub Issues linked to the Project | Create or verify issues using the backlog below. |
| Development plan and responsibilities | This document and issue assignments | Replace role placeholders with team members and agree on dates. |
| Working initial GitHub Actions | Workflow file and successful run on GitHub | Still to implement and verify. |

## Proposed repository structure

```text
roomzy/
  README.md
  documentation/       # Planning documents and prototype artifacts
  mobile/             # React Native / Expo application
  backend/            # FastAPI service
  ml/                 # Data preparation, training, and evaluation
  .github/workflows/  # GitHub Actions workflows
```

Only the README and documentation are included in this draft. Create component directories with their initial files as work begins. Keep raw datasets, secrets, local databases, and generated model artifacts out of Git by default; record how approved data and model artifacts are obtained.

## Responsibilities

Roles describe work areas, not an assumed team size. One member may own several roles. Replace every TBD with a real team member before finalizing the plan and assign actual GitHub accounts to issues.

| Work area | Accountable member | Responsibility |
| --- | --- | --- |
| Coordination and documentation | TBD | Confirm scope, maintain the Project, check milestone completeness, and consolidate documentation. |
| Mobile and prototype | TBD | Refine screens, implement navigation and forms, and implement local saving and comparison. |
| Backend and location | TBD | Define the API, validate requests, integrate location services, and handle partial results. |
| Data and ML | TBD | Obtain suitable data, prepare the training pipeline, evaluate models, and document limitations. |
| CI and integration | TBD | Maintain checks and verify the complete mobile-to-backend journey. |

## Initial GitHub issue backlog

Each row is an issue draft. Create it with the stated description, assign a team member, set its priority, and add its URL here. P0 means required for the planning milestone; P1 means core implementation; P2 means a later enhancement after the core journey works. These are proposed priorities, not existing GitHub labels.

| ID | Issue and description / acceptance criteria | Owner role | Priority | GitHub link |
| --- | --- | --- | --- | --- |
| PLAN-01 | **Confirm scope and review documentation.** Decide entry workflow, initial coverage, storage scope, and platform; review all five documents and record unresolved decisions. | Coordination | P0 | TBD |
| PLAN-02 | **Review prototype and navigation.** Walk through UC-01 to UC-05, check important error states, and approve the wireframes or add visual mockups with repository-accessible exports. | Mobile and prototype | P0 | TBD |
| PLAN-03 | **Establish repository access and structure.** Verify team and assistant access; add initial component files and link approved documentation from the README. | Coordination | P0 | TBD |
| PLAN-04 | **Establish initial CI.** Add meaningful checks for current files; obtain a successful push or pull-request run and link its result. | CI and integration | P0 | TBD |
| PLAN-05 | **Assign work and organize the Project.** Create this initial backlog as GitHub Issues with actual assignees and priorities; agree on development stages and dates. | Coordination | P0 | TBD |
| DATA-01 | **Validate a rental data source.** Document source, permitted use, coverage, fields, sample count, collection dates, and rent definition; decide whether it supports the planned first version. | Data and ML | P1 | TBD |
| DESIGN-01 | **Specify suitability rules and API.** Agree on component formulas, weights, missing-data handling, request/response examples, and version fields. | Backend and location | P1 | TBD |
| MOBILE-01 | **Build the entry and preferences flow.** Implement the approved screens, field validation, confirmed location selection, and loading/error states. | Mobile and prototype | P1 | TBD |
| ML-01 | **Train and evaluate a baseline.** Prepare a reproducible split and pipeline, report held-out errors against the median baseline, and export an artifact with metadata. Depends on DATA-01. | Data and ML | P1 | TBD |
| API-01 | **Serve apartment analysis.** Validate inputs, load the trusted model artifact, and return versioned results or explicit unavailable states. Depends on DESIGN-01 and ML-01. | Backend and location | P1 | TBD |
| GEO-01 | **Implement location analysis.** Select providers, document attribution and usage limits, and return labeled straight-line distances with unavailable-data handling. | Backend and location | P1 | TBD |
| MOBILE-02 | **Save and compare apartments.** Persist local records, confirm deletion, flag outdated results, and compare two apartments consistently. | Mobile and prototype | P1 | TBD |
| EXPLAIN-01 | **Present model explanations.** Select a supported explanation method, label its scope accurately, and display it in the result screen. Depends on ML-01. | Data and ML | P1 | TBD |
| INTEGRATE-01 | **Verify the complete journey.** Exercise preferences, entry, analysis, saving, restart, comparison, and failure recovery with the real backend and model; record results and limitations. | CI and integration | P1 | TBD |

## Development stages

Only the planning deadline was supplied. Dates for later stages must follow the remaining course milestones and team availability.

| Stage | Deliverable | Dependencies / exit criteria | Target |
| --- | --- | --- | --- |
| 1. Planning and repository setup | Reviewed design, prototype, assigned issues, access, and initial working CI. | Planning milestone checklist complete and available on GitHub. | Before 20 November 2026, 08:00 |
| 2. Data and contract validation | Usable data source, model input schema, scoring specification, and API examples. | Data supports the selected rent definition and geography; scope revised if necessary. | TBD |
| 3. First complete analysis | Mobile entry calls FastAPI and receives an evaluated model result. | Baseline evaluation recorded; invalid input and unavailable-model paths work. | TBD |
| 4. Location, persistence, and explanation | Location details, local saves, comparison, and a supported explanation. | UC-01 through UC-05 work with consistent units and versions. | TBD |
| 5. Integration and refinement | Demonstrable application and updated documentation. | Complete-journey verification recorded; known limitations documented. | TBD |

After the planning milestone, mobile interface work can progress using clearly labeled development fixtures while data and backend work proceed. Demonstration values must not be presented as trained predictions in the final application.

## Initial GitHub Actions plan

For the current documentation stage, a workflow should check Markdown formatting and repository-relative links on pushes and pull requests. It must perform actual checks and fail when they fail. Record a successful GitHub run before treating this requirement as complete.

As runnable components are introduced, extend CI with the project's chosen TypeScript checks, Python checks, and focused tests for input validation, scoring, and unavailable-component behavior. Do not add jobs that silently skip nonexistent applications and report them as tested. Training a full model or calling live map providers is not required for every commit; normal checks should use small deterministic fixtures.

This draft does not create a workflow or claim a successful run.

## Review before publishing

- Confirm the proposed product choices and resolve contradictions across documents.
- Decide whether the text wireframes meet the expected prototype fidelity; add visual exports if needed.
- Replace owner and issue-link placeholders and agree on later dates where known.
- Identify a plausible dataset and record the status honestly; keep unverified assumptions visible.
- Verify repository access, actual Issues, initial structure, and a successful Actions run.
- Review the final GitHub view of the Markdown and Mermaid diagrams, then ensure all approved material is pushed before the deadline.
