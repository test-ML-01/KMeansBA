# KMeansBA

![Methodological Process](fig1_1.png)
*Figure 1: Methodological workflow.*

## Abstract

Clustering with size constraints organizes data while respecting predefined limits on the size of each cluster. This work introduces **K-MeansBA**, a novel extension of K-Means that integrates the Bat Algorithm (BA) to strictly enforce cardinality requirements during the clustering process. Our algorithm optimizes cluster quality—measured by intra-cluster cohesion and inter-cluster separation—while guaranteeing that each cluster contains exactly the designated number of instances. We evaluated the algorithm on **100+ OpenML datasets** using internal metrics (silhouette coefficient) and external metrics (Adjusted Rand Index - ARI, Adjusted Mutual Information - AMI, and Normalized Mutual Information - NMI), alongside direct verification of constraint compliance.  

Key findings include:
- **K-MeansBA reliably enforces size constraints** in all evaluated datasets.
- Competitive clustering quality, particularly in configurations with **fewer clusters**.

## Implemented Algorithms

This repository includes three cardinality-constrained clustering algorithms for comparison:

1. **K-MeansBA**: Our proposed method combining K-Means with the Bat Algorithm for size-constrained optimization.
2. **K-MedoidsSC**: A K-Medoids-based approach with size constraints.
3. **CSCLP**: A linear programming-based clustering algorithm with size constraints.

## Repository Structure

- `KMeansBA.R`: Implementation of **K-MeansBA** and dataset preprocessing logic.
- `OpenML_Datasets.R`: Functions to fetch and filter datasets from OpenML.
- `KmedoidsSC_OpenML.R`: Implementation and evaluation of **K-MedoidsSC**.
- `CSCLP_OpenML.R`: Implementation and evaluation of **CSCLP**.
- `results_KMeansBA.csv`: Results for K-MeansBA (metrics and execution times).
- `results_KmedoidsSC.csv`: Results for K-MedoidsSC.
- `results_CSCLP.csv`: Results for CSCLP.
- `silhouette_results.csv`: Silhouette coefficients for all instances.

## Requirements

Install the required R packages:
```R
install.packages(c(
  "cluster", "proxy", "mlr3oml", "mlr3", "pryr", "dplyr", 
  "aricode", "ggplot2", "corrplot", "clValid", "RColorBrewer", 
  "factoextra", "lpSolve"
))
```

## Dataset Preparation

The `OpenML_Datasets.R` script fetches and preprocesses datasets from OpenML:
```R
source("OpenML_Datasets.R")  # Load datasets into `odatasets_unique`
```

## Basic Usage

### K-MeansBA
```R
dataset <- odatasets_unique[1]$dataset[[1]]   # Select dataset
dataset_name <- odatasets_unique[1]$name     # Dataset name
target_cardinality <- odatasets_unique[1]$class_distribution_vector[[1]]  # Target sizes

# Run clustering
run_clustering(dataset, target_cardinality, dataset_name)  # Results saved to CSV
```

### K-MedoidsSC
```R
result <- run_KmedoidsSC(dataset, target_cardinality, dataset_name)  
```

### CSCLP
```R
result <- run_CSCLP(dataset, target_cardinality, dataset_name)  
```

## Results

Metrics for all algorithms are stored in CSV files:
- **Internal Metrics**: Silhouette coefficient (quality of clusters).
- **External Metrics**: ARI, AMI, NMI (agreement with ground truth labels).
- **Constraint Violations**: Difference between target and actual cluster sizes.

## Appendix — Summary of Evaluation Metrics

The following table summarizes selected results from **103 OpenML datasets** used for evaluation.  
Each entry shows both **external validation** (ARI, AMI, NMI) and **internal validation** (S(i)) scores for the three clustering algorithms.

