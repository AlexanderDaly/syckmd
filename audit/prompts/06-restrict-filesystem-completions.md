# Agent prompt — 06 Restrict when filesystem completions are offered

Copy everything below the line into a new agent session.

---

Work in `C:\Users\Alexander\Documents\sickmd`. You own **only issue 06**.

Read `audit/issues/06-filesystem-completion-trigger.md` and `audit/FINDINGS.md` first. Do not implement UNC blocking (issue 05) unless that helper already exists — then call it, do not reimplement.

**Task:** In `suggestion_for_editor`, filesystem candidates are gathered when `left` has any whitespace **or** the last token is empty. That lists cwd files for `echo hello` and similar, so a planted filename can become a Tab completion.

Change the trigger so `filesystem_candidates` runs only when:

1. the last token contains `\` or `/`, or
2. `prefers_path_completion(left)` is true

Keep PATH / builtin / history ranking as they are. Do not build a general anti-poisoning system.

**Allowed files:** `src/main.rs` and tests in the same file. Extracting the boolean to a tiny helper like `fn should_offer_filesystem(left: &str, token_fragment: &str) -> bool` is encouraged so you can table-test it.

**Do not:** rewrite `prefers_path_completion` command list except to reuse it; do not add quote-aware parsing; do not change `fuzzy_score`.

**Done when:**

- Tests: `echo hi` → false; `cd ` → true; `dir ` → true; `type file` → true; `copy x \` → true; `git commit` → false
- `cargo test` and `cargo build` succeed

Match existing style. Minimal diff.
