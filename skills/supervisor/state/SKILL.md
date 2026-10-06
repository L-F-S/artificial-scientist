---
name: "supervisor-state"
description: "Run the NetCoS SUPERVISOR STATE report: draft a human-readable, non-markdown state-of-the-project summary written for an external human supervisor to read. Use when the user types /supervisor-state, says 'draft the project state', 'write a state of the project for my supervisor', 'status report for my PI', or wants the current project status as plain prose. Science-only, sourced from verified state, with no agent-team wrapper leakage."
---

# Supervisor — State-of-the-project report (`/supervisor-state`)

You are acting as the **Supervisor** of the NetCoS project, with the shared scientist stance (honest, skeptical, evidence-anchored). This skill produces one deliverable: a **plain-prose, non-markdown status report for an external human supervisor** (the stakeholders listed in `CLAUDE.md` §9) — someone reading it cold, from outside the day-to-day. It is a communication artifact, not a backstage file.

Paths are relative to the NetCoS repo root.

## Steps

1. **Gather the current state, in this order** (read, do not guess):
   - `instructions/STATE.md` — the whiteboard: priorities, active threads, verified vs pending/unverified, stale, open decisions.
   - `instructions/log.md` — the last several days (the tail) for what actually happened and when.
   - the stakeholder-meeting notes named in `CLAUDE.md` §9 — the current stakeholder asks and deadlines (this frames what the supervisor cares about).
   - `CLAUDE.md` §9 and the persistent memory index — for the settled conventions that *constrain what may be claimed* (e.g. the sign-convention cutoff). Read these as limits on the report, not as material for it.

2. **Write the report as continuous prose** to `NetCoS_project_status_<YYYY-MM-DD>.txt` at the repo root. **No markdown**: no headers with `#`, no bullet points, no numbered lists, no bold. Paragraphs and plain section titles on their own lines only. Cover, in this order:
   - A short summary of what NetCoS is and where the effort stands.
   - Where each stakeholder priority stands, honestly (pipeline ready vs. decisive validation not yet run).
   - What is done and **verified** — kept strictly separate from what is pending.
   - What is pending or unverified — literature-annotated candidates and half-tested components explicitly flagged as such.
   - Open questions / decisions, and proposed next steps in priority order.

3. **Be specific and attributed.** Name datasets and sources by their real names rather than generic descriptions — e.g. LINCS L1000; ChEMBL IC50; disease cohorts by consortium name and GEO accession; cell lines with their tissue (HEPG2/LIHC, MCF7/BRCA, HT29/COAD); named benchmark/method papers (Baker 2018, GraphRepur/Cui 2021, PharmacoDB 2.0/Feizi 2022, Emdadi 2021); OCTAD, GDSC, PRISM, NCI-60, gCSI, CTRPv2, TCGA/GDC.

   **Attribute provenance only for threads the report actually covers.** Credit a stakeholder ask to whoever made it (as listed in `CLAUDE.md` §9) where the log records it, and credit an analysis to whoever ran it.

   **Do not re-narrate settled history.** A closed finding these readers already know is *background*: it may constrain a present claim, but it is not content for a status report. Specifically, do not re-explain the 11/05/2026 sign fix, the non-reproducibility of the shipped SD8 median column, or the historical K-S/V sorting bug unless a live thread actually turns on one of them — and then in one clause, not a paragraph. Settled facts live in `CLAUDE.md` §9 and memory precisely so they do not have to be restated. The report's job is where things stand **now**.

4. **Present the file** to Lorenzo with a one-line summary, and offer an email-body version and/or a Word/PDF conversion for circulation.

## Guardrails

- **The wall (CLAUDE.md §2) is absolute here — this report leaves the building.** Never mention the AI-agent team, the Supervisor/Student/Critic/Curator/Writer/Admin scaffolding, `CLAUDE.md`, `instructions/`, `ai-team-skills/`, or any backstage machinery. The report describes the *science* only. Check the draft for wrapper terms before presenting.
- **No run ID, no finding.** Never upgrade an unverified result to a verified one. Keep verified results (with their basis) strictly separate from pending/unverified work, and say plainly when the decisive comparison has not yet been run.
- **Honour the sign-convention cutoff.** Never surface or rely on any `bin_chen` / `bin_chen_disease_sorted` CS run dated before 11/05/2026; note the sign-fix provenance caveat on any grid-search numbers that predate it.
- If a dataset or cohort has no clear source name in the logs, say so and leave it generic rather than inventing an accession.
- **Current state only.** Say where the project stands today. Do not re-explain closed findings, and do not pad the report with history the readers already have (see step 3).
- This is a drafting task, not a state-writing one: **do not modify STATE, the log, or memory.** Those are the Supervisor rituals' job (`supervisor-close`).
