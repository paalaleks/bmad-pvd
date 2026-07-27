# Module Setup

Standalone module self-registration. This file is loaded when:
- The user passes `setup`, `configure`, or `install` as an argument
- The module is not yet registered and silent or interactive registration was requested by SKILL.md
- The skill's first-run init flow detects this is a fresh installation

**Registered means any of:** `{project-root}/_bmad/config.yaml` has a top-level `pvd` key; `{project-root}/_bmad/config.toml` has `[modules.pvd]`; or `{project-root}/_bmad/module-help.csv` contains skill `pvd-product-vision`.

## Silent / accept-all mode

When SKILL.md requests silent registration (BMad already configured; user did not pass setup):

1. Do **not** ask for `user_name`, language, or `output_folder` — reuse existing values from `config.toml` / `config.yaml` / `config.user.*` (map `user_name` / `communication_language` from `config.user.toml` when present).
2. Run Write Files + Create Output Directories with those defaults.
3. Also ensure `{project-root}/_bmad/custom/config.toml` (or team custom) contains an empty or minimal `[modules.pvd]` table so toml-based installs recognize the module — create the file section if missing (do not wipe other custom tables).
4. **Team customizes:** if missing, copy each of these from `./assets/` into `{project-root}/_bmad/custom/` (leave existing files unchanged — no prompt in silent mode): `bmad-prd.toml`, `bmad-code-review.toml`, `bmad-ux.toml`, `bmad-create-epics-and-stories.toml`, `bmad-create-story.toml`.
5. Tell the user in one line that PVD was registered; then Return to Skill.

Interactive prompting below applies only to explicit setup/configure or true greenfield (no BMad config yet).


## Overview

Registers this standalone module into a project. Module identity (name, code, version) comes from `./assets/module.yaml` (sibling to this file). Collects user preferences and writes them to three files:

- **`{project-root}/_bmad/config.yaml`** — shared project config: core settings at root (e.g. `output_folder`, `document_output_language`) plus a section per module with metadata and module-specific values. User-only keys (`user_name`, `communication_language`) are **never** written here.
- **`{project-root}/_bmad/config.user.yaml`** — personal settings intended to be gitignored: `user_name`, `communication_language`, and any module variable marked `user_setting: true` in `./assets/module.yaml`. These values live exclusively here.
- **`{project-root}/_bmad/module-help.csv`** — registers module capabilities for the help system.

Both config scripts use an anti-zombie pattern — existing entries for this module are removed before writing fresh ones, so stale values never persist.

`{project-root}` is a **literal token** in config _values_ (the data written into the files above) — never substitute it there. It signals to the consuming LLM that the value is relative to the project root, not the skill root. **This does not apply to the filesystem path _arguments_ passed to the scripts below** (the `--*-path`, `--*-dir`, and `--target` arguments): those are real paths, so you **must** resolve `{project-root}` to the actual project root before running, or the scripts will write to a literal `{project-root}/` directory under the skill folder. The scripts reject an unresolved token with an error.

## Check Existing Config

1. Read `./assets/module.yaml` for module metadata and variable definitions (the `code` field is the module identifier)
2. Check if `{project-root}/_bmad/config.yaml` exists — if a section matching the module's code is already present, inform the user this is an update (reconfiguration)

If the user provides arguments (e.g. `accept all defaults`, `--headless`, or inline values like `user name is BMad, I speak Swahili`), map any provided values to config keys, use defaults for the rest, and skip interactive prompting. Still display the full confirmation summary at the end.

## Collect Configuration

Ask the user for values. Show defaults in brackets. Present all values together so the user can respond once with only the values they want to change (e.g. "change language to Swahili, rest are fine"). Never tell the user to "press enter" or "leave blank" — in a chat interface they must type something to respond.

**Default priority** (highest wins): existing config values > `./assets/module.yaml` defaults.

### Core Config

Only collect if no core keys exist yet in `config.yaml` or `config.user.yaml`:

- `user_name` (default: BMad) — written exclusively to `config.user.yaml`
- `communication_language` and `document_output_language` (default: English — ask as a single language question, both keys get the same answer) — `communication_language` written exclusively to `config.user.yaml`
- `output_folder` (default: `{project-root}/_bmad-output`) — written to `config.yaml` at root, shared across all modules

### Module Config

Read each variable in `./assets/module.yaml` that has a `prompt` field. The module.yaml supports several question types:

- **Text input**: Has `prompt`, `default`, and optionally `result` (template), `required`, `regex`, `example` fields
- **Single-select**: Has a `single-select` array of `value`/`label` options — present as a choice list
- **Multi-select**: Has a `multi-select` array — present as checkboxes, default is an array
- **Confirm**: `default` is a boolean — present as Yes/No

