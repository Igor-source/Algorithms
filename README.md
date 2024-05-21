# Algorithms
This is my repository about working with data 
https://drive.google.com/drive/folders/14vhxhZHn7Jnjqs-ylEo9E7tZoJLzAGNv?usp=drive_link
At the beginning, a dataset was given consisting of SMILES molecules for which all descriptors were unloaded from RdKit and morder. During the work, it was necessary to reduce the data dimension from 2000 features to 100 or less. Data processing was carried out in the following stages:

Data Curation
1.Data cleaning
2.Data inspection
3. Missing data handling
4. Outlier detection

Feature engineering
Feature Selection
1. Drop features with high correlation (Unsupervised)
2. Pearson's (Supervised)
3. Spearman (Supervised)
PCA – used to reduce the dimensionality of data.

The result was data, which was then divided into training and test samples. The models were trained using two libraries LGBM and XGB.
