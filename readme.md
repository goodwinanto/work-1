FIX the existing Agent Hub UI (edit only its own files; never edit agent files, PROMPTS.md or anything in .protegrity-work; keep the .bak backups). Do not run agents. Do not rebuild from scratch.

REMOVE the extra groups Copilot added earlier: Code discovery, Input and catalogue (intake), Orchestration, Report and evidence, Validation and testing, and any similar group. They must not appear as cards, sidebar groups or filters anywhere. Remove the old groups sidebar entirely. Do not add them back.

THE ONLY TWO GROUPS are "Code analysis" and "Code transformation". Phase numbers decide the group, taken from the agent file names:
- Code analysis = phases 1 to 4 (including the catalogue parser, which is phase 1, and the impact report, which is phase 4).
- Code transformation = phases 5 to 8 (including validation, build and test, unit tests, and the evidence report).
Each engine has its own folder and its own agents. Never mix them:
- ".NET engine" = the Inline folder (the one containing "Inline"). Analysis = its phase 1-4 agents; transformation = its phase 5-8 agents.
- ".NET Core engine" = the Core folder (the one containing "Core"). Analysis = its phase 1-4 agents; transformation = its phase 5-8 agents.

NAVIGATION (breadcrumbs on every screen, all clickable, plus a back button)
Home > Java (greyed, "Coming soon") | .NET
.NET > ".NET engine" | ".NET Core engine"
Engine > two method cards: "DSG (REST API)" (greyed, "Coming soon", not clickable, also not by keyboard) and "SQL" (active).
SQL page > two cards: "Code analysis" and "Code transformation", each listing ONLY that engine's agents for those phases as workers. Keep what already exists on this page: the Orchestrator guide button (it is a guide, never a group, never a phase node), the project values form, the progress bar, the Approval connector, the flows, the Report panel and the phase panels.

LEFT SIDEBAR (if kept): show only this tree: Engine > SQL > Code analysis / Code transformation > phases. No other groups.

RULES
- Hide Core's older phase 6 infra-build-test agent behind a "Show older agent" toggle labelled "may be superseded", so the Core transformation card normally shows 4 phases.
- The orchestrators, PROMPTS.md and Python helpers are not workers and do not appear as cards.
- Methods are data-driven (config: id, title, status, agentsFolder), so DSG can be added later without changing shared UI code. Keep progress, project values and history separate per engine and method.

CHECK before you finish and report it in your answer: for each engine, the number of agents in Code analysis and in Code transformation (expected 4 and 4), and list the file names in each. Report any agent that does not fit a phase 1-8 and where you put it. Do not claim you tested something you did not.
