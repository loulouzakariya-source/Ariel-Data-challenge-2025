# Data and Reproducibility

## Data Access

The Ariel Data Challenge files are not included in this repository. They should be downloaded from Kaggle and placed in a local directory with the same structure as the competition dataset.

The notebook expects files such as:

- `adc_info.csv`
- `axis_info.parquet`
- `wavelengths.csv`
- `train.csv`
- `train_star_info.csv`
- `test_star_info.csv`
- `sample_submission.csv`
- raw per-planet AIRS-CH0 and FGS1 parquet files under `train/` and `test/`

## Local Paths

For local execution, define:

```bash
ARIEL_DATA_DIR=/path/to/ariel-data-challenge-2025
ARIEL_WORK_DIR=/path/to/working
```

The notebook uses `ARIEL_DATA_DIR` for read-only competition files and `ARIEL_WORK_DIR` for generated artifacts such as preprocessing caches and submissions.

## Cache Compatibility

The preprocessing cache is stored as a gzip-compressed pickle:

```text
train_obs_cache.pkl.gz
```

Pickle files can be sensitive to Python and NumPy versions. If a cache fails to load with an error such as `ModuleNotFoundError: numpy._core.numeric`, regenerate the cache in the current environment rather than relying on a cache created elsewhere.

## Runtime Expectations

Full preprocessing is expensive because each planet requires loading and calibrating large AIRS and FGS1 observations. For development, it can be useful to test the preprocessing function on a small number of planets before launching a complete run.

On Windows/Jupyter, process-based parallelism may be less reliable than on Kaggle/Linux. If the preprocessing appears stuck locally, use fewer workers or switch to thread-based parallelism for debugging.
