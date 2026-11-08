# Dataset files

Source: Scania CV AB, **APS Failure at Scania Trucks**, distributed through the
[UCI Machine Learning Repository](https://doi.org/10.24432/C51S51).

`raw/` contains the original training CSV, test CSV and dataset description.
Their original Scania copyright and GPLv3-or-later notices are preserved.
A copy of the licence is included in [LICENSE.txt](LICENSE.txt).

`processed/` contains cleaned copies produced by the notebook. Changes made on
8 November 2026: removed eight mostly missing columns and one constant column,
standardised the class labels, filled remaining missing measurements with training
medians and exported without a pandas row index. These changes are described in
the notebook. The source dataset notices and licence accompany these copies.
