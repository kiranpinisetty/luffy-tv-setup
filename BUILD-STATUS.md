# Luffy Gear 5 build status

The local CleanYourTV release APK and deployable theme were prepared and
tested on 2026-10-30. See [PROJECT-STATUS.md](PROJECT-STATUS.md) for the
authoritative current state and [DEVELOPMENT.md](DEVELOPMENT.md) for commands.

## Local artifacts

- Theme source: `themes/luffy-gear5/`
- Deployable theme: `themes/luffy-gear5-deployable/`
- Theme archive: `luffy-gear5-theme.zip`
- APK candidate: `themes/luffy-gear5-deployable/cleanyourtv-release.apk`
- APK package: `com.gathin.tvhome`, version `3.1`, version code `10`
- APK build: release, JDK 17, compile/target SDK 34, min SDK 26

The APK is signed with the Android debug certificate and is an installation
candidate, not a production distribution artifact.

## Current theme

- Four verified dock packages in order: YouTube, Netflix, Prime Video,
  JioHotstar
- `fill_dock: false`, leaving slots 5 and 6 empty
- Six columns, glass dock, dark background, wallpaper enabled
- `power_on_home_always: true` is present locally but has not been transferred
  since the latest configuration change

This file is retained as a short build record; detailed status belongs in
[PROJECT-STATUS.md](PROJECT-STATUS.md).
