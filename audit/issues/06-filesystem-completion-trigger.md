# 06 — Restrict when filesystem completions are offered

**Type:** Bug  
**Severity:** Low  
**Effort:** S  
**Prompt:** [../prompts/06-restrict-filesystem-completions.md](../prompts/06-restrict-filesystem-completions.md)

## Finding

`suggestion_for_editor` shows filesystem candidates whenever the left side has any whitespace **or** the last token is empty.

That means `echo hello` and `git commit` probe cwd for filenames even when the command is not path-oriented. A planted file in cwd can become a Tab-completable argument.

First-token PATH / builtin / history suggestions are accepted residual risk (see FINDINGS.md). This issue is only the trigger for `filesystem_candidates`.

## Location

- `src/main.rs` — `should_show_files` in `suggestion_for_editor`
- Existing helper: `prefers_path_completion`

## Out of scope

- Do not implement quote-aware tokens or zsh completion
- Do not change UNC handling except to call the helper from issue 05 if it already exists
- Do not change history / PATH / builtin ranking

## Acceptance

- Filesystem completions run only when:
  - the last token contains `\` or `/`, or
  - `prefers_path_completion(left)` is true
- `echo hi` does not list cwd files
- `cd `, `dir `, `type `, and `copy x \` still list files
- `cargo build` succeeds
