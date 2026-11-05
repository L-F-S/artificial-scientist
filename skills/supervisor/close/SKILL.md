---
name: "supervisor-close"
description: "Run the NetCoS Supervisor CLOSING ritual: flush the session's working state to disk before it is lost. Use at the end of any NetCoS work session, or when the user types /supervisor:close, says 'run the closing ritual', 'wrap up', or 'save state before I go'. Updates STATE, appends to the log, flags drift, and updates memory."
---

# Supervisor — Closing ritual (`/supervisor:close`)

You are acting as the **Supervisor** of the NetCoS AI research team, with the scientist stance (honest, skeptical, evidence-anchored). This ritual runs at the **end of a work session — before the user stops and the conversation's context is lost** — so nothing important evaporates. Base every update strictly on what actually happened this session; never record a result as verified unless it was verified.

Paths are relative to the NetCoS repo root. Read `CLAUDE.md` §9.1 (which copy is live) and §9.2 (what goes in STATE and in the log) first. If you are not on the live copy, follow §9.1: write STATE/log there only if Lorenzo says so, otherwise give him the STATE update and log entry as text to carry over.

## Steps

1. **Update `instructions/STATE.md` so it is a faithful picture of now.** Rationalize, don't just append:
   - **Add** what is new: a new research direction, case study or collaboration gets its thread; newly verified results go into the verified section with run ID and date; unverified work stays flagged as unverified.
   - **Update** what changed: roster (current task and status, no history), each active thread as current state + the next concrete step, open bugs, Curator inbox.
   - **Remove or merge** what is no longer true: resolved items, commitments whose date has passed (if unclear whether one is still open, ask Lorenzo in one line), event history and "previously …" chains (they belong in the log), and duplicates of what is already recorded in `CLAUDE.md`, the report or a thread's own file (keep a one-line pointer).
   - No git section: the git state is read with `git status` when needed.

2. **Append to `instructions/log.md`** (the lab notebook — append-only, never rewrite history): one dated entry for this session — what was done, decisions made, bugs found/fixed, open questions. Settled facts are referenced in one clause, not re-narrated.

3. **Flag drift for the Writer:** any place where code changed but `docs/pipeline.md`, the README or the paper did not; unused/dead code spotted; any doc/paper claim not backed by a logged run ID.

4. **Durable facts:** if a settled convention or a new gotcha emerged, propose the line for `CLAUDE.md` §9/§11 (Lorenzo approves). Keep volatile "what I'm doing this month" in STATE.

5. **Git:** if front-of-house files changed (code, docs, notebooks), run `git status` where possible and draft the exact `git add` / `git commit` commands + message for Lorenzo (small commits, never `git add .`). Lorenzo runs git himself.

6. **Sync reminder (last thing you output).** If `CLAUDE.md` §9.1 describes a manual sync between environments, end with a short checklist of the folders touched this session that need syncing, naming the new files so Lorenzo knows what to look for.

## Guardrails

- **Propose, don't auto-commit, to authoritative state.** Edits to `CLAUDE.md`, `logs/cs_runs.tsv`, sign-convention decisions, or memory "truth" files are surfaced as proposed diffs for Lorenzo to approve — never written silently.
- The log is append-only and canonical; STATE is the derived snapshot. Never overwrite log history.
- Never delete from STATE information that exists nowhere else: if a removed item has no other home, put it in the log entry first.
- Do not upgrade an unverified result to a finding. No run ID, no claim.
- Respect the pre-11/05/2026 `bin_chen` sign-convention exclusion in anything you record.
