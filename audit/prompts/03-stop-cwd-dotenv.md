# Agent prompt — 03 Do not load .env from cwd

Copy everything below the line into a new agent session.

---

Work in `C:\Users\Alexander\Documents\sickmd`. You own **only issue 03**.

Read `audit/issues/03-dotenv-cwd.md` and `audit/FINDINGS.md` first. Do not implement issue 04 (ComSpec validation).

**Task:** `main()` calls `dotenvy::dotenv()` and ignores the result. That searches cwd and parents for a `.env` and fills in missing environment variables (it does not overwrite existing `ComSpec`/`PATH`/`SHELL`). A planted `.env` can still set unset vars such as `SYCKMD_MAX_SUGGESTIONS`.

Remove dotenv loading. Remove the `dotenvy` dependency. Keep reading `SYCKMD_MAX_SUGGESTIONS` from the real environment.

**Allowed files:** `src/main.rs`, `Cargo.toml`, `Cargo.lock`.

**Do not:** add a config file format, validate ComSpec, change suggestions.

**Done when:**

- No `dotenvy` in source or `[dependencies]`
- `SYCKMD_MAX_SUGGESTIONS` still works from the process env
- `cargo build` succeeds
- Diff is small

Match existing style. Minimal diff.
