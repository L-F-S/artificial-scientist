# STATE — hcc-reversal (fictional example)

*Whiteboard. Overwritten at every `supervisor-close`. Holds what is true now. History lives in `log.md`. Last updated: 14/03/2026.*

## Priorities
1. Decide whether the reversal ranking beats a random-compound baseline on the known-drug set (decisive check, not yet run).
2. Methods draft for the signature-construction step.

## Roster
```
agent      | name     | discipline | current task                               | status  | needs
Supervisor | —        | —          | session orchestration                      | active  | —
Student    | Rosalind | Biotech    | rebuild LIHC signature with batch covariate | active  | —
Critic     | Fisher   | Stats      | review run R-0007 before it is trusted      | idle    | R-0007 output
Curator    | —        | —          | weekly sweep (last: 09/03)                  | idle    | —
Writer     | Orwell   | —          | methods: signature construction             | blocked | R-0007 verified
```

## Active threads
- **Signature v2** (R-0007): tumour vs adjacent normal, with sequencing plate as a covariate. Run completed, **not yet reviewed**.

## Verified results
- R-0004 (02/03/2026): the pipeline recovers the reference implementation's scores on its toy dataset, max abs diff 1e-12. Basis: diff against the reference.

## Pending / unverified
- R-0007 ranking. The Critic has not reviewed it, so no claim may be made.

## Stale / do not use
- R-0001 to R-0003: computed before the gene-ranking direction fix (05/03). Ignore entirely.

## Curator inbox
- [09/03] Paper proposing a cell-line-matched signature. Relevant to Signature v2. Pursue?

## Git pending
- `src/signature.py`, `docs/pipeline.md`: commit message drafted in log 14/03.

## Why we are NOT pursuing
- Single-cell signatures: no matched normal at useful depth. Revisit if a dataset appears.
