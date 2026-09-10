# HANDOFF — iLoader one-click Apple Watch companion install physically validated

Date: 2026-09-10

## Objective

Preserve the exact result of the Windows one-click iPhone + Apple Watch companion installation work, while keeping public productization/upstream work separate from application-specific development.

## Repository / branch

- repository: `Rzbck/iloader`
- branch: `feat/watch-companion-support-20260909`
- physically validated iLoader application commit: `70f37e9b4afc659ab44ec1944c034093f4cda416`
- this branch may now contain later documentation-only commits; always verify current HEAD before new source changes.

Matching backend used by the validated build:

- repository: `Rzbck/isideload`
- branch: `feat/watch-companion-support-20260909`
- pinned backend revision in the physically validated iLoader build: `f7b9f3da570edd6824c29680545e710846d07df5`

## Physical result

The one-click path was validated on real hardware:

- the iPhone IPA installs;
- the iPhone app launches;
- the embedded Apple Watch companion is discovered/provisioned/signed;
- the paired Watch receives the app;
- the Watch app installs and launches physically.

Do not conflate this with build-only or CI-only evidence.

## Generic implementation areas

The working path spans both repositories.

### iLoader

- pins the matching patched isideload revision;
- preserves the selected usbmux transport/device context;
- supports the companion-device connection/install path required after the iPhone installation;
- CI was adjusted so slash-named feature branches build.

### isideload

- discovers nested app-ID-bearing bundles in the IPA;
- rewrites companion/bundle relationships coherently when sideload signing changes identifiers;
- provisions/signs the Watch bundle explicitly;
- includes the paired Watch in provisioning where required;
- handles watchOS platform/profile selection in the validated flow;
- preserves capability-aware provisioning including HealthKit-related bundles.

## Public/project separation

The Watch Tracker application and its Health/GPS/activity data are not part of this project and must not be copied into a public Watch-sideloading repository.

Public-facing work should be application-agnostic. Remove or generalize:

- local Windows paths;
- tracker-specific bundle identifiers;
- physical device identifiers;
- temporary diagnostic artifacts;
- investigation comments that do not explain a permanent invariant.

Never commit certificates, private keys, provisioning profiles or Apple credentials.

## Clean publication entry point

Read:

`docs/WATCH_COMPANION_PORTING_NOTES.md`

That document is the preferred starting point for a new publication/upstream-review conversation. Use the older `HANDOFF.md` only when the investigation chronology is needed.

## Upstream/review plan

1. verify both feature-branch HEADs and CI state;
2. compare each branch with its current upstream-compatible base;
3. extract the minimal generic patch series, keeping the known-good revisions as regression references;
4. sanitize docs/examples;
5. prepare the backend/isideload review first;
6. prepare the corresponding iLoader integration review second;
7. open pull requests as review requests;
8. do not merge into `main`, publish a release/crate, or delete historical branches without explicit approval.

## Next step

Create/use a dedicated public-project conversation for cleanup and upstream preparation. It must begin by reading this handoff, the clean porting notes, and the matching isideload handoff/docs, then verifying GitHub state before any source change.
