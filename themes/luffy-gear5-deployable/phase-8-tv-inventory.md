# Phase 8 — Read-only Xiaomi TV inventory

Inventory target: `192.168.1.8`  
Project: `/Users/Kiran/tv/cleanyourtv`  
Collected with: `TV_HOST=192.168.1.8 tools/venv/bin/python tools/tv.py`  
All device operations used in this inventory were read-only shell queries. No package, setting, file, Home-app, or reboot operation was performed.

## Device identity

| Property | Value |
|---|---|
| Model | `MiTV-AXSO0` |
| Manufacturer | `Xiaomi` |
| Android release | `9` |
| SDK | `28` |
| Build fingerprint | `Xiaomi/dangal/dangal:9/PI/6036:user/release-keys` |

## Installed matching applications

`enabled=0` in the package dump means the component is in Android's default enabled state, not explicitly disabled. `pm list packages -d` returned no matching packages, so none of the matching packages is explicitly disabled.

| App / role | Package name | Version name | Version code | Installed / enabled state | Leanback launchable activity |
|---|---|---:|---:|---|---|
| Xiaomi TV Home / PatchWall candidate | `com.mitv.tvhome.atv` | `4.4.23` | `147` | Installed; default enabled (`enabled=0`) | `com.mitv.tvhome.atv/.app.BannerLogoA` |
| Xiaomi TV Home / launcher candidate | `com.mitv.tvhome.mitvplus` | `3.9.16` | `65` | Installed; default enabled (`enabled=0`) | `com.mitv.tvhome.mitvplus/.app.SplashActivity` |
| Google TV launcher | `com.google.android.tvlauncher` | `7.7.15-968255986-f` | `1010943512` | Installed; default enabled (`enabled=0`) | No package-scoped Leanback activity was returned; Home resolution below identifies `/.MainActivity` |
| Legacy Google Leanback launcher | `com.google.android.leanbacklauncher` | `90.11.5-100-4440948` | `1901105100` | Installed; default enabled (`enabled=0`) | No activity returned |
| YouTube for Android TV | `com.google.android.youtube.tv` | `5.30.304` | `530304320` | Installed; default enabled (`enabled=0`) | `com.google.android.youtube.tv/com.google.android.apps.youtube.tv.activity.ShellActivity` |
| YouTube Music for TV | `com.google.android.youtube.tvmusic` | `1.12.07` | `112007300` | Installed; default enabled (`enabled=0`) | `com.google.android.youtube.tvmusic/com.google.android.apps.youtube.tvmusic.activity.MainActivity` |
| Netflix | `com.netflix.ninja` | `8.3.11 build 12041` | `12041` | Installed; default enabled (`enabled=0`) | `com.netflix.ninja/.MainActivity` |
| Amazon Prime Video | `com.amazon.amazonvideo.livingroom` | `6.24.13+v16.0.1.93-allAbis` | `606024130` | Installed; default enabled (`enabled=0`) | `com.amazon.amazonvideo.livingroom/com.amazon.ignition.IgnitionActivity` |
| Jio / JioCinema TV | `com.jio.media.stb.ondemand` | `4.0.4` | `2007` | Installed; default enabled (`enabled=0`) | `com.jio.media.stb.ondemand/com.v18.voot.ui.JVHomeActivity` |
| Jio Xiaomi companion package | `com.jio.media.stb.ondemand.xiaomi` | `1.0.9` | `9` | Installed; default enabled (`enabled=0`) | No activity returned |
| JioHotstar / Hotstar | `in.startv.hotstar` | `5.2.9` | `1151` | Installed; default enabled (`enabled=0`); `virtual=true` in package dump | `in.startv.hotstar/.ui.splash.TVSplashActivity` |

### Package-name matching notes

- No package containing `cleanyourtv` was listed.
- No package literally named `PatchWall` was listed. The Xiaomi TV Home packages present are `com.mitv.tvhome.atv` and `com.mitv.tvhome.mitvplus`; the exact product branding could not be determined from the package-manager output alone.
- No package literally named `Prime Video` was listed; the installed Amazon TV package is `com.amazon.amazonvideo.livingroom`.
- Both `com.google.android.youtube.tv` and `com.google.android.youtube.tvmusic` are installed.

## Currently configured Home activity

Read-only resolver queries returned:

```text
cmd package resolve-activity --brief \
  -a android.intent.action.MAIN \
  -c android.intent.category.HOME

priority=2 preferredOrder=0 match=0x108000 specificIndex=-1 isDefault=true
com.google.android.tvlauncher/.MainActivity
```

The same `com.google.android.tvlauncher/.MainActivity` result was returned when `DEFAULT` was included with `HOME`. Therefore the currently resolved Home activity is:

```text
com.google.android.tvlauncher/.MainActivity
```

`cmd package get-home-activity` was unavailable on this TV's Android 9 package manager (`Unknown command: get-home-activity`), so the resolver result is the authoritative determination from the available read-only interface.

## Internal storage

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/block/data 4.4G  2.8G  1.5G  66% /data
```

Free internal storage: **1.5G** on `/data`.

## CleanYourTV status

CleanYourTV is **not installed**:

- `pm list packages` contained no CleanYourTV package.
- `pm path com.cleanyourtv` returned no path.
- `cmd package resolve-activity ... -p com.cleanyourtv` returned `No activity found`.
- Version: **could not be determined because the package is absent**.

## Query scope

The collected read-only query set used `getprop`, `pm list packages`, `pm list packages -d`, `dumpsys package`, `cmd package resolve-activity`, and `df -h /data`. The helper's documented `--batch` interface was used for grouped shell queries.
