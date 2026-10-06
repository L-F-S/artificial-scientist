# Example instance (fictional)

What the backstage files of an AS instance look like after a few sessions. The project, the dates, the runs and the numbers are **invented for illustration**. They are not results.

The fictional project ranks LINCS L1000 compounds by how strongly they reverse a hepatocellular-carcinoma signature (TCGA-LIHC tumour vs adjacent normal).

| File | Owner | Semantics |
|---|---|---|
| `instructions/STATE.md` | Supervisor | whiteboard, overwritten each session: what is true now |
| `instructions/log.md` | Supervisor | lab notebook, append-only: science only |
| `admin/STATE.md` | Admin | team-machinery whiteboard |
| `admin/log.md` | Admin | append-only notebook for infrastructure and meta work |

In a real instance all of these are gitignored and live next to the project's `CLAUDE.md`.
