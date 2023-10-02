# XGB regressor for predicting gene expression from chromatine landscape

The topic of this project is to use chromatin information to predict gene expression. The goal is to build a machine learning model that uses different available chromatin tracks to accurately predict expression of
genes in an unseen cell line.

## Feature extraction, modelling and results
To extract features we considered the base pair signal referred to *window_size* pairs on the left and on the right of the considered gene TSS. Information was then aggregated taking moving averages of size given by *resolution* and sliding with step of *stride*.
With these features we trained a gradient boosting regressor. Moreover, hyperparameters tuning (on number of estimators, learning rate and depth of the tree)was performed using a validation set based different chromosomes of the same cell line. Finally, the tuned model was trained again on the train and the validation sets and tested on the test set (different cell line). The chosen metric is spearman's correlation (higher is better) and here we report a table summarizing results.

|   window_size |   resolution |   stride |   score_test |   score_val |   time |   n_features |   n_estimators |   learning_rate |   max_depth |
|--------------:|-------------:|---------:|-------------:|------------:|-------:|-------------:|---------------:|----------------:|------------:|
|           100 |           10 |        1 |       0.6982 |      0.7945 |    772 |         1407 |            100 |      0.023961   |           8 |
|           500 |           20 |       10 |       0.7029 |      0.8024 |    324 |          707 |            300 |      0.00677113 |           8 |
|          1000 |           30 |       20 |       0.6979 |      0.795  |    285 |          707 |            166 |      0.0106233  |           6 |
|          2000 |           45 |       35 |       0.6879 |      0.7952 |    325 |          805 |            300 |      0.00431297 |           7 |
|          3000 |           50 |       45 |       **0.7071** |      0.7929 |    409 |          938 |            300 |      0.00597397 |          10 |




## Data
For two different cell lines: X1,X2 the following data regarding histone modifications and/or chro-
matin accessibility has been collected:
* H3K27me3 (ChIP-seq)
* H3K4me1 (ChIP-seq)
* H3K4me3 (ChIP-seq)
* H3K27ac (ChIP-seq)
* H3K36me3 (ChIP-seq)
* H3K9me3 (ChIP-seq)
* Chromatin Accessibility (DNase-seq)
* Gene Expression (obtained using the CAGE experiment)
* Gene Information (obtained using CAGE and RefSeq)
Regarding the partition in train validation and test set:
Let for each cell line chr denote the list of all non-sex chromosomes (i.e., autosomes). We define A, B and C as disjoint
subsets of chr such that A + B + C = chr. Specifically, we define these sets as:
• A = {chr2,chr3,chr4,chr5,chr6,chr7,chr8,chr9,chr10,chr11,chr12,chr13,chr15,chr16,
chr17,chr18,chr20,chr21,chr22}
• B = {chr14,chr19}
• C = {chr1}

