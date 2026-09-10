# HANDOFF — Apple Watch companion public project / upstream review

Date: 2026-09-10

## Goal

Turn the physically validated Apple Watch companion sideloading work into clean, upstream-reviewable contributions without mixing any application repository or personal data into iLoader.

## Public coordination repository

- repository: `Rzbck/iloader-watch-companion`
- visibility: public
- default branch: `main`
- tracking issue: `#1` — `Prepare clean upstream Watch companion contribution`
- primary continuation handoff: `HANDOFF.md` in that repository

The public repository is now the coordination/source-of-truth entry point for architecture, compatibility evidence, testing rules, security, contribution guidelines, repository settings, and upstream review status. It intentionally does not duplicate the complete iLoader/isideload source trees.

## Regression anchors

- iLoader physically validated source: `70f37e9b4afc659ab44ec1944c034093f4cda416`
- CI run: `34436587217` — SUCCESS
- isideload backend used by that build: `f7b9f3da570edd6824c29680545e710846d07df5`
- matching isideload CI run: `34436382782` — SUCCESS

The matching Windows one-click path installed and launched both the iPhone host and embedded Watch companion on physical hardware.

## Source repository / regression branch

- repository: `Rzbck/iloader`
- branch: `feat/watch-companion-support-20260909`
- current branch HEAD at this checkpoint: `cd5d1dc55824d4f6929675b445cc7e533e96d8e5`
- all commits after the validated source anchor `70f37e9b...` are documentation-only at this checkpoint

This branch is a **regression branch**, not the future upstream PR branch. It preserves the exact path that reached physical success.

## Why a fresh PR branch is required

The regression branch has diverged from current `main` and contains historical HANDOFF/debug documentation in addition to the small integration changes. Do not send this historical branch directly upstream.

After the isideload review shape is stable, create a fresh iLoader cleanup/review branch from the relevant current upstream base and port only the integration changes that still belong in iLoader.

## iLoader responsibilities

The generic iLoader-side work includes:

- pinning/integrating the matching isideload backend;
- preserving the selected usbmux device/transport context;
- orchestrating the companion-device installation path;
- keeping the existing iLoader UX/build flow intact.

Do not move backend provisioning/signing logic into iLoader if it belongs in isideload.

## Sanitation contract

Do not publish or add to a review patch:

- local workstation paths;
- real device identifiers;
- Apple credentials, certificates, private keys, provisioning profiles or pairing records;
- Watch Tracker source, Health/GPS/activity data, or tracker-specific identifiers;
- comments that only narrate discarded debugging hypotheses.

Use generic examples and comments that explain permanent invariants/platform constraints.

## Review sequence

1. continue from `Rzbck/iloader-watch-companion` issue `#1`;
2. clean/minimize and review `isideload` first;
3. run exact-SHA tests/CI there;
4. open a draft PR to `nab138/isideload`;
5. after backend review shape is stable, create a fresh iLoader review branch;
6. port only the required iLoader integration changes;
7. run exact-SHA build/CI and sanitation review;
8. open the matching draft PR to `nab138/iloader` and link the backend PR;
9. do not merge, release, or delete/rewrite known-good regression anchors without explicit approval.

## First files to read in a new conversation

1. `HANDOFF.md` in `Rzbck/iloader-watch-companion`;
2. `docs/IMPLEMENTATION_MAP.md` there;
3. public issue `#1` there;
4. this `PUBLIC_REVIEW_HANDOFF.md`;
5. `HANDOFF_ONE_CLICK_WATCH_SUCCESS.md`;
6. `docs/WATCH_COMPANION_PORTING_NOTES.md`;
7. `HANDOFF_PUBLIC_REVIEW.md` in `Rzbck/isideload`.

Then verify GitHub branch HEADs and upstream refs before modifying source.