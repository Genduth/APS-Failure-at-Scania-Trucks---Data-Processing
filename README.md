# Scania Trucks — Data Cleaning with Pandas

A straightforward data-cleaning project using the APS Failure at Scania Trucks dataset.

**[View the Jupyter notebook](notebooks/01_scania_aps_data_cleaning.ipynb)**

The data is included: **[original CSVs](data/raw/)** · **[cleaned CSVs](data/processed/)**.

## What do “train” and “test” mean?

These names come from the original prediction challenge:

- **Training data:** 60,000 records normally used to build a prediction model.
- **Test data:** 16,000 separate records normally used to check how well that model works on data it has not learned from.

For this project, we are simply cleaning the two supplied files. You do not need to
build a prediction model to follow the notebook.

## Steps

1. Load the training and test CSVs.
2. Inspect the rows, columns, data types and summary statistics.
3. Check missing values and remove columns with more than 50% missing in training.
4. Check duplicates and tidy class labels.
5. Remove a constant measurement column.
6. Fill the remaining gaps with medians calculated from training.
7. Check the results and export the cleaned files.

The notebook uses **pandas only**. Each step has a short explanation and saved output.

## Results

The files contain 60,000 training records and 16,000 test records. Cleaning removes eight
columns with more than 50% missing values and one constant column. The final files each
contain 161 measurements and the class label, with no missing cells.

`pos` identifies an APS component failure; `neg` identifies a failure in another component.
The feature names are anonymised. The notebook keeps their original names.

## Run it

1. Clone or download this repository, including the `data` folder.
2. Install pandas with `python -m pip install -r requirements.txt`.
3. Open `notebooks/01_scania_aps_data_cleaning.ipynb` in Jupyter and run all cells.

The notebook uses paths relative to the `notebooks` folder. The original files are in
`data/raw/`; the cleaned copies are `data/processed/train_clean.csv` and
`data/processed/test_clean.csv`. Running the notebook recreates those cleaned copies.

**Source:** [APS Failure at Scania Trucks — UCI Machine Learning Repository](https://doi.org/10.24432/C51S51).
Original source notices and the accompanying dataset licence are retained in [data](data/README.md).

**Author:** [George Warlow](https://github.com/Genduth)
