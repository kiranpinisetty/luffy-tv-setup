# Development workflow

## Source and deployable theme

Edit the source theme first:

```text
themes/luffy-gear5/
```

Propagate the complete validated configuration and supported assets to:

```text
themes/luffy-gear5-deployable/
```

The two `config.json` files must remain identical. The deployable directory
also contains the installation candidate APK and the Phase 8 inventory; those
are artifacts, not theme schema inputs.

## Configuration

`dock_apps` is an ordered list of package names. `fill_dock: false` leaves
unused slots empty. Every other key must be verified against
`/Users/Kiran/tv/cleanyourtv/launcher/app/src/main/java/com/gathin/tvhome/Config.java`
and `cleanyourtv/themes/README.md`.

The current verified dock is YouTube, Netflix, Prime Video, and JioHotstar.
`power_on_home_always` is supported by the parser and is currently enabled in
both local configs. It requires the optional HomeKeyService on the TV.

## Validation

```bash
python3 -m json.tool themes/luffy-gear5/config.json >/dev/null
python3 -m json.tool themes/luffy-gear5-deployable/config.json >/dev/null
cmp -s themes/luffy-gear5/config.json \
  themes/luffy-gear5-deployable/config.json
```

Check wallpapers with:

```bash
file themes/luffy-gear5/wallpapers/*
find themes/luffy-gear5-deployable/wallpapers -type f -print
```

`tools/theme.sh` accepts `.jpg`, `.jpeg`, `.png`, and `.webp` collection
files. It writes them under the launcher's `wallpapers/` directory.

## APK build

The launcher source is in `/Users/Kiran/tv/cleanyourtv/launcher`. Its Gradle
configuration uses compile/target SDK 34, min SDK 26, Java 17, and application
ID `com.gathin.tvhome`.

```bash
cd /Users/Kiran/tv/cleanyourtv/launcher
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home
export ANDROID_HOME=/Users/Kiran/Library/Android/sdk
./gradlew :app:assembleRelease
```

The release output is `app/build/outputs/apk/release/app-release.apk`.
Copy it to the deployable workspace with a new filename; do not overwrite a
previous candidate without approval.

Verify an APK locally:

```bash
aapt2 dump badging path/to.apk | head
apksigner verify --verbose --print-certs path/to.apk
stat -f '%z' path/to.apk
```

## Packaging and deployment

The existing archive is `luffy-gear5-theme.zip`. Inspect its contents before
replacing it. Theme transfer uses the existing CleanYourTV script, not a new
copy mechanism:

```bash
cd /Users/Kiran/tv/cleanyourtv
TV_HOST=192.168.1.8 tools/theme.sh \
  /Users/Kiran/tv/luffy-gear5/themes/luffy-gear5-deployable
```

This is a TV modification and requires explicit approval. Verify the remote
destination and checksums afterward as described in [TV-SETUP.md](TV-SETUP.md).

## Testing discipline

Run local JSON and file validation before any device operation. Use the
smallest relevant launcher build or test command. Keep source and deployable
configurations synchronized. Never infer a TV package name from another model.
Do not modify application source for a theme-only feature.
