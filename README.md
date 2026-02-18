# pi-notify-core

[![CI](https://github.com/rbright/pi-notify-core/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/rbright/pi-notify-core/actions/workflows/ci.yml)

`pi-notify-core` is a small shared library for Pi notification extensions.

npm package: `@rbright/pi-notify-core`

It provides:
- turn-completion detection/wiring helpers (`agent_start`, `message_end`, `agent_end`)
- deterministic text sanitization/truncation for notifications

Pi agent project: https://github.com/badlogic/pi-mono

## Install

```bash
bun add @rbright/pi-notify-core
# or
npm install @rbright/pi-notify-core
```

## Usage

```ts
import { registerTurnCompletionHooks, sanitizeNotificationText } from '@rbright/pi-notify-core';

registerTurnCompletionHooks(pi, () => {
  const title = sanitizeNotificationText('Pi', 'Pi', 64);
  const body = sanitizeNotificationText('Turn complete — awaiting feedback', 'Turn complete', 180);
  sendNotification(title, body);
});
```

## API

### `registerTurnCompletionHooks(api, onTurnComplete)`
Registers completion handlers with dedupe per agent run:
- resets state on `agent_start`
- triggers on final assistant `message_end` (non-tool, non-error, non-aborted)
- falls back to `agent_end`

### `createTurnCompletionTracker(onTurnComplete)`
Lower-level tracker if you need custom event wiring.

### `shouldNotifyFromAssistantMessage(message)`
Returns `true` for assistant messages that represent a completed turn.

### `sanitizeNotificationText(input, fallback, maxLength)`
Strips control chars/semicolons, normalizes whitespace, and truncates with `…`.

### `truncateText(input, maxLength)`
Truncates string with ellipsis behavior used by sanitizer.

## Development

```bash
just deps
just lint
just typecheck
just test
just build
just check
just precommit-install
just precommit-run
```

## Publishing

Manual publish (`@rbright/pi-notify-core`):

```bash
bun run check
bun run build
npm publish --access public
```

Automated publish is available via GitHub Actions (`.github/workflows/publish.yml`) and runs on:
- `workflow_dispatch`
- tag pushes matching `v*`

Required repository secret:
- `NPM_TOKEN` (npm token with publish permission)
