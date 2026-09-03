# Issue index

Source: [FINDINGS.md](FINDINGS.md). One issue, one agent. Do not combine these in a single session.

| ID | Type | Effort | Issue | Prompt |
|---|---|---|---|---|
| 01 | Task | S | [Unused HTTP/JSON crates](issues/01-unused-http-deps.md) | [prompt](prompts/01-remove-unused-http-deps.md) |
| 02 | Bug | S | [User input compiled as regex](issues/02-user-input-regex.md) | [prompt](prompts/02-stop-user-input-regex.md) |
| 03 | Bug | S | [`.env` loaded from cwd](issues/03-dotenv-cwd.md) | [prompt](prompts/03-stop-cwd-dotenv.md) |
| 04 | Bug | S | [ComSpec/SHELL trusted](issues/04-comspec-shell-trust.md) | [prompt](prompts/04-validate-comspec-shell.md) |
| 05 | Bug | M | [UNC path enumeration](issues/05-unc-path-enumeration.md) | [prompt](prompts/05-block-unc-enumeration.md) |
| 06 | Bug | S | [Eager filesystem completions](issues/06-filesystem-completion-trigger.md) | [prompt](prompts/06-restrict-filesystem-completions.md) |
| 07 | Bug | S | [Loose parent-shell match](issues/07-parent-shell-match.md) | [prompt](prompts/07-tighten-parent-shell-match.md) |
| 08 | Task | S | [LICENSE / README / todo.md](issues/08-docs-license-todo.md) | [prompt](prompts/08-fix-docs-license-todo.md) |

Suggested order if running serially: **01 → 03 → 04 → 02 → 05 → 06 → 07 → 08**.

01/03/08 do not touch ranking. 02/05/06 all touch suggestion code — do not run those three in parallel on the same worktree. 04 and 07 both touch shell detection/spawn — do not run those two in parallel on the same worktree.
