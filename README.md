# APS Failure at Scania Trucks — Data Processing

A pandas data-cleaning project examining how to prepare operational vehicle data without discarding most of the failure cases.

**[Open the executed Jupyter notebook](notebooks/01_scania_aps_data_cleaning.ipynb)**

The notebook downloads the original UCI files, profiles their quality, explains each treatment decision, validates the results and exports traceable datasets. Pandas is the only data-analysis library; Python's standard library handles downloading and file management. No predictive model is fitted.

## The operational problem

Scania's air pressure system (APS) supports functions including braking and gear changes. In this dataset, `pos` identifies failure of a specified APS component and `neg` identifies failure of other components. **Negative does not mean a healthy truck.**

Missing measurements are the central preparation challenge. Removing every incomplete row would leave a tiny and substantially different sample. The project measures that effect before choosing a cleaning policy.

## Main findings

| Check | Training | Test |
|---|---:|---:|
| Source records | 60,000 | 16,000 |
| APS cases | 1,000 | 375 |
| Missing feature cells | 850,015 (8.33%) | 228,680 (8.41%) |
| Complete records | 591 | 165 |
| APS cases remaining after complete-case deletion | 76 | 30 |
| Exact duplicate labelled rows | 0 | 0 |

- APS training cases have **17.15%** of their measurements missing on average, compared with **8.18%** for other-component failures. This is an association within the supplied sample, not a causal finding.
- Complete-case deletion would remove **99.02% of training records** and **92.40% of APS training cases**. The APS share among the surviving records would increase from 1.67% to 12.86%.
- **Eight features** are more than 50% missing in training. These are flagged for review and retained in the conservative output.
- **`cd_000`** has a constant observed value. Its missingness is treated separately from that constant value.
- No exact cross-split feature matches or conflicting labels for identical feature groups were found. The absence of truck IDs and dates prevents stronger conclusions about real-world independence.

The source is an expert-selected dataset with anonymised features, rather than a representative fleet history. Sample label shares must not be interpreted as fleet failure rates. The source's 10 versus 500 misclassification costs are **challenge units**, not measured savings or currency.

## What the notebook does

1. Downloads the official source archive and verifies pinned SHA-256 checksums.
2. Locates the real CSV header beneath the source notice and validates the schema.
3. Audits original tokens before converting the documented `na` marker to missing values and measurements to nullable numeric columns.
4. Profiles missingness by feature, row, class and split, including incomplete histogram bins.
5. Checks exact duplicates, conflicting labels, train/test overlap, non-finite values, negative values and constant features.
6. Retains all 76,000 records, all 170 source features, original labels, observed numerical values and zeros in conservative cleaned exports.
7. Produces a **separate preparation example** using training-only medians and training-defined missing indicators. Labels and source identifiers are excluded from its feature columns.
8. Runs assertions and export read-back checks, then saves compact audit reports and row-level lineage.

**Validation:** all 29 recorded checks passed. The notebook also completed a second run from an empty data folder, downloading the source again and reproducing the quality reports. The rendered notebook was inspected for readable tables and complete outputs.

There is no blanket row deletion, zero-filling, outlier clipping, class rebalancing or unsupported renaming of anonymised measurements. The median-filled example is a starting point for later validation, not evidence that median imputation is optimal. Its filled histogram bins must not be interpreted as measured physical distributions.

## Run locally

Use Python 3.11 or later. This notebook was executed with pandas 2.3.3. Install JupyterLab if you do not already have a Jupyter environment:

```bash
git clone https://github.com/Genduth/APS-Failure-at-Scania-Trucks---Data-Processing.git
cd APS-Failure-at-Scania-Trucks---Data-Processing
python -m pip install -r requirements.txt
python -m pip install jupyterlab
python -m jupyterlab
```

Open `notebooks/01_scania_aps_data_cleaning.ipynb` and choose **Restart Kernel and Run All Cells**. The kernel's working directory should be the repository or its `notebooks` folder. The first run requires internet access to download the original archive; later runs reuse the verified local files. A checksum mismatch stops execution for source review.

`requirements.txt` contains the single analysis dependency, pandas. JupyterLab is the notebook interface, not an additional analysis library. The notebook imports no NumPy, scikit-learn, plotting or statistical libraries.

## Files and outputs

```text
notebooks/
  01_scania_aps_data_cleaning.ipynb   Executed analysis, decisions and validation
reports/
  dataset_summary.csv               Reconciled record and missing-value counts
  feature_quality_profile.csv       Feature-level quality checks for both splits
  class_missingness.csv             Missingness by source class
  complete_case_impact.csv           Class retention under complete-case deletion
  histogram_completeness.csv        Completeness of the seven histogram families
  baseline_feature_policy.csv       Training statistics and preparation decisions
  validation_checks.csv             Executed checks, including CSV read-back
  source_manifest.json              Source provenance and exact checksums
  ...                              Supporting audit tables
data/raw/                          Downloaded originals; excluded from Git
data/processed/                    Generated datasets; excluded from Git
requirements.txt
```

Running the notebook creates the following CSVs for each of `train` and `test`:

| File | Contents |
|---|---|
| `{split}_clean.csv` | Class plus all 170 original features; missing values remain `NA`. |
| `{split}_row_quality.csv` | Source file, physical line, traceability ID and quality flags. |
| `{split}_baseline_features.csv` | Separate training-median-filled values and missing indicators. |
| `{split}_labels.csv` | Class labels for the preparation example. |

Each uses `source_record_number` as a key **within its source split**. It is a row position, not a truck identifier or model input. The conservative cleaned files can be loaded with:

```python
import pandas as pd

train = pd.read_csv(
    "data/processed/train_clean.csv",
    index_col="source_record_number",
    na_values="NA",
)
```

Raw and processed datasets are not committed. The executed notebook and compact audit outputs remain available on GitHub; running the notebook regenerates the large files.

## Source and notices

**APS Failure at Scania Trucks [Dataset]. (2016). UCI Machine Learning Repository.**
[Dataset page](https://archive.ics.uci.edu/dataset/421/aps+failure+at+scania+trucks) · [DOI: 10.24432/C51S51](https://doi.org/10.24432/C51S51)

Dataset creator: Scania CV AB. Donors: Tony Lindgren and Jonas Biteus. The notebook downloads the source description and preserves the original files unchanged.

UCI currently displays a CC BY 4.0 notice; the original description and CSV headers contain Scania's copyright and GPLv3-or-later notice. Both are identified here rather than replacing the original notices or assigning a new dataset licence. Source data is obtained directly from UCI.

## Author

[George Warlow](https://github.com/Genduth)
