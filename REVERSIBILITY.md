# Reversibility and device state

This document records changes actually made to the Xiaomi TV and verified
rollback commands. It does not claim that every recovery path has been tested.

## Target

- Serial: `192.168.1.8:5555`
- Model: `MiTV-AXSO0`
- Android: 9, SDK 28
- User: 0

## Current verified package state

- `com.gathin.tvhome`: installed and enabled.
- `com.google.android.tvlauncher`: disabled-user for user 0.
- `com.mitv.tvhome.atv`: enabled and untouched.
- `com.mitv.tvhome.mitvplus`: enabled and untouched.
- Current resolved HOME: `com.gathin.tvhome/.MainActivity`.
- Physical Home opens CleanYourTV.

## Changes made

CleanYourTV was installed as a test APK. The Luffy theme was transferred to:

```text
/sdcard/Android/data/com.gathin.tvhome/files
```

The theme configuration and wallpaper checksums matched after transfer.
Google TV was then disabled only for user 0:

```bash
adb -s 192.168.1.8:5555 shell pm disable-user --user 0 \
  com.google.android.tvlauncher
```

The TV was rebooted once after that test. ADB returned, CleanYourTV remained
the resolved Home activity, and Xiaomi TV Home appeared in the foreground
briefly during startup.

## Verified Google TV restoration

This is the verified restoration command:

```bash
adb -s 192.168.1.8:5555 shell pm enable --user 0 \
  com.google.android.tvlauncher
```

It restores Google TV's enabled state for user 0. It may not by itself select
Google TV as the preferred Home activity; verify with:

```bash
adb -s 192.168.1.8:5555 shell cmd package resolve-activity --brief \
  --user 0 -a android.intent.action.MAIN \
  -c android.intent.category.HOME
```

Do not run restoration or takeover commands without approval. Keep the
previous Home resolver and package states recorded before changing them.

## Accessibility rollback

Verified current accessibility state:

```text
enabled_accessibility_services = null
accessibility_enabled = 0
```

HomeKeyService is not enabled. If it is later enabled, rollback must restore
the exact prior service list, not blindly clear other services. For the
currently recorded state, the rollback would be:

```bash
adb -s 192.168.1.8:5555 shell settings put secure \
  enabled_accessibility_services ''
adb -s 192.168.1.8:5555 shell settings put secure \
  accessibility_enabled 0
```

This procedure has not been used as a future-service rollback test.

## Startup limitation and pending work

PatchWall starts during boot through Xiaomi receivers, so it may appear before
CleanYourTV even though CleanYourTV resolves HOME. CleanYourTV has no native
`BOOT_COMPLETED` receiver. The local theme now contains
`"power_on_home_always": true`, but that latest configuration has not yet been
transferred to the TV. HomeKeyService is also not enabled.

Automatic startup with HomeKeyService and `power_on_home_always` is pending and
has not been tested. Do not describe it as working until a controlled reboot
verifies it.