Ask using the prompt with its default value. Apply `result` templates when storing (e.g. `{project-root}/{value}`). Fields with `user_setting: true` go exclusively to `config.user.yaml`.

## Write Files

Write a temp JSON file with the collected answers structured as `{"core": {...}, "module": {...}}` (omit `core` if it already exists). Values inside this JSON keep the literal `{project-root}` token. Then run both scripts — they can run in parallel since they write to different files.

In the commands below, replace `{project-root}` in every path argument with the actual project root (e.g. `/home/me/myapp`) before running — these are filesystem paths, not config values.

```bash
python3 ./scripts/merge-config.py --config-path "{project-root}/_bmad/config.yaml" --user-config-path "{project-root}/_bmad/config.user.yaml" --module-yaml ./assets/module.yaml --answers {temp-file}
python3 ./scripts/merge-help-csv.py --target "{project-root}/_bmad/module-help.csv" --source ./assets/module-help.csv --module-code {module-code}
```

Both scripts output JSON to stdout with results. If either exits non-zero, surface the error and stop.

Run `./scripts/merge-config.py --help` or `./scripts/merge-help-csv.py --help` for full usage.

## Create Output Directories

After writing config, create any output directories that were configured. For filesystem operations only (such as creating directories), resolve the `{project-root}` token to the actual project root and create each path-type value from `config.yaml` that does not yet exist — this includes `output_folder` and any module variable whose value starts with `{project-root}/`. The paths stored in the config files must continue to use the literal `{project-root}` token; only the directories on disk should use the resolved paths. Use `mkdir -p` or equivalent to create the full path.

If `./assets/module.yaml` contains a `directories` array, also create each listed directory (resolving any `{field_name}` variables from the collected config values).

If `./assets/module.yaml` contains a `directories` array, also create each listed directory (resolving any `{field_name}` variables from the collected config values).

## Install team customizes

Create `{project-root}/_bmad/custom/` if missing. Ask for each override separately (default **Yes**). None modify upstream skill code; all use BMad customization merge (`_bmad/custom/{skill}.toml`).

### BMM PRD override (PVD extension)

Ask once (default **Yes**): install the lightweight BMM PRD customize so child PRDs load PVD facts and phase boundaries?

- **Yes (default):** Copy `./assets/bmad-prd.toml` to `{project-root}/_bmad/custom/bmad-prd.toml`.
  - If a file already exists, show a short diff/summary and ask before overwrite (do not clobber silently).
- **No:** Skip. User can copy later from the skill assets.

### Code-review override (team preference)

Ask once (default **Yes**): install the autonomous `bmad-code-review` customize (skip progress checkpoints, auto-apply patches, HITL only for real decisions; push to `main` when a review marks the story done)?

- **Yes (default):** Copy `./assets/bmad-code-review.toml` to `{project-root}/_bmad/custom/bmad-code-review.toml`.
  - If a file already exists, show a short diff/summary and ask before overwrite (do not clobber silently).
- **No:** Skip. User can copy later from the skill assets.

### UX HTML-mock HITL (before epics)

Ask once (default **Yes**): install `bmad-ux` customize so Finalize requires inexpensive HTML mock HITL (or skip with rationale) before epics handoff?

- **Yes (default):** Copy `./assets/bmad-ux.toml` to `{project-root}/_bmad/custom/bmad-ux.toml`.
  - If a file already exists, show a short diff/summary and ask before overwrite (do not clobber silently).
- **No:** Skip.

### Epics + story UI-mock pointers

Ask once (default **Yes**): install create-epics / create-story customizes so UI-heavy stories cite approved UX mockups or UX skip rationales?

- **Yes (default):** Copy `./assets/bmad-create-epics-and-stories.toml` and `./assets/bmad-create-story.toml` into `{project-root}/_bmad/custom/`.
  - If a file already exists, show a short diff/summary and ask before overwrite (do not clobber silently).
- **No:** Skip.

## Confirm

Use the script JSON output to display what was written — config values set (written to `config.yaml` at root for core, module section for module values), user settings written to `config.user.yaml` (`user_keys` in result), help entries added, fresh install vs update. Also report for each team customize whether it was installed, skipped, or left unchanged.

If `./assets/module.yaml` contains `post-install-notes`, display them (if conditional, show only the notes matching the user's selected config values).

Then display the `module_greeting` from `./assets/module.yaml` to the user.

## Return to Skill

Setup is complete. Resume the main skill's normal activation flow — load config from the freshly written files and proceed with whatever the user originally intended.
