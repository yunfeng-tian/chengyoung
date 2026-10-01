# Using Project Hints (.goosehints)

`.goosehints` is a plain text file that puts your project conventions, build commands, and gotchas into context so ChengYoung asks less, guesses less, and stays on track.

## Two kinds of hints files

| Kind | Location | Applies to |
| --- | --- | --- |
| Global hints | `.goosehints` in the config directory | Every session |
| Project hints | `.goosehints` in the session's working directory or any parent directory, for example `.goosehints` at the project root | Sessions inside that directory tree |

Where the config directory lives:

| Platform | Path |
| --- | --- |
| Linux / macOS | `~/.config/goose/` |
| Windows | `%APPDATA%\Block\goose\config\` |
| Custom root | With the `GOOSE_PATH_ROOT` environment variable set, the config directory is `<GOOSE_PATH_ROOT>/config/` |

The directory names keep the upstream `Block/goose` naming so that existing config and data remain compatible.

## Load order

1. **Global hints**: the `.goosehints` in the config directory is loaded first and applies to every project.
2. **Project hints**: then each directory from the git root down to the session's working directory is loaded in order, so the content closest to the current directory comes last.
3. **Subdirectory hints**: as a session reads files or runs commands in deeper directories, the `.goosehints` of those directories is appended incrementally.

## Referencing other files

Use `@relative/path` inside `.goosehints` to pull in the contents of other files, for example:

```text
@README.md
@docs/development-setup.md
```

Files ignored by `.gitignore` are not pulled in, which keeps sensitive files such as `.env` out of the context.

## Relationship to AGENTS.md

`AGENTS.md` is also read by default (from the project root down to the session's working directory, plus `~/.agents/AGENTS.md` in your home directory), so it can live alongside `.goosehints`.

## Making changes take effect

- Restart the session (or start a new one) afterwards so the hints are read again.
- Hints only apply to sessions whose working directory chain contains the file: the settings dialog shows the path being read and written, and the directory switcher shows the current session's working directory. Edits take effect only when the two agree.

## Troubleshooting

- **Saved the file but nothing changed?** Check in order: is the path part of the current session's working directory chain, was the session restarted, and is the file blocked by permissions or ignore rules?
- **Is the "Developer" extension required?** No. Hints are loaded directly by the ChengYoung core, independently of any extension toggle.
- **Can I put secrets in it?** No. Hints become part of the model context, so keep sensitive values in environment variables or the system credential manager.
