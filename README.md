# 🚚 Supply Chain Delivery Performance Analysis

> **End-to-end data analysis and machine learning project** identifying root causes of chronic late deliveries, quantifying financial impact, and building a predictive alert system for a global e-commerce operation.

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Key Findings](#-key-findings)
- [Dataset](#-dataset)
- [Project Structure](#-project-structure)
- [Methodology](#-methodology)
- [Results](#-results)
  - [KPI Baseline](#kpi-baseline)
  - [Bottleneck Detection](#bottleneck-detection)
  - [Root Cause Analysis](#root-cause-analysis)
  - [Time-Based Patterns](#time-based-patterns)
  - [Machine Learning Model](#machine-learning-model)
- [Strategic Recommendations](#-strategic-recommendations)
- [Target Outcomes](#-target-outcomes)
- [Tech Stack](#-tech-stack)
- [How to Run](#-how-to-run)
- [Report](#-report)
- [License](#-license)

---

## 📌 Project Overview

This project presents a comprehensive analysis of delivery operations for a global e-commerce company managing end-to-end order fulfilment across multiple regions. The analysis covers **172,765 orders** spanning **January 2015 through January 2018**.

### Business Problem

Actual shipping times frequently deviate from scheduled delivery windows, leading to:
- Eroded customer trust
- Reduced order profitability
- An inability to make reliable delivery commitments at the point of purchase

### Objectives

- Understand delivery performance across all dimensions — region, shipping mode, time, and customer segment
- Quantify the financial impact of delays on order profitability
- Identify the primary operational bottlenecks driving late deliveries
- Build a predictive model to flag high-risk orders **before** they are shipped
- Deliver actionable, data-driven recommendations

---

## 🔍 Key Findings

| Metric | Value |
|--------|-------|
| Overall Late Delivery Rate | **54.71%** 🔴 |
| On-Time Delivery Rate | **45.29%** 🟢 |
| Total Orders Analysed | **172,765** |
| Total Profit (Profitable Orders) | **$7.5M** 🟢 |
| Profit at Risk (Delayed Orders) | **$2.1M** 🔴 |
| Predictive Model Accuracy (Random Forest) | **74%** 🟡 |
| Average Order Profit | **$22.03** |
| 90th Percentile Delay | **3 Days** |

> **Headline:** More than half of all orders arrive late. This is not an edge case — it is the default experience for the majority of customers.

---

## 📦 Dataset

| Property | Detail |
|----------|--------|
| Orders | 172,765 |
| Period | January 2015 — January 2018 |
| Scope | Global, multi-region |
| Categories | Sporting goods, fitness equipment, outdoor gear, footwear, apparel |
| Target Variable | `Late_delivery_risk` (binary: 0 = on-time, 1 = late) |

> ⚠️ **Note:** The raw dataset is not included in this repository. Place your data file in the `data/raw/` directory before running the notebooks. See [How to Run](#-how-to-run) for details.

---

## 📁 Project Structure

```
supply-chain-analysis/
│
├── data/
│   ├── raw/                        # Raw dataset (not tracked by git)
│   └── processed/                  # Cleaned & feature-engineered data
│
├── notebooks/
│   ├── 01_data_cleaning.ipynb      # Data cleaning and preprocessing
│   ├── 02_eda_profitability.ipynb  # Profitability distribution analysis
│   ├── 03_bottleneck_detection.ipynb  # Delay rates across dimensions
│   ├── 04_root_cause_analysis.ipynb   # Central Africa deep dive
│   ├── 05_time_patterns.ipynb      # Temporal delay pattern analysis
│   └── 06_ml_model.ipynb          # Random Forest model + SMOTE
│
├── reports/
│   └── Supply_Chain_Delivery_Performance_Report.docx
│
├── visuals/                        # All exported charts and figures
│
├── requirements.txt
├── .gitignore
└── README.md
```

---

## 🔬 Methodology

### 1. Data Cleaning & Preprocessing
- Removed duplicates and handled missing values
- Parsed and standardised date fields to compute actual vs. scheduled delivery windows
- Derived `delay_days` = actual delivery date − scheduled delivery date
- Classified orders as on-time or late based on `delay_days > 0`

### 2. Exploratory Data Analysis
- Profitability classification into three tiers: Profitable, Break-Even, Loss-Making
- Delay cohort distribution analysis
- Profit vs. delay days cross-tabulation

### 3. Bottleneck Detection
Delay rates computed across six operational dimensions:
- Shipping mode
- Geographic region
- Customer segment
- Product department
- Payment status
- Order time (hour, day, month)

### 4. Root Cause Analysis
Deep-dive into **Central Africa** (highest-delay region at 58.7%), ranking driver factors by delay percentage.

### 5. Machine Learning Pipeline

```
Raw Features
    │
    ▼
Frequency Encoding (categorical variables)
    │
    ▼
Stratified Train / Test Split (80/20)
    │
    ▼
SMOTE Oversampling (address class imbalance)
    │
    ▼
Random Forest Classifier
    │
    ▼
Evaluation: Precision / Recall / F1 / Accuracy
```

**Class imbalance handling:**

| Split | Class 0 — On-Time | Class 1 — Late |
|-------|-------------------|----------------|
| Before SMOTE | 59,030 | 79,182 |
| After SMOTE | 79,182 | 79,182 |

---

## 📊 Results

### KPI Baseline

| KPI | Value | Status |
|-----|-------|--------|
| Total Orders | 172,765 | — |
| Late Deliveries | 94,523 | 🔴 |
| Late Delivery Rate | 54.71% | 🔴 |
| On-Time Rate | 45.29% | 🟢 |
| Total Profit | $7.5M | 🟢 |
| Profit at Risk | $2.1M | 🔴 |
| 90th Pct Delay | 3 Days | 🟡 |
| Avg Order Profit | $22.03 | 🟡 |

---

### Bottleneck Detection

#### By Shipping Mode — #1 Lever

| Shipping Mode | Delay Rate | Status |
|---------------|------------|--------|
| First Class | **100.0%** | 🔴 Critical |
| Second Class | **79.8%** | 🔴 High Risk |
| Standard Class | **39.8%** | 🟡 Acceptable |
| Same Day | **0.0%** | 🟢 Excellent |

> First Class and Second Class operate in **complete contradiction** to their brand promise. This is the single most impactful variable to fix.

#### By Region

| Region | Delay Rate |
|--------|------------|
| Central Africa | 58.7% 🔴 |
| South Asia | 57.2% 🟡 |
| Eastern Europe | 56.8% 🟡 |
| Western Europe | 55.9% 🟡 |
| North America | 55.3% 🟡 |
| Southeast Asia | 54.8% 🟡 |

> All regions cluster between **55–59%**, confirming a **company-wide systemic issue**, not a localised logistics failure.

#### By Customer Segment

| Segment | Delay Rate |
|---------|------------|
| Consumer | 55.4% |
| Corporate | 55.1% |
| Home Office | 54.5% |

> Spread < 1% — no segment receives preferential service.

---

### Root Cause Analysis

Top driver factors within Central Africa (58.7% region average):

| Rank | Factor | Delay Rate | Severity |
|------|--------|------------|----------|
| 1 | First Class Mode Assignment | 100.0% | 🔴 Critical |
| 2 | Payment Review — Processing Stalls | 80.0% | 🔴 Critical |
| 3 | Outdoors Dept — Inventory & Carrier Gaps | 61.3% | 🔴 Critical |
| 4 | Golf Dept — Stock Availability Issues | 60.8% | 🔴 Critical |
| 5 | Late-Evening Cutoff Window (Hour 21) | 57.1% | 🟠 High |
| 6 | Midday Bottleneck (Hours 11–12) | 56.7% | 🟠 High |
| 7 | Peak Season Surge (Aug–Oct) | 55.4% | 🟡 Medium |
| 8 | Pending Payment Orders | 55.0% | 🟡 Medium |
| 9 | Monday Order Volume Spikes | 55.5% | 🟢 Low |
| 10 | Holiday Constraints (December) | 55.2% | 🟢 Low |

---

### Time-Based Patterns

| Dimension | Peak | Insight |
|-----------|------|---------|
| Monthly | Aug–Sep: **55.4%** 🔴 | Mid-year promotions overwhelm fulfilment capacity |
| Monthly | December: **55.2%** 🔴 | Q4 holiday surge — temporary logistics partners needed |
| Monthly | July: **~53.75%** 🟢 | Seasonal variation is plannable |
| Hour of Day | Hour 21: **57.1%** 🔴 | Orders miss processing cutoff windows |
| Hour of Day | Hours 11–12: **56.7%** 🟡 | Midday volume bottleneck |
| Day of Week | Spread: **< 1%** 🟢 | Not a meaningful operational lever |

---

### Machine Learning Model

**Random Forest Classifier — Test Set Performance (34,553 records)**

| Metric | Class 0 — On-Time | Class 1 — Late | Overall (Weighted) |
|--------|-------------------|----------------|--------------------|
| Precision | 0.68 🟡 | 0.78 🟢 | 0.74 🟢 |
| Recall | 0.72 🟢 | 0.75 🟢 | 0.74 🟢 |
| F1-Score | 0.70 🟡 | 0.77 🟢 | 0.74 🟢 |
| Accuracy | — | — | **0.74** 🟢 |
| Support | 14,758 | 19,795 | 34,553 |

**Key Takeaways:**
- **74% overall accuracy** — correctly classifies nearly 3 in 4 orders; ready for production deployment
- **78% precision on late orders** — when the model flags an order as late, it is right 78% of the time; sufficient for triggering interventions without excessive false alarms
- **75% recall on late orders** — captures 3 in 4 actual late orders; the 25% miss rate is acceptable for a first-generation system
- **Class 0 harder to predict (0.68)** — on-time orders share characteristics with late ones, given the systematic nature of delays

---

## 💡 Strategic Recommendations

| Priority | Recommendation | Target Impact |
|----------|---------------|---------------|
| 🔴 **Critical** | Immediately audit First & Second Class shipping capacity | Reduce late rate from 54.71% to <45% |
| 🟠 **High** | Deploy the predictive alert system in production | Intercept 75% of at-risk orders pre-shipment |
| 🟠 **High** | Resolve payment processing bottlenecks (auto-escalate after 2hrs) | Reduce payment-related delays by ~25% |
| 🟡 **Medium** | Develop seasonal surge capacity plans (Aug–Oct, Dec) | Reduce seasonal peak delay by ~5% |
| 🟡 **Medium** | Default eligible orders to Standard Class | Improve mode-level delays by 15–40% |
| 🟡 **Medium** | Investigate high-delay departments in Central Africa | Reduce Africa dept delays to <55% |
| 🟢 **Low** | Review pricing & discounts for loss-making orders (18.7%) | Reduce loss-making share to <12% |
| 🟢 **Low** | Continuously retrain the predictive model (quarterly) | Improve recall from 75% to >80% |

---

## 🎯 Target Outcomes

| Priority Area | Current State | Target State |
|---------------|--------------|--------------|
| Late Delivery Rate | 54.71% 🔴 | <30% within 12 months 🟢 |
| First Class On-Time Rate | 0% 🔴 | >80% 🟢 |
| Second Class On-Time Rate | 20.2% 🔴 | >60% 🟢 |
| Predictive Model Accuracy | 74% 🟡 | >82% (enhanced model) 🟢 |
| Loss-Making Orders | 18.7% 🔴 | <12% 🟢 |
| Profit at Risk | $2.1M 🔴 | Reduce by 40% 🟢 |

---

## 🛠 Tech Stack

| Tool | Purpose |
|------|---------|
| **Python 3.x** | Core analysis language |
| **Pandas** | Data manipulation and cleaning |
| **NumPy** | Numerical computation |
| **Matplotlib / Seaborn** | Data visualisation |
| **Scikit-learn** | Random Forest classifier, train/test split, metrics |
| **imbalanced-learn** | SMOTE oversampling |
| **Jupyter Notebook** | Interactive analysis environment |

---

## ▶️ How to Run

### 1. Clone the repository

```bash
git clone https://github.com/your-username/supply-chain-analysis.git
cd supply-chain-analysis
```

### 2. Create a virtual environment

```bash
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Add the dataset

Place your raw data file in:

```
data/raw/supply_chain_data.csv
```

### 5. Run the notebooks in order

```bash
jupyter notebook
```

Open and run the notebooks from `01_data_cleaning.ipynb` through `06_ml_model.ipynb` in sequence.

---

## 📄 Report

The full formatted analysis report is available in the `reports/` directory:

```
reports/Supply_Chain_Delivery_Performance_Report.docx
```

It includes:
- Executive summary with KPI dashboard
- Profitability distribution analysis
- Bottleneck detection across 6 dimensions
- Root cause analysis (Central Africa deep dive)
- Time-based delay patterns
- ML model results with performance metrics
- 8 prioritised strategic recommendations
- Target outcome scorecard

---

## 🤝 Contributing

Contributions are welcome. To contribute:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Commit your changes: `git commit -m 'Add: your feature description'`
4. Push to the branch: `git push origin feature/your-feature-name`
5. Open a Pull Request

---

## 📬 Contact

For questions or feedback about this project, feel free to open an issue or reach out via GitHub.

---

## 📝 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

<div align="center">

**Built with data, driven by impact.**

*172,765 orders · 3 years of data · 1 clear direction*

</div>

