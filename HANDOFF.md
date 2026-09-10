# HANDOFF — iLoader Watch companion investigation archive

Date: 2026-09-10

This file is retained as the detailed investigation archive for the Apple Watch companion sideloading chantier.

For new work, do **not** start from this chronology. Use these cleaner entry points instead:

- `HANDOFF_ONE_CLICK_WATCH_SUCCESS.md` — exact physically validated result and regression anchors;
- `docs/WATCH_COMPANION_PORTING_NOTES.md` — application-agnostic publication/upstream plan;
- matching `Rzbck/isideload` feature branch/HANDOFF/docs — backend source of truth.

## Purpose of this archive

The original investigation explored several failed hypotheses around paired-Watch registration, Developer Mode, bundle relationships, provisioning platform selection, nested signing and companion transport. Those details remain useful when diagnosing a regression, but they are not suitable as the public-facing explanation of the final feature.

## Validated regression anchors

- iLoader physically validated revision: `70f37e9b4afc659ab44ec1944c034093f4cda416`
- matching isideload revision pinned by that build: `f7b9f3da570edd6824c29680545e710846d07df5`

Later feature-branch commits may be documentation-only. Verify the actual branch/CI state before changing source.

## Physical outcome

The final one-click Windows path successfully installed and launched both the iPhone host app and its embedded Apple Watch companion on real hardware.

## Repository hygiene

Do not copy application-specific activity data, GPS/Health data, local workstation paths, Apple credentials, signing keys, provisioning profiles or physical-device identifiers into public docs or review patches.

## Next step

A separate publication/upstream-review chantier should sanitize and minimize the generic patches, prepare `isideload` for upstream review first, then the smaller iLoader integration. Do not merge to `main` or publish a release without explicit approval.
