# Release Playbook

## Purpose

This playbook defines how feature work moves through the environments and into production.

The current project is greenfield. For now, everything that is approved and merged into `develop` is expected to go into the first production release.

The main goal is to remove ambiguity:

- `build` is for developer confidence.
- `int` is for QA validation of individual feature branches.
- `develop` is the integrated first release.
- `pre-prod` is the final production rehearsal.
- `prod` is the live production environment.

## Current Assumptions

1. The team is following Git Flow principles.
2. Developers work on `feature/*` branches.
3. The team has one shared `int` environment.
4. QA currently tests feature branches in `int`.
5. The project has not had its first production release yet.
6. For the first release, anything merged into `develop` is expected to go to production.
7. `pre-prod` is not for feature testing. It is the final environment before production.

## Core Rule

Until the first production release:

> `develop` is the release.

That means:

> If a feature is merged into `develop`, it is considered approved for the first production deployment.

This rule is important because it prevents `develop` from becoming a dumping ground for unfinished or uncertain work.

## Branches

### `feature/*`

Used for individual feature development.

Examples:

```text
feature/customer-search
feature/payment-dashboard
feature/user-roles
```

Rules:

- Developers create feature branches from `develop`.
- Feature branches may be deployed to `build`.
- Feature branches may be deployed to `int` for QA.
- QA feedback is fixed on the same feature branch.
- A feature branch is merged into `develop` only after QA approval.

### `develop`

Used for integrated, QA-approved work.

Rules:

- Only QA-approved feature branches should be merged into `develop`.
- Anything merged into `develop` is expected to be part of the first production release.
- `develop` should always be deployable to `pre-prod`.

### `main` or `master`

Used for production code.

Rules:

- Production deployments should come from `main` or `master`.
- The production commit should be tagged.
- After the first release, `main` or `master` should represent what is live in production.

### Future: `release/*`

Release branches are optional for now.

Introduce them later when the team needs to freeze a release while new work continues on `develop`.

Example:

```text
release/1.1.0
release/1.2.0
```

## Environment Responsibilities

## Build

Purpose:

Developer confidence and early testing.

Rules:

- Any developer may deploy a feature branch to `build`.
- `build` can be unstable.
- `build` is not a QA approval environment.
- No release decision is made from `build`.

Example deployed version:

```text
0.1.0-feature-customer-search.3+abc1234
```

## INT

Purpose:

QA validation of individual feature branches.

Rules:

- Feature branches are deployed to `int` when ready for QA.
- Because there is only one `int` environment, only one feature can be tested there at a time.
- QA tests the feature branch in `int`.
- If issues are found, the developer fixes the same feature branch and redeploys it to `int`.
- Once QA approves, the feature branch can be merged into `develop`.

Important distinction:

> `int` proves that a feature works on its own. It does not prove that the full production release is ready.

Example deployed version:

```text
0.1.0-feature-customer-search.7+def4567
```

## Develop

Purpose:

Integrated first release.

Rules:

- `develop` contains only features that have passed QA in `int`.
- `develop` represents the current planned first production release.
- No unfinished work should be merged into `develop`.

Before the first production release:

```text
develop = first release candidate
```

## Pre-Prod

Purpose:

Production rehearsal.

Rules:

- `pre-prod` receives the complete application from `develop`.
- `pre-prod` is not for individual feature branches.
- `pre-prod` should be treated as the final checkpoint before production.
- Stakeholders, UAT users, or business representatives validate the full application here.
- If a blocking issue is found, it is fixed, merged into `develop`, and redeployed to `pre-prod`.

Important distinction:

> `pre-prod` answers: are we comfortable putting this exact version into production?

Example deployed version:

```text
0.1.0-beta.1+789abcd
```

Alternative if the team prefers a shorter label:

```text
0.1.0-b.1+789abcd
```

Definition:

In this team, `beta` means the full application deployed to `pre-prod` for final validation before production.

## Prod

Purpose:

Live production.

Rules:

- Production receives the approved version from `pre-prod`.
- The production commit is tagged.
- The UI must show the production version.
- Release notes must be available for the deployed version.

Example production version:

```text
0.1.0
```

## Current First Release Flow

```text
feature/* -> build optional -> int -> develop -> pre-prod -> main/master -> prod
```

Detailed flow:

1. Developer creates a `feature/*` branch from `develop`.
2. Developer optionally deploys the feature branch to `build`.
3. Developer deploys the feature branch to `int` for QA.
4. QA tests the feature in `int`.
5. Developer fixes QA feedback on the same feature branch.
6. QA approves the feature.
7. Feature branch is merged into `develop`.
8. `develop` accumulates the first production release.
9. When ready, `develop` is deployed to `pre-prod`.
10. Pre-prod validates the full application.
11. Approved commit is merged or promoted to `main` or `master`.
12. Production deployment is tagged and released.

