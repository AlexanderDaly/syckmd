# 01 — Remove unused HTTP/JSON crates

**Type:** Task  
**Severity:** Low (supply-chain surface, not a live backdoor)  
**Effort:** S  
**Prompt:** [../prompts/01-remove-unused-http-deps.md](../prompts/01-remove-unused-http-deps.md)

## Finding

`Cargo.toml` declares `reqwest` (blocking + json), `serde`, and `serde_json`. Nothing in `src/main.rs` imports or calls them. Git history never used them either.

They still pull hyper, native-tls, openssl, and related crates at build time.

`todo.md` mentions an unauthenticated local service on `127.0.0.1:47653`. That server is not in this tree. Likely leftover from a planned completion sidecar.

## Location

- `Cargo.toml` lines 10–12
- `Cargo.lock` (large transitive tree under `reqwest`)

## Out of scope

- Do not add a local HTTP completion service
- Do not touch ranking, spawn, or dotenv (issues 02–07)
- Do not remove `dotenvy` here (issue 03)

## Acceptance

- `reqwest`, `serde`, `serde_json` gone from `[dependencies]`
- `Cargo.lock` regenerated so those crates are no longer required by `syckmd`
- `cargo build` succeeds
- No behavior change
