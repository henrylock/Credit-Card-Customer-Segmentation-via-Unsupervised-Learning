# Credit Card Customer Segmentation 

## Project Overview

This project aims to analyze the credit card usage patterns and financial behaviour of the credit cardholders to identify distinct **customer segments** using **unsupervised learning**, which translates into actionable business insights for targeted marketing and risk mitigation.
This project also compares **K-Means** and **Hierarchical Agglomerative Clustering**, while investigating whether **PCA and UMAP dimensionality reduction** can improve clustering quality in a high-dimensional financial dataset.


## Dataset


The dataset used in this project is the [Credit Card Customer Data](https://www.kaggle.com/datasets/fhabibimoghaddam/customer-credit-card-data) sourced from **Kaggle** . It contains **18 features** tracking the financial behavior of **8,950 cardholders**, such as balance, purchase frequency, and credit limits.

> **Note on Data Access**: The original dataset is not included in this repository. Please obtain the dataset from the original Kaggle source and place it in your project directory before running the notebook.

EDA: Most financial features (e.g., Purchases, Cash Advance) show extreme right-skewness, indicating a small number of high-value customers.

The correlation heatmap shows the relationships between key customer attributes.

![Correlation Heatmap](image/correlation_matrix.png)

Multicollinearity was found between some features (e.g. `PURCHASES` and `ONEOFF_PURCHASES` having high correlation), necessitating dimensionality reduction to remove redundancy.


## Key Results


| Model                      | Optimal k | Silhouette | Davies-Bouldin |
| -------------------------- | --------: | ---------: | -------------: |
| K-Means — Baseline         |         6 |     0.3046 |         1.1160 |
| Hierarchical — Baseline    |         2 |     0.5621 |         1.5873 |
| **Hierarchical + PCA 90%** |     **3** | **0.7833** |     **0.6155** |

The best-performing configuration was **Hierarchical Agglomerative Clustering + PCA (90%)**, achieving a Silhouette Score of **0.7833** and Davies-Bouldin Score of **0.6155**.

PCA reduced the impact of highly correlated and redundant variables, allowing the clustering algorithm to identify clearer behavioural structures in the customer base.

## Customer Segments

The final model identified three distinct customer profiles:


![Segmentation Visualization](image/segmentation_visualization.png)


![Cluster Characteristic](image/cluster_characteristic.png)

### 🟪 Cluster 0 — Regular Card Users

**8,548 customers (98.98%)**

The majority of customers exhibit relatively average balances and spending behaviour, with no particularly extreme financial characteristics.

### 🟩 Cluster 1 — At-Risk Customers

**65 customers (0.75%)**

This segment is characterised by relatively low current purchases but unusually high minimum payments, suggesting customers dealing with accumulated outstanding balances.

### 🟨 Cluster 2 — High-Value VIP Customers

**23 customers (0.27%)**

This small segment shows exceptionally high purchasing activity, credit limits, and payments. Mean purchases are approximately **$27,505**, with a mean credit limit of approximately **$16,000** and payments of approximately **$28,138**.

## Business Insights

The segmentation demonstrates how unsupervised learning can translate financial behaviour into actionable customer strategies:

| Segment              | Potential Business Strategy                                                                              |
| -------------------- | ---------------------------------------------------------------------------------------------------------|
| Regular Card Users   | Increase engagement through targeted promotions, purchase incentives, and relevant card benefits         |
| At-Risk Customers    | Encourage healthier credit usage through targeted financial education, and appropriate credit products   |
| High-Value VIPs      | Focus on retention through premium services, exclusive rewards, personalised offers, and loyalty benefits|

The VIP segment represents a potential revenue-focused target for premium customer strategies, while the at-risk segment may require risk mitigation rather than additional credit exposure.

## Methodology

### 1. Data Preparation

* Data Cleaning: Records with missing values (<5% of data) were dropped. Duplicate records were checked.
* Exploratory data analysis
* Correlation analysis
* Normalization: Robust scaling using `RobustScaler` to handle skewed distributions and outliers, ensuring all features contributed fairly to distance-based models


### 2. Baseline Clustering

Two clustering approaches were evaluated:

#### **K-Means**

- **Optimal k:** 6
- **Selection method:** Highest Silhouette Score
- **Observation:** Provided high granularity, identifying six distinct groups (low spenders, cash borrowers, high-value customers, etc.), but suffered from overlapping clusters and noise.

#### **Hierarchical Agglomerative Clustering**

- **Selected linkage:** Ward
- **Optimal k:** 2
- **Observation:** Produced a simpler split between high spenders and casual users.

Cluster quality was evaluated using:

* Silhouette Score
* Davies-Bouldin Index

| Model                      | Optimal k | Silhouette | Davies-Bouldin | 
| -------------------------- | --------: | ---------: | -------------: |
| K-Means — Baseline         |         6 |     0.3046 |         1.1160 |                
| **Hierarchical — Baseline**|         2 |  **0.5621**|         1.5873 |

Comparison: While achieving a higher Silhouette score than K-Means, Hierarchical Agglomerative Clustering model was limited to a simple split between "high spenders" and "casual users," failing to capture more nuanced financial behaviour

### 3. Dimensionality Reduction

Two dimensionality-reduction techniques were investigated:

#### **Principal Component Analysis (PCA)**

PCA was evaluated at 90%, 95%, and 99% explained-variance thresholds. Determined that 7 components capture 90% of the variance, effectively denoising the data.

#### **UMAP**

UMAP parameters were optimized via grid search (n=5, d=0.1) to capture non-linear local connectivity, resulting in dense, well-separated clumps.

### 4. Enhanced Clustering

The reduced feature representations were subsequently combined with K-Means and Hierarchical Clustering to determine whether reducing redundancy and noise could improve segmentation quality.

![Model Comparison](image/model_comparison.png)


## Limitations

The clustering results should be interpreted in the context of the available dataset. The optimal number of clusters depends on the business objective, and this project primarily uses the Silhouette Score for automated cluster selection. The dataset also contains highly correlated purchasing variables and very small high-value/at-risk segments, making some clusters sensitive to extreme observations.

## Conclusion

This project demonstrates an end-to-end application of **unsupervised machine learning for customer segmentation**, from exploratory analysis and feature engineering through dimensionality reduction, model comparison, cluster evaluation, and business interpretation.

The analysis shows that reducing redundant dimensions with **PCA 90%** substantially improved clustering quality, enabling the identification of mainstream, at-risk, and high-value customer profiles.

## Technical Stack

**Python · Pandas · NumPy · Scikit-learn · SciPy · Matplotlib · Seaborn · PCA · UMAP**

