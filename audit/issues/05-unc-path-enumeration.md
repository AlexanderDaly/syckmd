# 05 — Block UNC / network path enumeration in completion

**Type:** Bug  
**Severity:** Medium  
**Effort:** M  
**Prompt:** [../prompts/05-block-unc-enumeration.md](../prompts/05-block-unc-enumeration.md)

## Finding

`filesystem_candidates` calls `fs::read_dir` on the directory derived from the last token. On Windows, typing `cd \\server\share\` (or completing toward it) enumerates that path and can trigger SMB/NTLM authentication.

`handle_cd` will also `is_dir` / `canonicalize` a UNC target. Changing cwd to a network path is a user action; the keystroke-time `read_dir` is the bug.

## Location

- `src/main.rs` — `filesystem_candidates`
- Optionally guard `handle_cd` so completion-adjacent probes do not hit the network; do not forbid an explicit Enter on `cd \\server\share` unless you document that choice

## Out of scope

- Do not redesign path completion (issue 06 is when files are offered, this issue is network paths)
- Do not rewrite ranking weights
- Do not block `\\?\` local extended paths that are not UNC

## Acceptance

- Helper recognizes Windows UNC (`\\server\...`, `//server/...`, `\\?\UNC\...`)
- `filesystem_candidates` returns empty for UNC / network roots and does not `read_dir` them
- Local paths (`C:\...`, relative, `\\?\C:\...`) still complete
- Unit tests for the UNC detector
- `cargo build` succeeds
