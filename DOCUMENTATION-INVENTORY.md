# Documentation inventory

The Luffy workspace contains upstream CleanYourTV documentation, project
instructions, Luffy design history, and device-specific records. The inventory
below defines what is authoritative after the documentation cleanup.

## Authoritative Luffy documents

- [README.md](README.md): project entry point and upstream documentation bridge.
- [CLAUDE.md](CLAUDE.md): agent instructions and safety rules.
- [PROJECT-STATUS.md](PROJECT-STATUS.md): current implementation and TV state.
- [TV-SETUP.md](TV-SETUP.md): verified commands and device procedures.
- [REVERSIBILITY.md](REVERSIBILITY.md): changes and rollback records.
- [DEVELOPMENT.md](DEVELOPMENT.md): local development and packaging workflow.
- [AI-PROMPTS.md](AI-PROMPTS.md): reusable task prompts.
- [ROADMAP.md](ROADMAP.md): unimplemented ideas.
- [BUILD-STATUS.md](BUILD-STATUS.md): concise build record.
- [AGENTS.md](AGENTS.md): existing repository-wide agent guidance.
- [PROMPT.md](PROMPT.md): original project task prompt.

## Retained historical and upstream documents

- `phase-3c-readiness-checklist.md`, `phase-5-audit-report.md`,
  `phase-6-theme-report.md`, and `phase-7a-wallpaper-report.md` are phase
  records. They are retained because they document decisions and evidence.
- `themes/luffy-gear5/README.md`, `design-spec.md`, and
  `compatibility-report.md` are retained design records and now carry a
  historical-status note.
- `themes/README.md`, `docs/`, `tools/`, `addons/`, `examples/`, and translated
  README files are upstream CleanYourTV documentation or examples. They remain
  useful references and were not duplicated or deleted.
- `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `SECURITY.md`, and
  `.github/pull_request_template.md` are repository governance documents.

## Deletion decision

No Markdown files were deleted. The workspace contains 288 Markdown files,
many of which are upstream or phase-specific. None was sufficiently
unambiguous as redundant to justify deletion. This conservative choice
preserves upstream guidance and historical evidence while the root documents
provide a single current entry point.

## Cleanup boundary

The cleanup changed documentation only. It did not change launcher source,
theme assets, JSON values, APKs, or TV state.
