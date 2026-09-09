# HANDOFF — iLoader one-click Apple Watch companion install physically validated

Date: 2026-09-09

## Objective

Record the final exact Windows iLoader build and the physical iPhone + Apple Watch result for the embedded Watch companion installation chantier.

## Repository / branch

- repository: `Rzbck/iloader`
- local worktree: `E:\_Project\IOS APP\_Tools\iloader-watch\iloader`
- branch: `feat/watch-companion-support-20260909`
- physically validated iLoader code SHA: `88ca24bbb6fd028f4f180a5f28a5683fba10e7f5`
- commit: `build(watch): pin proxy-per-forward backend`
- pinned isideload SHA: `9d43554571360cb27c701efbe1fbd1f5456769ae`

This file is a final physical-validation supplement. The documentation commit that adds this file is NOT the code SHA of the tested Windows installer.

## Exact CI / artifact

Workflow run:

`34406032515`

The first Windows attempt failed only after the iLoader application had successfully compiled, while Tauri attempted to download WiX:

`An existing connection was forcibly closed by the remote host. (os error 10054)`

This was an external packaging/download failure, not a Watch code compile failure.

The failed jobs were rerun on the SAME exact iLoader SHA and Windows completed successfully, including `Upload Windows EXE`.

Exact Windows artifact:

- name: `windows-exe`;
- artifact ID: `10125812841`;
- artifact ZIP digest: `sha256:739826aa9404bda38d666f6e10c7e0b1690b7d63918053d40e0f16bf5672da74`;
- setup path: `E:\_Project\IOS APP\_Tools\iloader-watch\artifacts\88ca24bbb6fd028f4f180a5f28a5683fba10e7f5\nsis\iloader_2.3.1_x64-setup.exe`;
- local setup SHA-256: `4FE492056689602C9F02A35763959A14E11A522562825990C579C9390A74AEB9`.

## Regression IPA used

The final test intentionally reused the same previously known-good application IPA so only the tooling path changed:

`WatchSensorLab-companion-unsigned-f539fe4105df.ipa`

## Physical result — SUCCESS

The user physically confirmed the exact build above now works end to end:

- iPhone application installs successfully;
- Apple Watch companion installs successfully;
- Watch Sensor Lab works correctly on the real Apple Watch;
- the one-click iLoader iPhone + Watch path is physically validated for this IPA.

This is real hardware validation, not merely a successful build/CI result.

## Final blocker and progression

The immediately preceding exact build could already install the iPhone app and reach the Watch, but failed after accepted Watch trust/pairing while forwarding `com.apple.mobile.installation_proxy`:

`Failed to forward Apple Watch installation proxy`

`device socket io failed`

The final isideload backend changed CompanionProxy lifetime to match the working pymobiledevice3 model: a fresh `com.apple.companion_proxy` service connection is used for each forwarding start/stop command instead of keeping one persistent socket through multiple forwards.

With iLoader `88ca24b...` pinning isideload `9d435545...`, the physical test succeeded. This strongly identifies persistent CompanionProxy reuse as the final transport-lifetime blocker.

Earlier iLoader transport work remains valid: `DeviceInfo.id` / exact usbmux transport selection is preserved instead of resolving only by UDID. That fix was physically insufficient by itself but remains a correctness improvement and should not be reverted.

## Important history to preserve

- initial official iLoader path rewrote the iPhone bundle ID but not the Watch companion relationship;
- isideload gained nested `Watch/*.app` bundle rewrite/provisioning/signing;
- Watch registration and Watch-specific developer-services request routing were added;
- explicit Watch signing was added;
- direct Watch `streaming_zip_conduit` installation succeeded via Python and proved the signing/provisioning chain could work;
- automatic one-click direct Watch install was then added to isideload;
- stale/persistent CompanionProxy use was progressively isolated by the physical pairing and installation_proxy failures;
- fresh CompanionProxy per forward produced the final end-to-end success.

Do not return to the disproved profile-platform assumption and do not restart the unusable Watch syslog investigation.

## Validation boundary

Physically validated only for the exact chain recorded above and the known-good `f539fe...` IPA.

The new tracker/product application branch is a separate validation target and must be built/tested independently.

## Next step

Keep this exact iLoader installed while testing the application chantier in `Rzbck/ios-godot-lab`, branch `feat/watch-sensor-tracker-recorder-20260909`.

Do not merge to `main`, publish a release, or change the installer identity without explicit user approval.
