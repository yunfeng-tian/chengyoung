# Using Project Hints (.cyhints)

`.cyhints` is a plain text file that puts your project conventions, build commands, and gotchas into context so ChengYoung asks less, guesses less, and stays on track.

## Two kinds of hints files

| Kind | Location | Applies to |
| --- | --- | --- |
| Global hints | `.cyhints` in the config directory | Every session |
| Project hints | `.cyhints` in the session's working directory or any parent directory, for example `.cyhints` at the project root | Sessions inside that directory tree |

Where the config directory lives:

| Platform | Path |
| --- | --- |
| Windows | `%APPDATA%\ChengYoung\agent\config\` |
| macOS | `~/Library/Application Support/ChengYoung/agent/config/` |
| Linux | `$XDG_CONFIG_HOME/ChengYoung/agent/config/` (or `~/.config/ChengYoung/agent/config/` when unset) |
| Custom root | With the `GOOSE_PATH_ROOT` environment variable set, the config directory is `<GOOSE_PATH_ROOT>/config/` |

Sessions and configuration live in ChengYoung's own data directory (the kernel's `agent` subdirectory, next to the app's settings and logs), so an install of the upstream `Block/goose` CLI never interferes with them.

Since 1.13.0 the first launch **copies** data from older versions (≤1.12.0, previously kept in `%APPDATA%\Block\goose\`) into it; that old directory is left untouched and you can delete it yourself.

## Load order

1. **Global hints**: the `.cyhints` in the config directory is loaded first and applies to every project.
2. **Project hints**: then each directory from the git root down to the session's working directory is loaded in order, so the content closest to the current directory comes last (directories outside a git repository only have the working-directory layer).
3. **Subdirectory hints**: as a session reads files or runs commands in deeper directories, the `.cyhints` of those directories is appended incrementally.

## Referencing other files

Use `@relative/path` inside `.cyhints` to pull in the contents of other files, for example:

```text
@README.md
@docs/development-setup.md
```

Files ignored by `.gitignore` are not pulled in, which keeps sensitive files such as `.env` out of the context.

## Relationship to AGENTS.md

`AGENTS.md` is also read by default (from the project root down to the session's working directory, plus `~/.agents/AGENTS.md` in your home directory), so it can live alongside `.cyhints`.

The upstream name `.goosehints` is still read when `.cyhints` is absent, so projects that already have one keep working without renaming. The editor in the app always writes `.cyhints`. Note that a blank or unreadable `.cyhints` falls back the same way, so **emptying it does not disable hints** — the legacy file takes over again; to disable them entirely, delete both files.

## Making changes take effect

- Restart the session (or start a new one) afterwards so the hints are read again.
- Hints only apply to sessions whose working directory chain contains the file. The editor in Settings reads and writes the `.cyhints` in the **current session's working directory** — the same directory shown by the switcher — and the "hint file chain" lists each layer's read state in injection order (will be read / shadowed / blank). With no session (new chat screen) it falls back to the most recent session's directory; if you then start a session in another directory, reopen the editor.

## Troubleshooting

- **Saved the file but nothing changed?** Check in order: was the session restarted (or a new one started), was the new session started outside the git root (the chain breaks there and hints stop applying), and is the file blocked by permissions or ignore rules?
- **Is the "Developer" extension required?** No. Hints are loaded directly by the ChengYoung core, independently of any extension toggle.
- **Can I put secrets in it?** No. Hints become part of the model context, so keep sensitive values in environment variables or the system credential manager.
