# Phase 3C read-only TV compatibility checklist

Status: preparation only. No TV connection, network discovery, ADB command,
developer-option change, setting change, package operation, build, install, or
device-facing project script was run.

## Current local findings

### Mac setup

- A standalone `adb` executable is not currently installed or available on
  `PATH`.
- CleanYourTV includes a Python ADB-over-TCP client at
  `cleanyourtv/tools/tv.py`. It does not require the standalone Google `adb`
  binary when used with its Python dependencies, but it does require an
  approved TV debugging connection and `TV_HOST`.
- The repository also supports an existing native ADB transport through
  `TV_ADB_BINARY`, but that is not available from the current local check.
- No packages were installed and no PATH or environment configuration was
  changed.

### Repository workflow assessment

The repository documents a pre-change snapshot in `PROMPT.md`: package list,
disabled-package list, `getprop` output, and settings should be saved before
anything is changed. That full backup is separate from the narrower restore
record made by the disable workflow.

`tools/tv.py` is a general transport wrapper. Its shell-command form can run
arbitrary commands, including destructive commands, so the wrapper itself
cannot be treated as read-only. Only explicitly selected inspection commands
are suitable for this phase.

The following command types are read-only when run exactly as shown in a
later, separately approved inventory session:

- `getprop` and selected `getprop` values
- `pm list packages` and `pm list packages -d`
- `cmd package resolve-activity ...`
- `cmd package query-activities ...`
- `dumpsys package <package>`
- `dumpsys activity ...`, `dumpsys window ...`, and selected input/status
  dumps
- `settings list`, `settings get`, and selected read-only settings queries
- `df -h /data /sdcard`

They query state but do not change packages, settings, Home ownership, files,
or app data. They still require debugging access and expose device metadata to
the local computer.

The following repository operations are **not** read-only and are prohibited
for this phase:

- `tools/disable.sh`
- `tools/restore.sh`
- `launcher/build-and-install.sh`
- `tools/theme.sh`
- `tools/tv.py --push`, `--pull`, `--screenshot`, `--reboot`, or arbitrary
  shell commands not individually reviewed
- `privacy.py --write`, because it creates a local debloat list and is not
  needed for compatibility inventory
- Any command containing `pm disable`, `pm enable`, `pm uninstall`,
  `pm install`, `cmd package set-home-activity`, `settings put`, `am start`,
  `input`, `reboot`, `content write/delete`, `rm`, or file-copy operations

`tools/privacy.py` audit mode performs package/settings/dumpsys queries and
prints a report, but it still opens an ADB connection and should not be used
until the owner approves the connection. Its `--write` mode must not be used.
The simplest inventory should use a reviewed, fixed list of read-only
queries rather than a general-purpose command batch.

## Information already supplied

These items are recorded from the existing project context and do not need to
be recollected unless the TV shows something different:

- Product: Xiaomi Mi TV 4X 65-inch, India model
- Model number: `L65M5-5SIN`
- Factory operating system: Android TV 9.0 Pie
- Factory resolution: 3840 x 2160
- Factory RAM: 2 GB
- Nominal storage: 16 GB
- Xiaomi describes the product as PatchWall with Android TV

The factory values do not establish the current firmware, installed Android
release, API level, available space, launcher owner, or package inventory.

## Manual TV checklist

Complete these checks on the physical TV without enabling debugging or
changing packages. Record the exact displayed text, not just a yes/no answer.

### 1. Device identity and software

Open the equivalent of **Settings → Device Preferences → About**. Firmware
menus may instead use **Settings → System → About**.

- [ ] Model/product name:
- [ ] Model number: expected `L65M5-5SIN`
- [ ] Android TV OS version:
- [ ] Android security patch level:
- [ ] Build number:
- [ ] Firmware/version string:
- [ ] Kernel or additional build information, if shown:
- [ ] Date and time of observation:

Do not press Build seven times during this checklist. Developer options and
debugging are not required for the manual evidence.

### 2. Storage

Open **Settings → Device Preferences → Storage**, or the equivalent Storage
page.

- [ ] Total internal storage:
- [ ] Used storage:
- [ ] Available/free storage:
- [ ] Largest applications shown:
- [ ] Any storage warning or update warning:

