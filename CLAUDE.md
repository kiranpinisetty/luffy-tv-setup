# Luffy Gear 5 project instructions

This repository contains the Luffy Gear 5 theme work for the CleanYourTV
Android TV launcher. Read [AGENTS.md](AGENTS.md), [README.md](README.md),
[PROJECT-STATUS.md](PROJECT-STATUS.md), and the relevant document in
[TV-SETUP.md](TV-SETUP.md), [DEVELOPMENT.md](DEVELOPMENT.md), or
[REVERSIBILITY.md](REVERSIBILITY.md) before significant work.

## Authoritative locations

- CleanYourTV source: `/Users/Kiran/tv/cleanyourtv`
- Theme source: `themes/luffy-gear5/`
- Deployable theme: `themes/luffy-gear5-deployable/`
- Current project state: [PROJECT-STATUS.md](PROJECT-STATUS.md)
- Theme configuration: the two `config.json` files above; keep them identical.
- Launcher parser and schema: `/Users/Kiran/tv/cleanyourtv/launcher/app/src/main/java/com/gathin/tvhome/Config.java`
  and `/Users/Kiran/tv/cleanyourtv/themes/README.md`

Inspect the existing implementation before proposing architecture. Do not
rebuild working functionality, invent CleanYourTV keys, or create duplicate
documentation. Verify every configuration key against the parser.

## Safety rules

Do not modify the TV unless the user explicitly authorizes that exact
operation. Never use root or privileged installation, uninstall Google TV or
Xiaomi system applications, disable Xiaomi packages, clear data, or reboot
without explicit approval. Preserve existing accessibility services. Explain
the impact of any device-facing command and wait when approval is required.
Keep changes minimal and reversible, and never claim a feature works without
testing or verification.

When starting work, inspect this file, [README.md](README.md),
[PROJECT-STATUS.md](PROJECT-STATUS.md), and the relevant technical
documentation, then inspect the actual source and current configuration.
Treat `PROJECT-STATUS.md` as the project-state reference, but verify device
state before executing any TV operation.

The upstream CleanYourTV documentation remains authoritative for the
upstream checkout. This file documents only the Luffy workspace and its
device-specific history.

@AGENTS.md
