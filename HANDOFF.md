# HANDOFF — iLoader Watch companion experiment

Date: **2026-09-09**

## Objective

Build a Windows iLoader variant that can correctly re-sign/install an IPA containing an embedded Apple Watch companion app, using the patched `Rzbck/isideload` backend.

This is a separate tooling chantier from the application repo `Rzbck/ios-godot-lab`.

## Repository / branch

- repo: `Rzbck/iloader` (fork of `nab138/iloader`)
- local path: `E:\_Project\IOS APP\_Tools\iloader-watch\iloader`
- branch: `feat/watch-companion-support-20260909`
- upstream iLoader baseline/tag 2.3.1 commit: `8547013c50b86087fb542bc14aafa4c6c60e6638`
- last code HEAD before this HANDOFF commit: `2f026f437e973577c4ea0431b5206fb5a2156fe0`
- after this HANDOFF commit, always re-fetch before asserting current HEAD.

## Dependency pin

`src-tauri/Cargo.toml` was changed so iLoader 2.3.1 uses the patched fork:

- repo: `Rzbck/isideload`
- exact backend SHA: `04d25c73742d29c2b8936c6f95741f22640ef6da`

Do not replace this with a moving branch while testing; exact-SHA reproducibility matters.

## CI validation

Workflow run: `34367071071`

Exact tested iLoader SHA: `2f026f437e973577c4ea0431b5206fb5a2156fe0`

Result: **SUCCESS** across the full matrix, including:

- Windows `Build` PASS;
- Windows `Upload Windows EXE` PASS;
- macOS build/artifact PASS;
- Linux x64/ARM package builds PASS.

Windows artifact:

- artifact name: `windows-exe`
- artifact id: `10110888972`
- artifact digest: `sha256:f5398d658baf67aa9884986ee97296020ed964f00194628f72dbc962a16e29dc`
- setup path inside artifact: `nsis/iloader_2.3.1_x64-setup.exe`
- setup SHA-256 verified locally/user-side: `cd1d4d2ebf58754ecf734a8702e210cf26925b218cce92f7b69d8e43c6ce5eb4`

## Local Windows installation

Artifact was downloaded to:

`E:\_Project\IOS APP\_Tools\iloader-watch\artifacts\2f026f437e973577c4ea0431b5206fb5a2156fe0\nsis\iloader_2.3.1_x64-setup.exe`

Because the patched build keeps the same product identity/version as official iLoader 2.3.1, the installer detects the existing iLoader and offers **Add/Reinstall components**. The user used the patched build for the hardware test.

Potential future quality-of-life improvement: give the experimental fork a distinct Windows app identity/name (for example `iLoader Watch Test`) so official and patched builds can coexist. This has NOT been implemented yet.

## Hardware observations

Using the patched iLoader and the same Watch Sensor Lab IPA:

### Success

- iLoader UI reports signing/install operation completed;
- Watch Sensor Lab is installed on the real iPhone;
- user confirmed the iPhone app works/launches.

This is the first real hardware confirmation that the patched `isideload` path gets past the previous iPhone-side `InvalidCompanionAppBundleIdentifier` failure.

### Remaining Apple Watch failure

On the paired Apple Watch:

- Watch Sensor Lab appears but currently has no proper app icon;
- when the user taps/installs it, watchOS reports that the app cannot be installed because **its integrity could not be verified**.

The missing icon is a separate application packaging/assets issue and is NOT considered the likely cause of the integrity failure.

## Error history

1. Original IPA/device rejection:

`InvalidWatchKitApp` because the Watch app lacked `WKWatchKitApp`/`WKApplication` true.

Application-side fix: `WKApplication = true` for the modern single-target watchOS app.

2. Next rejection with official iLoader:

`InvalidCompanionAppBundleIdentifier` because iLoader rewrote the main iPhone ID to `com.rzbck.watchsensorlab.<TEAM_ID>` but did not rewrite the embedded Watch app's `WKCompanionAppBundleIdentifier`.

Tooling fix: patched `Rzbck/isideload` to treat `Watch/*.app` as first-class bundles, rewrite IDs, register App IDs/profiles, and persist plist changes before signing.

3. Current state with patched iLoader:

- iPhone install succeeds;
- Watch install reaches watchOS but fails integrity verification.

## Leading investigation — NOT YET PROVEN

The current `isideload` install flow registers only the directly connected iPhone before provisioning. It does not yet discover/register the paired Watch UDID.

Leading hypothesis: the Watch provisioning profile may not contain the physical Apple Watch device, causing watchOS integrity/provisioning rejection.

