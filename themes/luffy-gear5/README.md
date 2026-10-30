# Luffy Gear 5 theme draft

> **Historical draft note:** This file describes the pre-inventory design
> draft. The verified current state is in the workspace
> [PROJECT-STATUS.md](../../PROJECT-STATUS.md). The target was later verified
> as `MiTV-AXSO0`; the theme was built, installed, and transferred during
> subsequent work.

This is a local, uninstalled draft for a One Piece / Monkey D. Luffy-themed
CleanYourTV home screen targeting a Xiaomi Mi TV X 65-inch Android TV. The
exact model identifier and firmware remain unverified. It is intentionally
outside the upstream checkout:

```text
/Users/Kiran/tv/luffy-gear5/themes/luffy-gear5/
```

## Local preview

[Open the 1920x1080 preview](./preview.svg). It is an original abstract
composition used only to judge layout, contrast and spacing. It is not a
launcher screenshot and does not represent installed applications or final
licensed artwork.

## Design direction

- Dark charcoal/navy background with white interface text.
- Red-and-gold Haki-inspired visual direction, with the caveat that ordinary
  themes do not expose a custom focus-color key.
- Full-screen, dark cinematic Gear 5 Luffy artwork supplied locally and
  licensed or user-owned.
- Six-slot horizontal glass dock with a modest 70% opacity.
- Six columns and no visible grid row below the dock, keeping the home screen
  spacious on a 65-inch display.
- Straight, non-overshooting focus movement.
- Existing basic and advanced launcher effects enabled, with the documented
  ambient background glow and per-app glow enabled for a restrained focused
  app emphasis.
- Date on its own line and full-white clock header for readability.
- No idle clock takeover and no greeting, reducing startup and visual clutter.

## Pending app selection

The intended order is YouTube, Netflix, Prime Video, JioHotstar, then two
unselected slots. `dock_apps` is intentionally still an empty list.
Application package names must be obtained from the exact TV during the later,
explicitly approved compatibility phase. Do not copy package names from
another Xiaomi model or from an example theme.

`fill_dock` is `false` so unknown or missing applications cannot silently
populate the dock with unintended apps.

## Pending artwork

No wallpaper file is included. `wallpaper` is therefore `false` until a
properly licensed or user-supplied static image is available.

Required artwork characteristics:

- Static 16:9 image, preferably 3840x2160 and no smaller than 1920x1080.
- Locally stored `wallpaper.jpg` or a local `wallpapers/` collection.
- Character and bright cloud detail kept clear of the clock and dock.
- Darkened lower area or sufficient contrast for white dock text.
- Written permission, an appropriate license, or user ownership documented
  before use.
- No video wallpaper, remote download, tracker, or continuously generated
  effect.

When licensed artwork is available, place it here:

```text
/Users/Kiran/tv/luffy-gear5/themes/luffy-gear5/wallpaper.jpg
```

Then change `"wallpaper": false` to `"wallpaper": true` after local asset
validation. No wallpaper has been downloaded for this draft.

## Focus behavior and implementation boundary

The existing launcher supports the requested movement and restrained focus
behavior through:

- `animations`: basic focus, glass, title and tile-entry motion.
- `advanced_effects`: existing tile lift, icon zoom, smooth ambient-color
  transitions and app-opening effects.
- `focus_overshoot: false`: direct movement without springing past the target.
- `reduced_motion: false`: balanced motion for the initial design; a
  reduced-motion variant can be tested later.

The existing theme configuration does **not** provide a general
red-and-gold focus color for ordinary themes. `app_glow` uses the focused
application's color, and `ambient_glow` also derives its color from the focused
app. The documented `accent_color` key belongs to the festive skin, not a
general Luffy theme. Therefore this draft does not pretend to implement a
Haki-colored spotlight.

The least invasive implementation is to keep the current theme-only settings,
use red/gold in the licensed wallpaper and dock artwork, and use the existing
white focus treatment with direct movement. A Java change could add a custom
focus palette, but it would require source changes, a new build, and device
validation for a decorative effect; it is not justified before compatibility
testing.

See [design-spec.md](./design-spec.md) for the confirmed requirements and
supportability decisions.

## Verified configuration keys used

These keys are documented by the checked-out `cleanyourtv/themes/README.md`
and read by the checked-out launcher `Config.java`:

- Dock: `dock_apps`, `fill_dock`, `columns`, `rows_below_dock`, `dock_row`,
  `dock_title`, `dock_glass`, `dock_opacity`
- Appearance: `background_color`, `wallpaper`, `wallpaper_true_color`,
  `wallpaper_dim`
- Motion/focus: `animations`, `advanced_effects`, `reduced_motion`,
  `focus_overshoot`
- Clock/date: `date_below_clock`, `header_white`, `show_greeting`,
  `home_idle_clock_minutes`
- Glow: `ambient_glow`, `app_glow`

The theme does not invent a clock-format, seconds, character-tracking,
eye-tracking, boot-animation, or ordinary-theme custom accent-color key. Those
features are not part of this draft.

## Applying and resetting later

Do not run these commands during Phase 1. After device compatibility review and
explicit installation approval, the repository documents the following
device-facing operations:

```bash
TV_HOST=<tv-ip> tools/theme.sh /Users/Kiran/tv/luffy-gear5/themes/luffy-gear5
TV_HOST=<tv-ip> tools/theme.sh --reset
```

`tools/theme.sh` validates that `config.json` is a JSON object, transfers the
theme files to the launcher's application storage, clears old wallpaper
collections, and restarts Home. `--reset` removes the overlay and wallpaper
files before restarting Home. Neither command was run in Phase 1.
