# 🚚 FedEx Supply Chain Logistics — EDA & BI Dashboards

> Exploratory Data Analysis on the SCMS Delivery History Dataset to uncover delivery performance patterns, freight cost drivers, and vendor reliability across a global pharmaceutical supply chain.

---

## 📊 Project Stats

| | |
|---|---|
| **Records** | 10,324 shipments |
| **Features** | 33 columns |
| **Countries** | 40+ |
| **Time Period** | 2006 – 2015 |
| **Charts** | 20+ visualizations |
| **Dashboards** | 2 (Power BI) |

---

## 📁 Repository Structure

```
📦 fedex-supply-chain-eda/
├── 📓 FedEx_Supply_Chain_EDA.ipynb        # Main Jupyter Notebook (full EDA)
├── 📊 FedEx_PowerBI_Dashboard.pbix        # Power BI report (2 dashboards)
├── 📂 SCMS_Delivery_History_Dataset.csv   # Raw dataset
├── 📄 Fed-Ex.pdf                          # Original project brief & rubric
└── 📖 README.md                           # This file
```

---

## 🧠 Business Context

FedEx Logistics manages a complex global supply chain for pharmaceutical products across Africa, Asia, and the Americas. This project analyzes historical delivery data to:

- Identify factors contributing to **shipment delays**
- Understand **freight cost drivers** across shipment modes and INCO terms
- Detect **country-level and product-level patterns** to optimize delivery planning
- Provide **actionable recommendations** to reduce costs and minimize delays

---

## ❓ Business Questions Answered

1. Are shipments managed by specific teams (e.g., PMO-US) more likely to be on time?
2. Does shipment mode (Air / Truck / Ocean / Air Charter) influence on-time delivery?
3. Do shipments from certain countries experience more delays than others?
4. Is there a delivery performance difference based on lead time (PO → Scheduled Delivery)?
5. Does the INCO term type impact vendor delivery performance?
6. Are heavier shipments more likely to incur higher insurance costs?

---

## 🗂️ Dataset Description

**File:** `SCMS_Delivery_History_Dataset.csv`

| Column | Type | Description |
|---|---|---|
| `ID` | int | Unique identifier per logistics record |
| `Country` | str | Destination country |
| `Managed By` | str | Managing team (e.g., PMO - US) |
| `Fulfill Via` | str | Direct Drop or From RDC |
| `Vendor INCO Term` | str | Shipment agreement terms (EXW, FCA, DDP…) |
| `Shipment Mode` | str | Air, Truck, Ocean, Air Charter |
| `Scheduled Delivery Date` | date | Planned delivery date |
| `Delivered to Client Date` | date | Actual delivery date |
| `PO Sent to Vendor Date` | date | Date PO was issued to vendor |
| `Product Group` | str | ARV, HRDT, ACT, ANTM, MRDT |
| `Brand` | str | Generic, Determine, Kaletra… |
| `Line Item Quantity` | int | Total units shipped |
| `Line Item Value` | float | Total shipment value (USD) |
| `Weight (Kilograms)` | str* | Shipment weight — requires numeric conversion |
| `Freight Cost (USD)` | str* | Freight cost — contains text entries, needs cleaning |
| `Line Item Insurance (USD)` | float | Insurance cost for the line item |

> \* These columns are stored as strings in the raw CSV due to non-numeric entries like *"Freight Included in Commodity Cost"*. Handled during data wrangling.

---

## ⚙️ Setup & Installation

### Prerequisites
- Python 3.8+
- Jupyter Notebook
- Power BI Desktop (free) — for `.pbix` file

### Install dependencies

```bash
pip install pandas numpy matplotlib seaborn plotly jupyter
```

### Run the notebook

```bash
# Clone the repo
git clone https://github.com/your-username/fedex-supply-chain-eda.git
cd fedex-supply-chain-eda

# Launch Jupyter
jupyter notebook FedEx_Supply_Chain_EDA.ipynb
```

> **Note:** Place `SCMS_Delivery_History_Dataset.csv` in the same directory as the notebook before running.

Use **Kernel → Restart & Run All** to execute all cells sequentially without errors.

---

## 🔧 Data Wrangling Steps

