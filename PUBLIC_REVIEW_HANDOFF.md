# HANDOFF — Apple Watch companion public project / upstream review

Date: 2026-09-10

## Goal

Publish the generic Apple Watch companion sideloading work as a clean public project and prepare the underlying iLoader/isideload changes for upstream review.

This is separate from any application repository.

## Regression anchors

- iLoader physically validated source: `70f37e9b4afc659ab44ec1944c034093f4cda416`
- isideload backend pinned by that build: `f7b9f3da570edd6824c29680545e710846d07df5`

The matching Windows one-click path installed and launched both the iPhone host and embedded Watch companion on physical hardware.

## Source repositories

- `Rzbck/iloader`, branch `feat/watch-companion-support-20260909`
- `Rzbck/isideload`, branch `feat/watch-companion-support-20260909`

Branch HEADs may be ahead because of documentation-only commits. Always compare against the regression anchors above before claiming a source revision was physically tested.

## Public repository design

Recommended repository name: `Rzbck/iloader-watch-companion`.

The public repository is a landing page and integration guide, not a duplicate source monorepo. It should contain:

- README;
- architecture;
- compatibility/validation matrix;
- sanitized testing procedure;
- upstream contribution status;
- security/credential hygiene;
- license;
- this continuation handoff.

Actual source changes stay in the iLoader/isideload forks and eventually become focused upstream PRs.

## Sanitation contract

Do not publish local paths, device identifiers, Apple credentials, certificates, private keys, provisioning profiles, application Health/GPS/activity data or test-app-specific identifiers. Generic placeholder identifiers are fine.

## Review sequence

1. create the public landing repository;
2. re-audit source diffs against upstream;
3. clean/minimize isideload patch series;
4. run CI/tests and preserve known-good regression references;
5. open a draft/review PR to `nab138/isideload`;
6. clean/minimize iLoader integration;
7. open the matching draft/review PR to `nab138/iloader`;
8. respond to review feedback;
9. do not merge, release or delete known-good branches without explicit approval.

## First files to read in a new conversation

1. `PUBLIC_REVIEW_HANDOFF.md` in `Rzbck/iloader`;
2. `HANDOFF_ONE_CLICK_WATCH_SUCCESS.md`;
3. `docs/WATCH_COMPANION_PORTING_NOTES.md`;
4. `HANDOFF_PUBLIC_REVIEW.md` in `Rzbck/isideload`;
5. `docs/WATCH_COMPANION_PORTING_NOTES.md` in `Rzbck/isideload`.

Then verify GitHub branch HEADs/CI and continue from the exact upstream-review step.