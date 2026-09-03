# 03 — Do not load `.env` from the current directory

**Type:** Bug  
**Severity:** Medium  
**Effort:** S  
**Prompt:** [../prompts/03-stop-cwd-dotenv.md](../prompts/03-stop-cwd-dotenv.md)

## Finding

`main` calls `dotenvy::dotenv()` with the result ignored. That searches the current directory and parent directories for a `.env` and populates environment variables that are not already set. Existing process values are preserved; `.env` cannot overwrite `ComSpec`, `PATH`, or `SHELL` if they are already present.

A planted `.env` can still set missing variables, including `SYCKMD_MAX_SUGGESTIONS`. The only env var this program documents is `SYCKMD_MAX_SUGGESTIONS`. dotenv is not needed for that.

## Location

- `src/main.rs` — `main()`
- `Cargo.toml` — `dotenvy = "0.15"`

## Out of scope

- Do not add a config-file format
- Do not validate `ComSpec` here (issue 04)
- Do not change suggestion ranking

## Acceptance

- No `dotenvy` usage
- Crate removed from `Cargo.toml` / `Cargo.lock`
- `SYCKMD_MAX_SUGGESTIONS` still read from the real process environment
- `cargo build` succeeds