Also observed: `Developer Mode` is absent from the Watch's `Settings > Privacy & Security`, not just disabled. With no Mac/Xcode available, a Windows-only development pairing / Developer Mode path is being investigated.

## pymobiledevice3 diagnostic attempt

A separate local diagnostic environment is being created at:

`E:\_Project\IOS APP\_Tools\pymobiledevice3-watch`

Purpose: enumerate paired companion devices from Windows through the USB-connected iPhone, obtain Watch identity/UDID if exposed, and investigate supported developer-mode/device-service paths.

Current status: `pip install -U pymobiledevice3` encountered repeated intermittent DNS errors (`getaddrinfo failed`) against PyPI/NVIDIA index, though some package metadata/downloads subsequently began. **Do not assume pymobiledevice3 is installed yet.**

First planned commands after successful installation are read-only USB/companion listings.

## Upstream contribution plan

Do NOT open an upstream PR yet. First obtain a real physical Watch install/launch.

If hardware succeeds:

1. clean/refactor the generic `isideload` patch;
2. preserve regression tests;
3. document reproduction errors and real-device validation;
4. open PR to `nab138/isideload` first;
5. if accepted/appropriate, open a small separate `nab138/iloader` dependency/update PR.

No payment/bounty has been established; treat upstream contribution as open-source contribution unless a maintainer explicitly offers compensation.

## Next exact step

Continue the Windows companion-device investigation:

1. finish `pymobiledevice3` installation;
2. enumerate the iPhone and paired Watch without mutating state;
3. obtain Watch identifier/UDID if possible;
4. verify Developer Mode requirements/path;
5. patch provisioning flow only after confirming the missing device-registration hypothesis;
6. rebuild exact-SHA iLoader;
7. retest the same Watch Sensor Lab IPA on real iPhone + Watch.

## Do not modify

- upstream `nab138/iloader` directly;
- `main` in this fork without explicit approval;
- `Rzbck/ios-godot-lab` from this tooling chantier;
- hard-coded user Team IDs or WatchSensorLab-specific names in generic tooling logic.

## Update - explicit Watch signing backend pinned in iLoader

Exact tested iLoader code SHA: 045caa99d3122cd2bcba878588b1678a9cf5f6dc
Commit: build(watch): pin explicit Watch bundle signing backend

Pinned isideload backend:
dd4109c6ead22823f956e3f0f20d480e4b9965df

CI run: 34386364284

- Windows: SUCCESS; build PASS; Windows EXE upload PASS.
- macOS: SUCCESS; build PASS; DMG upload PASS.
- Linux ARM: successful builds/artifacts.
- Ubuntu x64 jobs: failed before build during apt-get update.
- Ubuntu x64 failure is external: Google Chrome apt repository Hash Sum mismatch.

Exact Windows artifact:
- name: windows-exe
- artifact ID: 10118032239
- artifact ZIP digest: sha256:437e6131cf44c9af193bf2e42c57db484a5a9caeb93b69fb457990da6e95f665

This build has NOT yet been physically validated on iPhone or Apple Watch.

Next exact step:
1. Download the exact Windows artifact from run 34386364284.
2. Install that exact iLoader build.
3. Reinstall the same Watch Sensor Lab IPA.
4. Verify iPhone launch.
5. Verify whether the Watch companion installs and launches instead of remaining a placeholder.

## Physical validation - explicit Watch signing build

Exact Windows installer physically used:

- iLoader code SHA: `045caa99d3122cd2bcba878588b1678a9cf5f6dc`
- pinned isideload SHA: `dd4109c6ead22823f956e3f0f20d480e4b9965df`
- run: `34386364284`
- setup path: `E:\_Project\IOS APP\_Tools\iloader-watch\artifacts\045caa99d3122cd2bcba878588b1678a9cf5f6dc\nsis\iloader_2.3.1_x64-setup.exe`
- setup SHA-256: `F97E4349EAE12F7D2275E6DC5A84C7DDE6B347F52C779689DD62166BD8703915`

Physical result after uninstalling the previous iPhone app and reinstalling the SAME Watch Sensor Lab IPA with this build:

- iPhone app is reinstalled successfully: **physically validated**;
- iLoader reports signing/install complete;
- Apple Watch installation still does **not** finalize;
- Watch installation database still reports the Watch Sensor Lab bundle as `IsPlaceholder = True`;
- Watch `SequenceNumber` changed from the previous observed value `1501` to `1505`;
- Watch data-container UUID and bundle-installation path also changed.

Interpretation:

- the new installation attempt definitely reached watchOS and refreshed/recreated the Watch placeholder;
- this is not merely a stale previous placeholder;
- however the real Watch app still failed to finalize installation;
- therefore the explicit nested Watch `sign_bundle()` patch at `dd4109c...` did **not** resolve the physical integrity/install failure by itself.

