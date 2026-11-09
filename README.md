# macOS keeps asking 'Allow "2.1.xxx" to access...' after each Claude Code update

## Why

macOS stores a privacy permission by the path of the program. Claude Code puts each version in a new path. After each update, macOS asks for each permission again.

This repo runs Claude Code from one fixed path. macOS asks one time for each permission.

## Install

```sh
git clone https://github.com/<you>/claudestable.git && cd claudestable && ./install
```

Then run `claude daemon stop --any` and start claude again.

## Check

```sh
./check
```

## Uninstall

```sh
./uninstall
```

This removes the setting from `~/.claude/settings.json`. It moves the launcher and the fixed copy to a dated folder in `/tmp`.

## How it works

1. Claude Code starts each background session through `~/.local/bin/claude-stable-exec`. The setting `env.CLAUDE_CODE_PROCESS_WRAPPER` names this launcher.
2. When the version changes, the launcher copies the new version to `~/.local/share/claude/stable/claude`. The copy is an APFS clone, so it uses no extra disk space.
3. The launcher runs the fixed copy.
4. If the program is not the current version, the launcher runs the command as it is.

## Limits

- The launcher depends on `CLAUDE_CODE_PROCESS_WRAPPER` and the `versions/` layout. If either changes, the launcher falls back to a plain exec. The worst case is the old prompts.
- A `claude` started by hand in a terminal does not use the launcher.
- Each new permission type asks one time.
