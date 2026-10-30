# Phase 6 Luffy Gear 5 theme report

Status: local preparation only. No APK was built, no device was contacted,
and no theme was pushed or installed.

## 1. Files changed

Changed in the separate Luffy workspace:

- `themes/luffy-gear5/config.json`
  - Enabled the verified `ambient_glow` and `app_glow` settings.
  - Kept `dock_apps` empty because the target TV has not been inventoried.
  - Kept `wallpaper` disabled because no approved artwork is available.
- `themes/luffy-gear5/README.md`
  - Updated the description of the enabled glow settings.

Created:

- `phase-6-theme-report.md`

No file in `/Users/Kiran/tv/cleanyourtv` was changed.

## 2. Authoritative schema review

The checked-out source of truth was reviewed in:

- `cleanyourtv/themes/README.md`
- `cleanyourtv/launcher/app/src/main/java/com/gathin/tvhome/Config.java`
- `cleanyourtv/launcher/app/src/main/res/values/config.xml`
- Built-in examples including `themes/six-columns/config.json` and
  `themes/aurora/config.json`

The Luffy configuration uses only documented/readable properties:

| Group | Keys used | Result |
|---|---|---|
| Dock | `dock_apps`, `fill_dock`, `columns`, `rows_below_dock`, `dock_row`, `dock_title`, `dock_glass`, `dock_opacity` | Six-column glass dock; empty unknown slots remain empty. |
| Background | `background_color`, `wallpaper`, `wallpaper_true_color`, `wallpaper_dim` | Dark static background; wallpaper waits for approved artwork. |
| Motion | `animations`, `advanced_effects`, `reduced_motion`, `focus_overshoot` | Smooth direct focus movement with advanced effects enabled. |
| Clock/date | `date_below_clock`, `header_white`, `show_greeting`, `home_idle_clock_minutes` | Readable white clock/date, no greeting or idle takeover. |
| Glow | `ambient_glow`, `app_glow` | Existing focused-app and ambient glow enabled. |

No invented red/gold focus-color key, seconds-clock key, character-tracking
key, boot-animation key, or video-wallpaper key was added.

## 3. Final local configuration

```json
{
  "dock_apps": [],
  "fill_dock": false,
  "columns": 6,
  "rows_below_dock": 0,
  "dock_row": true,
  "dock_title": false,
  "dock_glass": true,
  "dock_opacity": 70,
  "background_color": "#080A10",
  "wallpaper": false,
  "wallpaper_true_color": true,
  "wallpaper_dim": 18,
  "icon_only_buttons": false,
  "ambient_glow": true,
  "app_glow": true,
  "focus_overshoot": false,
  "animations": true,
  "advanced_effects": true,
  "reduced_motion": false,
  "date_below_clock": true,
  "header_white": true,
  "show_greeting": false,
  "home_idle_clock_minutes": 0
}
```

The six-slot intent is represented by `columns: 6` and `fill_dock: false`.
The four preferred apps remain documented in the README, but no package names
are inserted into `dock_apps`.

## 4. Wallpaper status

No wallpaper file exists in the Luffy theme directory. The local preview SVG
is an abstract layout mockup and is not suitable as the launcher wallpaper.

The required future asset is:

- a static landscape 16:9 image;
- 3840x2160 preferred, with at least 1920x1080 as a practical floor;
- Gear 5 Luffy artwork supplied by the owner or covered by a suitable license;
- bright white hair and cloud-like effects;
- dark cinematic treatment with sufficient contrast for white interface text;
- the character and important details kept above or away from the dock area;
- locally stored as `wallpaper.jpg` or in a supported local `wallpapers/`
  collection.

Until that asset and its provenance are provided, `"wallpaper": false` is the
correct safe setting. No image was downloaded or fabricated.

## 5. Dock package status

The intended order remains:

1. YouTube
2. Netflix
3. Amazon Prime Video
4. JioHotstar
5. Empty
6. Empty

No authoritative local documentation identifies the installed package names
for this specific `L65M5-5SIN` TV. Package-name matches in other CleanYourTV
examples are not sufficient evidence and were not copied.

The missing information is a later approved read-only device inventory that
records, for each app, its installed package, visible label/version, enabled
state, and launchable TV/Leanback activity. Until then, `dock_apps: []` is
intentional and prevents accidental app substitution.

## 6. Validation results

Local validation performed:

- JSON syntax: passed.
- Top-level JSON type: object, as required by the theme loader.
- Theme keys: all keys used are documented in the checked-out theme schema
  and handled by the launcher configuration parser.
- Value ranges:
  - `columns: 6` is within the documented 1-8 range.
  - `rows_below_dock: 0` is within the documented 0-3 range.
  - `dock_opacity: 70` is within the documented 30-100 range.
  - `wallpaper_dim: 18` is within the documented 0-90 range.
- Dock consistency: `dock_apps` is empty and `fill_dock` is false, so no
  unverified applications can fill the dock.
- Wallpaper path/dimensions: not applicable; no wallpaper file exists.
- Theme transfer shape: compatible with `tools/theme.sh`, which requires a
  theme directory containing an object-valued `config.json` and optionally
  supported local wallpaper files.

The following were deliberately not run:

- `tools/theme.sh`
- `tools/tv.py`
- ADB or network discovery
- APK build or installation

## 7. Exact next steps for later device application

After the compatibility inventory is approved and reviewed:

1. Obtain or provide the licensed/user-supplied wallpaper and record its
   provenance.
2. Place the validated local image in
   `themes/luffy-gear5/wallpaper.jpg` or a supported wallpaper collection.
3. Validate its dimensions and file format locally.
4. Set `"wallpaper": true` only after the asset is present and approved.
5. Use the read-only device inventory to identify the four actual package
   names and launchable TV activities.
6. Update `dock_apps` with those verified package names in the requested
   order; leave the last two positions empty.
7. Re-run JSON/schema, asset, and dock consistency validation.
8. Only after explicit installation approval, use the repository’s documented
   theme-transfer procedure against the already-installed compatible
   CleanYourTV launcher.
9. Keep the reset procedure and recovery record available before any push.

This theme overlay does not itself install or replace the launcher. Whether an
APK build is needed remains dependent on the later TV inventory and whether a
compatible CleanYourTV launcher is already installed.
