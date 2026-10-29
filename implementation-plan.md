# Phase 3B implementation plan: Xiaomi Mi TV 4X 65

Status: planning only. This document does not authorize a build, device
connection, ADB session, installation, launcher change, package change, or
theme update.

## Scope and evidence status

The target is the Xiaomi Mi TV 4X 65-inch India model, `L65M5-5SIN`. The
owner-provided and Xiaomi-published hardware information is:

- Factory operating system: Android TV 9.0 Pie
- Resolution: 3840 x 2160
- RAM: 2 GB
- Storage: 16 GB

These are factory specifications, not a current runtime snapshot. The
installed Android release, API level, security patch, firmware/build,
launcher/Home resolver, PatchWall relationship, free storage, installed
applications, and launchable activities remain unverified.

The CleanYourTV source was inspected locally at
`/Users/Kiran/tv/cleanyourtv`. The separate Luffy workspace is
`/Users/Kiran/tv/luffy-gear5`. The original CleanYourTV checkout and the
existing Luffy files are unchanged by this phase.

Evidence labels used below:

- **Verified**: directly supported by checked-out source or documentation.
- **Inferred**: a reasonable conclusion from verified source and the reported
  factory specification, but not tested on this TV.
- **Unknown**: requires a read-only inspection of the actual TV.

## 1. Architecture and proposed changes

### Existing architecture

CleanYourTV is a native Android TV Home application:

- Plain Java launcher code in
  `cleanyourtv/launcher/app/src/main/java/com/gathin/tvhome`.
- AndroidX RecyclerView and dynamic animation dependencies.
- Runtime theme overlay loaded from the launcher's external files directory.
- Theme configuration overlays the built-in `config.xml`; malformed or
  non-object JSON is ignored rather than becoming launcher code.
- The launcher has no network permission and no background services according
  to `docs/launcher.md`.
- The package ID is normally `com.gathin.tvhome`.

The theme is therefore a data-and-assets customization, not a second launcher
implementation. The desired design should use the existing launcher and its
theme mechanism.

### Proposed implementation

1. Keep the upstream launcher Java and Android resources unchanged.
2. Keep the current Luffy theme configuration unchanged until the device
   compatibility evidence is collected.
3. Later, after the wallpaper provenance and TV inventory are approved:
   - add a properly licensed or user-supplied local `wallpaper.jpg`;
   - set the existing wallpaper-related keys for the final artwork;
   - populate `dock_apps` with verified package names in the requested order;
   - retain `fill_dock: false` so two undecided slots remain empty;
   - validate the JSON locally;
   - build the standard release APK without source changes.
4. Use the launcher’s existing Home selection and recovery flow for testing;
   do not disable PatchWall or the stock launcher during the initial plan.

No Java, Kotlin, layout, drawable, manifest, or resource modification is
currently required. A custom red-and-gold focus renderer would require source
and resource work and is not recommended for the first implementation.

## 2. Theme features: supported and unsupported

The authoritative theme schema is `cleanyourtv/themes/README.md`, and
`launcher/app/src/main/java/com/gathin/tvhome/Config.java` reads the overlay
values. The current Luffy draft is
`themes/luffy-gear5/config.json`.

### Supported by the existing theme system

The requested appearance can be represented with these verified keys:

| Requirement | Supported setting | Plan |
|---|---|---|
| Six dock slots | `columns`, `dock_apps`, `fill_dock` | Use six columns; provide four verified apps and leave two slots empty with `fill_dock: false`. |
| Requested app order | `dock_apps` | YouTube, Netflix, Prime Video, JioHotstar, then two empty positions. Package names must be discovered on the TV. |
| Translucent dock | `dock_row`, `dock_glass`, `dock_opacity` | Keep the dock as its own glass row with a restrained opacity. |
| Dark cinematic background | `background_color`, `wallpaper`, `wallpaper_dim` | Use a locally supplied licensed image, darkened for text and remote focus. |
| 4K colour quality | `wallpaper_true_color` | The current draft enables it; on 2 GB hardware this must be validated for memory and frame pacing. |
| Smooth remote focus movement | `animations`, `focus_overshoot` | Keep animations enabled. Choose overshoot based on actual remote responsiveness. |
| Tile lift and icon zoom | `advanced_effects` | Supported, but optional if the 2 GB device shows memory or frame-pacing pressure. |
| Reduced motion fallback | `reduced_motion` | Supported. Enable only if testing shows motion or accessibility problems. |
| Readable clock/date | `date_below_clock`, clock defaults | Keep the date on its own line and verify contrast against the artwork. |
| Focus-adjacent glow | `ambient_glow`, `app_glow` | Supported effects, but they are not a custom Haki palette. |

