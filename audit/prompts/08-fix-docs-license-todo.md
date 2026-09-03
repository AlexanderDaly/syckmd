# Agent prompt — 08 Fix LICENSE / README mismatch and stale todo.md

Copy everything below the line into a new agent session.

---

Work in `C:\Users\Alexander\Documents\sickmd`. You own **only issue 08**.

Read `audit/issues/08-docs-license-todo.md` and `audit/FINDINGS.md` first. Do not change Rust code. Do not relicense.

**Task:**

1. `README.md` says MIT. `LICENSE` is GNU GPLv3. Treat GPLv3 as the actual license. Update the README license sentence to match `LICENSE`.
2. `todo.md` claims a local unauthenticated service on `127.0.0.1:47653`. That service is not in this repository. Remove or rewrite that claim so it is not stated as current fact. A one-line note that a sidecar was considered and is not present is fine.
3. `todo.md` claims “no fuzzy matching”. `fuzzy_score()` exists. Remove or correct that claim.
4. At the top of `todo.md`, link to `audit/FINDINGS.md` and `audit/ISSUES.md` for security work. You may keep remaining product-gap bullets (quote-aware tokens, Ctrl+R, ranking, speed).

**Allowed files:** `README.md`, `todo.md`. Do not edit `LICENSE` text. Do not edit `audit/FINDINGS.md`.

**Do not:** implement wishlist features, touch `src/`, or change crate metadata.

**Done when:**

- README license line is GPLv3
- `todo.md` does not present port 47653 or “no fuzzy matching” as current product facts
- `todo.md` starts with links to `audit/FINDINGS.md` and `audit/ISSUES.md`

Minimal diff. No tone rewrite of the README beyond the license line.
