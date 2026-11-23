# Depot CI migration

Status: draft; remote validation and connection verification are required before merge.

## Decision

Run compatible Linux validation and maintenance workflows in native Depot CI.
Keep GitHub orchestration where protected environments or GitHub publishing
identities are required. The original native-migrated workflows become manual
fallbacks, so scheduled/PR/main events have one automatic executor after merge.
This avoids duplicate publishing, deployment or mirror jobs.

## Validation and activation

The workflow YAML and affected-source filters were checked locally. No production
release, protected deployment, mirror push or catalog-refresh PR was used as a test.
Native validation still needs a successful run using the chosen Depot organization.
The CLI currently reports no matching Code Access connection and rejects native
run creation as unauthorized, even though GitHub reports the app installed.
Resolve that organization connection before merging. Do not import production
secrets wholesale or change protected-environment controls to work around it.

Merging `.depot/workflows` activates its automatic triggers. Do not merge while
remote validation is pending. Revert the migration commit to restore the original
GitHub automatic triggers and remove the native definitions.

## Alternatives and consequences

Keeping every workflow on GitHub would retain its orchestration/billing dependency.
Copying all workflows into native Depot would lose unsupported environment gates
and could duplicate side effects. This migration uses both executors deliberately;
GitHub-managed Depot runner jobs still depend on GitHub being able to dispatch them.

Sources: [compatibility](https://depot.dev/docs/ci/compatibility),
[CI quickstart](https://depot.dev/docs/ci/quickstart),
[managed runners](https://depot.dev/docs/github-actions/quickstart).

## Workflow ownership

- Native Depot: affected-source CI, snapshot verification, and daily catalog drift.
- Original GitHub definitions remain manual fallbacks only.
- Drift retains its existing write permissions and may open a catalog-refresh PR;
  do not dispatch it merely to test this migration.
- Documentation-only merges skip compilation. Manifest/lockfile and shared-library
  changes still select the affected checks.
