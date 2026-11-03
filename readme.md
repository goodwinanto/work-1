READ-ONLY, with one exception: you may create one new file, agent-hub-spec/agent-descriptions.md, containing your final answer. Do not edit, rename or delete anything else.

Read every agent .md file, orchestrator, PROMPTS.md, Python helper and overview document in this workspace (both suites, Inline and Core, plus 03 Integration Service Analysis Agent, 04 SSIS, 05 SSRS, 06_HistoryDataTokenization, SP_Analysis_Prompt and sql agent). Cite the file path for every statement. Write UNKNOWN where the files do not say. Do not guess.

PART A: IS THIS HOW IT WORKS?
For each statement below, answer CORRECT, PARTLY CORRECT or WRONG, then explain in one or two lines with evidence. If it is not fully correct, say what really happens.
1. The agents fall into two groups: CODE ANALYSIS (read-only, ends with an impact report) and CODE TRANSFORMATION (changes code and then validates and tests it, plus the evidence report). In both suites, phases 1 to 4 are analysis and phases 5 to 8 are transformation.
2. The user only supplies a repo folder and a PII catalogue sheet. Each agent reads its own instructions and finds what else it needs.
3. Each phase hands its result to the next as files on disk. Nothing has to be uploaded by hand between phases.
4. Each phase can run in a fresh chat session and still pick up earlier results from the work folder.
5. The impact report needs the user's approval before phase 5 (transformation) can start.
6. Analysis and transformation can run at different times and by different people, as long as the analysis output is kept and approved.
7. No agent creates git branches, commits code or opens pull requests.
8. The Inline suite is the older approach and the Core suite is the newer one.
9. The agents are real Copilot custom agents that can be run by name from the command line, without needing questions answered in a chat.

PART B: DETAILED DESCRIPTION OF EVERY AGENT
One entry per agent, grouped as Code analysis, then Code transformation, then Other (orchestrators, helpers, prompt files). Use this layout:
- Name, file path, suite, phase number.
- Purpose in 2 to 3 plain sentences, written for someone who has never seen it.
- Group, and why.
- Inputs: files it reads (exact names and folders), placeholders it needs, anything it asks the user.
- Outputs: files it writes (exact names, folders, formats) and which later agent uses them.
- Does it change my source files? Which ones and how.
- Where it stops to ask the user a question or for approval, with the exact wording.
- What can go wrong, and what the user must do about it.

PART C: THE WHOLE FLOW IN ONE PAGE
For each suite: the order of all agents, what goes in, what each phase hands to the next (file names), where the approval happens, and what comes out at the end. Then explain in plain words how Inline and Core differ and when to use each.

PART D: GAPS
List everything you were not sure about or could not read, and the questions I should ask the .NET colleague who built these agents.
