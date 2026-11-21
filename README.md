# Customer RFM Segmentation & Lifecycle Strategy (SQL Quartiles vs. K-Means Clustering)

## 1. Business Objective
Segment **793** unique e-commerce and retail customers (**EUR 2,261,536.81** total revenue) into actionable lifecycle cohorts to tailor VIP retention, upsell, and win-back marketing budgets. This project benchmarks a traditional deterministic SQL `NTILE(4)` spend quartile split against an unsupervised **K-Means Clustering** model across **Recency, Frequency, and Monetary (RFM)** dimensions.

## 2. Repository Contents
* **`customer_segmentation_dual_path.ipynb`**: End-to-end Google Colab notebook computing customer-level RFM metrics, comparing Path 1 (SQL `NTILE(4)` spend buckets) against Path 2 (`StandardScaler` + `KMeans(n_clusters=4)`), evaluating cluster cohesion via Silhouette Score, and generating cluster profiles.
* **`Customer_RFM_Segments.csv`**: Complete customer-level output table mapping all **793** accounts to both their `Normal_Spend_Quartile` and behavioral `ML_Cluster` assignment.
* **`rfm_kmeans_clusters.png`**: Scatter visualization plotting Recency (days since last order) against Total Monetary Spend (EUR) across the 4 discovered customer clusters.

## 3. Dual-Track Comparison: SQL `NTILE(4)` Quartiles vs. K-Means Clustering

### Path 1: Normal SQL `NTILE(4)` Spend Quartiles (One-Dimensional Split)
Forces an equal 25% headcount split (`198–199` buyers per bucket) solely on historical spend, ignoring whether high-spend customers churned over a year ago.
| Quartile Bucket | Customers | Avg Recency (Days) | Avg Frequency | Avg Spend (EUR) | Total Revenue (EUR) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Q4_VIP** | 198 | 105.6 | 7.8 | EUR 6,316.60 | EUR 1,250,680.80 |
| **Q3_High** | 198 | 113.9 | 7.1 | EUR 2,858.90 | EUR 566,063.90 |
| **Q2_Mid** | 198 | 161.1 | 5.8 | EUR 1,632.60 | EUR 323,264.60 |
| **Q1_Low** | 199 | 216.2 | 4.1 | EUR 610.70 | EUR 121,527.50 |

### Path 2: Unsupervised K-Means Clustering (Multi-Dimensional RFM Personas — Silhouette Score: `0.354`)
Discovers natural behavioral boundaries across Recency, Frequency, and Monetary spend simultaneously:
| Cluster & Persona | Customers | Avg Recency (Days) | Avg Frequency | Avg Spend (EUR) | Total Revenue (EUR) | Recommended Marketing Action |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **Cluster 3: Ultra-VIP Whales** | **68** (8.6%) | 120.7 | 8.2 | **EUR 9,236.50** | **EUR 628,078.90** | White-glove account management, early access & exclusive loyalty perks |
| **Cluster 0: Active Core Loyalists** | **279** (35.2%) | **71.1** | **8.5** | EUR 3,226.80 | **EUR 900,289.30** | Subscription/bundle cross-sell to elevate high-frequency buyers into VIP tier |
| **Cluster 2: Developing Regulars** | **347** (43.8%) | 101.6 | 4.7 | EUR 1,712.40 | EUR 594,186.80 | Automated repeat-purchase nudges & category recommendations |
| **Cluster 1: Dormant / Churned Risk** | **99** (12.5%) | **556.3** | 3.6 | EUR 1,403.90 | EUR 138,981.80 | Targeted win-back reactivation vouchers (isolated from active promo spend) |

## 4. Key Insights & Executive Takeaways
* **Pareto Revenue Concentration:** K-Means isolates **68 Ultra-VIP Whales** (`8.6%` of customers) who spend **EUR 9,236.50** on average and generate **27.8% of total revenue**, whereas SQL quartiles dilute VIP targeting across 198 accounts.
* **Eliminating Wasted Win-Back Spend:** By separating the **99 Dormant Buyers** (`556.3` days since last order) into Cluster 1, marketing teams can deploy aggressive reactivation discounts exclusively to lapsed accounts without eroding margins on the **279 Active Core Loyalists** (`71.1` days recency).

## 5. Tech Stack & Dataset
* **Tools:** Python (`pandas`, `numpy`, `scikit-learn`, `matplotlib`), Google Colab
* **Dataset:** [Superstore Sales Dataset (`rohitsahoo/sales-forecasting` — Kaggle)](https://www.kaggle.com/datasets/rohitsahoo/sales-forecasting)
