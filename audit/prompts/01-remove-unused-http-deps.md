# Agent prompt — 01 Remove unused HTTP/JSON crates

Copy everything below the line into a new agent session.

---

Work in `C:\Users\Alexander\Documents\sickmd`. You own **only issue 01**.

Read `audit/issues/01-unused-http-deps.md` and `audit/FINDINGS.md` first. Do not implement any other audit issue.

**Task:** Remove unused crates `reqwest`, `serde`, and `serde_json` from this Rust project. They are declared in `Cargo.toml` and locked in `Cargo.lock` but never imported in `src/main.rs`. Git history never used them.

**Allowed files:** `Cargo.toml`, `Cargo.lock`. You may run `cargo build` / `cargo tree`. Do not edit `src/main.rs`. Do not remove `dotenvy` (issue 03). Do not add network code.

**Done when:**

- `[dependencies]` no longer lists those three crates
- `Cargo.lock` no longer has `syckmd` depending on them (regenerate with cargo, do not hand-edit)
- `cargo build` succeeds
- Diff is dependency-only

Match existing style. Minimal diff. No extra refactors, no README edits, no new features.
