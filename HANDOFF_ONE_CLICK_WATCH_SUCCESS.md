# HANDOFF — iLoader one-click Apple Watch companion install physically validated

Date: 2026-09-10

## Objective

Preserve the exact result of the Windows one-click iPhone + Apple Watch companion installation work, and prepare the generic capability for a clean public project plus upstream review without mixing application-specific code.

## Repository / branch

- repository: `Rzbck/iloader`
- branch: `feat/watch-companion-support-20260909`
- physically validated iLoader source revision: `70f37e9b4afc659ab44ec1944c034093f4cda416`
- later commits on this branch may be documentation-only; always verify current HEAD before source changes.

Matching backend used by the validated build:

- repository: `Rzbck/isideload`
- branch: `feat/watch-companion-support-20260909`
- pinned backend revision: `f7b9f3da570edd6824c29680545e710846d07df5`

## Physical result

The one-click path was validated on real hardware: the iPhone IPA installs and launches, and its embedded Apple Watch companion is discovered, provisioned, signed, delivered to the paired Watch, installed and launched physically.

## Public/project separation

The Watch Tracker application and its Health/GPS/activity data are not part of this project and must not be copied into a public Watch-sideloading repository. Never commit certificates, private keys, provisioning profiles or Apple credentials.

## Clean publication entry point

Read `docs/WATCH_COMPANION_PORTING_NOTES.md` first. Use historical `HANDOFF.md` only when investigation chronology is needed.

## Dedicated public repository

Use a separate public documentation/integration repository rather than duplicating both source trees. Keep actual patches in `Rzbck/isideload` and `Rzbck/iloader` for upstream review.

## Review order

1. verify both feature-branch HEADs and CI;
2. clean/minimize isideload against upstream;
3. run tests/CI and preserve the known-good anchor;
4. open an upstream isideload review/PR;
5. clean/minimize iLoader integration;
6. open the matching iLoader review/PR;
7. do not merge/release without explicit approval.

## Next conversation

Start from this file, the clean porting notes, and the matching isideload `HANDOFF_PUBLIC_REVIEW.md`. Verify GitHub state before source changes.
