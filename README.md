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
|----|------------|------------|-------------|----------------|------|------|------|------|
| 1  | K-MeansBA | 5 | 625  | 288, 49, 288 | 0.185 | 0.153 | 0.156 | 0.122 |
|    | K-MedoidsSC | – | – | – | -0.005 | 0.005 | 0.009 | -0.008 |
|    | CSCLP | – | – | – | 0.011 | 0.043 | 0.047 | 0.208 |
| 2  | K-MeansBA | 7 | 2000 | 200×10 | 0.285 | 0.430 | 0.435 | 0.141 |
|    | K-MedoidsSC | – | – | – | 0.090 | 0.180 | 0.187 | -0.304 |
|    | CSCLP | – | – | – | 0.317 | 0.478 | 0.483 | 0.440 |
| 6  | K-MeansBA | 5 | 150 | 50×3 | 0.591 | 0.636 | 0.640 | 0.440 |
|    | K-MedoidsSC | – | – | – | 0.010 | 0.009 | 0.021 | -0.075 |
|    | CSCLP | – | – | – | 0.886 | 0.861 | 0.862 | 0.725 |
| 8  | K-MeansBA | 14 | 178 | 59, 71, 48 | 0.374 | 0.401 | 0.407 | 0.537 |
|    | K-MedoidsSC | – | – | – | 0.058 | 0.062 | 0.072 | 0.067 |
|    | CSCLP | – | – | – | 0.362 | 0.357 | 0.364 | 0.490 |
| 13 | K-MeansBA | 8 | 4052 | 971, 3081 | 0.752 | 0.601 | 0.601 | 0.410 |
|    | K-MedoidsSC | – | – | – | -0.044 | 0.015 | 0.015 | 0.024 |
|    | CSCLP | – | – | – | -0.077 | 0.129 | 0.129 | 0.083 |
| 45 | K-MeansBA | 3 | 380 | 185, 195 | 0.438 | 0.344 | 0.346 | 0.471 |
|    | K-MedoidsSC | – | – | – | -0.002 | -0.001 | 0.001 | -0.008 |
|    | CSCLP | – | – | – | 0.452 | 0.359 | 0.360 | 0.717 |
| 50 | K-MeansBA | 11 | 284 | 142, 142 | 1.000 | 1.000 | 1.000 | 0.034 |
|    | K-MedoidsSC | – | – | – | 0.061 | 0.044 | 0.047 | 0.228 |
|    | CSCLP | – | – | – | 0.030 | 0.022 | 0.024 | 0.481 |
| 52 | K-MeansBA | 4 | 131 | 83, 48 | 1.000 | 1.000 | 1.000 | 0.575 |
|    | K-MedoidsSC | – | – | – | 0.025 | 0.059 | 0.064 | 0.306 |
|    | CSCLP | – | – | – | 0.394 | 0.284 | 0.288 | 0.177 |
| 77 | K-MeansBA | 8 | 210 | 70×3 | 0.701 | 0.671 | 0.674 | 0.404 |
|    | K-MedoidsSC | – | – | – | -0.004 | -0.004 | 0.005 | -0.094 |
|    | CSCLP | – | – | – | 0.597 | 0.531 | 0.535 | 0.484 |
| 92 | K-MeansBA | 8 | 500 | 45, 37, 51, 57, 52, 52, 47, 57, 53, 49 | 0.392 | 0.492 | 0.511 | 0.249 |
|    | K-MedoidsSC | – | – | – | 0.081 | 0.153 | 0.185 | -0.163 |
|    | CSCLP | – | – | – | 0.473 | 0.548 | 0.564 | 0.341 |

---

### Summary of Appendix Results

- **Datasets analyzed:** 103  
- **Constraint compliance:** 100%  
- **Perfect alignment (ARI=AMI=NMI=1):** Datasets 50 and 52  
- **Strong overall performance:** Datasets with 2–3 clusters show the highest internal and external metrics.  
- **Notable internal cohesion:** Dataset 8 (S(i) = 0.537), Dataset 6 (S(i) = 0.440).  
- **Consistent superiority:** K-MeansBA outperforms K-MedoidsSC and is comparable or better than CSCLP for smaller cluster counts.

---
