# Security audit — 2026-09-03

Static review of `AlexanderDaly/syckmd` (fork of `xsysctl/syckmd`), commit `05cb4a7`.

**Verdict:** No obvious malware, C2, or exploit payloads were found in the reviewed source and git history. Runtime execution and independent crates.io checksum review were not performed. This is a local Windows shell wrapper with ghost completion. Findings below are real bugs and leftover surface, not implants.

## Scope

Reviewed:

- `src/main.rs` (only source file)
- `Cargo.toml` / `Cargo.lock`
- All 7 git commits
- Hidden files, git hooks, submodules, `build.rs`
- README media URL (resolves to a real `.mp4` on GitHub user-attachments)

Not done: executing the binary, independent crates.io compromise review of every checksum.

## What is not malware

| Behavior | Why it is here |
|---|---|
| Reads keystrokes | Line editor |
| `Command` spawn of typed lines | Product: wrap CMD / PowerShell |
| ToolHelp process snapshot | Detect parent shell |
| Console / PSReadLine history | Suggestion corpus |
| PATH directory listing | Executable-name suggestions |

Nothing in this tree sends data off-box. Git history never contained HTTP call sites in Rust source.

## Accepted residual risk

Do not file extra issues for these:

- Enter runs the typed line via `cmd /D /C` or `powershell -NoLogo -Command`.
- An attacker who already controls `PATH` or `ConsoleHost_history.txt` can influence suggestions.
- `ComSpec` / `SHELL` are process environment after a basename/existence footgun check. Authenticating the binary (SystemRoot, signature) is out of scope; planting a file named `cmd.exe` and setting `ComSpec` is the same class as controlling `PATH`.
- Full zsh/fish-grade completion trust is out of scope for this pass.

## Findings (actionable)

Tracked as local issues under `audit/issues/`. GitHub Issues are disabled on this fork.

| ID | Severity | Title |
|---|---|---|
| [01](issues/01-unused-http-deps.md) | Low | Unused `reqwest` / `serde` / `serde_json` still pulled at build time |
| [02](issues/02-user-input-regex.md) | Low | Last token compiled as a regex (literal vs regex semantics) |
| [03](issues/03-dotenv-cwd.md) | Medium | `dotenvy::dotenv()` loads cwd `.env` into process env |
| [04](issues/04-comspec-shell-trust.md) | Medium | `ComSpec` / `SHELL` used as spawn target with no check |
| [05](issues/05-unc-path-enumeration.md) | Medium | Completion `read_dir` on UNC / network paths |
| [06](issues/06-filesystem-completion-trigger.md) | Low | Filesystem completions offered too eagerly |
| [07](issues/07-parent-shell-match.md) | Low | Parent-shell match uses `contains("sh")` / `contains("cmd")` |
| [08](issues/08-docs-license-todo.md) | Low | README says MIT; LICENSE is GPLv3; `todo.md` is stale |

## Stale notes in upstream `todo.md`

- “Local service on `127.0.0.1:47653`” — **not present** in this tree or git history. Combined with unused `reqwest`, it looks like a planned Fig-style sidecar that was never committed (or was stripped before the initial commit).
- “No fuzzy matching” — **false**. `fuzzy_score()` exists; it only ranks candidates that already prefix-match left-of-cursor.

## Dispatch

One agent per issue. Prompts: `audit/prompts/`. Index: `audit/ISSUES.md`.
