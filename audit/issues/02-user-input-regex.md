# 02 — Stop compiling user input as a regex

**Type:** Bug  
**Severity:** Low (wrong match semantics; Rust `regex` does not catastrophically backtrack)  
**Effort:** S  
**Prompt:** [../prompts/02-stop-user-input-regex.md](../prompts/02-stop-user-input-regex.md)

## Finding

`build_query_regex` passes the last token (up to 96 chars, unescaped) to `RegexBuilder::new`. That regex is then `is_match`'d against candidates on every keystroke.

Rust's `regex` crate uses finite automata and does not hang on catastrophic backtracking. Unescaped input still costs bounded CPU per candidate and, more importantly, changes match semantics: `foo.txt` is a regex dot, not a literal filename.

## Location

- `src/main.rs` — `build_query_regex`
- Callers: `suggestion_for_editor` (history/builtins/executables and filesystem branches)

## Out of scope

- Do not rewrite `fuzzy_score` or ranking weights
- Do not add zsh-style glob completion
- Do not touch UNC handling (issue 05) except if a shared helper is strictly required
- Do not remove the `regex` crate unless nothing else needs it (nothing else does — removing it is allowed if you switch off regex entirely)

## Acceptance

- User tokens are matched as literals (case-insensitive substring / prefix), not as regex
- Regex metacharacters in the token (`(a+)+$`, `****`, `.`) do not change match semantics
- `foo.txt` still matches a candidate containing `foo.txt`
- Existing prefix-match ghost completion still works
- Add a unit test for literal matching of metacharacters and a pathological token
