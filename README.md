# 🛒 Online Retail Growth Analytics

> **End-to-end customer analytics pipeline** built on the UCI Online Retail dataset.  
> Designed to answer the core growth questions any fintech or e-commerce company faces:  
> *Who are my best customers? Why do they leave? Which products drive retention?*

---

## 📌 Project Overview

This project applies a full growth analytics stack — data cleaning, cohort analysis, RFM segmentation, CLV modeling, and product-market fit scoring — to a real-world transactional dataset of **541,909 orders** from a UK-based online retailer (2010–2011).

The methodology and mental models are directly transferable to fintech contexts: customer lifetime value, activation funnels, retention loops, and product-led growth are as relevant for a neobank as for an e-commerce platform.

---

## 📂 Repository Structure

\`\`\`
online-retail-growth-analytics/
│
├── 00_DATA_CLEANING_EDA.ipynb          # Data quality assessment & cleaning pipeline
├── 01_COHORT_RETENTION_ANALYSIS.ipynb  # Monthly cohort retention & churn curves
├── 02_RFM_CLV_ANALYSIS.ipynb           # RFM segmentation & customer lifetime value
├── 03_PRODUCT_MARKET_FIT.ipynb         # Hero products, repurchase rates & segment overlap
│
├── online_retail_clean.csv             # Cleaned dataset (output of notebook 00)
├── rfm_segments.csv                    # RFM segment assignments (output of notebook 02)
├── hero_products.csv                   # Hero product list (output of notebook 03)
│
└── data/
    └── online_retail.csv               # Raw UCI dataset (source)
\`\`\`

---

## 🔢 Dataset at a Glance

| Metric | Raw | Cleaned |
|---|---|---|
| Transactions | 541,909 | 386,038 |
| Unique customers | 4,372 | 4,337 |
| Unique products | 4,070 | 3,575 |
| Date range | Dec 2010 – Dec 2011 | Dec 2010 – Dec 2011 |
| Total revenue | — | **€8,601,939** |
| Rows removed | — | 155,871 (28.8%) |

**Cleaning decisions:** removed rows with missing CustomerID (24.9%), invalid/zero unit prices (0.5%), returns & cancellations (2.0%), and test/administrative SKUs (3.1%). All decisions are documented with business rationale in notebook 00.

---

## 📊 Notebooks & Key Findings

### `00` · Data Cleaning & EDA

Systematic data quality audit and reproducible cleaning pipeline.

- **24.9% of transactions** had no CustomerID — excluded as they can't inform customer-level analysis
- Revenue is **highly right-skewed**: median customer spends €653 vs mean of €1,983
- Dataset spans 13 months with a clear Q4 seasonality spike

---

### `01` · Cohort Retention Analysis

*Business question: What % of customers return in subsequent months?*

| Metric | Value |
|---|---|
| M1 Retention (avg) | **21.4%** |
| M3 Retention (avg) | **24.9%** |
| M6 Retention (avg) | **24.4%** |
| M0→M1 Churn | **78.6%** ← primary leak |

**Key findings:**
- **M3 re-engagement anomaly:** average retention at M3 (24.9%) is *higher* than M1 (21.4%), indicating non-monthly purchase cycles — customers skip months and return. Standard monthly retention models would undercount loyalty.
- **First cohort outperforms by +18pp:** the Dec-2010 cohort retains at 36.7% at M1 vs 19.0% for all subsequent cohorts. This advantage is persistent: +17pp at M3, +14pp at M6.
- **Implication:** identifying the acquisition profile of the 2010-12 cohort is the single highest-leverage retention lever.

---

### `02` · RFM Segmentation & CLV Analysis

*Business question: Who are my best customers and which are at risk?*

| Segment | Customers | % of Base | Avg CLV | Revenue Share |
|---|---|---|---|---|
| Champions | 915 | 21.1% | €6,031 | **64.2%** |
| Loyal | 551 | 12.7% | €2,031 | 13.0% |
| Potential | 527 | 12.2% | €1,438 | 8.8% |
| Lost | 1,908 | 44.0% | €451 | 10.0% |
| At-Risk | 192 | 4.4% | €1,373 | 3.1% |
| New | 244 | 5.6% | €338 | 1.0% |

**Key findings:**
- **Pareto confirmed:** Champions = 21% of customers, 64% of revenue. A standard 80/20 becomes 80/21.
- **Lost segment is the largest (44%)** but generates only 10% of revenue — high volume, low value. Reactivation ROI must be modeled carefully.
- **Cohort × RFM cross-validation:** the Dec-2010 cohort has 46.8% Champions vs 14.5% in all other cohorts (+32.3pp). This bridges notebooks 01 and 02: the retention advantage of early adopters is *fully explained* by their RFM profile.

---

### `03` · Product-Market Fit Analysis

*Business question: Which products drive retention and high-value customers?*

| Metric | Value |
|---|---|
| Products analyzed | 3,781 |
| Avg repurchase rate (eligible) | **66.8%** |
| Hero products identified | **15** |
| Hero product revenue | €899,374 (**10.5% of total**) |
| Hero repurchase rate (avg) | **68.1%** |

**Hero product criteria:** top 15 products scored by `Revenue × Repurchase Rate`, with ≥50 unique customers and ≥20 first-time buyers.

**Key findings:**
- **REGENCY CAKESTAND 3 TIER** is the #1 hero product: €142K revenue, 72% repurchase rate, 881 unique customers.
- **Champions penetration of hero products: 91.5%** vs 51.7% for Lost customers. Hero product adoption is the strongest predictor of segment membership.
- **2010-12 cohort has 79.5% hero product adoption** vs 64.4% for other cohorts (+15.1pp) — reinforcing the early-adopter quality thesis.
- **Implication for onboarding:** routing new users toward hero products in the first session is likely to replicate the Dec-2010 cohort's retention profile.

---

## 🔗 Cross-Notebook Narrative

All four notebooks build a single, coherent thesis:

\`\`\`
High-quality acquisition (Dec-2010 profile)
        ↓
Early hero product adoption (+15pp vs later cohorts)
        ↓
Higher RFM scores → Champions segment (×3.2 vs rest)
        ↓
Superior retention at M1, M3, M6 (+14 to +18pp)
        ↓
64% of total revenue from 21% of customers
\`\`\`

The growth lever is not "acquire more customers" — it is **replicate the 2010-12 acquisition profile and activate hero products in onboarding**.

---

## 🛠️ Tech Stack

| Tool | Usage |
|---|---|
| Python 3.x | Core language |
| pandas | Data manipulation & aggregation |
| NumPy | Numerical operations |
| matplotlib / seaborn | Visualizations |
| Jupyter Notebooks | Analysis environment |
| VS Code | Development environment |

---

## 🚀 How to Run

```bash
# 1. Clone the repository
git clone https://github.com/your-username/online-retail-growth-analytics.git
cd online-retail-growth-analytics

