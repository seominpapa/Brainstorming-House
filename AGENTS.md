## graphify

This project has a knowledge graph at `graphify-out/` with god nodes, community structure, and cross-file relationships.

When the user types `/graphify`, invoke the `skill` tool with `skill: "graphify"` before doing anything else.

Rules:
- ALWAYS read `graphify-out/GRAPH_REPORT.md` before reading source files, running grep/glob searches, or answering codebase questions. The graph is your primary map of the codebase.
- IF `graphify-out/wiki/index.md` EXISTS, navigate it instead of reading raw files.
- Keep `llm-wiki/wiki/**/*.md` content in Korean by default: H1 titles, section headings, summaries, key points, and Obsidian-style `[[...]]` page links should be Korean unless a product/company name is normally written in English.
- Do not show table-of-contents or operational files as graph content nodes. Exclude `llm-wiki/AGENTS.md`, `llm-wiki/wiki/index.md`, `llm-wiki/wiki/log.md`, and raw manifest files from the visual knowledge graph.
- The HTML knowledge graph must show directed arrows for file-to-file wiki links. A line `A -> B` means `A.md` contains an Obsidian-style link that resolves to `B.md`; bidirectional arrows mean both pages link to each other.
- When regenerating the local HTML graph, prefer `node scripts/build-wiki-graph.mjs`; it outputs `graphify-out/graph.json`, `graphify-out/graph.html`, `graphify-out/wiki-graph.json`, and `graphify-out/wiki-graph.html` with Korean UI labels and arrow markers.
- For cross-module "how does X relate to Y" questions, prefer `graphify query "<question>"`, `graphify path "<A>" "<B>"`, or `graphify explain "<concept>"` over grep; these traverse the graph's EXTRACTED + INFERRED edges instead of scanning files.
- On this Windows workspace, prefer `C:\Users\com\AppData\Roaming\uv\tools\graphifyy\Scripts\python.exe -m graphify ...` if the `graphify` shim fails on the workspace path.
- After modifying code or wiki structure, run `C:\Users\com\AppData\Roaming\uv\tools\graphifyy\Scripts\python.exe -m graphify update . --force` to keep the graph current (AST-only, no API cost).
