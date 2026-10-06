# Artificial Scientist

**An agentic research-team framework for computational biology, developed on [NetCoS](https://github.com/knowmics-lab/netcos).**

Artificial Scientist (AS) reproduces a **team of computational and data scientists**: a Supervisor, Students, a Critic, a Curator, a Writer, and an Admin that maintains the team itself. They are Claude agents working alongside a human scientist, and they share one non-negotiable stance: **every agent follows the scientific method.** They form hypotheses, look for disconfirming evidence, anchor every claim to data, quantify instead of asserting, and never present an unverified result as a finding. The agents are defined less by what they can do than by the scientific discipline they are held to. It is not a model or a library. It is an **operating manual plus a set of agent skills**, coordinated entirely through plain files. It runs on Claude (Cowork / Claude Code). Its first and current instance is [NetCoS](https://github.com/knowmics-lab/netcos), a network-enhanced connectivity-score pipeline for drug repurposing (Knowmics Lab, University of Catania).

> Status (October 2026): **Phase 0 complete, Phase 1 active.** All six agents have run on real research material. The scheduled cadence (weekly literature sweep, weekly review) is not yet built, on purpose: see [Phases](#phases).

---

## Why this exists

LLM agents are good at bounded tasks with a checkable answer, and bad at knowing when they are wrong. A single assistant that writes the code, interprets the result and drafts the paper is fluent, fast and dangerous. In a real research project it will eventually state an unverified result with confidence.

Research has an old answer to this: the scientific method, enforced socially through review, replication and an explicit record of what was done. AS rebuilds that social machinery for agents. Claims need evidence, results need an independent check before they are trusted, every number carries its provenance, and a lab notebook records what happened. AS treats reliability as a design problem. The useful question is not "how capable is the agent?" but "what structure makes errors surface *before* a human relies on them, and keeps the human in charge of what is true?"

## Design in five rules

1. **Coordination through shared files only.** Agents never hold references to each other. They read and write a lab notebook (`log.md`, append-only), a whiteboard (`STATE.md`, overwritten to show only what is true now), and a sources folder. Adding an agent means writing one definition, and nothing gets rewired.
2. **Notebook ≠ whiteboard ≠ memory.** What happened (log), what is true now (STATE) and what must never be forgotten (memory) have different write semantics and live in different files.
3. **Every producing task passes an adversarial Critic before the human sees it.** The loop runs at least one round and at most three. The Critic must *name the external ground-truth check* that would prove a result (held-out data, a reference implementation to diff against, a known-answer test). Plausibility is not enough, because Critic and worker share a base model and therefore share blind spots.
4. **The human is the gate on anything authoritative.** Changes to the manual, the run registry, sign conventions or published claims are proposed by agents and decided by the human. No logged run ID, no claim.
5. **A wall between backstage and front-of-house.** The research product (code, docs, paper) never mentions the agent team. The team is private research operations *around* the product, not part of it.

The full rationale, including what was deliberately *not* built, is in [`docs/design.md`](docs/design.md).

## The team

| Agent | Role | Skill |
|---|---|---|
| **Supervisor** | Faces the human on science work. Orients each session, assigns work, runs the Critic loop, detects docs and code drift, drafts git commands. | `supervisor-open`, `supervisor-close`, `supervisor-state` |
| **Student(s)** | The hands. Scoped analyses, ports and runs, each spawned fresh with a discipline persona (Biotech / CS-ML / Stats). | spawned as sub-agents |
| **Critic** | The adversary. Reviews any worker output and names the ground-truth check. | `critic-run` |
| **Curator** | Literature sweep. Every source is backed by a fetched artifact, and what was excluded is reported along with what was kept. Also a co-investigator mode for reading one paper *with* the human. | `curator-run`, `curator-teach` |
| **Writer** | The only agent that writes for the outside world. Drafts only from verified, run-ID-cited artifacts, following the direction of truth code → docs → paper. | `writer-run` |
| **Admin** | Maintains the team itself: skill quality, roster ⇔ skill correspondence, architecture drift, phase tracking. Never does the science. | `admin-run` |

Every conversation is owned by exactly one human-facing agent: the Supervisor for science, the Admin for machinery. Naming another agent picks *who does the work*, never *who talks to the human*.

```
request ──► owner (Supervisor | Admin) ──► orient (read log, STATE, memory)
                      │
                      ▼
               worker(s) produce ──► Critic review ──► worker revises   (≥1 round, cap 3)
                      │
                      ▼
          owner returns artifact + Critic verdict + residual caveats ──► human decides
```

## Phases

The architecture is only allowed to grow when the current machinery has earned its keep. The Admin owns the phase tracker, which names the concrete criterion that opens each next phase (kept private on NetCoS, since it references unpublished work).

- **Phase 0 (complete):** STATE dashboard and roster, session rituals as skills, Critic with the ground-truth mandate, Writer on methods only.
- **Phase 1 (active):** scheduled weekly Curator and Supervisor review, skills for repeatable procedures.
- **Phase 2 (parked):** orchestrated Student → Critic → Writer sequences with human gates, a live dashboard.
- **Phase 3 (build last):** a standing autonomous loop via the Agent SDK, with the gates baked in.

## Lessons from the NetCoS instance

These are recorded in the instance's Admin log and STATE, which are written by the agents and checked by the human. They are the reason the framework looks the way it does.

- **Skipping the Critic once was enough to prove it necessary.** In one session the Critic was not invoked, and four unverified claims reached the human, who caught all four himself. Three were false or overstated. The fourth turned out to be correct, but had been asserted from model recall before anyone checked it. Three of the four shared a single root cause: *asserting a negative from a partial search* ("X does not appear in the repo"). When the Critic later ran on real material, it caught a construction bias in a head-to-head benchmark and an unadjusted best-of-k selection p-value before either reached the paper.
- **Skills are invoke-only context.** A rule that lives only inside a skill does not govern any session that does not call that skill. Always-true rules belong in the always-loaded manual, while skills hold procedures.
- **Don't trust that the manual is loaded.** In a test run in Cowork (a single fresh session), content referenced from the project's `CLAUDE.md` was not in context, consistent with the file not being auto-loaded at all. The manual is read because the session-opening skills read it explicitly. In Claude Code, which does load `CLAUDE.md`, this may differ.
- **Stale snapshots are silent.** STATE was once rebuilt from a truncated copy of the meeting notes, and two stakeholder meetings never reached it. Prose reminders to "re-read" are the kind of instruction that gets skipped. Checks should be mechanical.
- **Build nothing early.** Every piece of machinery added before the previous one has run on real work becomes untested surface. The phase tracker exists mainly to say *no*.

## What is in this repository

```
AS.md                  The operating manual: public sections of the NetCoS CLAUDE.md   (generated)
skills/                Source of the eight agent skills, as used on NetCoS             (generated)
example/               A fictional instance: what STATE, log and the Admin files look like
docs/design.md         Design rationale and the "why we are NOT doing X" list
tools/                 The export script
```

**How it stays in sync.** The [NetCoS](https://github.com/knowmics-lab/netcos) project folder is the single source of truth. Its `CLAUDE.md` is split by construction: §1–8 and §10 describe the team and must stay publishable, while §9 and §11 hold instance-specific facts and house conventions and stay private. `tools/export_from_instance.py` copies the public sections and the skill sources here. Before writing anything, it checks every file against a private denylist (people, unpublished collaborations, private files) and validates skill frontmatter. On any hit it writes nothing and lists the offending lines. Files marked *generated* are never edited by hand.

The skills are published as they run on NetCoS, so they still mention its paths and conventions. Generalising them is a deliberate later step, once more agents have been exercised. Abstracting before then would mean abstracting guesses.

## Using it on your own project

1. Copy `AS.md` into your project as `CLAUDE.md` and replace the NetCoS-specific references. Add your own §9 (instance facts) and §11 (house conventions).
2. Create `instructions/STATE.md`, `instructions/log.md` and `admin/` following [`example/`](example/).
3. Install the skills from `skills/` in Claude (Settings → Capabilities), adapting paths.
4. Start every session with `supervisor-open` and end it with `supervisor-close`.

## Author

Lorenzo Federico Signorini, Knowmics Lab, University of Catania. The manual and skills were designed by the author and co-written with Claude, inside the system they describe.

License: MIT.

---

*This is a work in progress: the framework is being built and tested on a live research project, and it will change as the team proves (or disproves) what works. Feedback, questions and criticism are welcome. Please open an issue.*
