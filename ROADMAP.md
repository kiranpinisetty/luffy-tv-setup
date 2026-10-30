# Roadmap

The following ideas are planned only. They are not implemented and must not be
described as current features.

## Gear 5 visual experience

- Dark cinematic background refinements.
- Luffy character artwork improvements.
- Eye-opening animation when the launcher loads.
- Glowing eyes.
- Independent eye and pupil layers.
- Pupil movement based on the focused application.
- Idle blinking.
- Smooth transitions between app selections.

## Launcher improvements

- Better dock animations.
- Smoother remote-control navigation.
- More expressive app focus effects.
- Additional custom wallpapers.
- A startup animation.
- Performance optimization for Android 9 TV hardware.

## Intended eye-tracking approach

1. Keep eyes and pupils as separate transparent artwork layers.
2. Detect the focused dock item using the launcher’s existing focus state.
3. Animate pupil positions toward the selected application.
4. Bound animation work and bitmap sizes for the target TV hardware.

This would require source changes and device validation. It is not a
theme-only feature currently supported by CleanYourTV.
