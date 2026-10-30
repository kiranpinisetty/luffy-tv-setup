# Phase 3A compatibility report: Xiaomi Mi TV 4X 65

> **Historical planning report:** This report predates the verified inventory
> and later deployment. It is retained for phase history. See
> [PROJECT-STATUS.md](../../PROJECT-STATUS.md) for current facts.

Status: planning only. No TV connection, ADB session, installation, package
change or launcher change has occurred.

## Confirmed device information

The owner identifies the TV as:

- Product: Xiaomi Mi TV 4X 65-inch, India model
- Model number: `L65M5-5SIN`
- Factory operating system: Android TV 9.0 Pie
- Resolution: 3840 x 2160
- RAM: 2 GB
- Storage: 16 GB

The official Xiaomi product page confirms the 65-inch Mi TV 4X product. Its
specification page confirms 3840 x 2160 resolution, PatchWall with Android TV,
2 GB RAM, 16 GB eMMC storage, a 64-bit quad-core A55 platform, Mali-470 MP3
GPU, three USB 2.0 ports, three HDMI ports, AV, LAN, dual-band Wi-Fi and
tuners:

- [Xiaomi Mi TV 4X 65 product page](https://www.mi.com/in/mi-tv-4x-65/)
- [Xiaomi Mi TV 4X 65 specifications](https://www.mi.com/in/mi-tv-4x-65/specs/)

The factory Android 9.0 statement is treated as owner-provided confirmed
information. The currently installed Android release, security patch, build,
firmware and package state are **not verified** and may differ after updates.

## Compatibility conclusions

| Question | Status | Evidence |
|---|---|---|
| Android TV platform | Confirmed by owner and Xiaomi specification | Factory OS is reported as Android TV 9.0; Xiaomi describes the platform as PatchWall with Android TV |
| Launcher API floor | Likely compatible, not yet device-verified | CleanYourTV launcher `minSdk` is 26 in `launcher/app/build.gradle`; Android 9 is API 28 |
| Current SDK/API | Unknown | Must be read from `ro.build.version.sdk` later |
| Current firmware/build | Unknown | Must be read from TV About screen and read-only properties later |
| Resolution suitability | Compatible in principle | 3840 x 2160 is suitable for the static 16:9 design |
| RAM suitability | Plausible, needs runtime validation | Launcher documentation targets TVs with 1 GB RAM, but wallpaper decoding and advanced effects still need testing on this exact firmware |
| Storage suitability | Unknown | Nominal 16 GB is not free space; Android, PatchWall, apps and updates consume it |
| Xiaomi settings fallback | Available in the launcher configuration | `config.xml` includes `com.xiaomi.mitv.settings/...MainActivity`, but installed component must be verified |
| HDMI/input behavior | Unknown for this exact model/build | Xiaomi examples preserve TV input services, but package names and input IDs are firmware-specific |
| Stock Home package | Unknown | Could be an Android TV launcher, Xiaomi launcher, or another firmware-specific component |
| PatchWall relationship to Home | Unknown | PatchWall may be a separate app, a Home claimant, a button target, or firmware-integrated content |
| Preferred streaming packages | Unknown | Must be inventoried on this TV; no package names are in the Luffy theme |
| CleanYourTV model-specific example | Not verified | Existing Xiaomi examples target other models and firmware generations |

This is not certification for the L65M5-5SIN. It is a readiness plan pending
the read-only device snapshot.

## Repository evidence reviewed

### Launcher platform and resource profile

The launcher documentation in
[docs/launcher.md](../../docs/launcher.md) states:

- Android 8.0/API 26 or newer is required.
- The launcher is plain Java and approximately 532 KB.
- It has no network permission and no background services.
- It is designed for TVs with as little as 1 GB RAM.
- Wallpaper and advanced effects can use additional memory.

The build file in
[launcher/app/build.gradle](../../launcher/app/build.gradle) sets `minSdk 26`,
`targetSdk 34`, Java 17 and a release build with shrinking enabled.

### Xiaomi precedent

The checked-in Xiaomi notes are not for this model:

- [xiaomi-mitv-mooq0-android11.md](../../docs/devices/xiaomi-mitv-mooq0-android11.md)
- [examples/xiaomi-mi-tv-4a/README.md](../../examples/xiaomi-mi-tv-4a/README.md)

They show that Xiaomi firmware can differ in:

- Android release and SDK
- Stock launcher package
- PatchWall package behavior
- Netflix/Prime package state
- HDMI and tuner service packages
- Remote key handling
- Whether a package survives a reboot when disabled

Those package lists must not be applied to `L65M5-5SIN` without a separate,
package-by-package review and approval.

## TV screens to inspect manually

Before any computer connection or ADB work, record photographs or notes from
the TV UI:

1. **Settings → Device Preferences → About**
   - Model
   - Android TV OS version
   - Android security patch level
   - Build
   - Kernel/build details if shown
2. **Settings → Device Preferences → Storage**
   - Total internal storage
   - Used storage
   - Available/free storage
   - Largest applications, if shown
3. **Settings → Device Preferences → About → Build**
   - Pressing Build seven times is the documented route to Developer options,
     but do not enable debugging during this planning phase.
4. **Settings → Device Preferences → Developer options**, only to determine
   whether USB debugging or another ADB option exists; do not enable it yet.
5. **Settings → Apps → See all apps → System apps**
   - Record the visible names of the stock Home/launcher and PatchWall if
     Xiaomi exposes them.
   - Do not disable, force-stop, uninstall or clear data.
6. **Settings → Device Preferences → Inputs / TV inputs**, if present
   - Record the visible HDMI, AV and Live TV entries.
7. **Settings → Remotes & accessories**
   - Confirm the physical remote and Bluetooth pairing remain available.

The exact menu labels can vary by Xiaomi firmware and region. If the TV uses
`Settings → System` rather than `Device Preferences`, follow the equivalent
About, Storage, Apps and Developer options pages.

## Later read-only device snapshot

Only after explicit approval for the compatibility phase, enable the minimum
debugging route offered by the TV and use the repository client. The
following are **planned read-only commands**, not executed in Phase 3A:

```bash
TV_HOST=<tv-ip> tools/tv.py 'getprop ro.product.model'
TV_HOST=<tv-ip> tools/tv.py 'getprop ro.product.device'
TV_HOST=<tv-ip> tools/tv.py 'getprop ro.build.version.release'
TV_HOST=<tv-ip> tools/tv.py 'getprop ro.build.version.sdk'
TV_HOST=<tv-ip> tools/tv.py 'getprop ro.build.display.id'
TV_HOST=<tv-ip> tools/tv.py 'getprop ro.build.fingerprint'
TV_HOST=<tv-ip> tools/tv.py 'pm list packages'
TV_HOST=<tv-ip> tools/tv.py 'pm list packages -d --user 0'
TV_HOST=<tv-ip> tools/tv.py 'settings list system'
TV_HOST=<tv-ip> tools/tv.py 'settings list secure'
TV_HOST=<tv-ip> tools/tv.py 'settings list global'
TV_HOST=<tv-ip> tools/tv.py 'cmd package resolve-activity --brief -a android.intent.action.MAIN -c android.intent.category.HOME'
TV_HOST=<tv-ip> tools/tv.py 'cmd package query-activities --brief -a android.intent.action.MAIN -c android.intent.category.HOME'
TV_HOST=<tv-ip> tools/tv.py 'cmd package query-activities --brief -a android.intent.action.MAIN -c android.intent.category.LEANBACK_LAUNCHER'
TV_HOST=<tv-ip> tools/tv.py 'dumpsys tv_input'
TV_HOST=<tv-ip> tools/tv.py 'dumpsys connectivity'
TV_HOST=<tv-ip> tools/tv.py 'df -h /data /sdcard'
```

The package and activity inventory should also include, for selected candidates
only:

```bash
TV_HOST=<tv-ip> tools/tv.py 'dumpsys package <package.name>'
```

`dumpsys package` is inspection only when used this way; it must not be
replaced with starting an unknown activity. Do not start factory/test packages
just to identify them.

### Safe app-package identification method

1. Export the complete package list.
2. Separately export enabled third-party/user packages if the firmware
   supports that filter.
3. Query `MAIN` + `LEANBACK_LAUNCHER` activities.
4. Match visible TV app labels to package/activity records.
5. Inspect a candidate with `dumpsys package <name>`.
6. Do not launch it, disable it or change its state during inventory.
7. Record the package name only after the label, launcher activity and owner
   choice agree.

The Luffy theme will remain with:

```json
"dock_apps": []
```

until this process is complete.

## Storage readiness

The 16 GB specification describes physical/eMMC capacity, not available
space. The later read-only check must record:

- `Settings → Device Preferences → Storage` available space
- `df -h /data /sdcard` output
- Whether the launcher APK can be staged without approaching a full `/data`
  partition
- Whether the desired local wallpaper can be stored safely

The launcher itself is small, but a full `/data` partition can make an install
fail. The installation script explicitly handles an install failure such as
“not enough space” before it disables the stock launcher; this is a reason to
check storage before installation, not a reason to retry blindly.

## Stock Android TV Home versus Xiaomi PatchWall

These are related but should not be assumed to be the same application:

- **Stock Android TV Home** is the Android `HOME` activity that receives the
  Home intent and presents apps/recommendations.
- **PatchWall** is Xiaomi's content/navigation layer and may be a separate
  Xiaomi package or firmware-integrated entry point. It may own a dedicated
  remote button, provide content rows, or participate in boot/Home behavior.

The actual relationship must be established by:

```text
cmd package resolve-activity ... HOME
cmd package query-activities ... HOME
pm list packages
dumpsys package <candidate>
```

Do not disable PatchWall merely because its name appears unnecessary. The
existing Xiaomi example explicitly warns that disabling PatchWall can make the
PatchWall button do nothing. The Luffy installation should initially leave
PatchWall and all other packages enabled.

### Restore path

The safest initial rollback is to use the launcher's own **Choose default home**
screen and select the original Home, provided the original launcher remains
enabled. The launcher documentation says some TVs do not provide a Home picker.

If the stock launcher was disabled during a later approved installation, use
the repository restore workflow before relying on the Home picker:

- [docs/restore.md](../../docs/restore.md)
- [tools/restore.sh](../../tools/restore.sh)
- [tools/recovery.py](../../tools/recovery.py)

The restore record returns packages to their pre-change state, restores the
Home owner recorded before the change, and restores the three animation scales.
It is not a full TV backup and does not restore packages disabled outside the
recorded list.

## Backup and recovery review

Before any package or launcher change, the repository procedure requires a
private backup containing:

- Full package list
- Disabled-package list
- `getprop` output
- All settings namespaces
- Screenshots/notes as appropriate

For the settings portion, the later read-only snapshot should export the
`system`, `secure` and `global` namespaces separately with `settings list
system`, `settings list secure` and `settings list global`. These exports are
device data and must remain private rather than being committed to the public
repository.

The repository's `tools/disable.sh` additionally records the state needed for
its selected list under:

```text
~/.cleanyourtv/
```

That recovery record includes, per listed package:

- Enabled, already-disabled or absent state
- Current Home component
- Window, transition and animator scales
- Known stock launcher state

The backup and recovery record must be kept together with the exact list that
was applied. The restore script cannot reconstruct changes made outside its
recorded list.

The repository also requires:

- Install and confirm the replacement Home before disabling stock Home.
- Keep TV settings, TV/input middleware, Bluetooth/remote pairing, keyboard,
  Play Store and Play Services enabled.
- Never uninstall packages.
- Never disable core Android packages or TV middleware.
- Close debugging and verify the relevant ADB ports are closed after approved
  work.

No backup was taken in Phase 3A because the TV was not contacted.

## Xiaomi/Android 9-era risks

### Firmware divergence

Factory Android 9 does not prove the current Android release. OTA updates may
change package names, Home resolution, input services and permissions.

### 2 GB RAM and GPU

The launcher is small, but the following still require validation:

- 4K wallpaper decode cost
- `wallpaper_true_color`
- `advanced_effects`
- Six large dock tiles
- App banner memory use
- Startup time after a cold boot

If frame pacing or memory pressure is poor, the least-invasive fallback is:

1. Keep the static wallpaper.
2. Set `reduced_motion: true`.
3. Consider `advanced_effects: false`.
4. Consider `wallpaper_true_color: false` if dark gradients band acceptably.

These are test decisions, not changes made by this report.

### Android 9 and theme transfer

The repository's theme script contains compatibility paths for older Android
versions, including Android 8/9 content-transfer behavior. That still requires
an approved device session and must not be tested now.

### USB port and network risk

The troubleshooting documentation records Xiaomi devices whose USB ports lose
power or change behavior while USB debugging is enabled. Ask what is attached
to every USB port and whether networking uses USB Ethernet before enabling
debugging. Prefer a route that does not interrupt the network, and finish any
approved session in one sitting if the adapter/debugging path is fragile.

### Home loss

Disabling the wrong Home or PatchWall package can leave a chooser, fallback
screen or unusable Home after reboot. The Home guard in
[launcher/build-and-install.sh](../../launcher/build-and-install.sh) must not
be bypassed with a force option without explicit approval.

### Inputs and remote

The 65-inch product specification lists HDMI, AV and tuners, but it does not
prove the Android input-service package names or IDs. Every HDMI/AV/tuner path
must be tested with the real remote after installation; an injected key is not
proof for accessibility-mediated keys.

### Firmware updates

Keep the firmware updater enabled initially. Firmware updates can restore
packages or return Home to the stock launcher, so post-update drift must be
checked separately.

## Installation-readiness checklist

Installation is **not ready** until every required item is checked:

- [ ] Exact model read from the TV matches `L65M5-5SIN`.
- [ ] Current Android release recorded.
- [ ] Current SDK/API recorded and is at least 26.
- [ ] Current firmware/build and fingerprint recorded.
- [ ] Current Home resolver and all HOME claimants recorded.
- [ ] Stock launcher package and component recorded.
- [ ] PatchWall package/component relationship documented.
- [ ] HDMI/AV/tuner input services and IDs recorded.
- [ ] Available `/data` and `/sdcard` storage recorded.
- [ ] USB devices and USB Ethernet usage recorded.
- [ ] USB/Wireless debugging availability confirmed without enabling it yet.
- [ ] Full package, disabled-package, property and settings backup plan ready.
- [ ] Preferred dock apps identified by actual package and launchable TV activity.
- [ ] Two remaining dock slots explicitly decided or left empty.
- [ ] Properly licensed/user-supplied wallpaper available locally.
- [ ] Wallpaper contrast and 4K memory plan reviewed.
- [ ] Launcher build environment available on the Mac.
- [ ] Stock Home recovery path demonstrated or documented.
- [ ] No package-disable list approved; initial installation will avoid debloating.
- [ ] Explicit approval given for the installation phase.

## Required evidence before Phase 4

The following artifacts should exist before any installation command is
considered:

1. A dated device identity snapshot.
2. Read-only package and launcher inventory.
3. Read-only TV-input inventory.
4. Storage snapshot.
5. Private full backup outside Git.
6. Confirmed recovery record strategy and stock Home package.
7. Final dock package/activity list.
8. Locally stored licensed wallpaper and its provenance.
9. A decision on `advanced_effects` and `reduced_motion` for this hardware.
10. Your explicit approval to install the launcher.

Until then, the project remains in compatibility planning.
