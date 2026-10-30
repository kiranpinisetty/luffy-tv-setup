# Project status

Last verified: 2026-10-30. This is the concise project-state reference; verify
the live TV before any new device operation.

## Implemented locally

- Luffy Gear 5 theme source and deployable directories.
- Six-column glass dock with dark wallpaper presentation.
- Verified dock order: YouTube, Netflix, Prime Video, JioHotstar.
- Empty dock slots 5 and 6 via `fill_dock: false`.
- Local `power_on_home_always: true` in both configs.
- CleanYourTV release APK candidate, package `com.gathin.tvhome`, version 3.1,
  version code 10, min SDK 26, target SDK 34.
- Theme transfer and APK installation were tested on the target TV.

## Current TV state

- Device: Xiaomi `MiTV-AXSO0`, codename `dangal`, Android 9 / SDK 28.
- ADB: `192.168.1.8:5555`.
- Android user: 0.
- CleanYourTV is installed, enabled, and resolves HOME.
- Google TV `com.google.android.tvlauncher` is disabled-user.
- Xiaomi `com.mitv.tvhome.atv` and `com.mitv.tvhome.mitvplus` remain enabled.
- Physical Home opens CleanYourTV with the Luffy theme.
- PatchWall appears first during boot after reboot.
- HomeKeyService is not enabled:
  `enabled_accessibility_services = null`, `accessibility_enabled = 0`.
- The latest `power_on_home_always` configuration has not been transferred.

## Tested

- Read-only inventory and package/activity identification.
- Release APK build and local signature/metadata verification.
- APK installation and explicit MainActivity launch.
- Theme transfer with matching local/remote checksums.
- Reversible Google TV disable test.
- HOME resolution and physical/Home-key behavior.
- Reboot with ADB recovery and post-reboot resolver verification.

## Not deployed or not verified

- The latest local theme config with `power_on_home_always: true`.
- HomeKeyService activation.
- Automatic CleanYourTV startup after boot.
- Gear 5 eye tracking, blinking, startup animation, or custom focus palette.
- Any root, privileged, or firmware modification.

## Pending work

1. Obtain approval before transferring the updated theme.
2. Obtain approval before enabling `com.gathin.tvhome/.HomeKeyService`.
3. Test automatic power-on Home behavior only after recording accessibility
   state and confirming rollback.

## Safety and recovery

The verified Google TV restoration command is documented in
[REVERSIBILITY.md](REVERSIBILITY.md). Do not assume it restores the preferred
Home resolver; verify afterward. No complete recovery scenario has been
certified, and no claim should be made that automatic startup works until it
is tested.

## Documentation cleanup state

This documentation cleanup changed documentation only. It did not build,
install, transfer, reboot, or modify the TV, application code, theme assets,
or theme configuration values.
