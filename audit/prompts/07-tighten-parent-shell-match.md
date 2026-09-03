# Agent prompt — 07 Tighten parent-shell process matching

Copy everything below the line into a new agent session.

---

Work in `C:\Users\Alexander\Documents\sickmd`. You own **only issue 07**.

Read `audit/issues/07-parent-shell-match.md` and `audit/FINDINGS.md` first. Do not implement issue 04 (ComSpec validation).

**Task:** `detect_parent_shell_name` treats a parent as a shell if the exe name `contains` `pwsh`, `powershell`, `cmd`, `bash`, `zsh`, `fish`, or `sh`. `contains("sh")` matches unrelated processes and can pick the wrong `ShellProfile`.

Replace substring checks with **exact basename** matching (case-insensitive) against:

- `cmd.exe`
- `powershell.exe`
- `pwsh.exe`
- `bash.exe` / `bash`
- `zsh.exe` / `zsh`
- `fish.exe` / `fish`
- `sh.exe` / `sh`

Keep the parent walk (up to 32). Only change how a name is classified.

**Allowed files:** `src/main.rs` and tests in the same file. Extract `fn is_shell_exe_name(name: &str) -> bool` (or an enum-returning classifier) for tests.

**Do not:** change spawn args, history loading, or ComSpec.

**Done when:**

- Table tests: `cmd.exe` / `powershell.exe` / `pwsh.exe` match; `SearchHost.exe`, `SecurityHealthService.exe`, `hash.exe` do not
- `cargo test` and `cargo build` succeed

Match existing style. Minimal diff.
