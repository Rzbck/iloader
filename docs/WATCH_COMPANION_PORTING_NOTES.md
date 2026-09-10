# Apple Watch companion sideloading — porting handoff

This document is the clean entry point for the watchOS companion sideloading work developed and physically validated on the `feat/watch-companion-support-20260909` line.

The implementation is intentionally split across two repositories:

- `Rzbck/iloader`: desktop integration, transport selection and installer flow;
- `Rzbck/isideload`: signing, bundle discovery, provisioning and watchOS companion installation logic.

## What was physically validated

A single iPhone IPA containing an embedded Apple Watch companion app can be processed through the patched iLoader/isideload path and installed from Windows so that:

- the iPhone app installs and launches;
- the embedded Watch app is discovered and provisioned;
- rewritten bundle relationships remain coherent;
- the Watch bundle is explicitly signed;
- the paired Watch is reached through the companion-device path;
- the Watch app installs and launches on physical hardware.

This is a hardware result, not merely a compile/CI result.

## Current validated integration points

The iLoader branch pins the matching patched `Rzbck/isideload` revision and contains the desktop-side transport/companion connection changes required by the validated flow.

Do not publish a release or merge this branch to `main` solely from this document. Re-verify the exact branch HEAD, CI and the referenced isideload revision first.

## Public cleanup / upstream plan

For a professional upstream contribution:

1. keep application-specific projects and activity data out of these repositories;
2. reduce the existing investigation history to a focused implementation explanation;
3. remove local Windows paths, test bundle identifiers, device identifiers and temporary diagnostic details from public-facing docs;
4. preserve the generic code changes and regression rationale;
5. prepare the `isideload` contribution first because most watchOS provisioning/signing behavior lives there;
6. then prepare the corresponding smaller iLoader integration contribution;
7. open pull requests as review requests; do not merge or publish a release without an explicit decision.

## Safety / repository hygiene

Never commit signing certificates, private keys, provisioning profiles, Apple credentials or physical-device identifiers. Existing ignore rules already exclude the local key directory and provisioning profiles; verify the final diff again before every public review.

## Next conversation / project

A dedicated GitHub/publication conversation should start by reading:

- `HANDOFF_ONE_CLICK_WATCH_SUCCESS.md` for the exact physically validated result;
- `HANDOFF.md` only when the investigation chronology is needed;
- this file for the clean publication plan;
- the matching `Rzbck/isideload` feature branch and its current HANDOFF before changing source.

The goal of that separate chantier is to turn the already-working prototype patches into a minimal, reviewable, application-agnostic contribution.