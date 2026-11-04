TASK: MODIFY the existing "Agent Hub" web UI. Do not rebuild it from scratch. This is UI only: do not run any agent and do not add chat in this step.

SAFETY RULES
- Edit only Agent Hub's own files (server.js, index.html, config files). NEVER edit, rename or delete any agent .md file, PROMPTS.md, orchestrator, Python helper, source code, or anything inside .protegrity-work or TempScriptFiles.
- Node.js only, no npm packages, no CDN or external requests, localhost only.
- FIRST copy server.js and index.html to server.js.bak and index.html.bak. Then read both files and tell me in 10 lines or fewer what exists today and what you will change. Then make the changes.
- KEEP what already works: agent scanner (it must scan .github/agents), History, Settings, folder browser, saved runs, Copy/Save of output. Reuse existing code and styling where possible.

NAVIGATION (exact structure, breadcrumbs on every screen, each part clickable, plus a back button)
Home
- "Java": greyed out, badge "Coming soon", not clickable (also not by keyboard).
- ".NET": opens the .NET screen.
.NET screen: two cards
- ".NET" = the Inline suite: apps that use SQL strings, stored procedures, ADO.NET or Dapper.
- ".NET Core" = the Core suite: apps that use EF Core and DbContext.
Add a note: do not use both suites on one repo without a path-by-path plan.
Method screen (for each suite), with the hint "How does your app protect data?"
- "DSG (REST API)": "The app calls a REST API to tokenize and detokenize data." Greyed, "Coming soon", not clickable.
- "SQL functions": "Protection happens through SQL functions or views in the database." Active, opens the suite page.
Make methods data-driven: a config list per suite {id, title, description, status: "active" | "coming-soon", agentsFolder, projectValues, gates}, so DSG can be switched on later with no change to shared UI code. Do NOT assume DSG uses the same phases or form fields as SQL functions. Keep project values, progress and history separate per suite and method.

SUITE PAGE (SQL functions)
1. Top: an "Orchestrator guide" button. It opens a plain-English summary of the 8-phase flow, the files handed between phases and the approval points, read from the suite's orchestrator file. It has no Run button and is never a phase node. The orchestrator is the coordinator; the 8 phase agents are the workers.
2. A project values form built from the suite's PROMPTS.md placeholders (Inline: AppName, workspace-root, input-excel-path. Core also: sln-path, agents-dir). Saved per project. Show clearly which values are still empty.
3. A two-stage progress bar: "Code analysis & approval (phases 1-4)" then "Transformation, validation & delivery (phases 5-8)".
4. TWO CARDS side by side (stacked on narrow screens):
 - "Code analysis" (phases 1-4): does not change code, output is the impact report, progress like "2 of 4 done", status Not started / In progress / Report ready.
 - "Code transformation" (phases 5-8): status Locked / Ready / In progress / Done. Locked until the impact report exists and the user confirms approval (store the time).
 - An "Approval" connector between the cards explaining that approval is given in Copilot chat and confirmed here.
5. Clicking a card opens its flow: phase nodes with arrows and approval gate markers. In the analysis flow add a Report panel that shows the phase 4 impact report once it exists. Clicking a node opens a side panel with a plain-English summary, inputs, outputs and logs (reuse the existing agent panel).
6. Every node shows the agent's real action in a few words and a badge "Edits files" or "Does not edit files", decided from what the agent file says it writes, not from its phase number. (Inline phase 6 can build and apply user-approved fixes, and phase 7 writes tests, so they are "Edits files".)

AGENT RULES
- Get phase numbers from the file names (phase1 to phase8). Never hardcode agent names. Remove the old groups (Catalogue intake, Validation, Evidence reporting, Orchestration and so on) from the screens. Use agent-hub-spec/ui-groups.json only as a source of summaries if it exists; otherwise read the agent file.
- Core has an older phase 6 infra-build-test agent that the orchestrator replaced with phase 7. Hide it behind a "Show older agent" toggle labelled "may be superseded".
- Approval gates (read the exact wording from each suite's orchestrator and PROMPTS.md): Inline after phases 1, 2 and 4. Core in phase 1 (catalogue confirmation), after phase 4, and after phase 6 before phase 7. A gate counts as passed when its evidence file exists (for example approved-plan.json); otherwise show "answered in Copilot chat" with a manual confirm button.
- The suites never share files. Inline uses reference-list.csv, transformation-log.md, test-results.md. Core uses impact-analysis.json, approved-plan.json, transformation-log.json, validation-report.json/.md, build-result.json, test-result.json. Keep all expected file names in ONE config object so they are easy to change; read any name you are unsure of from the orchestrator rather than guessing, and mark it "unverified".

PROGRESS FROM DISK
- A setting "Work folder" with the default <workspace-root>/.protegrity-work. READ ONLY. Tick a phase when its expected files exist. Some files may be hash-checked, so never edit, move or copy them. Add .protegrity-work, runs and the saved project values to .gitignore, because they can contain PII catalogue data.
- If the work folder already has results, show a warning that a new run may overwrite them.

DESIGN
Clean, light and dark themes, one accent colour, responsive, keyboard accessible, plain-English labels.

WHEN DONE
List the files you changed, how to restart the server, anything you could not find or are unsure about, and anything you did not do. Do not claim you tested something you did not.
