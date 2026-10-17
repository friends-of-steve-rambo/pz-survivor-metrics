# Survivor Metrics — Agent Instructions

## Project

- Project Zomboid mod for tracking and displaying server-player metrics,
  primarily real-world online time. Future plans include metric-driven
  gameplay effects.
- Repository: `pz-survivor-metrics`
- Mod ID: `SurvivorMetrics`
- Target: Project Zomboid Build 42 only.
- Language: Lua, using the game's embedded runtime.
- Declarative game definitions: Project Zomboid script format (ZedScripts),
  when required by the implementation.

## Compatibility and Packaging

- Support Build 42 only. Do not add Build 41 compatibility.
- Keep mod files under `Contents/mods/SurvivorMetrics/42/`.
- Keep exactly one `mod.info` at that path; do not create a root-level
  mod manifest. Keep the mod ID `SurvivorMetrics` stable.
- Treat the repository root as the Steam Workshop staging directory.
  Keep `workshop.txt` and `preview.png` at the root.
- Keep only uploadable game content inside `Contents/`.
  Keep documentation, Git metadata, and development tools outside it.
- Do not commit generated logs, temporary files, or game caches.
- Do not edit installed Workshop copies of the mod.

## Core Requirements

- Measure real-world time while the server considers a player connected,
  not in-game time. Count AFK time unless requirements change.
- The server is authoritative for session tracking and accumulated totals.
  Clients must not be able to modify recorded totals.
- Identify players using a stable identifier; do not rely on display
  names alone.
- Handle connections, disconnections, and reconnections without
  double-counting. Disconnected players must not accumulate time.
- Persist accumulated time across server restarts. Handle unexpected
  shutdowns without counting offline time, and minimize lost progress.
- Keep metric collection and persistence separate from client UI
  and future gameplay effects.

## Lua and Code Organization

- Use syntax and APIs supported by the game's embedded Lua runtime.
  Do not assume modern Lua features or external APIs are available.
- Verify unfamiliar game APIs and events against the target Build 42
  installation or reliable references. Do not invent game methods,
  events, fields, or callbacks.
- Prefer event-driven logic over unnecessary per-tick polling.
- Use local variables and functions by default. Keep mod-owned globals
  under the `SurvivorMetrics` namespace and avoid naming collisions.
- Organize Lua code by responsibility:
  - `media/lua/server/`: authoritative tracking and persistence.
  - `media/lua/shared/`: code required by both client and server.
  - `media/lua/client/`: UI, input, and presentation.
- Create directories only when needed. Prefer events and targeted hooks
  over replacing entire vanilla files.

## Verification

- Run available static checks and tests when the required tools are
  installed and configured.
- Verify unfamiliar game APIs against the target Build 42 installation
  or reliable references.
- When a game or server test environment is available, verify session
  tracking, reconnections, persistence, and recovery after shutdown.
- If runtime testing is unavailable, explicitly report what remains
  unverified and provide manual verification steps.
- Report which checks were actually run and their results.

## Scope Control

- Prefer the smallest implementation that meets the requirements.
- Do not add frameworks, abstractions, configuration layers, or
  placeholder APIs without a concrete need.
- Do not implement speculative features before their requirements
  are defined.
- Update `README.md` when setup or development instructions change.

## References

- Target-version game files and APIs are the primary reference.
- Lua API documentation:
  https://demiurgequantified.github.io/ProjectZomboidLuaDocs/
- Build 42 API and script definitions:
  https://pz-wiki-modding.github.io/PZ-API-Docs/scripts.html