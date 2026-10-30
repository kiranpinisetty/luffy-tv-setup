# TV setup and verification

This document records commands verified against the Xiaomi `MiTV-AXSO0`
running Android 9 / SDK 28 at `192.168.1.8:5555`. Commands marked
**read-only** do not change the TV. Commands marked **changes TV** require
explicit approval.

## ADB connection

From `/Users/Kiran/tv/cleanyourtv`:

```bash
TV_HOST=192.168.1.8 tools/venv/bin/python tools/tv.py --probe
```

This uses the existing authenticated Python ADB helper and is read-only. It
reports model, Android release, SDK, manufacturer, secure ADB state, and TCP
port.

The native client is:

```text
/Users/Kiran/Library/Android/sdk/platform-tools/adb
```

## Device and package inspection

Read-only examples:

```bash
adb -s 192.168.1.8:5555 shell am get-current-user
adb -s 192.168.1.8:5555 shell pm list packages
adb -s 192.168.1.8:5555 shell pm list packages -e
adb -s 192.168.1.8:5555 shell dumpsys package com.gathin.tvhome
adb -s 192.168.1.8:5555 shell df -h /data
```

Verify the launcher package and activity:

```bash
adb -s 192.168.1.8:5555 shell pm path com.gathin.tvhome
adb -s 192.168.1.8:5555 shell cmd package resolve-activity --brief \
  --user 0 -p com.gathin.tvhome \
  -a android.intent.action.MAIN -c android.intent.category.HOME
```

Resolve the current Home activity:

```bash
adb -s 192.168.1.8:5555 shell cmd package resolve-activity --brief \
  --user 0 -a android.intent.action.MAIN \
  -c android.intent.category.HOME
```

Launch CleanYourTV explicitly (changes only the foreground activity):

```bash
adb -s 192.168.1.8:5555 shell am start -W \
  -n com.gathin.tvhome/.MainActivity
```

Check the foreground activity read-only:

```bash
adb -s 192.168.1.8:5555 shell dumpsys activity activities \
  | grep -m 1 -E 'mResumedActivity|ResumedActivity'
```

## APK installation

Only with explicit approval:

```bash
adb -s 192.168.1.8:5555 install \
  /Users/Kiran/tv/luffy-gear5/themes/luffy-gear5-deployable/cleanyourtv-release.apk
```

This changes the TV by installing or updating the APK. Verify afterward with
`pm path` and an explicit activity launch. Do not set Home automatically as
part of an installation unless separately approved.

## Theme transfer

Only with explicit approval, from the CleanYourTV checkout:

```bash
cd /Users/Kiran/tv/cleanyourtv
TV_HOST=192.168.1.8 tools/theme.sh \
  /Users/Kiran/tv/luffy-gear5/themes/luffy-gear5-deployable
```

This changes the TV. The verified destination is:

```text
/sdcard/Android/data/com.gathin.tvhome/files
```

The script writes `config.json` at that directory and accepted wallpaper
files under `wallpapers/`. It restarts CleanYourTV Home to reload the theme.
Verify without changing anything:

```bash
adb -s 192.168.1.8:5555 shell cat \
  /sdcard/Android/data/com.gathin.tvhome/files/config.json
adb -s 192.168.1.8:5555 shell find \
  /sdcard/Android/data/com.gathin.tvhome/files -maxdepth 2 -type f -print
```

## Accessibility and startup diagnosis

Read-only:

```bash
adb -s 192.168.1.8:5555 shell settings --user 0 get secure \
  enabled_accessibility_services
adb -s 192.168.1.8:5555 shell settings --user 0 get secure \
  accessibility_enabled
adb -s 192.168.1.8:5555 shell dumpsys accessibility
```

CleanYourTV has no `BOOT_COMPLETED` receiver. Its supported optional startup
path is `HomeKeyService`, component
`com.gathin.tvhome/.HomeKeyService`, together with the theme key
`power_on_home_always`. Enabling the service changes secure settings and
requires explicit approval. Xiaomi PatchWall has its own boot receivers and
may appear briefly before CleanYourTV.

## Reboot and Home changes

These are device-changing commands and require explicit approval:

```bash
adb -s 192.168.1.8:5555 shell cmd package set-home-activity \
  --user 0 com.gathin.tvhome/.MainActivity
adb -s 192.168.1.8:5555 reboot
```

The Xiaomi firmware continues to resolve Google TV while it is enabled because
its HOME filter has priority 2 and CleanYourTV has priority 0. The tested
reversible takeover disabled only Google TV; do not repeat or extend that
procedure without approval.
