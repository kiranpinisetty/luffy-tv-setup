# Luffy Gear 5 design specification

Status: local design specification only. No TV compatibility or installation
work has started.

## Target

- Device description: Xiaomi Mi TV X 65-inch Android TV.
- Exact model identifier: pending verification.
- Firmware/build/API level: pending verification.
- Launcher: existing CleanYourTV launcher.
- Implementation preference: theme configuration before Java changes.

## Confirmed visual design

- Full-screen Gear 5 Luffy artwork with a dark cinematic treatment.
- Artwork must be licensed, user-owned, or supplied by the user. Until then,
  use only the original abstract preview and keep the real wallpaper disabled.
- Dark background with white, readable interface text.
- Clock and date remain visually separate and readable.
- Six-slot horizontal translucent dock at the bottom.
- Preferred order:
  1. YouTube
  2. Netflix
  3. Prime Video
  4. JioHotstar
  5. Unselected
  6. Unselected
- Actual package names remain intentionally unspecified until device checks.
- Smooth remote navigation without focus overshoot.
- Red-and-gold Haki-inspired overall visual language.

## Current theme configuration

The draft [config.json](./config.json) uses six columns and an empty
`dock_apps` list. `fill_dock` is false so the launcher cannot silently fill
the six intended slots with unknown applications.

The following existing launcher behavior is used:

| Requirement | Existing support | Draft decision |
|---|---|---|
| Six-slot layout | `columns` supports six; `dock_apps` controls order | `columns: 6`; packages pending |
| Translucent dock | `dock_glass`, `dock_opacity` | Enabled at 70% |
| Smooth movement | `animations`, `focus_overshoot` | Animations on; overshoot off |
| Tile lift/icon zoom | `advanced_effects` | Enabled for initial preview |
| Reduced motion fallback | `reduced_motion` | Off initially; test later if needed |
| Clock/date readability | `date_below_clock`, `header_white` | Date below clock; white header |
| Dark background | `background_color` | `#080A10` |
| Full-screen artwork | `wallpaper`, local wallpaper files | Disabled until artwork is supplied |

## Haki focus assessment

The current theme schema supports focus movement and existing visual effects,
but it does not expose a normal-theme red-and-gold focus palette:

- `focus_overshoot` controls movement behavior, not color.
- `animations` controls basic motion.
- `advanced_effects` controls existing lift, zoom and color transitions.
- `app_glow` derives color from the focused app.
- `ambient_glow` derives the dock glow from the focused app.
- `accent_color` is documented for the festive skin, not ordinary themes.
- `cinematic_white_focus` applies only with `cinematic_home` and preserves a
  white focus treatment; it is not a red/gold palette.

### Least invasive recommendation

Keep the focus treatment theme-only and use:

```json
{
  "animations": true,
  "advanced_effects": true,
  "focus_overshoot": false,
  "ambient_glow": false,
  "app_glow": false
}
```

Use red and gold in the final licensed wallpaper and any supported local
artwork. This preserves startup, memory and compatibility characteristics.

### Optional future source change

A custom red-and-gold spotlight would require Java/resource changes to the
launcher’s focus rendering, followed by a rebuild and broader validation. It
would be decorative, not required for navigation or accessibility, so it
should not be attempted unless the theme-only result is reviewed and found
insufficient.

## Explicitly out of scope

- Guessing package names.
- Device-specific HDMI or remote mappings.
- Character or eye tracking.
- Blinking or boot animations.
- Video wallpaper.
- Background services.
- TV operations, ADB, installation and debloating.
