# HANDOFF — iLoader Watch companion investigation archive

Date: 2026-09-10

This file is retained as the investigation archive for the Apple Watch companion sideloading chantier.

For new work, start from:

- `HANDOFF_ONE_CLICK_WATCH_SUCCESS.md` — exact physically validated result and regression anchors;
- `docs/WATCH_COMPANION_PORTING_NOTES.md` — clean public/upstream plan;
- the matching `Rzbck/isideload` feature branch/HANDOFF/docs — backend source of truth.

## Validated regression anchors

- iLoader physically validated source revision: `70f37e9b4afc659ab44ec1944c034093f4cda416`
- matching isideload revision pinned by that build: `f7b9f3da570edd6824c29680545e710846d07df5`

Later feature-branch commits may be documentation-only. Verify actual HEAD, diff and CI before changing source.

## Physical outcome

The final one-click Windows path successfully installed and launched both the iPhone host app and its embedded Apple Watch companion on real hardware.

## Archive purpose

The original investigation included failed hypotheses around paired-Watch registration, Developer Mode, bundle relationships, provisioning platform selection, nested signing and companion transport. Keep those details only when diagnosing a regression; do not use them as the public-facing explanation of the final feature.

## Repository hygiene

Do not copy application-specific activity data, GPS/Health data, local workstation paths, Apple credentials, signing keys, provisioning profiles or physical-device identifiers into public docs or review patches.

## Next step

A separate publication/upstream-review chantier should sanitize and minimize the generic patches, prepare `isideload` for upstream review first, then the smaller iLoader integration. Do not merge to `main`, publish a release or delete the known-good branch without explicit approval.
