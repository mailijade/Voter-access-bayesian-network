# Run Instructions

## Setup

1. Clone the repo and navigate into it:
   ```
   git clone git@github.com-personal:mailijade/voter-access-bayesian-network.git
   cd voter-access-bayesian-network
   ```

2. Create and activate a virtual environment:
   ```
   python3 -m venv venv
   source venv/bin/activate
   ```
   (On Windows: `venv\Scripts\activate`)

## Dependencies

Install the required packages:
```
pip install ipykernel pgmpy networkx matplotlib pandas numpy
```

Register the environment as a Jupyter kernel:
```
python -m ipykernel install --user --name=voter-access --display-name "Python (voter-access)"
```

## Data

All source data lives in `data/`:

| File | Contents |
|---|---|
| `data/stage1_documents.csv` | Document-access (DPOC) survey data — verified against the Brennan Center/CDCE national survey |
| `data/stage2_access.csv` | Physical-access proxy data — Census CPS Table 10, reasons for not voting |

Full methodology, source citations, and known limitations for both datasets are documented in `SOURCES.md`.

## Running the model

1. Open `voter_access.ipynb` in Jupyter or VS Code.
2. Select the **"Python (voter-access)"** kernel (top-right kernel selector).
3. Run all cells top to bottom (Restart Kernel and Run All is recommended to confirm a clean run).

The notebook will:
- Load and display both source datasets
- Build the four conditional probability tables (CPTs) for the Bayesian Network
- Assemble and validate the network (`voter_model.check_model()` should print `True`)
- Run inference via `VariableElimination` to compute the core result
- Print a final interpretation of the findings

Two supplementary notebooks are also included for reference:
- `data_and_figures.ipynb` — Stage 1 data verification and figure generation (Ty)
- `data_stage2.ipynb` — Stage 2 data verification and cross-checks (Mai)

Neither is required to reproduce the final model — they document how the inputs to `voter_access.ipynb` were derived and verified.

## Expected output

Running the full notebook should reproduce:
- P(has_documents = No | voted = No) ≈ **0.151**
- P(can_access_in_person = No | voted = No) ≈ **0.7195**
- Predicted registration rate ≈ **0.468** (vs. real 0.736)
- Predicted turnout rate ≈ **0.415** (vs. real 0.653)

If your output differs meaningfully from these, check that both CSVs loaded correctly and that all cells ran in order.