Current exact Watch record includes:

- bundle ID: `com.rzbck.watchsensorlab.59858TV9N2.watchkitapp`
- `SequenceNumber = 1505`
- `IsPlaceholder = True`
- `CFBundleShortVersionString = 0.1.0`

Next exact diagnostic step: capture the Apple Watch-side installation/syslog error during a fresh install attempt, using the existing Windows `pymobiledevice3` Watch proxy/lockdown path, so the next code change is based on the actual watchOS rejection reason rather than another signing hypothesis.

## Update - Watch diagnostics, fresh placeholder and exact signed bundle capture

Further Windows diagnostics were completed without changing iLoader or the application code.

### Watch service access

Using `pymobiledevice3 11.12.0` through the existing iPhone CompanionProxy / Watch lockdown forwarding path:

- Watch connection is healthy: `Watch7,14 / watchOS 26.6`;
- `OsTraceService.get_pid_list()` succeeds and returned **358 processes**;
- relevant Watch processes observed include `amfid`, `appconduitd`, `appstored`, `installcoordinationd`, `installd`, `misagent`, and `securityd`;
- global `OsTraceService.syslog()` produced no entries, including with `PROMISCUOUS`;
- classic `SyslogService` also produced no usable log lines;
- per-PID targeted os_trace capture during a real reinstall also produced a **0-byte** capture file.

Conclusion: live Watch syslog streaming through this forwarded lockdown path is not currently usable for this diagnosis. Do not spend more time filtering the empty captures.

### Fresh physical reinstall while targeted capture was active

The user reinstalled the SAME `WatchSensorLab-companion-unsigned-f539fe4105df.ipa` with the exact experimental iLoader build.

After that attempt, the Watch installation DB reported:

- bundle ID: `com.rzbck.watchsensorlab.59858TV9N2.watchkitapp`;
- `SequenceNumber = 1509` (previously `1505`);
- a new data-container UUID;
- a new bundle-installation path;
- **`IsPlaceholder = True` remains unchanged**.

The Watch UI wording during this attempt changed to approximately **“Impossible d’installer Watch Sensor Lab — Réessayer ultérieurement”**. Treat this only as a UI wording variation; the installation DB is definitive and still proves the real Watch app did not finalize.

This confirms again that the latest attempt reached/updated watchOS and is not a stale placeholder.

### Exact signed bundle captured before iLoader cleanup

Because isideload deletes the temporary signed app after installation, a local watcher copied the exact signed app bundle before cleanup.

Captured path:

`E:\_Project\IOS APP\_Tools\pymobiledevice3-watch\signed-captures\WatchSensorLab-signed-20260909_205028.app`

This copy corresponds to the same `f539fe...` IPA re-signed by the physically tested experimental iLoader path.

The embedded Watch app contains all of the files expected from the explicit Watch signing pass:

- `Watch\WatchSensorLabWatch.app\Info.plist`;
- `Watch\WatchSensorLabWatch.app\embedded.mobileprovision`;
- `Watch\WatchSensorLabWatch.app\_CodeSignature\CodeResources`;
- `Watch\WatchSensorLabWatch.app\WatchSensorLabWatch` executable.

Observed file sizes:

- Watch `embedded.mobileprovision`: **12720 bytes**;
- Watch `CodeResources`: **2182 bytes**;
- Watch executable: **371759 bytes**.

This is direct evidence that `dd4109c...` now embeds a profile and produces a Watch bundle signature; however physical installation still fails, so presence of these files alone is not sufficient.

### Exact Info.plist / provisioning inspection

From the captured signed bundle:

- main bundle ID: `com.rzbck.watchsensorlab.59858TV9N2`;
- Watch bundle ID: `com.rzbck.watchsensorlab.59858TV9N2.watchkitapp`;
- `WKCompanionAppBundleIdentifier` matches the rewritten main bundle ID: **true**;
- `WKApplication = true`;
- `WKRunsIndependentlyOfCompanionApp = false`;
- `CFBundleExecutable = WatchSensorLabWatch`;
- `DTPlatformName = watchos`;
- `MinimumOSVersion = 10.0`;
- `UIDeviceFamily = [4]`.

Embedded Watch provisioning profile:

- profile name: `iOS Team Provisioning Profile: com.rzbck.watchsensorlab.59858TV9N2.watchkitapp`;
- profile `Platform` array: **`['iOS', 'xrOS', 'visionOS']`**;
- **`watchOS` is absent from the profile Platform array**;
- expiration: `2026-09-16 18:50:59`;
- `ProvisionedDevices` count: **2**;
- the physical paired Watch UDID is present in `ProvisionedDevices`: **true**;
- profile `application-identifier` matches the rewritten Watch bundle: **true**;
- profile team identifier matches: **true**;
- `get-task-allow = true`.

