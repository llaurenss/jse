# Repository Guidelines

## Project Structure & Modules
- Core code: `scripting/*.sp` modules (Autosave, Core, Foresight, JumpBOT, Marker, Map Info, Scoreboard, Show Keys, Teleport, Tracker).
- Shared APIs/utilities: `scripting/include/*.inc` (e.g., `jse_core.inc`, `jse_utils.inc`); add new module interfaces here.
- `gamedata/*.txt` holds offsets/signatures for SDKCalls; keep versions aligned with your SourceMod build.
- `translations/` includes base phrase files plus locale folders; update the base file before copying strings into languages.
- `build/` is for compiled `.smx` artifacts; leave source folders clean.

## Editor Setup & Include Paths
- VS Code `SourcePawnLanguageServer` uses the bundled `spcomp.exe` at `C:/Users/Laurens/Desktop/tf2-server/sdk/sourcemod/1.12-b7217/spcomp.exe`.
- Include resolution is preconfigured to read from:
  - `C:/Users/Laurens/Desktop/tf2-server/plugins/jse/scripting/include`
  - `C:/Users/Laurens/Desktop/tf2-server/plugins/sjnetwork-deps/scripting/include`
  - `C:/Users/Laurens/Desktop/tf2-server/sdk/sourcemod/1.12-b7217/include`
- If running commands from WSL (agent/automation), mirror those include paths with `/mnt/c/...` entries so tools resolve the same headers.

## Build, Test, and Development Commands
- Prereqs: SourceMod 1.12+ plus the dependencies listed in `README.md`; maintainer handles compilation/testing and keeps artifacts out of git.
- Compile (maintainer): `spcomp -i scripting/include -o build/jse_core.smx scripting/jse_core.sp` (ensure `build/` exists); loop over `scripting/*.sp` to build all, then deploy to `tf/addons/sourcemod/plugins/` and `sm plugins reload <name>`.

## Coding Style & Naming Conventions
- SourcePawn with `#pragma semicolon 1` and `#pragma newdecls required`; prefer transitional syntax used in existing files.
- Tabs, width 4 for `.sp`/`.inc` (see `.editorconfig`).
- Prefixes: `g_h` for handles/ConVars, `g_b` for booleans, `g_i` for integers, `g_f` for floats; constants use `#define` with uppercase snake case.
- Group related ConVars and AutoExecConfig entries together; keep URL/version strings near the top of each module.

## Testing Guidelines
- Server validation is maintainer-run; include expected checks in PRs (key ConVars `jse_<module>_*`, autosave triggers, JumpBOT playback, tracker writes).
- Watch `addons/sourcemod/logs/` for errors after reloads/map changes and keep defaults safe for production; document new commands/ConVars briefly in code.

## Commit & Pull Request Guidelines
- Commit messages are short, imperative summaries (e.g., `Add settings for toggling bazooka projectile spread`); add issue numbers like `(#47)` when relevant.
- PRs should cover scope, testing done (reloads, map rotations, database hits), and any config or translation updates required for deploy.
- Attach screenshots or console snippets when UI/feedback changes are involved; avoid formatting-only PRs unless paired with a behavior change.
