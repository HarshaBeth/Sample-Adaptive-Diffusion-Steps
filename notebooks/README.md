# Notebooks Artifacts

This directory stores local experiment artifacts that are not committed to the repository.

The actual experiment notebooks are stored under `src/`, organized by task:

- `src/diffusion/`
- `src/gmm/`
- `src/regression/`

## Local artifact folders

The following folders are intentionally kept out of Git because they may contain large files, generated results, or evaluation artifacts:

- `checkpoints/`  
  Stores trained GMM models, scalers, and other local checkpoint files.

- `human_evals/`  
  Stores human evaluation outputs and related local files.

- `results csv files/`  
  Stores generated CSV files containing experiment results and correlation outputs.

Each folder contains a `.gitkeep` file so that the directory structure is preserved after cloning.

## Notes

CSV result files, model checkpoints, compressed datasets, and other large generated artifacts are ignored by `.gitignore`.

To reproduce results, rerun the notebooks in `src/` and regenerate the required local artifacts.
