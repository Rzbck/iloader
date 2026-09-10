# HANDOFF — iLoader one-click Apple Watch companion install physically validated

Date: 2026-09-10

## Objective

Preserve the exact result of the Windows one-click iPhone + Apple Watch companion installation work and prepare the generic capability for a clean public project plus upstream review.

## Regression anchors

- repository: `Rzbck/iloader`
- branch: `feat/watch-companion-support-20260909`
- physically validated iLoader source: `70f37e9b4afc659ab44ec1944c034093f4cda416`
- matching isideload source: `f7b9f3da570edd6824c29680545e710846d07df5`

Later branch commits may be documentation-only. Verify HEAD/diff/CI before source work.

## Physical result

The Windows one-click path installed and launched the iPhone host and embedded Apple Watch companion on physical hardware.

## Public separation

Do not copy Watch Tracker source or activity/Health/GPS data into this chantier. Never commit Apple credentials, certificates, keys, provisioning profiles or physical-device identifiers.

## Read next

1. `docs/WATCH_COMPANION_PORTING_NOTES.md`
2. matching `Rzbck/isideload/HANDOFF_PUBLIC_REVIEW.md`
3. matching isideload clean porting notes
4. historical `HANDOFF.md` only for regression archaeology

## Review order

Clean/minimize isideload first, run tests/CI, prepare upstream review; then clean the smaller iLoader integration and prepare its review. Do not merge or release without explicit approval.
