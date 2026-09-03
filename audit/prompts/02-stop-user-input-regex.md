# Agent prompt — 02 Stop compiling user input as a regex

Copy everything below the line into a new agent session.

---

Work in `C:\Users\Alexander\Documents\sickmd`. You own **only issue 02**.

Read `audit/issues/02-user-input-regex.md` and `audit/FINDINGS.md` first. Do not implement issues 05, 06, or ranking redesign.

**Task:** `build_query_regex` in `src/main.rs` compiles the user's last token (up to 96 chars, unescaped) with `RegexBuilder` and runs it against suggestion candidates on every keystroke. That is ReDoS and wrong match semantics (`foo.txt` is not a literal dot).

Replace this with **case-insensitive literal** matching (substring / prefix). You may delete `build_query_regex` and drop the `regex` crate if nothing else uses it.

**Allowed files:** `src/main.rs`, `Cargo.toml`, `Cargo.lock`, and new tests under `src/` (e.g. `#[cfg(test)]` in `main.rs` or `src/` modules if you must split a tiny helper — prefer tests in `main.rs` to avoid a drive-by module split).

**Do not:** rewrite `fuzzy_score`, change rank bonuses, add glob/zsh completion, touch UNC or dotenv.

**Done when:**

- User tokens are never passed to the regex engine as patterns
- `(a+)+$` / similar input does not hang ranking
- `foo.txt` matches a candidate containing `foo.txt`
- Prefix ghost completion still works
- Unit tests cover literal match vs regex metacharacters and a pathological token
- `cargo test` and `cargo build` succeed

Match existing style. Minimal diff.