# 2. Install dependencies
pip install pandas numpy matplotlib seaborn jupyter

# 3. Add the raw dataset
# Download UCI Online Retail dataset and place it as:
# data/online_retail.csv

# 4. Run notebooks in order
jupyter notebook 00_DATA_CLEANING_EDA.ipynb
```

> ⚠️ Run notebooks **in order** (00 → 01 → 02 → 03). Each notebook depends on the output CSV of the previous one.

---

## 📈 Business Implications Summary

| Priority | Action | Based On |
|---|---|---|
| 1 | Fix M0→M1 drop (79% churn) with onboarding nurture campaign | Notebook 01 |
| 2 | Profile Dec-2010 customers to inform acquisition targeting | Notebooks 01 + 02 |
| 3 | Push hero products in first session / onboarding flow | Notebook 03 |
| 4 | Design bundles for Potential and New segments | Notebooks 02 + 03 |
| 5 | Investigate At-Risk under-indexing on hero products | Notebooks 02 + 03 |

---

## 📄 Data Source

UCI Machine Learning Repository — [Online Retail Dataset](https://archive.ics.uci.edu/ml/datasets/Online+Retail)  
Chen, D., Sain, S.L., and Guo, K. (2012). Data mining for the online retail industry. *Journal of Database Marketing and Customer Strategy Management*, 19(3), 197–208.

---

*Built by Manuel Cabido · Open to feedback and collaboration*