The current draft already contains the layout and motion choices, keeps
`dock_apps` empty, and has `fill_dock: false`. It also currently has
`wallpaper: false` because no approved artwork is present. This phase does not
change that file.

### Not supported by theme configuration alone

- A true custom red-and-gold focus spotlight for every focused tile.
- Character tracking, eye tracking, or camera-driven effects.
- Video wallpaper or continuous animated wallpaper.
- A custom boot animation.
- Guaranteed control over Xiaomi PatchWall’s internal visuals.
- Guaranteed replacement of a firmware-protected Home resolver.

The existing focus system supplies movement, optional overshoot, tile lift,
icon zoom, ambient glow, and app-opening effects. It does not expose a general
theme key for arbitrary focus colours. A Java/resource change could implement
red-and-gold rendering, but it would increase APK scope, testing surface, and
firmware risk for a decorative effect. The safest recommendation is to use
the dark wallpaper, white launcher focus treatment, and restrained existing
glow first.

## 3. Six-slot dock configuration

The intended logical order is:

1. YouTube
2. Netflix
3. Prime Video
4. JioHotstar
5. Empty
6. Empty

The final JSON should use the actual package names found on the target TV:

```json
{
  "columns": 6,
  "dock_apps": [
    "<verified YouTube package>",
    "<verified Netflix package>",
    "<verified Prime Video package>",
    "<verified JioHotstar package>"
  ],
  "fill_dock": false
}
```

The angle-bracket values above are documentation placeholders only and must
not be copied into the launcher configuration. Do not infer package names from
another Xiaomi model, an internet list, or the CleanYourTV examples.

With `fill_dock: false`, missing entries are not silently replaced by the
first grid applications. The two remaining positions stay intentionally
unselected until the owner decides otherwise.

### Safe app identification plan for a later approved phase

Use a read-only inventory session only after explicit approval:

1. Record installed packages from the TV.
2. Filter candidate packages by visible app label, not by guessed package
   name.
3. Query each candidate for activities handling
   `MAIN` plus `LEANBACK_LAUNCHER`.
4. Confirm the activity starts the intended TV app and does not open a
   phone-oriented or setup screen.
5. Record package, activity, app label, enabled state, and whether it survives
   a normal reboot.
6. Use only the verified package name in `dock_apps`; use
   `launch_actions` only if the normal leanback launch is insufficient.

The inventory must not disable, uninstall, force-stop, clear data, or alter
the default Home.

## 4. Build prerequisites and future validation

### Verified build requirements

From `launcher/app/build.gradle` and `docs/launcher.md`:

- Android minimum: API 26 / Android 8.0.
- Reported factory Android 9 is API 28, so compatibility is plausible but not
  certified for the current firmware.
- `compileSdk 34`.
- `targetSdk 34`.
- Java source and target compatibility: Java 17.
- Android SDK platform 34 and build-tools 34 are required by the documented
  macOS setup.
- Gradle wrapper distribution: Gradle 8.9.
- Release builds use shrinking/resource shrinking and the debug signing
  configuration for sideload/update continuity.
- The normal application ID is `com.gathin.tvhome`.

The repository documents Python 3 for the helper scripts. A local build of
the launcher itself uses Gradle and the Android SDK; device helper scripts
also require the repository Python environment, but those helpers are not to
be run in this phase.

### Exact future local-only commands

These commands are listed for a later build/validation phase and are **not
executed by this plan**:

```bash
cd /Users/Kiran/tv/cleanyourtv/launcher
./gradlew assembleRelease --console=plain
./gradlew test --console=plain
```

The expected release artifact would be:

```text
/Users/Kiran/tv/cleanyourtv/launcher/app/build/outputs/apk/release/app-release.apk
```

Before any future build, verify the local toolchain without contacting the TV:

```bash
java -version
./gradlew --version
echo "$ANDROID_HOME"
```

For the theme workspace, future JSON-only validation can be performed without
device access:

```bash
python3 -m json.tool \
  /Users/Kiran/tv/luffy-gear5/themes/luffy-gear5/config.json >/dev/null
```

Do not run `launcher/build-and-install.sh`, `tools/theme.sh`,
`tools/disable.sh`, `tools/restore.sh`, or any command that invokes
`tools/tv.py` until the installation phase has been separately approved.

## 5. Xiaomi compatibility findings

### Verified or strongly evidenced

