Security work: see [audit/FINDINGS.md](audit/FINDINGS.md) and [audit/ISSUES.md](audit/ISSUES.md).

A local completion sidecar was considered and is not present.

No quote-aware token acceptance
Word-accept logic is whitespace-based; quoted arguments can be broken.

No path-aware/tab-aware filesystem completion model
Suggestion engine doesn’t understand filesystem context like zsh completion does.

No contextual ranking by directory/session/toolchain
Modern tools prioritize by cwd/project; current ranking is simple recency/frequency-like behavior.

No interactive history search (Ctrl+R-style) with preview/filter
zsh + fzf/fish provide much stronger retrieval than linear up/down navigation.

some accessibility and ranking improvements are possible

speed and resource usage are not optimized yet
