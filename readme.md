READ-ONLY. Do not edit, create or delete any file.

Read every agent .md file in this workspace (look in .github/agents in every folder, plus the orchestrators and PROMPTS.md files). Also read Protegrity_Agents_Overview.doc if you can open it, and tell me if you can't.

Give me ONE table, one row per agent, with these columns:
- Folder or suite
- Agent file name
- Group: Analysis (only reads code), Transformation (changes code), Verification (build or tests), Reporting (documents only), Orchestrator
- What it does, in plain English, max 15 words
- Changes my files? Yes or No
- Main inputs

Rules: keep the table narrow. Base the Group on what the agent file actually does, not on its name. If you are not sure, write "Not sure" and say why in one line below the table. Don't guess.

After the table, give 5 lines: how many agents are in each group, and which ones I must be careful with because they change code.
