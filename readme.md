I need to fix token and credit capture in the Protegrity Agent Hub usage collector, which reads GitHub Copilot CLI telemetry.

Problems:
1. Token totals look cumulative. A second Phase 1 run showed 120,427 tokens vs 81,787 for the first (difference ~38.6K), so runs seem to include earlier usage.
2. Credits show "Credits not reported".
3. Phase 4 shows "Usage not captured".
4. The By application / By agent / By workflow tables show "No runs" even though runs are listed.

Data sources (Copilot CLI, local):
- ~/.copilot/session-state/<session-id>/events.jsonl
- ~/.copilot/session-store.db (SQLite), table assistant_usage_events

Please do the following:
1. First, inspect the current collector code and tell me where tokens are read and aggregated. Do not change anything yet. Find out whether runs share a session ID or whether totals are summed across sessions.
2. Key every run by session ID. Per-run tokens must come only from that session. If a session is reused, store a snapshot at run start and report end minus start.
3. Read final tokens from the session.shutdown event (data.modelMetrics.<model>.usage). Treat inputTokens as a total that already includes cache, so uncached = input - cacheRead - cacheWrite. Do not add cached tokens on top of input. Report input, output, cacheRead, cacheWrite and total separately.
4. If there is no session.shutdown event (e.g. Phase 4), fall back to summing the per-call usage events and mark the run as "partial", not "not captured".
5. Credits: query ~/.copilot/session-store.db with SELECT SUM(total_nano_aiu) FROM assistant_usage_events WHERE session_id = ?, divided by 1e9. Open the DB read-only. If the table or column is missing, fall back to tokens x model rate from a config file (1 credit = $0.01) and label it "estimated".
6. Fix the By application / By agent / By workflow aggregations so they group the runs and sum tokens and credits.
7. Add a "Download usage report" output that includes session ID, phase, agent, model, input, output, cacheRead, cacheWrite, total tokens, credits, and status (measured / partial / estimated).

Rules:
- Read only usage metadata. Do not read, copy, log, or display prompt or response content from events.jsonl.
- Don't modify or delete anything under ~/.copilot.
- Keep changes minimal and show a diff summary.
- After changes, run the collector against my existing sessions and print a table: session ID, phase, tokens, credits, status. Flag any session where the numbers look cumulative.
