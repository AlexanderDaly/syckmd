# 02 — Stop compiling user input as a regex

**Type:** Bug  
**Severity:** Medium (local ReDoS of the wrapper)  
**Effort:** S  
**Prompt:** [../prompts/02-stop-user-input-regex.md](../prompts/02-stop-user-input-regex.md)

## Finding

`build_query_regex` passes the last token (up to 96 chars, unescaped) to `RegexBuilder::new`. That regex is then `is_match`'d against candidates on every keystroke.

Typing a pathological pattern (nested quantifiers, overlapping alternation) can hang suggestion ranking. Special regex characters in ordinary paths also change match semantics (`foo.txt` is not a literal dot).

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
- Input such as `(a+)+$` or `****` does not hang ranking
- `foo.txt` still matches a candidate containing `foo.txt`
- Existing prefix-match ghost completion still works
- Add a unit test for literal matching and a non-hanging pathological token
