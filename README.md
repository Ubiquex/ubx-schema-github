# ubx-schema-github

A real, frozen, versioned GitHub provider schema snapshot -- the pinnable
distribution artifact `ubx-provider-dynamic` and `ubiquex` resolve a
single `[providers.github]` entry against, with zero network calls at
schema resolution time (see `provider/acquireschema.go` in `ubiquex`, and
`internal/snapshot`'s own doc comment in `ubx-provider-dynamic`). The
resource/data-source split below is a real, internal discovery-time
detail -- one pin resolves both.

## What's here

GitHub's own real published identity is a GROUP of two members, both
fetched from the identical live GHEC (GitHub Enterprise Cloud) OpenAPI
description but built through genuinely different pipelines:

- `github` -- resource mode (91 real resource types -- UBI-181's own
  narrow create-verb allowlist admits 19 real, published operations a
  literal "create"/"insert" check missed, mostly GitHub's own real
  `create-or-update-*-secret`/`restore-package-*` shapes; 8 of those 19
  collide on typeName with an already-claimed resource reachable at a
  second real path -- the identical response schema, correctly kept once
  and skipped rather than disambiguated, this repo's own established
  policy, not new here).
- `github_ds` -- data-source mode (250 real, unclaimed read-only
  operations -- down from the pre-UBI-181 count: the same PR's own
  five-rule filter now excludes watch/operation-status/execution/
  computed/reference-duplication candidates).

- `manifest.json` -- the group's own real identity: `schema_format`,
  `provider`, one `version` for the WHOLE group, and which member names
  it bundles.
- `members/<name>.json` -- one real, complete, independently-diffable
  file per member (`github.json`, `github_ds.json`). Committed as
  separate files, not one combined blob, so a real version bump's own
  git diff shows exactly which members changed.
- `.github/workflows/hash-watch.yml` -- runs weekly (and on manual
  dispatch), regenerates every member from the live spec and opens a PR
  only when the group's own mechanically-derived version (the highest
  real change level found across every member -- `internal/snapshot`'s
  `AssembleGroup`) actually moves. Never auto-merges.
- `.github/workflows/publish.yml` -- manual-dispatch-only. Packs
  `manifest.json` and every `members/*.json` into one compressed archive
  (`snapshot.tar.gz`) and cuts a real GitHub Release tagged `v<version>`
  carrying exactly two assets: `snapshot.tar.gz` and `SHA256SUMS`. The
  archive exists purely so a real pinned resolution is still one real
  download regardless of how many members a group has -- the COMMITTED
  files (what a reviewer actually sees) are always the separate,
  per-member ones above.

## Consuming a real, published version

In `ubiquex`, one real pin resolves the whole group -- both real members
(`github` resource mode, `github_ds` data-source mode) are served
together from the SAME launch, the SAME real download:

```toml
[providers.github]
source  = "ubiquex/github"
version = "1.0.0"
```

`provider.AcquireSchema`'s own cache-by-source+version resolves ONE real
download and ONE extracted cache directory
(`~/.ubx/schemas/ubiquex/github/1.0.0/`) -- the launched process merges
every real member of the group (`internal/snapshot.MergeOpenAPIGroup`)
into one served schema, `ResourceSchemas` and `DataSourceSchemas`
together, exactly like a real, hand-written Terraform provider already
looks from the outside.

## Versioning

One real, mechanically-derived semver number for the WHOLE group, not
one per member: the highest real change level found across every
member (a brand new resource type or a field that gained write access
bumps MINOR; a resource type or field that disappeared, or a field that
lost write access, bumps MAJOR; a pure description-text change bumps
PATCH), plus an unconditional MAJOR if a member the group used to bundle
is gone entirely. See `internal/snapshot/diff.go` and `AssembleGroup` in
`ubx-provider-dynamic` for the real rule.

`v1.0.0` is this group's real, first-ever snapshot, built directly on
top of `ubx-provider-dynamic`'s UBI-182 resource/data-source collapse --
one `[dynamic_providers.github]` entry driving generation from the
start, no separate `github_ds` table to collapse away later, unlike
`ubx-schema-kubernetes`/`ubx-schema-datadog`, which were regenerated and
republished onto this shape after starting on the older, two-table one.

<!-- README-GEN:BEGIN -->
**Real, current published version:** `v1.0.0`

## Links

- Docs: https://docs.ubiquex.io
- Internals (architecture and design): https://github.com/Ubiquex/ubiquex-internals
- Linear board: https://linear.app/ubiquex
<!-- README-GEN:END -->
