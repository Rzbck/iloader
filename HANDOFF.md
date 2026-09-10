# HANDOFF — iLoader Watch companion investigation archive

Date: 2026-09-10

This file is retained as the investigation archive for the Apple Watch companion sideloading chantier.

For new work, start from `HANDOFF_ONE_CLICK_WATCH_SUCCESS.md`, `docs/WATCH_COMPANION_PORTING_NOTES.md`, and the matching `Rzbck/isideload` public-review handoff/docs.

## Validated regression anchors

- iLoader: `70f37e9b4afc659ab44ec1944c034093f4cda416`
- isideload backend pinned by that build: `f7b9f3da570edd6824c29680545e710846d07df5`

Later branch commits may be documentation-only. Verify actual HEAD, diff and CI before changing source.

## Physical outcome

The one-click Windows path installed and launched both the iPhone host app and its embedded Apple Watch companion on real hardware.

## Archive purpose

Old failed hypotheses remain relevant only for regression diagnosis. They are not the public-facing explanation of the final feature.

## Hygiene

Do not publish application activity/GPS/Health data, local paths, Apple credentials, signing keys, provisioning profiles or physical-device identifiers.

## Next step

A separate publication/upstream-review chantier should minimize the generic isideload patch first and the iLoader integration second. Do not merge or release without explicit approval.