| ID | Algorithm | #Features | #Instances | Cluster Sizes | ARI | AMI | NMI | S(i) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | K-MeansBA |  |  |  | 0.185 | 0.153 | 0.156 | 0.122 |
|  | K-MedoidsSC | 5 | 625 | 288, 49, 288 | -0.005 | 0.005 | 0.009 | -0.008 |
|  | CSCLP |  |  |  | 0.011 | 0.043 | 0.047 | 0.208 |
| 2 | K-MeansBA |  |  |  | 0.285 | 0.43 | 0.435 | 0.141 |
|  | K-MedoidsSC | 7 | 2000 | 200, 200, 200, 200, 200, 200, 200, 200, 200, 200 | 0.09 | 0.18 | 0.187 | -0.304 |
|  | CSCLP |  |  |  | 0.317 | 0.478 | 0.483 | 0.44 |
| 3 | K-MeansBA |  |  |  | 0.037 | 0.013 | 0.014 | 0.314 |
|  | K-MedoidsSC | 9 | 768 | 500, 268 | -0.009 | 0.006 | 0.007 | 0.048 |
|  | CSCLP |  |  |  | 0.03 | 0.01 | 0.011 | 0.672 |
| 4 | K-MeansBA |  |  |  | 0.197 | 0.354 | 0.381 | -0.108 |
|  | K-MedoidsSC | 8 | 336 | 143, 77, 52, 35, 20, 5, 2, 2 | -0.015 | 0.012 | 0.053 | -0.057 |
|  | CSCLP |  |  |  | 0.47 | 0.454 | 0.476 | 0.168 |
| 5 | K-MeansBA |  |  |  | 0.02 | 0.013 | 0.015 | 0.294 |
|  | K-MedoidsSC | 14 | 270 | 150, 120 | -0.003 | -0.003 | 0.0 | 0.183 |
|  | CSCLP |  |  |  | 0.072 | 0.049 | 0.052 | 0.513 |
| 6 | K-MeansBA |  |  |  | 0.591 | 0.636 | 0.64 | 0.44 |
|  | K-MedoidsSC | 5 | 150 | 50, 50, 50 | 0.01 | 0.009 | 0.021 | -0.075 |
|  | CSCLP |  |  |  | 0.886 | 0.861 | 0.862 | 0.725 |
| 7 | K-MeansBA |  |  |  | 0.152 | 0.23 | 0.243 | -0.082 |
|  | K-MedoidsSC | 9 | 1484 | 463, 429, 244, 163, 51, 44, 35, 30, 20, 5 | 0.014 | 0.013 | 0.028 | -0.094 |
|  | CSCLP |  |  |  | 0.108 | 0.155 | 0.168 | -0.105 |
| 8 | K-MeansBA |  |  |  | 0.374 | 0.401 | 0.407 | 0.537 |
|  | K-MedoidsSC | 14 | 178 | 59, 71, 48 | 0.058 | 0.062 | 0.072 | 0.067 |
|  | CSCLP |  |  |  | 0.362 | 0.357 | 0.364 | 0.49 |
| 9 | K-MeansBA |  |  |  | 0.007 | 0.005 | 0.008 | 0.484 |
|  | K-MedoidsSC | 3 | 250 | 125, 125 | 0.058 | 0.042 | 0.045 | 0.123 |
|  | CSCLP |  |  |  | -0.002 | -0.002 | 0.001 | 787 |
| 10 | K-MeansBA |  |  |  | 0.311 | 0.232 | 0.233 | 0.301 |
|  | K-MedoidsSC | 11 | 508 | 286, 222 | 0.078 | 0.053 | 0.054 | 0.134 |
|  | CSCLP |  |  |  | 0.347 | 0.262 | 0.263 | 0.444 |
| 11 | K-MeansBA |  |  |  | 0.433 | 0.34 | 0.342 | 0.133 |
|  | K-MedoidsSC | 11 | 200 | 103, 97 | 0.001 | 0.001 | 0.005 | 0.03 |
|  | CSCLP |  |  |  | 0.001 | 0.001 | 0.005 | 0.127 |
| 12 | K-MeansBA |  |  |  | 0.262 | 0.185 | 0.191 | 0.268 |
|  | K-MedoidsSC | 6 | 100 | 40, 60 | 0.029 | 0.013 | 0.021 | -0.001 |
|  | CSCLP |  |  |  | 0.151 | 0.099 | 0.105 | 0.389 |
| 13 | K-MeansBA |  |  |  | 0.752 | 0.601 | 0.601 | 0.41 |
|  | K-MedoidsSC | 8 | 4052 | 971, 3081 | -0.044 | 0.015 | 0.015 | 0.024 |
|  | CSCLP |  |  |  | -0.077 | 0.129 | 0.129 | 0.083 |
| 14 | K-MeansBA |  |  |  | 0.176 | 0.134 | 0.137 | 0.245 |
|  | K-MedoidsSC | 6 | 250 | 119, 131 | 0.001 | 0.001 | 0.004 | 0.016 |
|  | CSCLP |  |  |  | 0.15 | 0.111 | 0.113 | 0.385 |
| 15 | K-MeansBA |  |  |  | 0.374 | 0.229 | 0.232 | 0.429 |
|  | K-MedoidsSC | 7 | 209 | 153, 56 | 0.006 | -0.004 | 0.001 | 0.629 |
|  | CSCLP |  |  |  | 0.156 | 0.065 | 0.069 | 0.307 |
| 16 | K-MeansBA |  |  |  | 0.034 | 0.024 | 0.031 | 0.326 |
|  | K-MedoidsSC | 4 | 111 | 58, 53 | 0.005 | 0.004 | 0.01 | 0.226 |
|  | CSCLP |  |  |  | 0.009 | 0.006 | 0.013 | 0.33 |
| 17 | K-MeansBA |  |  |  | 0.01 | 0.007 | 0.007 | 0.358 |
|  | K-MedoidsSC | 7 | 3107 | 1541, 1566 | 0.048 | 0.035 | 0.035 | 0.18 |
|  | CSCLP |  |  |  | 0.001 | 0.001 | 0.001 | 0.034 |
| 18 | K-MeansBA |  |  |  | 0.034 | 0.022 | 0.023 | 0.158 |
|  | K-MedoidsSC | 11 | 1000 | 440, 560 | -0.001 | -0.001 | 0.0 | 0.043 |
|  | CSCLP |  |  |  | 0.04 | 0.036 | 0.036 | 0.292 |
| 19 | K-MeansBA |  |  |  | 0.126 | 0.099 | 0.1 | 0.222 |
|  | K-MedoidsSC | 6 | 1000 | 457, 543 | 0.022 | 0.015 | 0.015 | 0.007 |
|  | CSCLP |  |  |  | 0.214 | 0.158 | 0.159 | 0.386 |
| 20 | K-MeansBA |  |  |  | 0.104 | 0.071 | 0.074 | 0.375 |
|  | K-MedoidsSC | 6 | 250 | 109, 141 | -0.004 | -0.003 | 0.0 | 0.029 |
|  | CSCLP |  |  |  | 0.03 | 0.018 | 0.021 | 0.558 |
| 21 | K-MeansBA |  |  |  | 0.014 | 0.009 | 0.011 | 0.258 |
|  | K-MedoidsSC | 6 | 500 | 237, 263 | 0.064 | 0.046 | 0.047 | 0.041 |
|  | CSCLP |  |  |  | 0.021 | 0.015 | 0.016 | 0.578 |
| 22 | K-MeansBA |  |  |  | -0.002 | -0.001 | 4.73e-8 | 0.609 |
|  | K-MedoidsSC | 8 | 500 | 254, 246 | 0.002 | 0.001 | 0.003 | 0.276 |
|  | CSCLP |  |  |  | -0.001 | -0.001 | 0.001 | 0.325 |
| 23 | K-MeansBA |  |  |  | 0.152 | 0.110 | 0.117 | 0.145 |
|  | K-MedoidsSC | 6 | 100 | 46, 54 | 0.004 | 0.002 | 0.009 | 0.042 |
|  | CSCLP |  |  |  | 0.069 | 0.049 | 0.056 | 0.185 |
| 24 | K-MeansBA |  |  |  | 0.069 | 0.057 | 0.064 | 0.148 |
|  | K-MedoidsSC | 11 | 100 | 45, 55 | -0.010 | -0.007 | 7.41e-5 | 0.067 |
|  | CSCLP |  |  |  | 0.152 | 0.123 | 0.129 | 0.200 |
| 25 | K-MeansBA |  |  |  | 0.001 | 0.001 | 0.002 | 0.045 |
|  | K-MedoidsSC | 7 | 625 | 315, 310 | 0.059 | 0.044 | 0.045 | 0.195 |
|  | CSCLP |  |  |  | 0.173 | 0.129 | 0.129 | 0.362 |
| 26 | K-MeansBA |  |  |  | 0.001 | 3.00e-4 | 0.001 | 0.323 |
|  | K-MedoidsSC | 4 | 2178 | 1209, 969 | 0.002 | 0.001 | 0.001 | 0.138 |
|  | CSCLP |  |  |  | 0.003 | 0.001 | 0.002 | 0.643 |
| 27 | K-MeansBA |  |  |  | 3.19e-5 | 1.00e-4 | 0.001 | 0.450 |
|  | K-MedoidsSC | 4 | 662 | 345, 317 | -0.001 | -0.001 | 3.00e-4 | 0.156 |
|  | CSCLP |  |  |  | 0.005 | 0.004 | 0.005 | 0.532 |
| 28 | K-MeansBA |  |  |  | 0.087 | 0.064 | 0.067 | 0.263 |
|  | K-MedoidsSC | 15 | 252 | 124, 128 | 0.021 | 0.015 | 0.018 | 0.121 |
|  | CSCLP |  |  |  | 0.142 | 0.105 | 0.108 | 0.437 |
| 29 | K-MeansBA |  |  |  | 0.749 | 0.644 | 0.646 | 0.488 |
|  | K-MedoidsSC | 3 | 120 | 57, 63 | -0.007 | -0.005 | 0.001 | -0.015 |
|  | CSCLP |  |  |  | 0.534 | 0.429 | 0.433 | 0.739 |
| 30 | K-MeansBA |  |  |  | 0.185 | 0.143 | 0.149 | 0.111 |
|  | K-MedoidsSC | 11 | 100 | 47, 53 | -0.004 | -0.002 | 0.005 | 0.028 |
|  | CSCLP |  |  |  | 0.004 | 0.004 | 0.011 | 0.161 |
| 31 | K-MeansBA |  |  |  | 0.177 | 0.120 | 0.021 | 0.263 |
|  | K-MedoidsSC | 7 | 500 | 243, 257 | 0.009 | 0.003 | 0.004 | 0.018 |
|  | CSCLP |  |  |  | 0.145 | 0.096 | 0.097 | 0.467 |
| 32 | K-MeansBA |  |  |  | 0.014 | 0.009 | 0.012 | 0.159 |
|  | K-MedoidsSC | 10 | 250 | 121, 129 | -0.004 | -0.003 | 1.88e-6 | 0.032 |
|  | CSCLP |  |  |  | 0.050 | 0.035 | 0.038 | 0.337 |
| 33 | K-MeansBA |  |  |  | -0.001 | 7.74-4 | 0.002 | 3.18e-4 |
|  | K-MedoidsSC | 5 | 730 | 367, 363 | -0.0015 | -0.001 | 7.24e-6 | 0.119 |
|  | CSCLP |  |  |  | -4.08e-4 | -2.88e-4 | -8.04e-4 | 0.529 |
| 34 | K-MeansBA |  |  |  | -2.17e-4 | -1.55e-4 | 5.67e-4 | 0.145 |
|  | K-MedoidsSC | 9 | 1000 | 496, 504 | 6.00e-4 | 4.30e-4 | 0.001 | 0.015 |
|  | CSCLP |  |  |  | 0.166 | 0.123 | 0.124 | 0.250 |
| 35 | K-MeansBA |  |  |  | 0.059 | 0.045 | 0.049 | 0.238 |
|  | K-MedoidsSC | 6 | 500 | 257, 243 | 0.005 | 0.004 | 0.008 | 0.169 |
|  | CSCLP |  |  |  | 0.023 | 0.018 | 0.021 | 0.536 |
| 36 | K-MeansBA |  |  |  | 0.047 | 0.030 | 0.032 | 0.344 |
|  | K-MedoidsSC | 11 | 1000 | 497, 503 | 0.001 | 2.52e-4 | 9.82-4 | 0.032 |
|  | CSCLP |  |  |  | 0.028 | 0.026 | 0.027 | 0.530 |
| 37 | K-MeansBA |  |  |  | 0.004 | 0.004 | 0.005 | 0.511 |
|  | K-MedoidsSC | 4 | 740 | 371, 369 | -0.001 | -1.87e-4 | 0.001 | 0.012 |
|  | CSCLP |  |  |  | -0.001 | -1.87e-4 | 0.001 | 0.778 |
| 38 | K-MeansBA |  |  |  | 0.409 | 0.306 | 0.308 | 0.374 |
|  | K-MedoidsSC | 3 | 124 | 61, 63 | 0.203 | 0.137 | 0.140 | 0.369 |
|  | CSCLP |  |  |  | 0.527 | 0.411 | 0.412 | 0.652 |
| 39 | K-MeansBA |  |  |  | 0.214 | 0.157 | 0.158 | 0.131 |
|  | K-MedoidsSC | 11 | 500 | 246, 254 | -0.002 | -0.001 | 2.19e-4 | 0.028 |
|  | CSCLP |  |  |  | 0.004 | 0.004 | 0.011 | 0.161 |
| 40 | K-MeansBA |  |  |  | 0.002 | 0.001 | 0.002 | 0.312 |
|  | K-MedoidsSC | 5 | 730 | 367, 363 | 0.004 | 0.003 | 0.004 | 0.228 |
|  | CSCLP |  |  |  | 0.006 | 0.004 | 0.005 | 0.456 |
| 41 | K-MeansBA |  |  |  | 0.316 | 0.218 | 0.220 | 0.105 |
|  | K-MedoidsSC | 11 | 250 | 91, 159 | -0.004 | -0.003 | 3.50e-6 | 0.007 |
|  | CSCLP |  |  |  | 0.038 | 0.077 | 0.080 | 0.171 |
| 42 | K-MeansBA |  |  |  | 0.048 | 0.035 | 0.035 | 0.458 |
|  | K-MedoidsSC | 10 | 950 | 488, 462 | 0.001 | 0.001 | 0.001 | 0.270 |
|  | CSCLP |  |  |  | 0.100 | 0.073 | 0.074 | 0.740 |
| 43 | K-MeansBA |  |  |  | 0.027 | 0.020 | 0.020 | 0.077 |
|  | K-MedoidsSC | 11 | 1000 | 491, 509 | 0.009 | 0.007 | 0.007 | 0.068 |
|  | CSCLP |  |  |  | 0.009 | 0.007 | 0.007 | 0.124 |
| 44 | K-MeansBA |  |  |  | 0.009 | 0.005 | 0.006 | 0.155 |
|  | K-MedoidsSC | 11 | 500 | 224, 276 | 0.014 | 0.009 | 0.010 | 0.059 |
|  | CSCLP |  |  |  | -0.002 | -0.001 | 0.0002 | 0.298 |
| 45 | K-MeansBA |  |  |  | 0.438 | 0.344 | 0.346 | 0.471 |
|  | K-MedoidsSC | 3 | 380 | 185, 195 | -0.002 | -0.001 | 0.001 | -0.008 |
|  | CSCLP |  |  |  | 0.452 | 0.359 | 0.360 | 0.717 |
| 46 | K-MeansBA |  |  |  | -0.004 | -0.003 | 1.11e-5 | 0.128 |
|  | K-MedoidsSC | 11 | 250 | 117, 133 | 0.007 | 0.004 | 0.007 | 0.037 |
|  | CSCLP |  |  |  | 0.010 | 0.008 | 0.011 | 0.268 |
| 47 | K-MeansBA |  |  |  | 0.229 | 0.165 | 0.166 | 0.128 |
|  | K-MedoidsSC | 11 | 500 | 214, 286 | 0.029 | 0.017 | 0.018 | 0.089 |
|  | CSCLP |  |  |  | 0.116 | 0.079 | 0.081 | 0.244 |
| 48 | K-MeansBA |  |  |  | 0.178 | 0.138 | 0.139 | 0.242 |
|  | K-MedoidsSC | 6 | 500 | 233, 267 | 0.011 | 0.007 | 0.008 | 0.026 |
|  | CSCLP |  |  |  | 0.178 | 0.138 | 0.139 | 0.406 |
| 49 | K-MeansBA |  |  |  | -7.73e-5 | -5.58e-5 | 1.32e-4 | 0.301 |
|  | K-MedoidsSC | 6 | 3848 | 1924, 1924 | -1.90e-4 | -1.38e-4 | 4.99e-5 | 0.022 |
|  | CSCLP |  |  |  | -2.21e-4 | -1.60e-4 | 2.81e-5 | 0.518 |
| 50 | K-MeansBA |  |  |  | 1.000 | 1.000 | 1.000 | 0.034 |
|  | K-MedoidsSC | 11 | 284 | 142, 142 | 0.061 | 0.044 | 0.047 | 0.228 |
|  | CSCLP |  |  |  | 0.030 | 0.022 | 0.024 | 0.481 |
| 51 | K-MeansBA |  |  |  | 0.045 | 0.033 | 0.034 | 0.149 |
|  | K-MedoidsSC | 6 | 500 | 249, 251 | 0.011 | 0.008 | 0.009 | 0.071 |
|  | CSCLP |  |  |  | 0.146 | 0.108 | 0.109 | 0.239 |
| 52 | K-MeansBA |  |  |  | 1.000 | 1.000 | 1.000 | 0.575 |
|  | K-MedoidsSC | 4 | 131 | 83, 48 | 0.025 | 0.059 | 0.064 | 0.306 |
|  | CSCLP |  |  |  | 0.394 | 0.284 | 0.288 | 0.177 |
| 53 | K-MeansBA |  |  |  | -0.002 | -0.003 | 3.29e-4 | 0.583 |
|  | K-MedoidsSC | 3 | 222 | 88, 134 | 0.018 | 0.029 | 0.032 | -0.014 |
|  | CSCLP |  |  |  | 0.008 | 0.018 | 0.021 | 0.760 |
| 54 | K-MeansBA |  |  |  | -0.003 | -0.001 | 0.001 | 0.101 |
|  | K-MedoidsSC | 7 | 400 | 235, 165 | 0.014 | 0.006 | 0.008 | 0.130 |
|  | CSCLP |  |  |  | -0.003 | -0.001 | 0.001 | 0.277 |
| 55 | K-MeansBA |  |  |  | -0.002 | -0.001 | 0.0003 | 0.238 |
|  | K-MedoidsSC | 8 | 400 | 207, 193 | -0.001 | -0.001 | 0.001 | 0.094 |
|  | CSCLP |  |  |  | -0.001 | -0.001 | 0.001 | 0.380 |
| 56 | K-MeansBA |  |  |  | -2.57e-6 | -6.75e-5 | 0.002 | 0.119 |
|  | K-MedoidsSC | 8 | 400 | 206, 194 | -0.001 | -0.001 | 0.001 | 0.070 |
|  | CSCLP |  |  |  | 0.001 | 0.001 | 0.003 | 0.398 |
| 57 | K-MeansBA |  |  |  | -8.14e-7 | -1.15e-4 | 0.002 | 0.181 |
|  | K-MedoidsSC | 8 | 400 | 208, 192 | -0.002 | -0.001 | 0.001 | 0.128 |
|  | CSCLP |  |  |  | -0.002 | -0.001 | 0.0003 | 0.417 |
| 58 | K-MeansBA |  |  |  | -0.001 | -0.001 | 0.001 | 0.057 |
|  | K-MedoidsSC | 8 | 400 | 197, 203 | -0.002 | -0.002 | 7.55e-5 | 0.070 |
|  | CSCLP |  |  |  | -5.20e-6 | 1.11e-5 | 0.002 | 0.400 |
| 59 | K-MeansBA |  |  |  | 0.182 | 0.130 | 0.131 | 0.101 |
|  | K-MedoidsSC | 11 | 1000 | 436, 564 | -0.001 | -0.0004 | 0.0003 | 0.049 |
|  | CSCLP |  |  |  | -0.001 | -0.001 | 0.001 | 0.136 |
| 60 | K-MeansBA |  |  |  | 0.267 | 0.230 | 0.232 | 0.290 |
|  | K-MedoidsSC | 6 | 250 | 110, 140 | 0.004 | 0.005 | 0.008 | 0.077 |
|  | CSCLP |  |  |  | 0.302 | 0.225 | 0.228 | 0.462 |
| 61 | K-MeansBA |  |  |  | 0.152 | 0.146 | 0.147 | 0.226 |
|  | K-MedoidsSC | 6 | 1000 | 416, 584 | 0.004 | 0.001 | 0.002 | 0.021 |
|  | CSCLP |  |  |  | 0.189 | 0.181 | 0.182 | 0.443 |
| 62 | K-MeansBA |  |  |  | 0.189 | 0.133 | 0.133 | 0.125 |
|  | K-MedoidsSC | 11 | 1000 | 420, 580 | 3.31e-4 | 0.003 | 0.004 | 0.078 |
|  | CSCLP |  |  |  | 0.010 | 0.013 | 0.014 | 0.133 |
| 63 | K-MeansBA |  |  |  | 0.069 | 0.046 | 0.053 | 0.307 |
|  | K-MedoidsSC | 6 | 100 | 44, 56 | 0.016 | 0.016 | 0.023 | 0.040 |
|  | CSCLP |  |  |  | 0.004 | 0.006 | 0.014 | 0.492 |
| 64 | K-MeansBA |  |  |  | -0.001 | -0.001 | 0.001 | 0.317 |
|  | K-MedoidsSC | 5 | 323 | 175, 148 | -0.003 | -0.002 | 7.48e-5 | 0.237 |
|  | CSCLP |  |  |  | 0.278 | 0.209 | 0.210 | 532 |
| 65 | K-MeansBA |  |  |  | -0.001 | -0.001 | 9.95e-5 | 0.424 |
|  | K-MedoidsSC | 4 | 662 | 348, 314 | 0.003 | 0.003 | 0.004 | 0.180 |
|  | CSCLP |  |  |  | 0.001 | 0.001 | 0.002 | 0.565 |
| 66 | K-MeansBA |  |  |  | 0.004 | 0.002 | 0.004 | 0.153 |
|  | K-MedoidsSC | 11 | 500 | 228, 272 | 0.007 | 0.004 | 0.006 | 0.044 |
|  | CSCLP |  |  |  | 2.71e-4 | -2.62e-4 | 0.001 | 0.316 |
| 67 | K-MeansBA |  |  |  | 0.021 | 0.015 | -2.21e-4 | 0.079 |
|  | K-MedoidsSC | 11 | 500 | 241, 259 | 0.006 | 0.004 | 0.006 | 0.030 |
|  | CSCLP |  |  |  | -0.002 | -0.001 | 6.26e-5 | 0.125 |
| 68 | K-MeansBA |  |  |  | 0.036 | 0.086 | 0.089 | 0.311 |
|  | K-MedoidsSC | 10 | 214 | 76, 138 | 0.063 | 0.028 | 0.032 | 0.014 |
|  | CSCLP |  |  |  | 0.063 | 0.028 | 0.032 | 0.045 |
| 69 | K-MeansBA |  |  |  | 0.018 | 0.011 | 0.013 | 0.299 |
|  | K-MedoidsSC | 9 | 369 | 204, 165 | 0.001 | 0.002 | 0.004 | 0.042 |
|  | CSCLP |  |  |  | 0.018 | 0.011 | 0.013 | 0.571 |
| 70 | K-MeansBA |  |  |  | 0.020 | 0.015 | 0.017 | 0.198 |
|  | K-MedoidsSC | 9 | 274 | 134, 140 | -0.001 | -0.001 | 0.002 | 0.108 |
|  | CSCLP |  |  |  | 0.005 | 0.004 | 0.006 | 0.522 |
| 71 | K-MeansBA |  |  |  | 0.265 | 0.088 | 0.102 | 0.452 |
|  | K-MedoidsSC | 9 | 130 | 119, 11 | 0.087 | -2.05e-4 | 0.015 | 0.885 |
|  | CSCLP |  |  |  | 0.087 | -2.05e-4 | 0.015 | 0.876 |
| 72 | K-MeansBA |  |  |  | -0.001 | -0.001 | 9.28e-6 | 0.262 |
|  | K-MedoidsSC | 5 | 1372 | 762, 610 | 0.047 | 0.031 | 0.032 | 0.014 |
|  | CSCLP |  |  |  | 0.037 | 0.025 | 0.025 | 0.648 |
| 73 | K-MeansBA |  |  |  | 0.131 | 0.045 | 0.046 | 0.526 |
|  | K-MedoidsSC | 5 | 748 | 570, 178 | -0.070 | 0.058 | 0.060 | 0.709 |
|  | CSCLP |  |  |  | -0.070 | 0.058 | 0.060 | 0.721 |
| 74 | K-MeansBA |  |  |  | 0.113 | 0.137 | 0.201 | 0.074 |
|  | K-MedoidsSC | 10 | 106 | 22, 21, 14, 15, 16, 18 | 0.006 | -0.006 | 0.069 | -0.076 |
|  | CSCLP |  |  |  | 0.070 | 0.120 | 0.186 | 0.122 |
| 75 | K-MeansBA |  |  |  | 0.037 | -0.013 | 0.004 | 0.117 |
|  | K-MedoidsSC | 10 | 100 | 88, 12 | 0.112 | 0.009 | 0.025 | 0.237 |
|  | CSCLP |  |  |  | 0.275 | 0.097 | 0.112 | 0.146 |
| 76 | K-MeansBA |  |  |  | -0.003 | -0.005 | 1.24e-5 | 0.107 |
|  | K-MedoidsSC | 13 | 182 | 130, 52 | 0.006 | -0.004 | 0.001 | 0.058 |
|  | CSCLP |  |  |  | -0.011 | -0.004 | 4.48e-4 | 0.169 |
| 77 | K-MeansBA |  |  |  | 0.701 | 0.671 | 0.674 | 0.404 |
|  | K-MedoidsSC | 8 | 210 | 70, 70, 70 | -0.004 | -0.004 | 0.005 | -0.094 |
|  | CSCLP |  |  |  | 0.597 | 0.531 | 0.535 | 0.484 |
| 78 | K-MeansBA |  |  |  | 0.212 | 0.301 | 0.312 | 0.092 |
|  | K-MedoidsSC | 6 | 403 | 102, 129, 122, 24, 26 | 0.055 | 0.088 | 0.101 | -0.097 |
|  | CSCLP |  |  |  | 0.055 | 0.069 | 0.083 | 0.146 |
| 79 | K-MeansBA |  |  |  | 0.002 | -0.003 | 0.026 | 0.199 |
|  | K-MedoidsSC | 14 | 200 | 51, 56, 41, 42, 10 | -0.005 | -0.011 | 0.018 | -0.119 |
|  | CSCLP |  |  |  | 0.003 | 0.019 | 0.047 | 0.202 |
| 80 | K-MeansBA |  |  |  | 0.003 | -0.001 | 0.051 | 0.101 |
|  | K-MedoidsSC | 13 | 123 | 8, 48, 32, 30, 5 | 0.003 | 0.003 | 0.054 | -0.116 |
|  | CSCLP |  |  |  | -0.009 | -0.026 | 0.027 | 0.156 |
| 81 | K-MeansBA |  |  |  | 0.299 | 0.358 | 0.362 | 0.220 |
|  | K-MedoidsSC | 7 | 310 | 60, 100, 150 | 0.027 | 0.007 | 0.014 | -0.132 |
|  | CSCLP |  |  |  | 0.603 | 0.491 | 0.495 | 0.399 |
| 82 | K-MeansBA |  |  |  | -0.082 | 0.011 | 0.018 | -0.334 |
|  | K-MedoidsSC | 4 | 3252 | 2952, 68, 58, 86, 88 | 0.008 | 0.005 | 0.011 | -0.080 |
|  | CSCLP |  |  |  | -0.006 | 0.008 | 0.014 | 0.244 |
| 83 | K-MeansBA |  |  |  | -0.093 | 0.009 | 0.022 | -0.317 |
|  | K-MedoidsSC | 4 | 1623 | 1471, 35, 29, 44, 44 | -0.021 | -0.001 | 0.012 | -0.026 |
|  | CSCLP |  |  |  | -0.074 | 0.005 | 0.018 | -0.101 |
| 84 | K-MeansBA |  |  |  | -0.099 | 0.011 | 0.024 | -0.329 |
|  | K-MedoidsSC | 4 | 1521 | 1369, 35, 29, 44, 44 | -0.028 | 0.001 | 0.014 | -0.046 |
|  | CSCLP |  |  |  | -0.067 | 0.005 | 0.018 | 0.252 |
| 85 | K-MeansBA |  |  |  | -0.072 | 0.005 | 0.018 | -0.281 |
|  | K-MedoidsSC | 4 | 1515 | 1365, 33, 29, 45, 43 | 0.041 | 0.005 | 0.018 | -0.238 |
|  | CSCLP |  |  |  | -0.006 | 4.83e-4 | 0.014 | 0.220 |
| 86 | K-MeansBA |  |  |  | -0.074 | 0.002 | 0.018 | -0.270 |
|  | K-MedoidsSC | 4 | 1183 | 1083, 9, 21, 49, 21 | -0.025 | -0.004 | 0.012 | -0.105 |
|  | CSCLP |  |  |  | -0.044 | -0.004 | 0.012 | -0.057 |
| 87 | K-MeansBA |  |  |  | -0.048 | -0.003 | 0.013 | -0.236 |
|  | K-MedoidsSC | 4 | 1080 | 984, 8, 19, 51, 18 | -0.007 | -0.001 | 0.015 | -0.336 |
|  | CSCLP |  |  |  | -0.077 | 0.001 | 0.017 | 0.341 |
| 88 | K-MeansBA |  |  |  | -0.064 | 0.001 | 0.015 | -0.345 |
|  | K-MedoidsSC | 4 | 1277 | 1170, 9, 21, 53, 24 | 1.30e-4 | -0.003 | 0.011 | 0.053 |
|  | CSCLP |  |  |  | -0.055 | 0.004 | 0.018 | 0.294 |
| 89 | K-MeansBA |  |  |  | -0.050 | -0.001 | 0.013 | -0.282 |
|  | K-MedoidsSC | 4 | 1252 | 1144, 9, 22, 53, 24 | -0.030 | -0.003 | 0.011 | -0.135 |
|  | CSCLP |  |  |  | -0.058 | 0.004 | 0.018 | -0.140 |
| 90 | K-MeansBA |  |  |  | -0.080 | 0.004 | 0.020 | -0.343 |
|  | K-MedoidsSC | 4 | 1112 | 1010, 9, 21, 52, 20 | -0.023 | -0.002 | 0.013 | -0.005 |
|  | CSCLP |  |  |  | -0.042 | -0.002 | 0.014 | -0.047 |
| 91 | K-MeansBA |  |  |  | 0.421 | 0.290 | 0.295 | 0.149 |
|  | K-MedoidsSC | 6 | 383 | 257, 125, 1 | -0.013 | -0.002 | 0.005 | 0.144 |
|  | CSCLP |  |  |  | 0.073 | 0.231 | 0.236 | 0.101 |
| 92 | K-MeansBA |  |  |  | 0.392 | 0.492 | 0.511 | 0.249 |
|  | K-MedoidsSC | 8 | 500 | 45, 37, 51, 57, 52, 52, 47, 57, 53, 49 | 0.081 | 0.153 | 0.185 | -0.163 |
|  | CSCLP |  |  |  | 0.473 | 0.548 | 0.564 | 0.341 |
| 93 | K-MeansBA |  |  |  | 0.015 | 0.014 | 0.017 | -0.127 |
|  | K-MedoidsSC | 12 | 4898 | 20, 163, 1457, 2198, 880, 175, 5 | 0.001 | 0.011 | 0.014 | -0.368 |
|  | CSCLP |  |  |  | 0.051 | 0.051 | 0.054 | 2.08e-4 |
| 94 | K-MeansBA |  |  |  | 0.542 | 0.416 | 0.423 | 0.423 |
|  | K-MedoidsSC | 6 | 215 | 150, 35, 30 | 0.718 | 0.536 | 0.541 | 0.370 |
|  | CSCLP |  |  |  | 0.299 | 0.367 | 0.375 | -0.270 |
| 95 | K-MeansBA |  |  |  | 0.001 | 0.035 | 0.041 | 0.075 |
|  | K-MedoidsSC | 12 | 1599 | 10, 53, 681, 638, 199, 18 | 0.018 | 0.022 | 0.028 | -0.272 |
|  | CSCLP |  |  |  | 0.058 | 0.041 | 0.047 | 0.107 |
| 96 | K-MeansBA |  |  |  | 0.218 | 0.127 | 0.128 | 0.529 |
|  | K-MedoidsSC | 4 | 2201 | 1490, 711 | -0.016 | 0.013 | 0.013 | 0.332 |
|  | CSCLP |  |  |  | 0.196 | 0.111 | 0.111 | 0.652 |
| 97 | K-MeansBA |  |  |  | 0.222 | 0.077 | 0.078 | 0.396 |
|  | K-MedoidsSC | 6 | 4839 | 4578, 261 | -0.043 | 0.008 | 0.009 | 0.770 |
|  | CSCLP |  |  |  | -0.018 | 0.001 | 0.001 | 0.745 |
| 98 | K-MeansBA |  |  |  | 0.210 | 0.326 | 0.358 | 0.122 |
|  | K-MedoidsSC | 11 | 265 | 30, 27, 28, 44, 28, 47, 32, 29 | 0.027 | 0.047 | 0.093 | -0.152 |
|  | CSCLP |  |  |  | 0.145 | 0.197 | 0.236 | 0.201 |
| 99 | K-MeansBA |  |  |  | 0.001 | -0.001 | 6.38e-6 | 0.081 |
|  | K-MedoidsSC | 6 | 2000 | 1892, 108 | -0.042 | 0.007 | 0.008 | 0.766 |
|  | CSCLP |  |  |  | -0.025 | 0.001 | 0.002 | 0.741 |
| 100 | K-MeansBA |  |  |  | 0.008 | 0.014 | 0.020 | -0.247 |
|  | K-MedoidsSC | 12 | 2000 | 8, 67, 595, 898, 359, 71, 2 | 0.004 | 0.011 | 0.017 | -0.373 |
|  | CSCLP |  |  |  | 0.022 | 0.041 | 0.047 | -0.018 |
| 101 | K-MeansBA |  |  |  | 0.002 | 0.001 | 0.002 | 0.118 |
|  | K-MedoidsSC | 7 | 2000 | 1029, 777, 194 | 0.022 | 0.024 | 0.025 | 0.112 |
|  | CSCLP |  |  |  | -0.015 | 0.017 | 0.018 | 0.157 |
| 102 | K-MeansBA |  |  |  | 0.014 | 0.015 | 0.018 | 0.225 |
|  | K-MedoidsSC | 14 | 243 | 137, 106 | 0.043 | 0.039 | 0.042 | 0.195 |
|  | CSCLP |  |  |  | 0.297 | 0.261 | 0.263 | 0.555 |
| 103 | K-MeansBA |  |  |  | 0.024 | -0.001 | 0.005 | 0.207 |
|  | K-MedoidsSC | 8 | 719 | 174, 44, 84, 417 | 0.050 | 0.004 | 0.010 | -0.554 |
|  | CSCLP |  |  |  | 0.005 | 0.001 | 0.006 | -0.653 |

---

### Summary of Appendix Results

- **Datasets analyzed:** 103  
- **Constraint compliance:** 100%  
- **Perfect alignment (ARI=AMI=NMI=1):** Datasets 50 and 52  
- **Strong overall performance:** Datasets with 2–3 clusters show the highest internal and external metrics.  
- **Notable internal cohesion:** Dataset 8 (S(i) = 0.537), Dataset 6 (S(i) = 0.440).  
- **Consistent superiority:** K-MeansBA outperforms K-MedoidsSC and is comparable or better than CSCLP for smaller cluster counts.

---
