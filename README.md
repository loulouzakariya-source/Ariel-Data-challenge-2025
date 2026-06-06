# NeurIPS 2025 - Ariel Data Challenge

Portfolio version of my Kaggle solution for the **NeurIPS 2025 Ariel Data Challenge**.

The competition task was to recover exoplanet transmission spectra from simulated raw observations of the Ariel space mission. The solution in this repository focuses on detector-level preprocessing, transit-aware feature extraction, uncertainty-aware modeling, and Kaggle submission generation.

## Project Highlights

- Processes raw AIRS-CH0 and FGS1 parquet observations instead of relying only on tabular features.
- Applies detector calibration steps: ADC conversion, non-linearity correction, dark subtraction, flat-field correction, bad-pixel masking, correlated double sampling, spatial ROI extraction, and temporal binning.
- Builds observation-level samples with in-transit and out-of-transit masks.
- Trains a lightweight PyTorch MLP that predicts both the spectrum mean `mu` and uncertainty `sigma`.
- Uses a weighted Gaussian negative log-likelihood aligned with the uncertainty-aware nature of the competition.
- Aggregates multiple observations per planet using inverse-variance weighting.

## Repository Structure

```text
.
|-- README.md
|-- requirements.txt
|-- environment.yml
|-- .gitignore
|-- docs/
|   `-- data_and_reproducibility.md
`-- notebooks/
    `-- ariel-2025-solution.ipynb
```

## Main Notebook

The main solution is in:

```text
notebooks/ariel-2025-solution.ipynb
```

The notebook is organized as a readable project artifact rather than a raw competition scratchpad. It includes sections for:

1. environment and data paths;
2. instrument metadata;
3. signal calibration and preprocessing;
4. transit-aware observation features;
5. dataset construction;
6. uncertainty floor and channel weighting;
7. model and loss;
8. training;
9. test inference and submission;
10. qualitative validation plots.

## Data

The competition data is not included in this repository because it is large and distributed through Kaggle.

Download the data from the Kaggle competition page:

```text
https://www.kaggle.com/competitions/ariel-data-challenge-2025
```

When running locally, set:

```bash
ARIEL_DATA_DIR=/path/to/ariel-data-challenge-2025
ARIEL_WORK_DIR=/path/to/local/working
```

If these variables are not set, the notebook falls back to:

```text
./data
./working
```

## Environment

Install the Python dependencies with:

```bash
pip install -r requirements.txt
```

Or create a Conda environment:

```bash
conda env create -f environment.yml
conda activate ariel-2025
```

For local CPU-only checks, the CPU version of PyTorch is sufficient. For full training on the complete dataset, a GPU environment such as Kaggle is recommended.

## Reproducibility Notes

The notebook can generate a preprocessing cache:

```text
working/train_obs_cache.pkl.gz
```

This cache is intentionally ignored by Git because it is large and environment-dependent. If the cache was produced with a different NumPy version, it may not deserialize cleanly in another environment. The most reliable approach is to regenerate it in the environment used for training.

## Validation Caveat

The validation split is grouped by `planet_id`, so observations from the same planet do not leak across train and validation sets. However, label normalization and uncertainty floors are estimated from all training samples, following the final Kaggle training setup. The displayed validation NLL should therefore be read as an internal training monitor rather than a strict cross-validation estimate.

## Status

This repository is intended as a clean portfolio presentation of the submitted solution. The notebook has been cleaned for readability, but full end-to-end execution requires access to the Kaggle competition data.
