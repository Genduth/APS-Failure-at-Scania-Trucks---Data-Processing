# Scania Trucks — Data Cleaning with Pandas

A straightforward data-cleaning project using the APS Failure at Scania Trucks dataset.

**[View the Jupyter notebook](notebooks/01_scania_aps_data_cleaning.ipynb)**

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

1. [Download the original dataset from UCI](https://archive.ics.uci.edu/static/public/421/aps+failure+at+scania+trucks.zip).
2. Extract the original CSVs into `data/raw/`, keeping their source notices intact.
3. Install pandas with `python -m pip install -r requirements.txt`.
4. Open `notebooks/01_scania_aps_data_cleaning.ipynb` in Jupyter and run all cells.

The notebook uses paths relative to the `notebooks` folder. It saves `train_clean.csv` and
`test_clean.csv` in `data/processed/`. Large data files are excluded from Git; running the
notebook creates the cleaned copies locally.

**Source:** [APS Failure at Scania Trucks — UCI Machine Learning Repository](https://doi.org/10.24432/C51S51).

**Author:** [George Warlow](https://github.com/Genduth)
