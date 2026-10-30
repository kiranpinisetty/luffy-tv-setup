# Reusable AI prompts

Every prompt below begins with the same rule: inspect the existing code,
configuration, and documentation before editing; verify CleanYourTV keys
against its parser; and explain any TV impact before requesting approval.

## Repository understanding

> Read `CLAUDE.md`, `README.md`, `PROJECT-STATUS.md`, and the relevant
> technical docs. Inspect both theme directories and the CleanYourTV parser.
> Summarize authoritative files, current implementation, pending work, and
> risks without modifying anything.

## New theme feature

> Inspect the CleanYourTV parser and theme examples for an existing supported
> key. Add the smallest configuration-only change to the source theme, copy it
> to deployable, validate JSON, and compare both files. Do not touch the TV.

## Launcher UI or dock animation

> Inspect the existing launcher Java/resources and tests before proposing a
> change. Reuse existing focus and animation helpers. Implement only the
> requested UI behavior, update directly related docs, build with JDK 17, and
> run focused validation. Do not deploy.

## Add dock apps

> Inventory the actual TV package names and launchable Leanback activities
> using read-only ADB only. Do not guess package IDs. After approval, update
> both theme configs in the existing order, keep empty slots intentional, and
> validate they remain identical.

## ADB connectivity

> Use the existing `tools/tv.py` helper and run only `--probe` first. Report
> the exact error on failure. Do not install packages, transfer files, reboot,
> or retry repeatedly without identifying a cause.

## Launcher startup diagnosis

> Run read-only package, resolver, activity, receiver, and accessibility
> diagnostics. Distinguish HOME resolution from vendor boot receivers. Do not
> press Home, reboot, disable packages, or change settings.

## Theme deployment

> Inspect `tools/theme.sh` and the parser first. Validate the deployable
> directory and supported wallpaper extensions. After explicit approval, use
> only the existing installer, verify its exact destination and checksums, and
> report every device change.

## Configuration validation

> Parse both `config.json` files, compare them byte-for-byte, check all keys
> against `Config.java` and `themes/README.md`, and report unsupported or
> misspelled keys. Do not modify files unless asked.

## Rollback

> Read the recorded state in `REVERSIBILITY.md` first. Explain exactly what
> the rollback changes, verify the current state read-only, and run only the
> explicitly approved rollback command. Never clear data or uninstall.

## Xiaomi TV testing

> Confirm ADB, user 0, package state, current HOME, and recovery readiness
> before any test. Keep both Xiaomi packages enabled unless the user gives
> separate explicit approval. Do not reboot until the user approves that exact
> step, and verify foreground activity afterward.

## Read-only review

> Review the requested files and report findings with paths and line numbers.
> Do not edit files, run device commands, install dependencies, or modify the
> TV. Separate verified facts from assumptions and preserve uncertain notes.
