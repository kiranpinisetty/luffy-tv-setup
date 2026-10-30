# Phase 7A wallpaper report

Status: local validation only. No APK was built, no TV was contacted, and no
theme was pushed or installed.

## 1. Wallpaper discovery and metadata

The generated source wallpaper was found at:

`/Users/Kiran/tv/luffy-gear5/themes/wallpaper.png`

Observed metadata:

- Format: PNG
- Dimensions: 1672 × 941 pixels
- Aspect ratio: approximately 16:9
- Colour: 8-bit RGB, 3 samples per pixel
- File size: 2,239,182 bytes
- SHA-256:
  `9cd55eff6a34b9958fa07a4ec06383507381ba4303efbbd0e00e30345dbc55cf`

The image is landscape and close to 16:9. It is below the launcher’s
approximately 8.4-million-pixel safety ceiling, but it is larger than the
theme documentation’s approximately 300 KB recommendation for low-memory
TVs. It is also below the preferred 3840×2160 source resolution. This is
valid for local loading, but performance and visual quality still require
later TV validation.

## 2. CleanYourTV loading behavior

The checked-out implementation and documentation were inspected in:

- `cleanyourtv/themes/README.md`
- `cleanyourtv/launcher/app/src/main/java/com/gathin/tvhome/Config.java`
- `cleanyourtv/launcher/app/src/main/java/com/gathin/tvhome/WallpaperPlayer.java`
- `cleanyourtv/tools/theme.sh`

Verified behavior:

- A single wallpaper uses the exact filename `wallpaper.jpg` beside
  `config.json`.
- A wallpaper collection uses JPEG, PNG, or WebP files in a `wallpapers/`
  directory beside `config.json`.
- Collection loading requires a positive `wallpaper_rotation_minutes` value.
- The launcher center-crops images to fill the display.
- `wallpaper_dim` is applied as a black overlay below launcher text.
- `wallpaper_true_color` controls RGB_565 versus ARGB_8888 decoding.
- `tools/theme.sh` transfers root `wallpaper.jpg` and supported files from the
  `wallpapers/` directory; it does not transfer a root `wallpaper.png`.

The original source file was not renamed or deleted. An unchanged copy was
created at the supported collection path:

`/Users/Kiran/tv/luffy-gear5/themes/luffy-gear5/wallpapers/luffy-gear5.png`

The source and collection copy have identical SHA-256 hashes.

## 3. Configuration change

Updated:

`/Users/Kiran/tv/luffy-gear5/themes/luffy-gear5/config.json`

Changes:

```json
{
  "wallpaper": true,
  "wallpaper_rotation_minutes": 1440
}
```

The one-image collection is loaded through the documented collection
mechanism. The 1440-minute interval is a supported positive value and avoids
the launcher’s collection-disabled path for a zero interval.

All existing Luffy settings were preserved:

- six-column layout;
- empty `dock_apps`;
- `fill_dock: false`;
- glass dock and 70% opacity;
- animations and advanced effects;
- direct focus movement;
- ambient and app glow;
- clock/date settings.

## 4. Validation results

Passed:

- Source wallpaper exists.
- Source format is PNG.
- Source dimensions are 1672×941.
- Source is landscape and approximately 16:9.
- Collection copy exists at the exact supported path.
- Source and collection copy hashes match.
- Collection filename uses the allowed safe character set.
- JSON syntax is valid.
- Top-level JSON value is an object.
- All configuration keys are documented/read by the checked-out launcher.
- `wallpaper: true` is paired with a positive supported rotation interval.
- Existing dock, motion, glow, clock, and date settings remain present.
- No package names were added.

Advisory limitations:

- The asset is 2.24 MB, above the documentation’s approximately 300 KB
  recommendation.
- The asset is 1672×941 rather than preferred 3840×2160.
- Actual decode performance, crop quality, readability, and memory use have
  not been tested on the 2 GB Xiaomi TV.

## 5. Files changed or created

Changed:

- `themes/luffy-gear5/config.json`

Created:

- `themes/luffy-gear5/wallpapers/luffy-gear5.png`
  - Unchanged copy of the supplied source image.
- `phase-7a-wallpaper-report.md`

The original `/Users/Kiran/tv/cleanyourtv` checkout was not modified.
The original source `/Users/Kiran/tv/luffy-gear5/themes/wallpaper.png` was
not deleted or renamed.

## 6. Next steps

1. Review the image quality and provenance.
2. If the file is owner-supplied or appropriately licensed, keep the local
   collection copy.
3. Complete the approved read-only TV inventory.
4. Evaluate whether the 2.24 MB asset is acceptable on the 2 GB TV. If not,
   prepare an optimized local derivative without replacing the original.
5. Verify the four actual dock package names and launchable TV activities.
6. Revalidate the theme after dock packages are known.
7. Only with explicit device-application approval, use the repository’s
   documented theme-transfer mechanism.

No theme transfer or device operation was performed in this phase.
