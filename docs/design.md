# Design rationale

This page explains *why* the manual (`AS.md`) is shaped the way it is. The manual says what to do. This page records the reasoning, so that changes can be argued with instead of guessed at.

## 1. Loose coupling through files

An "agent" here is not a process. It is a persona, a skill and an invocation pattern, and it lives for one conversation or one scheduled run. Persistence therefore cannot live in the agents, so it lives in files:

| File | Question it answers | Write semantics |
|---|---|---|
| `instructions/log.md` | What happened, and when? | append-only, never rewritten |
| `instructions/STATE.md` | Where are we right now? | overwritten, holds only what is true now |
| memory | What must never be forgotten in any session? | durable facts, rarely changed |
| `admin/log.md`, `admin/STATE.md` | The same two questions, about the *team* | as above, kept separate from science |
| `CLAUDE.md` | How does work get made here? | stable, edited only by approved proposal |

STATE is derived from the log the way `HEAD` is derived from git history: it is a convenient view, not the record. Merging the two fails in practice, because a multi-thousand-line notebook cannot also serve as a one-screen whiteboard.

Because agents only read and write these files, they never reference each other. Several instances of the same agent can run concurrently, and adding or retiring one never requires rewiring.

## 2. The Critic, and why plausibility is not enough

Critic and worker run on the same base model, so their errors are *correlated*: a mistake that looks right to one tends to look right to the other. A Critic that asks "does this look right?" therefore filters out shallow errors and lets the shared ones through.

The mandate is instead to **name the external ground-truth check**: a held-out split, a reference implementation to diff against, a known-answer test, an orthogonal dataset. When the check cannot be run inside the loop, the finding goes to the human with that check named, rather than into another autonomous round.

The loop is capped at three rounds. It is a filter, not a proof, and running it longer only adds rounds of agreement between two correlated reviewers.

## 3. The human gate

Automation proposes and the human decides on anything authoritative: the manual itself, the run registry, sign conventions and other "truth" files, the choice of research direction, and any claim in a paper. Full automation is reserved for work that is read-only, reversible or deterministically checkable, such as digests, literature gathering, flagging inconsistencies, and computations that carry their own recompute-and-compare check.

Failure modes designed against:

1. *The Curator steering priors.* Digests are dated and sourced, show what was excluded, and leave the selection to the human.
2. *The Writer outpacing verification.* Every claim cites a run ID, and unverified material is marked as such. Methods may be written before results, conclusions may not.
3. *Housekeeping drift.* The log is canonical and append-only. Housekeeping only appends and maintains the derived STATE.
4. *Echo chamber between Critic and worker.* The loop is capped, framed differently, anchored to external data, and spot-checked by the human.
5. *Laundering uncertainty into confidence.* Every reported number carries its run ID, a date and a valid/stale flag.

## 4. Always-loaded rules vs invoke-only procedures

A skill's content enters the context only when that skill is invoked. A rule that must hold in every session, such as a dispatch rule, the wall or a naming convention, therefore cannot live only in a skill. The manual holds the rules and skills hold the procedures. Chat dispatch (who owns a conversation) is prose in the manual for the same reason: it has to run *before* any skill is chosen.

Corollary, observed on the NetCoS instance (one test in Cowork): the manual itself is not guaranteed to be loaded. A project's `CLAUDE.md` is read when the session-opening skill reads it, not automatically. The opening rituals carry that load.

## 5. Two owners: Supervisor and Admin

The Supervisor is a scientist guiding experiments. The Admin is an engineer maintaining the apparatus. Merging them tends to produce a supervisor that tunes its own tooling instead of doing the research, or an admin that drifts into interpreting results. The boundary rule is short: if a change affects what the project *does or shows scientifically*, it belongs to the Supervisor. If it affects *how the team is built and run*, it belongs to the Admin.

## 6. Growth by phases

Each phase opens only on a concrete, observed criterion (for example, "the Critic has run on real output"), never on a plan. The central failure mode of agentic setups is machinery that was never exercised: schedules nobody reads, dashboards that duplicate a text file, orchestration layers for hand-offs that have happened twice.

## Why we are NOT doing X

Each of these is kept deliberately, with the reason.

- **No orchestration skill yet.** The manual's dispatch protocol has not been run enough times to know which hand-offs are worth automating.
- **No live dashboard.** At current scale it would duplicate `STATE.md`. It stays parked until it shows something STATE cannot.
- **No standing autonomous loop.** It would automate a loop nobody has yet observed working by hand.
- **No conventions moved into skills.** A convention that applies only when a skill is called is not a convention.
- **No generalised skills yet.** The skills are published as they run on one real project. Abstracting them before more agents have been exercised would mean abstracting guesses.