## ASCII Architecture Diagram

```text
                             FIRST RELEASE FLOW

  Developer Work
  --------------

      +-------------------------+
      | feature/customer-search |
      +-------------------------+
                  |
                  | optional deploy
                  v
      +-------------------------+
      | BUILD                   |
      | developer confidence    |
      | unstable is acceptable  |
      +-------------------------+
                  |
                  | deploy for QA
                  v
      +-------------------------+
      | INT                     |
      | QA tests one feature    |
      | branch at a time        |
      +-------------------------+
                  |
                  | QA approved
                  v
      +-------------------------+
      | develop                 |
      | integrated approved     |
      | first release content   |
      +-------------------------+
                  |
                  | deploy full app
                  v
      +-------------------------+
      | PRE-PROD                |
      | production rehearsal    |
      | final acceptance        |
      +-------------------------+
                  |
                  | approved for production
                  v
      +-------------------------+
      | main / master           |
      | production code         |
      | tagged release          |
      +-------------------------+
                  |
                  | deploy
                  v
      +-------------------------+
      | PROD                    |
      | live application        |
      | version visible in UI   |
      +-------------------------+
```

## Simplified Decision Diagram

```text
Feature ready?
    |
    v
Deploy feature branch to INT
    |
    v
QA approved?
    |
    +-- no --> fix on feature branch --> redeploy to INT
    |
    +-- yes
          |
          v
Merge to develop
          |
          v
Ready for first production release?
          |
          +-- no --> continue accumulating approved features
          |
          +-- yes
                |
                v
Deploy develop to pre-prod
                |
                v
Pre-prod approved?
                |
                +-- no --> fix issue --> merge to develop --> redeploy pre-prod
                |
                +-- yes
                      |
                      v
Tag release and deploy to prod
```

## Versioning Standard

Use semantic versioning:

```text
MAJOR.MINOR.PATCH
```

For the first production release, choose one:

```text
0.1.0
```

or:

```text
1.0.0
```

Recommendation:

Use `1.0.0` if the bank considers the first production deployment a formally supported business release.

Use `0.1.0` if the application is still considered early and subject to significant change after first deployment.

## Environment Version Examples

### Build

```text
0.1.0-feature-customer-search.3+abc1234
```

### INT

```text
0.1.0-feature-customer-search.7+def4567
```

### Pre-Prod

```text
0.1.0-beta.1+789abcd
```

### Prod

```text
0.1.0
```

## UI Version Display

The application dashboard should show the deployed version in the sidebar.

Example:

```text
Version 0.1.0-rc.1
```

The version should be clickable.

Clicking it should open release notes for that version.

## Release Notes Page

The release notes page should show:

- Version number
- Release date
- Environment
- Features added
- Changes made
- Fixes included
- Known issues, if any

Example:

```markdown
# Release 0.1.0

Released: 2026-05-27
Environment: Production

## Added

- Customer search by account number.
- Payment approval dashboard.
- User role management.

## Changed

- Updated dashboard navigation structure.

## Fixed

- Fixed validation message on customer setup.

## Known Issues

- Export is limited to 5,000 rows.
```

## Release Notes Rule

Every feature PR should include a release note entry.

Example PR section:

```markdown
## Release Note

Added customer search by account number.
```

When the release is prepared, these entries are collected into the release notes for the deployed version.

## Minimum Team Rules

1. Feature branches may deploy to `build`.
2. Feature branches may deploy to `int` for QA.
3. Only QA-approved feature branches merge into `develop`.
4. Anything merged into `develop` is expected to go to the first production release.
5. `pre-prod` is not for feature branches.
6. `pre-prod` receives the full application from `develop`.
7. `pre-prod` is the final production rehearsal.
8. Production deploys only the approved version from `pre-prod`.
9. Every production deployment must be tagged.
10. The app UI must show the deployed version.
11. The version should link to release notes.
12. Release notes should exist for every production release.

## Future Change After First Release

After the first production release, the team may need release branches.

Use release branches when:

- `develop` needs to stay open for new feature work.
- The next release needs to be frozen.
- Pre-prod testing takes several days.
- Some approved features need to wait for a later release.

Future flow:

```text
feature/* -> int -> develop -> release/x.y.z -> pre-prod -> main/master -> prod
```

This should not be introduced until the team actually needs it.
