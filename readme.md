READ-ONLY, except you may create two new files: agent-hub-spec/ui-groups.json and agent-hub-spec/ui-groups.md. Do not edit or delete anything else.

Use agent-hub-spec/agent-descriptions.md as your main source. Re-open an agent file only to check or fill a gap. Cite file paths. Write "UNKNOWN" where the files do not say. Never guess.

GOAL
I am building a UI with one CARD per group of agents. Clicking a card opens a node-style flow (like n8n): one node per agent, arrows between them, and for each node a smart summary, its inputs and outputs, and where its logs are.

GROUPS
- Always create "Code analysis agents" and "Code transformation agents", covering both suites (Inline and Core).
- Also create a separate group for any different kind of activity that does not fit those two, for example testing and validation, reporting and evidence, orchestration, or the SQL, SSIS, SSRS and tokenization folders. Name each group by what it does. Only create a group if agents really belong in it.
- Every agent goes in exactly one group.

ui-groups.json STRUCTURE
{
 "groups": [{
   "id", "title",
   "cardSummary": "max 25 words, plain English",
   "changesSourceCode": true/false,
   "youProvide": ["..."], "youGet": ["..."],
   "suites": ["Inline","Core"],
   "agents": ["agent ids in run order"],
   "flow": { "edges": [{"from","to","via":"exact file names handed over"}], "approvalGates": [{"between":["id","id"],"wording":"exact text asked","recordedIn":"file or UNKNOWN"}] }
 }],
 "agents": [{
   "id", "name", "file", "suite", "phase", "group",
   "smartSummary": "max 30 words: what is happening in this step",
   "steps": ["3 to 5 short steps of what it does"],
   "inputs": [{"name","kind":"file|folder|placeholder|user-answer","pathOrPattern","producedBy"}],
   "outputs": [{"name","pathOrPattern","format","usedBy"}],
   "logs": [{"name","pathOrPattern","what it shows"}],
   "changesSourceFiles": true/false, "whichFiles": "...",
   "asksUser": [{"when","wording"}],
   "risks": ["..."]
 }]
}
Checks: valid JSON, every agent appears once, every edge refers to an existing agent id.

ui-groups.md: maximum 40 lines for me to read. A table with Group | Card summary | Agent count | Changes code? Then the groups with their agents in order. Then the UNKNOWN items.

Finally show in the chat: the groups table and the list of any agents you were unsure how to group.