- CleanYourTV supports Android TV and Google TV in its general documentation.
- The launcher’s minimum API is 26.
- The reported factory Android 9 platform is above that floor.
- The reported 3840 x 2160 display is suitable for a static 16:9 composition.
- CleanYourTV has Xiaomi-specific documentation and examples, but they cover
  other model/firmware combinations.
- CleanYourTV’s launcher install script recognizes several possible stock
  launchers, including `com.mitv.tvhome`, but recognition is not proof that
  this TV uses that package.

### Inferred, not certified

- The standard launcher APK may run on the factory Android 9 platform.
- A static, darkened wallpaper with moderate effects is more appropriate for
  2 GB RAM than video or continuously rendered effects.
- The theme overlay should avoid rebuilding the APK.

### Unknown and required before installation

- Current `ro.build.version.sdk`, Android version, build fingerprint, and
  Xiaomi firmware.
- Current Home resolver and every Home claimant.
- Whether PatchWall is a separate app, a Home claimant, a button target, or
  firmware-integrated.
- Whether Xiaomi’s firmware permits a third-party Home while retaining the
  stock launcher enabled.
- Current free `/data` and shared storage.
- Actual package/activity pairs for the four preferred apps.
- HDMI, AV, tuner, remote, accessibility, and settings component behavior on
  this exact build.
- Reboot and firmware-update persistence.

The repository contains a Xiaomi AETP0/API 28 example where the stock Home
was `com.mitv.tvhome`, but that is evidence for that tested unit only. It must
not be substituted for an inventory of `L65M5-5SIN`.

## 6. Preserving PatchWall and the existing launcher

The first installation test should preserve the original launcher package and
PatchWall enabled. The purpose is to establish whether the new launcher can be
selected as Home without destructive package changes.

CleanYourTV’s documented recovery model records the prior Home owner,
per-package enabled state, and animation scales under `~/.cleanyourtv/` when
its disable/check workflows are used. The restore script replays that record,
but it does not remove the new launcher. The stock launcher can also be chosen
through Android’s Home picker when the stock package is enabled.

The Xiaomi AETP0 documentation demonstrates why firmware-specific verification
matters: a standard launcher install could succeed while Xiaomi Home remained
the effective Home. That document used a separate application ID on that
firmware. This is not a recommendation to use that ID here; it is a warning
that the target firmware must be checked before selecting an installation
method.

The safe testing policy is:

1. Do not disable PatchWall or any stock package.
2. Do not apply a debloat list.
3. Record the current Home component and package state first.
4. Install only after a private backup and recovery procedure are ready.
5. If the Home picker works, select the original Home to return to stock.
6. If the install script is later used, retain its recovery record and the
   exact package list; do not delete or overwrite the record.
7. Verify Home, HDMI/input access, TV settings, remote navigation, accessibility
   and app launching before any optional cleanup.

## 7. Pre-installation checks and rollback prerequisites

Installation is not authorized until all of these conditions are met:

- A TV-side About snapshot confirms `L65M5-5SIN`, current Android version,
  API level, security patch, firmware/build and fingerprint.
- The API level is at least 26.
- Free storage is recorded and sufficient for the APK, temporary transfer,
  theme files, and normal system operation.
- The Home resolver, all Home claimants, stock launcher component, and
  PatchWall relationship are documented.
- The four preferred apps each have a verified package and launchable TV
  activity.
- The final two dock slots are explicitly left empty or assigned by the owner.
- A properly licensed or user-supplied wallpaper is stored locally, with
  provenance recorded.
- The APK has passed local build and test validation.
- A private backup exists outside Git containing the device snapshot, package
  state, disabled-package state if any, Home component, settings relevant to
  recovery, and animation scales.
- The restore command and stock Home selection path have been rehearsed on
  paper for this exact firmware.
- No package-disable list is part of the initial launcher test.
- The owner gives explicit approval for the installation phase.

### Rollback prerequisites

Before installation, retain:

- The exact APK and its signing/build identity.
- The exact theme directory and JSON used.
- The original Home package/component.
- The CleanYourTV recovery record location, if the project scripts are used.
- A written manual fallback through TV Settings → Apps and the Android Home
  picker.
- A way to reach TV settings with the physical remote even if Home selection
  fails.

Rollback success means the original launcher owns Home after a restart, the
stock settings and PatchWall entry remain usable, preferred apps launch, TV
inputs work, and no package was disabled unintentionally.

