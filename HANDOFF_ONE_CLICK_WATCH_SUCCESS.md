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

Do not conflate this with build-only or CI-only evidence.

## Generic implementation split

### iLoader

- pins the matching patched isideload revision;
- preserves the selected usbmux transport/device context;
- supports the companion-device connection/install path required after the iPhone installation;
- CI supports slash-named feature branches.

### isideload

- discovers nested app-ID-bearing bundles in the IPA;
- rewrites companion/bundle relationships coherently when sideload signing changes identifiers;
- provisions/signs the Watch bundle explicitly;
- includes the paired Watch in provisioning where required;
- handles watchOS platform/profile selection in the validated flow;
- preserves capability-aware provisioning including the tested HealthKit path.

## Public/project separation

The Watch Tracker application and its Health/GPS/activity data are not part of this project and must not be copied into a public Watch-sideloading repository.

Public-facing work should be application-agnostic. Remove or generalize local paths, tracker-specific bundle identifiers, physical device identifiers, temporary diagnostic artifacts and investigation comments that do not explain a permanent invariant.

Never commit certificates, private keys, provisioning profiles or Apple credentials.

## Clean publication entry point

Read `docs/WATCH_COMPANION_PORTING_NOTES.md` first. Use the older `HANDOFF.md` only when investigation chronology is needed.

## Dedicated public repository

The user wants a separate public repository as the clean entry point for this capability. It should contain documentation, architecture, compatibility/testing notes, security guidance and links to the real source branches/future upstream PRs.

Do not duplicate both iLoader and isideload source trees into that third repository. Keeping source changes in their native forks preserves upstream history and makes pull requests reviewable.

Recommended files: `README.md`, `docs/ARCHITECTURE.md`, `docs/COMPATIBILITY.md`, `docs/TESTING.md`, `docs/UPSTREAM.md`, `SECURITY.md`, and `LICENSE`.

## Upstream/review plan

1. verify both feature-branch HEADs and CI state;
2. compare each branch with its current upstream-compatible base;
3. extract the minimal generic patch series, keeping the known-good revisions as regression references;
4. sanitize docs/examples;
5. prepare the backend/isideload review first;
6. prepare the corresponding iLoader integration review second;
7. open pull requests as review requests;
8. do not merge into `main`, publish a release/crate, or delete historical branches without explicit approval.

## Next conversation handoff

A dedicated Apple Watch sideloading/publication conversation must begin by reading this file, `docs/WATCH_COMPANION_PORTING_NOTES.md`, and the matching isideload branch HANDOFF/docs. Then verify GitHub branch HEADs and CI before modifying source.

The goal is cleanup/productization/upstream review of an already-working generic capability, not further Watch Tracker application development.
