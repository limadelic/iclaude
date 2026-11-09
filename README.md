# macOS keeps asking 'Allow "2.1.xxx" to access...' after each Claude Code update

## Why

macOS keys each privacy permission by the path of the program. Each Claude Code version has a new path. After each update, macOS asks again for each permission.

Background sessions and the daemon of Claude Code start without a terminal. Their prompts come from the program path too.

## Install

```sh
brew install limadelic/tap/iclaude
iclaude on
```

Restart claude.

## Use

```sh
iclaude on
iclaude off
iclaude status
```

- `on` pins claude to one path.
- `off` removes the pin.
- `status` shows the state.

## How it works

1. `iclaude on` sets `env.CLAUDE_CODE_PROCESS_WRAPPER` in `~/.claude/settings.json` to the path of `iclaude`.
2. Claude runs each program through `iclaude`. If the program is claude, `iclaude` copies it to `~/.local/share/iclaude/claude`.
3. `iclaude` runs the fixed copy. Its path does not change after an update.
4. Any other program runs as it is.

## Security

- One sh file. No sudo. No network.
- It does not edit the macOS privacy database.
- It keeps only the permissions that you already gave.

## Limits

- iclaude depends on `CLAUDE_CODE_PROCESS_WRAPPER`. If claude changes, iclaude runs the program as it is. The worst case is the old prompts.
- A running daemon needs a restart. Run `claude daemon stop` when background sessions are idle.

I, Claudius.
