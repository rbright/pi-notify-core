# AGENTS.md

## Scope
This file applies to the entire `pi-notify-core` repository.

## Mission
Provide a minimal, side-effect-free core library for Pi notification extensions.

## Architecture Rules

### Module boundaries
- `src/completion.ts`: turn-completion detection and Pi event-hook wiring helpers.
- `src/sanitize.ts`: notification text sanitization and truncation utilities.
- `src/index.ts`: exports only.

### Design constraints
- Keep this package transport-agnostic (no OSC/Slack/TTS specifics).
- Keep this package side-effect free (no I/O, no process mutation, no shelling out).
- Prefer small pure functions with deterministic behavior.
- Keep exported surface minimal and stable.

## Testing & Quality
Run before handoff:

1. `just lint`
2. `just typecheck`
3. `just test`
4. `just build`

## Tooling
- Use Bun for dependencies and scripts.
- Use ESLint + Prettier for code quality.
- Use Vitest for tests.
- Use Prek (`prek`) for pre-commit hooks.

## Safety
- Do not introduce secrets.
- Do not add runtime network access.
- Do not add side effects in core modules.
