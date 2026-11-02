You are helping me build "Agent Hub": a local web UI that runs my markdown-based GitHub agents (.NET code analysis and code transformation) and shows how a run flows. First write a short plan and list any assumptions, then build in small vertical slices. Do not add features I did not ask for.

CONTEXT
- My agents are markdown files in folders on disk. Some folders start with a number (01, 02, 03...) and run in that order; others are standalone.
- Agents are run today from a terminal command (Copilot CLI or similar). The UI wraps that command; it does not replace it.
- The code may be sensitive. Everything stays local: no telemetry, no CDN scripts, no cloud services, no external network calls.

TECH
- Node.js 18+, zero npm dependencies (http, fs, path, child_process only).
- Files: server.js, index.html (vanilla JS + CSS, no framework, no build step), README.md.
- Server listens on 127.0.0.1 only (default port 4173). Config in config.json: { agentsDir, runCommand, timeoutMinutes, flowEdges (optional) }. If a folder path is passed as argv[2], save it as agentsDir.

BACKEND REQUIREMENTS
1. Agent discovery: recursively scan agentsDir (max depth 4, skip dot-folders, node_modules, runs) for .md files. For each return: path (relative, forward slashes), group (its folder), file, name (frontmatter name, else first H1, else filename), description (frontmatter description, else first paragraph), kind ("agent" if *.agent.md or has frontmatter name/description/tools, else "doc"). Sort by group (numeric-aware), agents before docs.
2. Run: POST /api/run {agent, agentName, target, instructions, siblings, dryRun}.
- Compose the prompt = agent file text + "---" + "## Task" + "Target: <path>" + user instructions + (if siblings) the other .md files in the same folder as "## Reference: <name>".
- Save it to runs/<id>.prompt.md.
- If dryRun or runCommand is empty: stream the prompt back and finish with status "preview".
- Otherwise spawn runCommand with shell:true, cwd = target folder (or the file's folder, or agentsDir if no target). Replace {promptFile}, {agentFile}, {target} (double-quoted) in the command. Also write the prompt to the process stdin.
- Stream newline-delimited JSON over a chunked response: {t:"start",id,cwd}, {t:"out",d}, {t:"err",d}, {t:"end",status,code,ms}. Status values: done, failed, stopped, preview.
- Enforce timeoutMinutes. On Windows kill the process tree with taskkill /T /F; elsewhere use a process group.
- Save runs/<id>.json (meta) and runs/<id>.log (output).
3. POST /api/cancel {id}, GET /api/runs, GET /api/runs/:id, GET/PUT /api/config, GET /api/agents, GET /api/agent?path=, GET /api/browse?path= (lists folders and files; on Windows allow drive roots).
4. Security: reject requests whose Host is not 127.0.0.1 or localhost; require header "X-Agent-Hub: 1" on every non-GET request; block path traversal (agent paths must stay inside agentsDir); limit request body to 1 MB.

UI REQUIREMENTS (index.html)
1. Top bar: app name, current agents folder, buttons: Flow, Refresh, Settings.
2. Left sidebar with tabs Agents and History.
- Agents: search box, folders as collapsible groups with counts, each agent shows name + description; "doc" files look dimmer.
- History: list of past runs (agent name, status chip, time, target). Selecting one loads its output and form values.
3. Main area for a selected agent: name, relative path, description, and sub-tabs Run and Agent file (shows the raw markdown).
4. Run form: Target path (text input + Browse button opening a server-side folder/file picker modal), Extra instructions (textarea), checkbox "Include the other .md files in this agent's folder as reference", buttons Run agent, Stop (visible only while running), Preview prompt. If no runCommand is set, show a banner linking to Settings and label the main button "Build prompt".
5. Output panel with a thin status rail (animated while running; green, red or amber when finished), status text with duration, and a Raw / Formatted / Steps switch, plus Copy and Save .md.
- Raw: monospace, live streaming, stderr in a different color, auto-scroll.
- Formatted: small built-in markdown renderer (headings, bold, italic, inline code, code fences, lists, tables). Escape all HTML.
- Steps: vertical timeline built from headings in the output (# to ####, or lines starting with Step, Phase, Stage; ignore lines inside code fences). Show each step's time offset while running; the latest step pulses.
6. Flow view (Flow button toggles it):
- SVG graph, one node per agent folder. Folders whose names start with a number are chained in numeric order with arrows; other folders appear in a "Standalone agents" row with no arrows. If config.flowEdges is set (array of [fromFolder, toFolder]), use those edges instead.
- Node shows folder name, file count, and the latest run status ("Done 3 h ago", "Not run yet", "Running now"). Left color bar by status: done green, running blue (pulsing), failed red, stopped/preview amber, none grey. An arrow turns green when its source node is done.
- Clicking a node opens that folder's main agent.
7. Settings dialog: agents folder (with Browse), run command (monospace textarea, explain the placeholders), timeout minutes. Show a first-run screen asking for the agents folder when none is set.
8. Design: clean light theme with automatic dark mode (prefers-color-scheme), cool grey backgrounds, one blue accent, font stack "Segoe UI Variable Text", "Segoe UI", system-ui; mono "Cascadia Mono", Consolas. Sentence case, no all-caps labels. Visible keyboard focus, respect prefers-reduced-motion, responsive down to narrow screens.

QUALITY
- No hardcoded agent list; everything is read from disk.
- All errors shown in the UI in plain language (what went wrong and how to fix it).
- Add a short README with start command: node server.js "<agents folder>", and how to set the run command.
- After building, test with sample agent folders: preview run, a real run using a simple echo-style command, stop mid-run, history reload, bad target path, path traversal attempt, and a request without the X-Agent-Hub header. Report what you tested.
