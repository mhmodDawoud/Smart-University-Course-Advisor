# src/

All project code lives here, split by component:

| Folder | Component |
|---|---|
| `data/` | loading, cleaning, (synthetic) data generation |
| `search/` | **Searching** |
| `learning/` | **Learning** |
| `optimization/` | **Optimization** |
| `evaluation/` | baselines, metrics, experiments |
| `app/` | user interface / demo |

Agree on the inputs and outputs between components first (see `docs/DATA_SCHEMA.md`, section
"Interfaces between components") so people can work in parallel.
