# Phase 5 Gist analysis and project audit

Status: read-only audit. No Gist command, ADB command, build, installation,
theme push, TV connection, launcher change, package change, or file cleanup
was performed.

## 1. Gist reviewed

Source:

`https://gist.github.com/shivangg/0a1af5cec8230623c904e5dde5970de9`

The Gist contains one plain-text file titled **Sharingan TV setup**. The raw
content was read directly from GitHub. It has three numbered steps:

1. On the TV, open About, press Build seven times, enable Developer options,
   enable USB/ADB debugging, and put the TV and computer on the same Wi-Fi.
2. Give an AI coding assistant a prompt to clone CleanYourTV, follow
   `PROMPT.md`, and add:
   - dark hero wallpapers;
   - white logo-only icons;
   - spotlight glow on the focused app;
   - rolling clock seconds;
   - Sharingan eyes that track the selected app and blink;
   - a Sharingan spin on boot;
   - preview each change first.
3. Turn USB debugging off when finished.

There are no additional files, shell scripts, APKs, image assets, package
names, recovery scripts, or source patches in the Gist.

### What the Gist actually does

The Gist is an instruction prompt and workflow reminder. It does not itself
modify a launcher. It delegates the actual work to an AI assistant using the
CleanYourTV repository and its `PROMPT.md`.

The Gist does not provide a complete installation procedure. If the assistant
follows CleanYourTV’s full prompt, the resulting workflow may include a local
APK build, installation, Home selection, and optional package disabling, but
those actions come from CleanYourTV and the assistant’s interpretation of
`PROMPT.md`, not from executable content in the Gist.

### Access and security requirements

| Question | Finding |
|---|---|
| ADB required? | Yes, the first step explicitly requires USB/ADB debugging. |
| Network access required? | The Gist says the TV and computer should use the same Wi-Fi. |
| Root required? | No root requirement is stated. |
| Bootloader unlock required? | Not stated or required by the Gist. |
| APK included? | No. |
| Script included? | No. |
| Existing launcher modified directly? | No. It asks an assistant to use CleanYourTV. |
| Recovery procedure included? | No. It only says to turn USB debugging off afterward. |
| Package names included? | No. |

Turning USB debugging off is useful cleanup, but it is not a substitute for
backing up Home ownership, package state, settings, and the CleanYourTV
restore record.

### Compatibility with CleanYourTV

The Gist is compatible with CleanYourTV only at the workflow level: it points
the assistant at the repository and `PROMPT.md`. It does not describe the
repository’s verified theme schema.

The requested visual ideas divide into three groups:

| Gist request | CleanYourTV theme support | Assessment |
|---|---|---|
| Dark wallpaper | `background_color`, `wallpaper`, `wallpaper_dim` | Supported by the external theme mechanism. |
| White logo-only icons | Existing app artwork/configuration may be used, but there is no general theme key that converts every app to a custom white logo | Partially supported/needs asset or source work. |
| Focused-app spotlight | Existing focus movement, `ambient_glow`, and `app_glow` | Supported in a general form, not as a guaranteed custom palette. |
| Rolling clock seconds | The launcher clock behavior is not a documented theme key for second-by-second display | Not established; should not be promised. |
| Eyes track selected app and blink | No theme key or launcher capability found | Unsupported without custom rendering/source work and device testing. |
| Spin on boot | No theme key or documented launcher feature | Unsupported without source/boot integration work. |
| Preview changes first | A safe project workflow requirement | Recommended and already followed with a local SVG mockup. |

For the Luffy project, the Sharingan-specific character effects should be
replaced by the confirmed Gear 5 visual direction. The existing CleanYourTV
theme mechanism can provide the dark static background, translucent dock,
clock/date, and normal remote focus animation without a custom APK.

## 2. Workspace audit

### Original CleanYourTV checkout

Location:

`/Users/Kiran/tv/cleanyourtv`

The checkout is clean according to Git status. No upstream files were changed
by this audit or by the Luffy theme work.

The files required for a normal CleanYourTV launcher/theme workflow include:

- `README.md`, `PROMPT.md`, and project documentation;
- `launcher/`, including Gradle wrapper, `app/build.gradle`, Java source,
  Android resources, manifest, and tests;
- `themes/`, including the authoritative theme schema and built-in examples;
- `tools/`, including validation, theme transfer, recovery, and build helpers;
- `docs/` and relevant device documentation;
- `examples/` for device-specific reference material.

The exact set required depends on the phase. A local launcher build needs the
`launcher/` project and its Gradle wrapper/configuration. A theme-only device
operation needs the theme documentation and `tools/theme.sh` plus its helper
dependencies, but those tools remain device-facing and were not executed.

### Luffy workspace

Location:

`/Users/Kiran/tv/luffy-gear5`

This repository contains the mirrored CleanYourTV source plus the Luffy-specific
files below:

| File | Role | Status |
|---|---|---|
| `themes/luffy-gear5/config.json` | Actual local theme overlay draft | Required for the Luffy theme; dock package list intentionally empty. |
| `themes/luffy-gear5/README.md` | Theme usage/design documentation | Required project documentation. |
| `themes/luffy-gear5/design-spec.md` | Supported/unsupported visual design decisions | Useful design documentation. |
| `themes/luffy-gear5/compatibility-report.md` | Phase 3A model and recovery planning | Planning documentation. |
| `themes/luffy-gear5/preview.svg` | Original 16:9 layout mockup | Preview-only asset; not a launcher screenshot or runtime asset. |
| `implementation-plan.md` | Phase 3B implementation plan | Planning documentation. |
| `phase-3c-readiness-checklist.md` | Phase 3C manual and ADB-readiness checklist | Planning documentation. |

