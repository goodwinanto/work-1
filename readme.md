STEP 0: UNDERSTAND THE TWO SUITES
Context: the previous developer delivered two folders: ".NET Splitted - Inline" and ".NET Core Splitted". Each has about 8 phase agents, an orchestrator, a PROMPTS.md and helper Python scripts. I do not know why there are two. Work it out from the files and answer:
1. Does the split make sense? What is each suite for: which kind of .NET application, which data-access style (direct SQL calls, ADO.NET, EF Core), and what kind of change it makes (for example wrappers at SQL call sites versus EF model setters and view mappings)? Cite the files.
2. Phase-by-phase comparison table: Phase | Inline agent | Core agent | What differs. Show where the phases do not line up (for example Core has two phase 6 agents and a phase 7 infra-build-test).
3. How do I choose the right suite for a given repository? List the signs to look for in the code.
4. Can both suites be used on the same repository? Would they overlap or conflict?
5. Python helper scripts: for each one give the name, which suite and agent calls it, what it does, its inputs and outputs, whether it changes files, and the Python version and packages it needs. Are the scripts in the two suites copies or different?
6. What is shared by both suites (prompt format, work folder, placeholders, approval step) and what is specific to one?
Start your answer with a 6-line plain summary of Step 0.

Add to the OUTPUT section:
F. The Step 0 answers, as tables and short bullets.
