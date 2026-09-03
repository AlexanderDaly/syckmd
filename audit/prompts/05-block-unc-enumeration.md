# Agent prompt — 05 Block UNC / network path enumeration

Copy everything below the line into a new agent session.

---

Work in `C:\Users\Alexander\Documents\sickmd`. You own **only issue 05**.

Read `audit/issues/05-unc-path-enumeration.md` and `audit/FINDINGS.md` first. Do not implement issue 06 (when filesystem completions fire) beyond calling your new helper if `filesystem_candidates` already runs.

**Task:** `filesystem_candidates` does `fs::read_dir` on the directory derived from the last token. On Windows, a token like `\\server\share\foo` causes SMB/NTLM on every keystroke. Block that.

Add a helper that detects Windows UNC / network paths **only when running on Windows** (keep the platform flag injectable so tests can cover the non-Windows branch on Windows CI):

- `\\server\share\...`
- `//server/share/...`
- `\\?\UNC\server\share\...` (case-insensitive `UNC`, including `\\?\unc\` and mixed case)

Do **not** treat local `\\?\C:\...` as UNC.
Do **not** treat `//server/share/...` as UNC on non-Windows (POSIX absolute path).

If the target directory is UNC, return `Vec::new()` **before** `dir.is_dir()` and **before** `fs::read_dir`.

Leave explicit `cd \\server\share` on Enter alone unless you must skip `canonicalize` on UNC; document that choice in the PR/commit message if you change `handle_cd`.

**Allowed files:** `src/main.rs` and tests in the same file.

**Do not:** change `should_show_files` / `prefers_path_completion` (issue 06), ranking weights, or regex matching (issue 02).

**Done when:**

- Unit tests for the detector (UNC vs local vs extended local, lowercase/mixed-case `\\?\UNC\`, and `windows=false` so `//server/share` is not UNC)
- `filesystem_candidates` never calls `is_dir` or `read_dir` on a UNC path
- Local relative and drive-letter paths still complete
- `cargo test` and `cargo build` succeed

Match existing style. Minimal diff.
