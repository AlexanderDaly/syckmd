# 08 — Fix LICENSE / README mismatch and stale todo.md

**Type:** Task  
**Severity:** Low  
**Effort:** S  
**Prompt:** [../prompts/08-fix-docs-license-todo.md](../prompts/08-fix-docs-license-todo.md)

## Finding

- `README.md` says MIT. `LICENSE` is GNU GPLv3 (added in commit `thonk LICENSE`).
- `todo.md` claims an unauthenticated service on `127.0.0.1:47653` (not in this tree).
- `todo.md` claims “no fuzzy matching” (`fuzzy_score` exists).

## Location

- `README.md` (License section)
- `LICENSE`
- `todo.md`

## Out of scope

- Do not relicense. Treat GPLv3 as the actual license unless the owner later says otherwise
- Do not rewrite the product README
- Do not implement wishlist items (Ctrl+R, quote-aware tokens, ranking)

## Acceptance

- README license line matches `LICENSE` (GPLv3)
- `todo.md` no longer states a local port-47653 service as current fact
- `todo.md` no longer states that fuzzy matching is absent
- Remaining product-gap notes can stay, or point at `audit/` for security work
- Link `todo.md` at the top to `audit/FINDINGS.md` / `audit/ISSUES.md`
