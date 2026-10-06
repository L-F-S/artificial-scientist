# Lab notebook — hcc-reversal (fictional example)

*Append-only. One dated entry per session. Records what happened this session, not settled background.*

---

## 05/03/2026 — gene-ranking direction fix

Critic (Fisher), reviewing R-0003, asked for a known-answer test: a compound whose signature is the exact negative of the disease signature must score as the strongest reverser. It scored as the weakest. Root cause: the drug-side gene ranking was sorted descending where the method requires ascending. Fixed in `src/rank.py`. R-0001 to R-0003 marked stale in STATE. A known-answer test was added to the test suite.

## 09/03/2026 — Curator digest #3

Kept 2, excluded 7 (5 off-topic, 2 not retrievable as full text: listed for manual fetch). Kept items are in `sources/`, with `.bib` entries appended. One item went to the STATE inbox.

## 14/03/2026 — Signature v2 run

Student (Rosalind) rebuilt the LIHC signature with sequencing plate as a covariate. Run R-0007, config snapshot `configs/r0007/`. Per-analysis note: `other_outputs/signature_v2/NOTES.md`. Not yet reviewed by the Critic: Writer blocked on it. Git commands for the two changed front-of-house files drafted for the human.
