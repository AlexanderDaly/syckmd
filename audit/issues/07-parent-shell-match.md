# 07 — Tighten parent-shell process matching

**Type:** Bug  
**Severity:** Low  
**Effort:** S  
**Prompt:** [../prompts/07-tighten-parent-shell-match.md](../prompts/07-tighten-parent-shell-match.md)

## Finding

`detect_parent_shell_name` walks parents and treats a process as a shell if the executable name `contains` `pwsh`, `powershell`, `cmd`, `bash`, `zsh`, `fish`, or `sh`.

`contains("sh")` matches many unrelated names. `contains("cmd")` is also broad. A wrong match can select the wrong `ShellProfile` (wrong prompt, wrong history, wrong spawn args).

## Location

- `src/main.rs` — `detect_parent_shell_name`

## Out of scope

- Do not change `ComSpec` validation (issue 04)
- Do not add support for additional shells
- Do not change suggestion ranking

## Acceptance

- Match on executable basename, case-insensitive, against an exact set: `cmd.exe`, `powershell.exe`, `pwsh.exe`, `bash.exe`, `zsh.exe`, `fish.exe`, `sh.exe` (and POSIX names without `.exe` if you keep the non-Windows walk)
- `SearchHost.exe` / `SecurityHealthService.exe` must not match
- `cmd.exe` and `powershell.exe` still match
- Unit test the matcher with a table of names
- `cargo build` succeeds
