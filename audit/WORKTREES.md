# Isolated worktrees

Created so eight agents can land the 2026-09-03 audit issues without sharing a working tree.

Parent checkout stays on `main` at `C:\Users\Alexander\Documents\sickmd`.
Do not merge these branches until every agent has finished.

| ID | Branch | Worktree |
|---|---|---|
| 01 | `audit/01-unused-http-deps` | `.worktrees/01-unused-http-deps` |
| 02 | `audit/02-user-input-regex` | `.worktrees/02-user-input-regex` |
| 03 | `audit/03-dotenv-cwd` | `.worktrees/03-dotenv-cwd` |
| 04 | `audit/04-comspec-shell` | `.worktrees/04-comspec-shell` |
| 05 | `audit/05-unc-enumeration` | `.worktrees/05-unc-enumeration` |
| 06 | `audit/06-filesystem-trigger` | `.worktrees/06-filesystem-trigger` |
| 07 | `audit/07-parent-shell-match` | `.worktrees/07-parent-shell-match` |
| 08 | `audit/08-docs-license-todo` | `.worktrees/08-docs-license-todo` |

`.worktrees/` is gitignored. Merge order after agents finish: **01 → 03 → 04 → 02 → 05 → 06 → 07 → 08**.