## 8. Risks and mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Factory specs differ from current firmware | Build or Home behavior differs | Capture current About and read-only runtime evidence first. |
| Xiaomi protects or reclaims Home | New launcher does not persist | Keep stock Home enabled; verify Home resolver and reboot behavior before any cleanup. |
| PatchWall is mistaken for stock Home | Loss of Xiaomi content or recovery path | Identify package/component relationship; never disable by name alone. |
| Wrong streaming package/activity | Dock item fails or opens the wrong UI | Query installed labels and leanback activities on the target TV. |
| 2 GB RAM pressure | Jank, slow startup, or process eviction | Use static wallpaper; benchmark with advanced effects; enable reduced motion or disable advanced effects if necessary. |
| 4K wallpaper memory cost | Decode failure or frame pacing issues | Use a properly sized static image, dark scrim, and compare `wallpaper_true_color` against memory/frame results. |
| Full storage | APK/theme transfer fails | Record free storage before installation and preserve headroom. |
| Remote/input regression | Reduced TV usability | Test Home, Back, D-pad, volume, Input, HDMI/AV, settings, microphone and accessibility with the real remote. |
| Firmware update drift | Home/package state changes later | Keep updates enabled initially and re-check after updates. |
| Unlicensed artwork | Legal and distribution risk | Use owner-supplied or properly licensed local artwork only. |

## 9. Phase-by-phase execution checklist

### Phase 3B: this plan

- [x] Read the compatibility report.
- [x] Inspect theme schema and launcher configuration parsing.
- [x] Inspect launcher build configuration and recovery documentation.
- [x] Create this implementation plan only.
- [x] Leave the existing theme configuration unchanged.
- [x] Perform no TV, ADB, network, install, or build operation.

### Phase 3C: approved read-only TV inventory

- [ ] Manually record About, Storage, Apps, Home/PatchWall and input details.
- [ ] If later approved, collect read-only package, activity, property and
      Home-resolver evidence.
- [ ] Identify the four dock packages and launchable TV activities.
- [ ] Confirm free space and current firmware.
- [ ] Keep all packages enabled and unchanged.

### Phase 3D: local asset and configuration finalization

- [ ] Obtain and document licensed/user-supplied wallpaper provenance.
- [ ] Add only the local wallpaper asset to the separate Luffy workspace.
- [ ] Update the theme JSON with verified package names and final wallpaper
      settings.
- [ ] Validate JSON and inspect the six-slot layout locally.
- [ ] Decide whether advanced effects remain enabled after hardware evidence.

### Phase 3E: local APK build and validation

- [ ] Confirm JDK 17, Android SDK platform/build-tools 34 and Gradle wrapper.
- [ ] Run the local Gradle release build and JVM tests.
- [ ] Inspect APK existence, size, package identity and signing consistency.
- [ ] Do not invoke device-facing scripts as part of local validation.

### Phase 4: installation, only after separate approval

- [ ] Confirm every installation-readiness condition.
- [ ] Preserve the stock launcher and PatchWall unless a later, explicit
      decision authorizes otherwise.
- [ ] Use the repository’s guarded install/recovery path rather than an
      improvised package command.
- [ ] Verify Home ownership and recovery immediately.
- [ ] Apply the theme only after the launcher itself is stable.
- [ ] Test remote navigation, clock/date, dock order, app launching, HDMI,
      settings, accessibility and restart persistence.

### Phase 5: post-install review

- [ ] Measure startup, focus responsiveness and memory behavior.
- [ ] Decide whether reduced motion or fewer effects are needed.
- [ ] Re-check PatchWall, firmware updates, inputs and stock Home recovery.
- [ ] Treat debloating as a separate optional phase requiring package-by-package
      review and approval.

## 10. Simplest implementation recommendation

Use the existing CleanYourTV APK unchanged at source level and implement the
Luffy appearance through one local theme overlay plus one approved static
wallpaper:

- six-column dock row;
- four verified app package names followed by two empty slots;
- dark background and wallpaper scrim;
- translucent glass dock;
- readable clock with date below;
- normal animations and existing focus movement;
- advanced effects enabled initially, with a reduced-motion fallback;
- no custom Java focus renderer, video wallpaper, debloating, or PatchWall
  removal.

This achieves the requested visual direction with the smallest change surface
and preserves the project’s existing recovery mechanisms.

## Single safest next action

Review this plan and, if acceptable, approve a **read-only TV compatibility
inventory only**. The next action must collect the current model/runtime,
Home/PatchWall, storage, and app/activity evidence without changing settings,
packages, launcher ownership, or network configuration. No build, theme push,
installation, or debloating should occur until that evidence is reviewed.
