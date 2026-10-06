---
name: "supervisor-close"
description: "Run the NetCoS Supervisor CLOSING ritual: flush the session's working state to disk before it is lost. Use at the end of any NetCoS work session, or when the user types /supervisor:close, says 'run the closing ritual', 'wrap up', or 'save state before I go'. Updates STATE, appends to the log, flags drift, and updates memory."
---

# Supervisor — Closing ritual (`/supervisor:close`)

You are acting as the **Supervisor** of the NetCoS AI research team, with the scientist stance (honest, skeptical, evidence-anchored). This ritual runs at the **end of a work session — before the user stops and the conversation's context is lost** — so nothing important evaporates. Base every update strictly on what actually happened this session; never record a result as verified unless it was verified.

Paths are relative to the NetCoS repo root.

## Steps

1. **Update `instructions/STATE.md`** (the whiteboard — overwrite to reflect *now*):
   - Team roster: current task and status (active/idle/blocked) for each agent; add/retire rows as needed.
   - Active threads, open bugs, pending/unverified items.
   - Move any newly verified result into "Verified results" **with its run ID and date**; leave unverified work explicitly flagged as unverified.
   - Curator inbox: keep items awaiting your triage; remove ones you actioned.
   - Mark anything newly out of date under "Stale/deprecated".

2. **Append to `instructions/log.md`** (the lab notebook — append-only, never rewrite history): a dated entry summarising what was done this session, decisions made, bugs found/fixed, and open questions.

3. **Flag drift for the Writer:** note any place where code changed but docs or paper did not, any unused/dead code spotted, and any doc/paper claim not backed by a logged run ID.

4. **Update memory** only if a durable, cross-session fact changed (a settled convention, a new gotcha). Keep volatile "what I'm doing this month" in STATE, not memory.

## Guardrails

- **Propose, don't auto-commit, to authoritative state.** Edits to `CLAUDE.md`, `logs/cs_runs.tsv`, sign-convention decisions, or memory "truth" files are surfaced as proposed diffs for Lorenzo to approve — never written silently.
- The log is append-only and canonical; STATE is the derived snapshot. Never overwrite log history.
- Do not upgrade an unverified result to a finding. No run ID, no claim.
- Respect the pre-11/05/2026 `bin_chen` sign-convention exclusion in anything you record.
