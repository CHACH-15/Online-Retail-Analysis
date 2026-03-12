
# Online Retail — Customer Behaviour & Segmentation Analysis

##  Live Report
 [View the full report](https://chach-15.github.io/Online-Retail-Analysis)

A full end-to-end data analysis project on a UK-based e-commerce retailer's transaction data. Covers data cleaning, exploratory analysis, return behaviour, geographic distribution, recency modelling, and unsupervised customer segmentation using KMeans clustering.

---

##  Repository Structure

```
online-retail-analysis/
├── OnlineRetail_Enhanced.ipynb    # Full analysis notebook (37 cells)
├── OnlineRetail_Report.html       # Visual report with all charts (rename to index.html for GitHub Pages)
├── data/
│   └── Online Retail.csv          # Source dataset (UCI Machine Learning Repository)
└── README.md
```

---

## 📌 Project Overview

| | |
|---|---|
| **Dataset** | UCI Online Retail — Kaggle |
| **Period** | December 2010 – December 2011 |
| **Transactions** | 541,909 rows |
| **Unique customers** | 4,372 |
| **Unique products** | 3,684 SKUs |
| **Countries** | 37 |
| **ML technique** | KMeans clustering (k=4) |

---

## Notebook — Analysis Pipeline

### 1. Data Loading
Loads the raw CSV with `InvoiceNo` forced as string to preserve leading zeros.

### 2. Data Quality Audit
```
Shape       : 541,909 rows × 8 columns
Missing     : CustomerID ~25%, Description <0.1%
Duplicates  : ~5,268 rows
Returns     : ~16% of transactions have negative quantity
Zero prices : removed before analysis
```

### 3. Data Preprocessing
| Step | Action |
|---|---|
| Parse dates | `pd.to_datetime` on `InvoiceDate` |
| Remove zero-price rows | Free/test items excluded |
| Drop duplicates | Avoid double-counting |
| Add `Revenue` column | `Quantity × UnitPrice` |

### 4. Monthly Revenue Trend
Area + line chart with annotated monthly £ values. Peak: **November 2011** (holiday pre-orders).

### 5. Returns Analysis
- Top 20 most returned products by units
- Top 20 best-sellers with **zero returns** (reliable SKUs)
- Anomaly investigation: **Customer 16446** returned ~80,000 units of a single product — identified and excluded

### 6. Top Customers
Ranked by revenue and by quantity. Top customer by revenue and by volume are **different customers** — bulk buyers ≠ high-spend buyers.

### 7. Geographic Distribution
- UK = ~85% of revenue
- Germany, France, Netherlands are top international markets
- Donut chart of revenue share + bar chart of unique customers per country

### 8. Recency Analysis
Recency computed as days since last purchase relative to dataset end date.

| Segment | Days | Share |
|---|---|---|
| Active | < 30 days | ~28% |
| Recent | 30–90 days | ~22% |
| Dormant | 90–180 days | ~15% |
| Churned | 180+ days | ~35% |

### 9. Customer Behaviour Clustering — KMeans (k=4)

**Features engineered per customer:**
- `PurchaseFrequency` — unique invoice count
- `AvgQuantity` — mean units per transaction
- `AvgSpendPerPurchase` — total revenue ÷ invoices
- `ReturnRate` — negative rows ÷ total rows

**Optimal k** selected using Elbow curve + Silhouette score analysis.

**PCA projection** (2D) for cluster visualisation.

| Cluster | Label | Recommended Action |
|---|---|---|
| 0 |  High-Value Loyalists | VIP programme, early access |
| 1 |  Frequent Returners | Quality review, product survey |
| 2 |  Regular Buyers | Upsell bundles, subscriptions |
| 3 |  Low-Engagement | Win-back email + discount |

### 10. Export for Power BI
Two CSV files written to Desktop:
- `online_retail_clustered.csv` — full transaction-level data with cluster labels
- `customer_segments.csv` — one row per customer with all behavioural features

---

##  Key Findings

- **UK dominates** — 85%+ of revenue; Germany and France are distant second and third
- **November spike** — holiday pre-orders cause a sharp revenue peak every year
- **Data anomaly** — Customer 16446 inflated return figures by 80k units; cleaned before modelling
- **35% of customers churned** — last purchased 180+ days ago; largest single segment
- **High-Value cluster** — fewer than 20% of customers drive 70%+ of total revenue
- **Return rate** — cluster 1 shows significantly higher return rates, indicating a product quality or expectation mismatch

---

##  How to Run

```bash
# Install dependencies
pip install pandas numpy matplotlib seaborn scikit-learn jupyter

# Launch notebook
jupyter notebook OnlineRetail_Enhanced.ipynb
```

> ⚠️ Update the CSV path in the first code cell to your local directory.

---

## 🛠 Tech Stack

| Tool | Purpose |
|---|---|
| Python 3 | Core language |
| Pandas | Data manipulation |
| Matplotlib / Seaborn | Visualisations |
| Scikit-learn | KMeans, PCA, StandardScaler, Silhouette |
| Jupyter Notebook | Interactive analysis |

---

##  Future Improvements

- Add full **RFM scoring** (Recency × Frequency × Monetary) per customer
- Build a **churn probability model** (Logistic Regression or Random Forest)
- **A/B test** retention campaigns per cluster and measure lift
- Flag return anomalies automatically with a z-score threshold
- Expand geographic analysis with a choropleth map

---

##  Report

A full visual report with all 10 charts is available in [`OnlineRetail_Report.html`](./OnlineRetail_Report.html).
Rename it to `index.html` and enable GitHub Pages to host it as a live site.

---

## 📦 Dataset Source

[UCI Machine Learning Repository — Online Retail Dataset](https://archive.ics.uci.edu/ml/datasets/Online+Retail)
