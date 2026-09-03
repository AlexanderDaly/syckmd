# Agent prompt — 04 Validate ComSpec / SHELL before spawn

Copy everything below the line into a new agent session.

---

Work in `C:\Users\Alexander\Documents\sickmd`. You own **only issue 04**.

Read `audit/issues/04-comspec-shell-trust.md` and `audit/FINDINGS.md` first. Do not implement issue 03 (dotenv) or issue 07 (parent-process name matching).

**Task:** `detect_shell_profile` uses `env::var("ComSpec")` (Windows CMD) and `env::var("SHELL")` (non-Windows) as `Command::new` program with no validation. If those env vars point at an attacker binary, every Enter runs it.

This is a footgun guard, not binary authentication. Do not require SystemRoot, Authenticode, or a Unix-shell allowlist. Planting a file named `cmd.exe` and setting `ComSpec` is the same class as controlling `PATH` (accepted residual risk).

Add a small helper that:

- Windows: only accept `ComSpec` if it is a non-empty path to an **existing file** whose basename is `cmd.exe` (case-insensitive). Otherwise use `"cmd"`.
- Non-Windows: only accept `SHELL` if it is a non-empty path to an existing file. Otherwise use `"/bin/sh"`.
- Do not accept a `ComSpec` of `C:\temp\evil.exe`.

PowerShell/pwsh already use hardcoded `"powershell"` / `"pwsh"`. Leave that.

**Allowed files:** `src/main.rs` (plus tests in the same file). Prefer a testable helper, e.g. `fn resolve_cmd_program(comspec: Option<&str>, exists: impl Fn(&Path) -> bool) -> String`, rather than hitting the real filesystem in tests.

**Do not:** sandbox user commands, rewrite quoting, change parent-shell detection, remove dotenv.

**Done when:**

- Validation helper exists and is unit-tested (good path, missing file, wrong basename, empty/unset)
- `cargo test` and `cargo build` succeed
- `run_command` still passes the typed line through; you are only hardening which program is spawned

Match existing style. Minimal diff.
