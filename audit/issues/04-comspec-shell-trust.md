# 04 — Validate ComSpec / SHELL before spawn

**Type:** Bug  
**Severity:** Medium  
**Effort:** S  
**Prompt:** [../prompts/04-validate-comspec-shell.md](../prompts/04-validate-comspec-shell.md)

## Finding

CMD profile uses `env::var("ComSpec")` as `Command::new` program. Non-Windows uses `env::var("SHELL")`. No check that the value is a real `cmd.exe` / shell binary.

If the environment is poisoned (including via issue 03), every Enter runs the attacker binary with the typed line as an argument.

PowerShell / pwsh paths hardcode `"powershell"` / `"pwsh"` (PATH search). That is acceptable for this issue; do not expand into a full PATH-trust model.

## Location

- `src/main.rs` — `detect_shell_profile`
- Spawn: `run_command`

## Out of scope

- Do not remove dotenv (issue 03)
- Do not change parent-process name matching (issue 07)
- Do not sandbox or quote-rewrite user commands

## Acceptance

- If `ComSpec` is unset, empty, or does not point at an existing file whose name is `cmd.exe` (case-insensitive), fall back to `cmd`
- If `SHELL` is unset, empty, or not an existing file, fall back to `/bin/sh`
- Refuse to spawn a `ComSpec` whose filename is not `cmd.exe` (e.g. `C:\temp\evil.exe`)
- `cargo build` succeeds; add a unit test for the validation helper
