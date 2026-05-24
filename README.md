# Predicting Molecular Properties Using Machine Learning

## Introduction
A dataset containing physical properties of molecules has been provided. The dataset consists of 133,886 rows and 17 columns. The target variable is GAP—the energy gap (or band gap)—defined as the minimum energy required to transition an electron from a bound (stable) state to a free or excited state. For a molecule to undergo a chemical reaction, absorb or emit a photon (light), or conduct an electric current, an electron must bridge this gap.
The objective of this assignment is to train a model capable of performing a regression task—specifically, predicting the value of GAP.

## Dataset
The QM9 dataset was obtained from [Hugging Face](https://huggingface.co/buckets/Itneed/qm9-csv-bucket/resolve/qm9_dataset.csv?download=true). It consists of a single CSV file containing 130,831 rows and 17 columns, describing various properties of small organic molecules.

## Project Goals
1. Generate molecular descriptors from SMILES
2. Analyze descriptor distributions and missing values
3. Detect redundant and highly correlated features
4. Reduce dimensionality of the feature space
5. Prepare dataset for downstream ML models

# Content:
-----------
* Descriptors.ipynb - Generation and aggregation of molecular descriptors from RDKit and Mordred into a unified dataset
* Data curation.ipynb - Data cleaning, missing value handling and Pearson/Spearman feature selection
* Outliers.ipynb - Outlier detection, normalization and dimensionality reduction using PCA and t-SNE
* Clustering.ipynb - Unsupervised clustering analysis and visualization of molecular data structure
* Model_training.ipynb - Training and evaluation of machine learning regression models

<h1>Workflow</h1>

<p align="center">
SMILES <br>
↓ <br>
Descriptor Generation (RDKit + Mordred) <br>
↓ <br>
Dataset Merging <br>
↓ <br>
Missing Value & Duplicate Handling <br>
↓ <br>
EDA & Visualization <br>
↓ <br>
Outlier Detection <br>
↓ <br>
Normalization <br>
↓ <br>
PCA / t-SNE <br>
↓ <br>
Clustering <br>
↓ <br>
Regression Modeling
</p>

## Descriptors.ipynb
- Merged RDKit and Mordred molecular descriptors into a single dataset.
- Performed missing value and duplicate analysis.
- Encoded SMILES strings into numerical format.
- Removed unnecessary object and technical columns.
- Exported the processed dataset to New_data1.csv.

## Data_curation.ipynb
- Removed non-numeric columns and analyzed missing values.
- Visualized NaN distribution using heatmaps.
- Removed columns containing NaN values and deleted duplicate rows.
- Eliminated zero-variance features.
- Performed Pearson and Spearman correlation analysis for feature redundancy detection.

### Visualizations

### Missing Value Heatmap
<img width="545" height="592" alt="image" src="https://github.com/user-attachments/assets/c0618ae5-60ed-4b8a-95a5-3902a3410258" />

The heatmap illustrates the distribution of NaN values ​​within the DataFrame. Pink indicates existing values, while blue represents missing data. The matrix reveals that missing values ​​are present in only eight specific columns, which were subsequently removed. 

### Correlation Heatmap
<img width="1357" height="1195" alt="image" src="https://github.com/user-attachments/assets/e3b88e79-06eb-4d7b-9484-b6c9def149c6" />

The correlation matrix demonstrates the degree of interrelationship between features. The higher this correlation, the more detrimental it is to the model, as—from the model's perspective—both features convey essentially the same information. Consequently, one of the features is removed in cases of high correlation. In this study, a threshold value of 0.75 was adopted.

## Outliers.ipynb
- Performed target variable (gap) distribution analysis and outlier detection using the IQR method.
- Removed anomalous samples and unnecessary descriptors (homo, lumo).
- Applied MinMax normalization to molecular descriptors.
- Performed PCA dimensionality reduction and analyzed explained variance.

### Visualisation

### The histogram and violin plot
<img width="1528" height="525" alt="newplot" src="https://github.com/user-attachments/assets/db4b1ba9-1cc1-4bb0-9b20-1c0556d42f1a" />
The histogram and violin plot demonstrate the distribution of the target variable gap. The plots revealed uneven density distribution and several extreme values separated from the main concentration of samples, indicating the presence of outliers in the dataset.

### Outlier Scatter Plot
<img width="1528" height="525" alt="newplot (1)" src="https://github.com/user-attachments/assets/2cc72624-e399-4fdb-a8cd-4d005ce007a6" />

The scatter plot, similarly to the violin plot, also demonstrates outliers but does so in a more explicit and point-wise manner. These samples were identified as outliers using the IQR method and removed during preprocessing.

### PCA Scatter Plot
<img width="570" height="455" alt="image" src="https://github.com/user-attachments/assets/dc7e9033-4073-4a6d-a4ad-958b30c32df9" />

The PCA projection showed that the reduced feature space preserved the global structure of the dataset. Most molecules remained distributed within a common region, indicating successful dimensionality reduction without significant information loss.

### t-SNE Visualization
<img width="626" height="590" alt="image" src="https://github.com/user-attachments/assets/65001192-2561-4d3d-b1f2-76b34e0f5c6b" />

The t-SNE visualization shows that molecules form several dense local groups in descriptor space, indicating the presence of structurally similar samples within the dataset. At the same time, no completely isolated clusters are observed, suggesting that the dataset remains relatively continuous without sharp separation between molecule groups.

## Clustering.ipynb
Dimensionality reduction and clustering methods (t-SNE, Agglomerative Clustering, K-Means) were applied to analyze the structure of molecular descriptor space.
<img width="862" height="547" alt="image" src="https://github.com/user-attachments/assets/cd6965a9-7d36-4bb6-825b-20611b87c13f" />

The t-SNE visualization revealed several local groups of similar molecules; however, smooth transitions and partial overlap between groups remained present.
<img width="7837" height="3944" alt="image" src="https://github.com/user-attachments/assets/cf920b23-47cf-4d63-ab66-69ccb679a26f" />

The dendrogram revealed several major branches that merge only at high linkage distances, indicating the existence of significantly different molecular groups.

<img width="787" height="547" alt="image" src="https://github.com/user-attachments/assets/e7270d2e-f3e9-49e9-bc0e-361815f0656c" />

K-Means also separated the data into groups, although cluster boundaries remained partially blurred.
<img width="844" height="547" alt="image" src="https://github.com/user-attachments/assets/79397243-e862-4841-82da-23327e2d8110" />

Comparison of clusters with target gap classes using ARI demonstrated low agreement, meaning that molecules may be similar in descriptor space while still having different gap values.

## Model_training.ipynb

- The prepared preprocessing pipeline was used to train LightGBM and XGBoost regression models.
- The dataset was split into training and test sets, while model stability was additionally evaluated using 10-fold cross-validation.
- Model performance was assessed using MSE, RMSE, MAE, and R² metrics.
- Both models achieved strong predictive performance with R² ≈ 0.87, meaning that approximately 87% of the target variable variance was successfully explained.
- Cross-validation results remained close to test set performance, indicating good model generalization and low overfitting.
- LightGBM demonstrated slightly better predictive performance compared to XGBoost, becoming the best-performing model in this study.
- The final results confirmed that the preprocessing pipeline, feature cleaning, dimensionality reduction, and descriptor transformation successfully produced an - informative feature space suitable for accurate molecular property prediction.

## Tools & Libraries

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-F7931E?style=for-the-badge&logo=scikit-learn)
![RDKit](https://img.shields.io/badge/RDKit-Chemoinformatics-green?style=for-the-badge)
![Mordred](https://img.shields.io/badge/Mordred-Descriptors-blue?style=for-the-badge)
![LightGBM](https://img.shields.io/badge/LightGBM-ML-green?style=for-the-badge)
![XGBoost](https://img.shields.io/badge/XGBoost-ML-red?style=for-the-badge)
