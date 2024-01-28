# **SMA**

# **SMA: Sparse Meta-Analysis**

## **Introduction**

Sparse Meta-Analysis (SMA) is a tool for high-dimensional analysis of genetic variants. Current high-throughput technology has generated data on many genetic variants, but how to select a relatively small subset of variants for building statistical models is not entirely clear. SMA is designed to select important variants (out of many genetic variants) by meta-analyzing multiple studies. It only requires summary statistics from each study, and does not need to directly access the raw data. The program has an interface in R.

## **Details**

Sparse Meta-analysis (SMA) is designed to conduct variable selection on genetic variants by combining information from multiple studies. Variable selection helps to identify important genetic variants and to build joint statistical models for risk prediction. The meta-analysis of multiple studies often yields better reproducibility and more accurate estimation. SMA accommodates popular models that are used in genetic data analysis, such as the least square model (for continuous traits) and the logistic regression model (for binary traits). Instead of directly accessing the raw data, SMA only requires summary statistics from each study and hence can significantly reduce the burden of collating raw data from multiple studies. The detail on how to use SMA is shown below.

### **Data**

Suppose that there are K studies that need to be meta-analyzed, and each study contains p genetic variants. Assume that within each study, traditional (i.e., un-regularized) GLM model, such as the least square regression or the logistic regression model, has been fitted on the considered p variants in a joint manner; it is recommended that the data are standardized (i.e., each column of the covariate matrix has mean 0 and variance 1) before fitting the GLM model. Then, the user will need to collect summary statistics from each of the K studies and organize the summary statistics into files described in the sequel. For each study, the first file should include the joint estimates of the regression coefficients for the considered p variants; the second file, if possible, should include the estimated covariance matrix of the p variants, or the diagonal parts of the covariance matrix if the full covariance matrix is not available. It is to be noted that the second file is not required but is highly helpful for achieving better estimation.

### **Example**

Suppose that there are three studies, and each study contains p genetic variants (for demonstration, we let p=50). We wish to apply SMA to the three studies to select important genetic variants out of the p variants. All the summary-statistics files, and the R script (SMA_main, SMA_function and X_main_block.so), are provided in the downloadable package. The three files, beta_study1.txt, beta_study2.txt, and beta_study3.txt, contain the joint estimates of the regression coefficients for the p variants. The files, Cov_study1.txt, Cov_study2.txt, and Cov_study3.txt, contain the estimated covariance matrices of the p variants. The files, Cov_diag_study1.txt, Cov_diag_study2.txt, Cov_diag_study3.txt, contain the diagonal parts of the estimated covariance matrices. Explanations are given below.

1\. If the full variance matrix is provided, the user should specify var_type=1; if only the diagonal parts of the variance matrix is provided, the user should specify var_type=3; if no variance components are provided, the user should specify var_type=2.

2\. If the user chooses to use the homogeneous structure, the het.flag should be set to equal to 0; if the user chooses to use the heterogeneous structure, the het.flag should be set to equal to 1. If the user has no strong prior knowledge on the potential model structure of the data, the heterogeneous structure is generally recommended.

3\. Sometimes, the K studies may belong to distinct subgroups. For example, study 1 and 2 may belong to 1 subgroup, while study 3 belongs to another subgroup. If the user wishes to use the subgroup structure for such a situation, the het.flag should be set to equal to 1, and the following code for (k in 1:K){ subgrp\[\[k\]\]=(k) } needs to be replaced by subgrp\[\[1\]\]=c(1,2) subgrp\[\[2\]\]=c(3), which puts study 1 and 2 in one subgroup, and study 3 in another subgroup.

4\. For data that contain a large number of genetic variants, we suggest that the user first conduct a marginal screening to reduce the SNP set to a moderate size, and then apply the SMA accordingly.

### **Results**

The resultant file, SMA_main.Rout is also provided in the download. This file shows the final model along with the estimated coefficients for each study:

Selected models:

\[\[1\]\] \[1\] 1 4 6 9 11 14 16 21 26 31

\[\[2\]\] \[1\] 1 4 6 9 11 14 16 21 26

\[\[3\]\] \[1\] 1 4 6 9 11 14 16 21

Estimated regression coefficients:

\[\[1\]\] \[1\] -1.727158 0.000000 0.000000 2.356388 0.000000 1.653851 0.000000 \[8\] 0.000000 -1.862300 0.000000 -1.983583 0.000000 0.000000 1.939899 \[15\] 0.000000 1.536504 0.000000 0.000000 0.000000 0.000000 -2.357414 \[22\] 0.000000 0.000000 0.000000 0.000000 1.656328 0.000000 0.000000 \[29\] 0.000000 0.000000 -1.670301 0.000000 0.000000 0.000000 0.000000 \[36\] 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 \[43\] 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 \[50\] 0.000000

\[\[2\]\] \[1\] -2.090573 0.000000 0.000000 2.110431 0.000000 2.499905 0.000000 \[8\] 0.000000 -2.116811 0.000000 -2.356289 0.000000 0.000000 1.829392 \[15\] 0.000000 3.066499 0.000000 0.000000 0.000000 0.000000 -2.108656 \[22\] 0.000000 0.000000 0.000000 0.000000 1.807596 0.000000 0.000000 \[29\] 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 \[36\] 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 \[43\] 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 \[50\] 0.000000

\[\[3\]\] \[1\] -1.933043 0.000000 0.000000 2.151083 0.000000 1.684562 0.000000 \[8\] 0.000000 -2.027514 0.000000 -2.104975 0.000000 0.000000 2.389827 \[15\] 0.000000 1.860224 0.000000 0.000000 0.000000 0.000000 -2.373566 \[22\] 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 \[29\] 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 \[36\] 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 \[43\] 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 \[50\] 0.000000

In the above results, the indices for SNPs selected in the first study are (1 4 6 9 11 14 16 21 26 31); the indices for SNPs selected in the second study are (1 4 6 9 11 14 16 21 26); and the indices for SNPs selected in the third study are (1 4 6 9 11 14 16 21). This pattern demonstrates that the model structures among the three studies are heterogeneous. For the estimated regression coefficients, if a covariate has its estimated coefficient equal to 0 in a given study, then it means that this covariate is not selected for that study. For example, in the first study, the 2nd and 3rd covariates have regression coefficients equal to 0, which indicates that these two SNPs are not active in the first study. If the raw data were standardized before fitting the traditional GLM model, then the estimated coefficients for the covariates shown above need to be restored to their original scale.

## **Download**

Zip file: [SMA](http://dlin.web.unc.edu/wp-content/uploads/sites/1568/2014/11/SMA.zip "SMA.zip")
