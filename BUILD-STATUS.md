# Luffy Gear 5 theme build status

## Status

The external theme package is prepared locally. No launcher APK was built,
and no TV connection, ADB command, theme push, or installation was performed.

## Deployable package

Directory:

`themes/luffy-gear5-deployable/`

Contents:

- `config.json`
- `wallpapers/luffy-gear5.jpg`

The package uses CleanYourTV's supported wallpaper collection mechanism. The
configuration keeps the dock package list empty until the target TV is
inventoried, so no package names have been guessed.

## Wallpaper

The supplied original remains unchanged at:

`themes/wallpaper.png`

The deployable package contains a local JPEG derivative at 1672x941 pixels,
approximately 366 KB, created to reduce transfer and decode cost. The original
PNG was preserved and is not included in the deployable package.

## Theme behavior

- Six-column dock
- Empty dock slots (`dock_apps: []`, `fill_dock: false`)
- Glass dock at 70% opacity
- Dark background
- Static wallpaper collection with a 1440-minute interval
- Wallpaper dimming at 18%
- Animations and advanced effects enabled
- Direct focus movement
- Existing ambient and app glow enabled
- White clock/date presentation

## Remaining TV-dependent items

- Verify current firmware and launcher compatibility.
- Identify actual YouTube, Netflix, Prime Video, and JioHotstar package names
  and launchable TV activities.
- Review wallpaper crop, contrast, startup time, memory use, and focus
  responsiveness on the 2 GB TV.
- Apply the package only after explicit device approval and recovery checks.