Nominal 16 GB storage is not the same as available space. The later
installation decision requires enough headroom for the APK, theme assets,
temporary transfer files, normal app updates, and rollback.

### 3. Current Home, stock launcher, and PatchWall

Without changing the default Home:

- [ ] What screen appears after pressing the remote Home button?
- [ ] Visible name/branding of that screen:
- [ ] Does a dedicated PatchWall button open PatchWall?
- [ ] If PatchWall opens, does it replace Home or appear as a separate content
      screen?
- [ ] Does pressing Back from PatchWall return to the previous screen or Home?
- [ ] In **Settings → Apps → See all apps**, record visible names and versions
      for the stock Home/launcher and PatchWall, if exposed:
- [ ] Do not disable, force-stop, uninstall, clear data, or change defaults.

The current Home package and component remain unknown until a later approved
read-only inventory or an equivalent TV UI screen exposes them.

### 4. Preferred applications

For each application, open **Settings → Apps → See all apps** and record the
visible app name and installed version. Do not infer package names from the
label and do not change app state.

| App | Installed? | Visible version | Enabled? | TV/leanback UI observed? |
|---|---:|---|---:|---:|
| YouTube | [ ] |  | [ ] | [ ] |
| Netflix | [ ] |  | [ ] | [ ] |
| Prime Video | [ ] |  | [ ] | [ ] |
| JioHotstar | [ ] |  | [ ] | [ ] |

These observations help, but they do not replace a later read-only activity
query. The final dock requires the actual package name and a launchable TV
activity for each app.

### 5. Remote, Settings, and inputs

Use the physical remote only; do not use an injected key or ADB input event.

- [ ] Home opens the expected current Home.
- [ ] Settings opens TV settings.
- [ ] Back returns from Settings without a stuck screen.
- [ ] D-pad navigation works.
- [ ] Volume and mute work.
- [ ] Input/Source opens the TV input selector.
- [ ] HDMI 1 can be selected, if connected.
- [ ] HDMI 2 can be selected, if connected.
- [ ] HDMI 3 can be selected, if connected.
- [ ] AV/Live TV entries, if shown, behave as expected.
- [ ] Microphone/voice button behavior, if present:
- [ ] Accessibility features currently in use:

Do not change input configuration, accessibility settings, remote pairing, or
developer options.

## Evidence still required

Before any computer-to-TV connection is considered, collect the manual fields
above. The following remain unknown even after the existing conversation
history:

- Current Android release and security patch
- Current build and firmware
- Current free storage
- Current Home/launcher identity
- PatchWall’s exact relationship to Home
- Installed versions and enabled state of the four preferred apps
- Remote and HDMI behavior on this exact TV
- Any active accessibility service or special remote configuration

After manual review, a separate explicit approval would still be required for
a read-only debugging inventory. That later inventory would collect, without
changing the TV:

1. `ro.build.version.sdk` and selected build properties
2. Installed and disabled package lists
3. Current Home resolver and all HOME claimants
4. Read-only package/activity evidence for the four preferred apps
5. Storage from `/data` and `/sdcard`
6. Relevant input-service and launcher component evidence
7. A dated local device snapshot stored outside Git

## Missing recovery prerequisites

Do not authorize installation until all of these are ready:

- The original Home package and component are identified.
- PatchWall’s package/component role is documented.
- The stock launcher remains enabled.
- A private device snapshot exists outside Git.
- The exact recovery path through TV Settings and the Android Home picker is
  documented for this firmware.
- The CleanYourTV recovery-record location and the exact applied list policy
  are understood if repository install tooling will later be used.
- The physical remote can reach Settings if the new Home fails.
- No debloat list is included in the initial launcher test.
- A licensed or user-supplied local wallpaper is available, or the theme
  remains wallpaper-free.
- The owner has separately approved installation after reviewing the evidence.

## Readiness decision

Current status: **not ready for device connection or installation**.

The safest next action is for the owner to complete the manual TV checklist
above and return the missing displayed values. A later read-only ADB
inventory, if approved separately, must use only reviewed inspection
commands. It must not enable debugging, alter settings, discover devices,
install or build an APK, push a theme, disable packages, or change Home
ownership as part of this phase.