| Step | Action | Reason |
|---|---|---|
| 1 | Convert `Freight Cost (USD)` to numeric | Contains text entries → coerce errors to NaN |
| 2 | Convert `Weight (Kilograms)` to numeric | Some entries reference other ASN records |
| 3 | Parse all date columns to `datetime` | Stored as strings; invalid dates → NaT |
| 4 | Fill `Shipment Mode` NaN → `"Unknown"` | 360 missing values (~3.5%) |
| 5 | Fill `Line Item Insurance` NaN → `0` | 287 records with no insurance entry |
| 6 | Create `Delivery_Delay_Days` | Actual − Scheduled delivery date |
| 7 | Create `On_Time` flag | Delayed if `Delivery_Delay_Days > 0` |
| 8 | Create `Lead_Time_Days` | PO Sent → Delivered (full pipeline duration) |
| 9 | Create `Freight_per_KG` | Freight Cost ÷ Weight (cost efficiency metric) |
| 10 | Extract `Delivery_Year` | For year-over-year trend analysis |

---

## 📈 Visualizations (20 Charts)

Following the **UBM Rule** — Univariate → Bivariate → Multivariate:

| # | Chart | Type | Analysis |
|---|---|---|---|
| 1 | Shipment mode distribution | Pie chart | Univariate |
| 2 | On-time vs delayed deliveries | Bar chart | Univariate |
| 3 | Top 15 countries by volume | Horizontal bar | Univariate |
| 4 | Product group distribution | Count plot | Univariate |
| 5 | Freight cost by shipment mode | Box plot | Bivariate |
| 6 | On-time rate by shipment mode | Grouped bar | Bivariate |
| 7 | Delay rate — top 10 countries | Bar + reference line | Bivariate |
| 8 | Delivery performance by INCO term | Stacked bar | Bivariate |
| 9 | Lead time distribution | Histogram + KDE | Univariate |
| 10 | Weight vs freight cost | Scatter by mode | Bivariate |
| 11 | Line item value by product group | Violin plot | Bivariate |
| 12 | First line designation vs delay | Count plot with hue | Bivariate |
| 13 | Shipment volume over time | Line + area fill | Univariate |
| 14 | Correlation heatmap | Heatmap | Multivariate |
| 15 | Pair plot | Pair plot with hue | Multivariate |
| 16 | Freight cost per KG by mode | Bar chart | Bivariate |
| 17 | Top 10 brands by volume | Horizontal bar | Univariate |
| 18 | Fulfill via vs delivery performance | 100% stacked bar | Bivariate |
| 19 | Weight vs insurance cost | Scatter + regression | Bivariate |
| 20 | Freight cost: mode × product group | Pivot heatmap | Multivariate |

---

## 📊 Power BI Dashboards

### Dashboard 1 — Delivery Performance
- **KPI Cards:** Total Orders · Late Deliveries % · On-Time Rate · Avg Delivery Time
- Donut chart: On-time vs delayed split
- Bar chart: Delay rate by shipment mode
- Horizontal bar: Top 10 countries by delay rate (with avg reference line)
- Line chart: Quarterly delay rate trend (2010–2015)
- Stacked bar: Fulfillment method vs performance
- **Slicers:** Shipment Mode · Country · Product Group · Delivery Year

### Dashboard 2 — Cost & Efficiency
- **KPI Cards:** Total Freight Cost · Avg Freight Cost · Median Freight Cost · Avg Cost per KG
- Horizontal bar: Top 10 vendors by median freight cost
- Scatter/bubble: Cost vs delivery time by mode (bubble = volume)
- Bar chart: Freight cost per KG by mode
- Donut chart: Total cost share by mode
- Treemap: Freight cost by product group

