# ci decisions

## [2026-11-23] — check affected source and build inputs
**What was decided:** Filter automatic validation and preview triggers by source,
tests, dependency manifests, lockfiles and workflow configuration. Keep manual
release controls and scheduled maintenance unchanged. Mirror these filters into
Depot configuration before activating that executor.
**Why:** Documentation and planning changes do not require application builds.
**What was rejected:** Running every check for every merge, ignoring shared
build inputs, or changing signing and publication behavior as part of filtering.
**Validation:** Parsed both executor configurations and checked source, dependency,
workflow and unrelated-document scenarios locally. Remote execution is pending.

**Migration:** Native Depot owns compatible automatic triggers; GitHub definitions
remain manual fallbacks. Protected preview remains GitHub-orchestrated. See
[`../.depot/README.md`](../.depot/README.md) for ownership, validation and rollback.

## [2026-11-23] — Supply BEAM image metadata on Depot
**What was decided:** Pin native jobs to Ubuntu 24.04 and set ImageOS=ubuntu24 on
setup-beam. Depot does not populate the GitHub image metadata that the action uses.
**Validation:** Workflow parsing and runner/metadata consistency checks pass.
Remote commit fetch failed before execution; retry the new pushed head.