The Luffy workspace currently has no approved Luffy wallpaper image, no
verified dock package names, no APK output, and no device snapshot.

The Luffy Git repository status is clean. The original source checkout is also
clean. The Luffy repository’s history includes the original Luffy theme commit,
the mirrored CleanYourTV source, and the planning documents.

## 3. Assets and possible cleanup candidates

### Current Luffy-specific assets

- `preview.svg` is an original abstract mockup and is still useful for design
  review. It is not consumed by CleanYourTV at runtime.
- No `wallpaper.jpg`, `wallpapers/` directory, screenshot, APK, or generated
  build output exists in the Luffy theme directory.
- `config.json` currently has `"wallpaper": false`, consistent with the lack
  of approved artwork.

### Candidates for eventual cleanup

These are not deleted in this phase:

1. `preview.svg` could be omitted from a final runtime-only theme package once
   the visual design is approved, but it should be retained while review is
   ongoing.
2. `compatibility-report.md`, `implementation-plan.md`, and
   `phase-3c-readiness-checklist.md` are planning documents rather than
   launcher inputs. They should remain in the project history or documentation
   area until the device work is complete, but they are not needed by
   `tools/theme.sh`.
3. The mirrored CleanYourTV source is not required inside a minimal theme-only
   distribution, but it is useful for reproducible schema/build/recovery
   review. It must not be removed unless a separate repository-layout decision
   is approved.
4. No duplicate Luffy wallpaper or APK asset was found.

No file is an obvious safe deletion at this stage. Cleanup would be a
packaging decision, not a prerequisite for applying the theme.

## 4. Build-related audit

The CleanYourTV launcher build files are upstream project files, not Luffy
theme files. They are required only if the launcher APK must be rebuilt.

Verified launcher build characteristics:

- `launcher/app/build.gradle` declares `compileSdk 34`, `targetSdk 34`,
  `minSdk 26`, and Java 17 source/target compatibility.
- The project uses Android Gradle Plugin 8.5.2 and Gradle wrapper 8.9.
- The launcher depends on AndroidX RecyclerView and DynamicAnimation.
- The expected release artifact is
  `launcher/app/build/outputs/apk/release/app-release.apk`.

The current Luffy theme itself does not require these build files to apply a
theme overlay. A build may still be required if the target TV does not already
have a compatible CleanYourTV launcher APK installed, but that is a separate
device-readiness and installation decision.

No build output is present, and no build was run during this audit.

## 5. Simplest safe implementation

### Option 1: CleanYourTV external theme mechanism

This is the recommended approach:

1. Keep the existing CleanYourTV launcher source unchanged.
2. Obtain a licensed or user-supplied local Gear 5 wallpaper.
3. Complete the approved read-only TV inventory.
4. Verify the four actual package names and launchable TV activities.
5. Update the separate Luffy theme overlay only after those facts are known.
6. Validate the JSON locally.
7. If a compatible CleanYourTV APK is already available on the TV, use the
   repository’s guarded theme mechanism in a separately approved device phase.

This approach changes only the overlay and local assets. It supports the dark
wallpaper, six-column dock, empty slots, clock/date, glass dock, and existing
focus movement without a launcher source fork.

### Option 2: Gist workflow

The Gist is not an alternative implementation mechanism. It is a short prompt
that tells an AI assistant to clone CleanYourTV and request additional
customization. It explicitly requires ADB/debugging and does not provide
recovery details, package verification, or a theme file.

It should be treated as inspiration only. Its unsupported requests must not be
treated as commitments for this project.

### Option 3: Custom APK/source modification

This should be deferred. It would be needed only for a verified requirement
that the theme schema cannot express, such as a custom red-and-gold focus
renderer or character-tracking animation. It increases build, signing,
compatibility, memory, and rollback risk without being necessary for the
confirmed initial appearance.

## 6. APK-build decision

A new APK is **not automatically necessary for the theme**. The external
theme mechanism is specifically designed to change wallpaper, background,
dock, columns, motion, and related visual settings without rebuilding the
launcher.

An APK build is required only if:

- the TV does not already have a compatible CleanYourTV launcher APK;
- the installed launcher version lacks a needed, verified theme key; or
- a later approved decision adds source/resource behavior such as custom
  Haki-colored focus rendering.

The current project has not established any of those conditions. Therefore,
do not build an APK merely because the Gist mentions extensive visual ideas.

## 7. Next implementation phase

The next safe phase is **Phase 3C completion / approved read-only device
inventory**, not installation:

1. Complete the manual TV checklist in
   `phase-3c-readiness-checklist.md`.
2. Review the current Android version, firmware/build, free storage, Home
   owner, PatchWall behavior, app versions, remote, and HDMI evidence.
3. Only with separate approval, run a fixed set of read-only inventory
   queries through the repository’s ADB client.
4. Record the exact package/activity pairs and decide whether an APK build is
   actually needed.

After that evidence is reviewed, the likely implementation path is:

- local licensed wallpaper preparation;
- local theme JSON update with verified packages;
- local JSON/asset validation;
- only then, if required, local launcher build and a separately approved
  device test.

## Audit conclusion

The Gist does not add a safer or simpler installation path. It requires ADB,
contains no recovery workflow, and asks for several effects that CleanYourTV’s
verified theme system does not support. The safest implementation remains the
existing CleanYourTV external theme overlay, with no custom APK/source change
unless the later device inventory proves one is necessary.