> All DAX measures are pre-built in the `.pbix` file. See the [Power BI DAX guide](#dax-measures) below.

---

## 🔢 DAX Measures (Power BI)

<details>
<summary><strong>Click to expand — all DAX measures</strong></summary>

### Calculated Columns
```dax
-- Delivery delay (negative = early, positive = delayed)
Delivery Delay Days =
    DATEDIFF('SCMS'[Scheduled Delivery Date], 'SCMS'[Delivered to Client Date], DAY)

-- On-time flag
On Time Flag =
    IF('SCMS'[Delivery Delay Days] <= 0, "On Time", "Delayed")

-- Full pipeline lead time
Lead Time Days =
    DATEDIFF('SCMS'[PO Sent to Vendor Date], 'SCMS'[Delivered to Client Date], DAY)

-- Cost efficiency metric
Freight per KG =
    DIVIDE('SCMS'[Freight Cost (USD)], 'SCMS'[Weight (Kilograms)], BLANK())
```

### Measures
```dax
Total Orders = COUNTROWS('SCMS')

Late Deliveries % =
    DIVIDE(
        COUNTROWS(FILTER('SCMS', 'SCMS'[On Time Flag] = "Delayed")),
        [Total Orders], 0
    )

On Time Rate % = 1 - [Late Deliveries %]

Avg Delivery Time =
    AVERAGEX(FILTER('SCMS', 'SCMS'[Lead Time Days] > 0), 'SCMS'[Lead Time Days])

Median Freight Cost =
    MEDIANX(FILTER('SCMS', NOT ISBLANK('SCMS'[Freight Cost (USD)])), 'SCMS'[Freight Cost (USD)])

Avg Freight per KG =
    AVERAGEX(
        FILTER('SCMS', NOT ISBLANK('SCMS'[Freight per KG]) && 'SCMS'[Freight per KG] > 0),
        'SCMS'[Freight per KG]
    )

Country Delay Rate =
    DIVIDE(
        CALCULATE(COUNTROWS('SCMS'), 'SCMS'[On Time Flag] = "Delayed"),
        [Total Orders], 0
    )

Vendor Rank =
    RANKX(ALL('SCMS'[Vendor]), [Median Freight Cost], , DESC, Dense)
```
</details>

---

## 🔑 Key Findings

### Delivery Performance
| Metric | Value |
|---|---|
| Overall on-time rate | **88.5%** |
| Late delivery rate | **11.5%** (1,186 shipments) |
| Avg delivery time | **108.9 days** |
| Highest delay country | **Burundi — 38.8%** |
| Best performing mode | **Air — 9.6% delay rate** |
| Worst performing mode | **Ocean — 17.5% delay rate** |

### Cost & Efficiency
| Metric | Value |
|---|---|
| Air cost per KG | **$9.51** (most expensive) |
| Ocean cost per KG | **$1.65** (most efficient) |
| Air Charter median freight | **$25,468** |
| Truck median freight | **$2,268** (cheapest) |
| Weight ↔ Insurance correlation | **0.62** |
| Value ↔ Insurance correlation | **0.96** |

### Biggest Insight — RDC vs Direct Drop
> Shipments fulfilled via **Regional Distribution Centers (RDC)** have a ~8% delay rate vs ~15% for **Direct Drop** shipments. Expanding RDC coverage is the single highest-impact operational change available.

---

## 💡 Business Recommendations

| Priority | Recommendation | Expected Impact |
|---|---|---|
| 🔴 High | Expand RDC coverage to Nigeria, Côte d'Ivoire, Haiti | ~50% delay reduction in those corridors |
| 🔴 High | Require approval gate for Air Charter bookings | Reduce high-cost, high-delay charter usage |
| 🟡 Medium | Shift eligible ARV cargo from Air → Ocean/Truck | Significant freight cost savings |
| 🟡 Medium | Introduce SLA tiers for first-line designation drugs | Protect critical treatment supply chains |
| 🟡 Medium | Standardize lead time windows to 80–120 days | Fix bimodal procurement pattern |
| 🟢 Low | Recalibrate insurance brackets by shipment value | Eliminate over-insurance on low-value shipments |
| 🟢 Low | Country-level risk reviews for top 5 delay countries | Tackle structural logistics failures |

---

## 🛠️ Libraries Used

| Library | Purpose |
|---|---|
| `pandas` | Data manipulation, cleaning, aggregation |
| `numpy` | Numerical computation |
| `matplotlib` | Base static visualizations |
| `seaborn` | Statistical visualizations |
| `plotly` | Interactive visualizations |
| `jupyter` | Interactive development environment |
| `Power BI Desktop` | Business intelligence dashboards |

---

## 📋 Evaluation Rubric Coverage

| Criterion | Marks | Where |
|---|---|---|
| Summary & Technical Documentation | 10 | Notebook — Project Summary section |
| Exploration: Head, Tail, Summary, Data Dict. | 5 | Notebook — Section 1 |
| Missing Values / Null Handling | 5 | Notebook — Section 3 (Data Wrangling) |
| Conclusions from Data, Correlation, Trends | 10 | All charts + Correlation Heatmap (Chart 14) |
| Accomplish Milestones from Problem Statement | 10 | Charts 5–12 (all 6 business questions) |
| At Least 5 Visualization Types | 15 | 20 charts across 8+ types |
| Final Summary of Conclusion | 5 | Notebook — Conclusion section |
| Commented Code | 5 | All code cells have inline comments |
| Proper Output Formatting | 5 | Consistent figure sizes, titles, axis labels |
| Modularity of Code | 5 | Functions used for repeated operations |
| Video Presentation | 20 | Separate deliverable |
| Fluency & Grammatical Accuracy | 5 | Separate deliverable |
| **Total** | **100** | |

---

## 👤 Author

**[Your Name]**
Data Science Capstone Project — 2024

---

*Dataset: SCMS Delivery History Dataset — Supply Chain Management System for global pharmaceutical distribution*
