# Pancreatic single cells imputation and clustering in healthy and Type 2 Diabetes Mellitus patients

## Description:

This project deals with missing values imputation in a real-world dataset and finding meaningful clusters
of cells. Data is given by pancreatic cells coming from healthy individuals and individuals with Type
2 Diabetes Mellitus. There are a total of 7 patients in the dataset.

The objective is to understand if the cellular composition of pancreatic cells differs between healthy and unhealthy pa-
tients, as well as if we can find reliable biomarkers of the cell types across patients. To do so, we want to
obtain clear clusters in our dataset that represent the underlying biology.

Two separate tasks are performed on the dataset:
1. imputing missing data in single-cell expression to correct for technical dropout. 
2. Performing clustering on your data to find biologically meaningful clusters

## Imputation:

### Method:
The imputation was performed using Markov Affinity-based Graph Imputation of Cells (MAGIC). 
Data was preprocessed normalizing by total read count and using a square root transformation to correct for right skewness of countuing data.
Hyperparamter optimization with a grid search has been carried out on the number of nearest neighbors from which to compute kernel bandwidth and the number of principal components used for defining those neighbors. 
Moreover, it was considered if differentiating by health status improved the imputation quality, which turned out not to be the case.

### Evaluation Criterium:
Evaluation will be based on correlation with paired bulk data. The expression you discover in your single cells should be reflected in the bulk sequencing, where technical dropout does not occur as much. For this purpose we will use Spearman’s ρ between the ”bulkified” data and bulk data.
By ”bulkified” data we mean here the patient-wide average of gene expression. We will com-
pute the Spearman’s correlation with the log-transformed expression of bulk data. We will
compute the average ρ for all patients in the test set.

### Results:
|  Correlation before imputation  | Correlation after imputation |
|---------------------------------|------------------------------|
| 0.935                           | **0.952**                    |

## Clustering:

### Method:
Several preprocessing steps were carried out before applying louvain clustering algorithm.

* Filtering both cells and genes based on number of read counts.
* Filtering highly variable genes.
* Normalizing cells by total read counts.
* Square root transformation.
* Batch correction based on sample and disease.

After this preprocessing pipeline the data was scaled again and then projected on a lower dimensional space with UMAP.


### Evaluation Criterium:
A clustering method must identify as best as possible the different ground-truth cell types present in the dataset. This would allow you later on to get meaningful biomarkers through differential gene expression analysis. Consequently 3 metrics will be taken into account fir evaluation.
* The silhouette score SSC in the Principal Component Analysis (PCA) transformed space (number of Principal Components PCs 50)
* The Adjusted Rand Index ARI
* The V-measure score V

### Results:

| V-measure | Adjusted Rand index | Silhouette Coefficient | Overall CLustering Score |
|-----------|---------------------|------------------------|--------------------------|
| 0.923     | 0.953               | 0.869                  | 0.915                    |