### Current strongest root-cause candidate — NOT YET PROVEN

The previous hypotheses about missing Watch registration, companion ID rewrite, missing Watch signing pass, and Developer Mode have now all been substantially narrowed or disproved:

- physical Watch UDID **is** in the embedded profile;
- companion ID is correct;
- Watch app has `WKApplication = true`;
- Watch app now has `embedded.mobileprovision`, `_CodeSignature`, and a signed executable path;
- Developer Mode is enabled;
- yet watchOS still leaves the app as a placeholder.

The strongest new evidence is that the profile downloaded/embedded for the Watch bundle is still identified as an **iOS Team Provisioning Profile** and its `Platform` list contains `iOS/xrOS/visionOS` but not `watchOS`, even though the bundle itself is a watchOS app and isideload requests Watch-specific routing.

Do **not** declare this the final root cause yet. The next code investigation must verify whether the Apple Developer Services request used for the Watch profile is actually requesting/returning the correct watchOS profile type, and whether the `Platform` field is expected to contain `watchOS` for a valid modern Watch development profile.

## Next exact step after this update

Do not rebuild or modify the app yet.

In `Rzbck/isideload`, inspect the exact Watch profile request/response path around `download_team_provisioning_profile` / App ID profile creation and the `DeveloperDeviceType::Watchos` request marker. Compare the actual Watch request parameters against the iOS profile request and determine why the returned embedded profile is still an iOS-family profile.

Only after that evidence should a new isideload code patch be made and pinned into a new exact-SHA iLoader build for physical retest.

## Update - Watch pairing succeeded; failure advanced to installation_proxy forwarding

Date: **2026-09-09**

### Exact physically tested build

iLoader:

`ce868720316adf1b92b4fb1083db2f230f36f7ca`

Pinned isideload:

`367d24c6443897d586493128bef0210203525157`

iLoader CI:

`34403672997` — SUCCESS.

Exact Windows setup SHA-256 physically used:

`390667937EC44A9D820D217183F46F6F263003E8C574B981EFC6E16CB68E58F7`

Regression IPA:

`WatchSensorLab-companion-unsigned-f539fe4105df.ipa`

### Physical result

The iPhone application installs successfully.

The previous failure at the initial Watch lockdown forwarding stage is gone.

First attempt:

- real Apple Watch displayed a pairing/trust request;
- user accidentally denied it;
- isideload failed with:
  `Failed to pair with the Apple Watch through companion proxy`
  `user denied pairing trust`.

After restarting the Watch and repeating the exact test:

- pairing/trust prompt appeared again;
- user accepted;
- Watch pairing succeeded;
- installation advanced further.

New physical failure:

`Failed to forward Apple Watch installation proxy`

`watch_install.rs:193`

`device socket io failed`

This proves that the exact `367d24c...` backend physically reached all of the following:

1. iPhone install;
2. Watch lockdownd forwarding;
3. forwarded Watch lockdown connection;
4. real Watch pairing/trust;
5. Watch lockdown session;
6. start of `com.apple.mobile.installation_proxy`.

The failure happens when the same persistent CompanionProxy connection is reused for the next `StartForwardingServicePort`.

### Root-cause-directed backend patch

The working pymobiledevice3 implementation opens a fresh
`com.apple.companion_proxy` lockdown service connection for each forwarding
start/stop command.

isideload has now been changed to follow that model instead of reusing one
persistent CompanionProxy socket.

New isideload SHA:

`9d43554571360cb27c701efbe1fbd1f5456769ae`

Commit:

`fix(watch): refresh companion proxy per forward`

isideload CI:

- run `34405740552`;
- exact SHA `9d43554571360cb27c701efbe1fbd1f5456769ae`;
- Windows SUCCESS;
- macOS SUCCESS;
- Ubuntu SUCCESS.

This new backend is CI-validated but NOT physically validated yet.

### Next exact step

Pin iLoader to `9d43554571360cb27c701efbe1fbd1f5456769ae`,
build an exact-SHA Windows installer, install it, then repeat the SAME
`f539fe...` IPA test on the same iPhone + Apple Watch.

Expected progression:

- if installation_proxy forwarding now succeeds, observe whether execution
  reaches `streaming_zip_conduit`;
- if streaming_zip_conduit completes, verify the Watch app actually installs
  and launches;
- if any failure remains, record the exact new isideload context/line and
  physical Watch state before making another change.

Do NOT declare the automatic one-click Watch path fixed until the real Watch
app installs and launches physically.
