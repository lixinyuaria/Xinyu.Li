# Comprehensive 5-Year Financial Analysis of the US Big Five Tech Giants

**Module:** ACC102 — Artificial Intelligence-Driven Data Analytics
**Track:** 2 — GitHub Data Analysis Project
**Companies analysed:** Apple (AAPL) · Microsoft (MSFT) · Alphabet (GOOGL) · Amazon (AMZN) · Meta (META)
**Fiscal window:** FY 2020 – FY 2024

---

## 📺 Product Link / Demo

- **Demo video (1–3 min):** _add your demo URL here before submission_
- **Main deliverable:** [`notebook.ipynb`](notebook.ipynb) — run top-to-bottom to reproduce every chart.

## 1. Problem & User

Over the last five fiscal years, how did revenue, profitability, and scale diverge across the US Big Five technology firms, and which financial metrics move together? The intended user is a **junior equity analyst** or **finance student** preparing a peer-comparison note.

## 2. Data

- **Source:** WRDS · Compustat North America — `comp.funda` joined with `comp.company`
- **Access method:** `wrds.Connection().raw_sql()` with a parameterised `LEFT JOIN`
- **Key fields:** `revt` (revenue), `cogs` (cost of goods sold), `ni` (net income), `at` (total assets), `oiadp` (operating income)
- **Date window:** `datadate BETWEEN '2020-01-01' AND '2024-12-31'`
- **Compustat screen applied:** `indfmt='INDL' · datafmt='STD' · popsrc='D' · consol='C'`
- **Access date:** printed automatically in the notebook at run time

## 3. Methods

1. **Parameterised SQL** via Python f-string (Week 6 lecture technique) — tickers and dates are Python variables, not hard-coded in the query.
2. **`LEFT JOIN`** between `comp.funda` (financial data, keyed by `gvkey`) and `comp.company` (ticker mapping).
3. **Data cleaning** with pandas — drop incomplete firm-years, coerce `fyear` to int, rescale to USD billions, recompute gross profit as `revt − cogs`.
4. **Derived ratios** — gross margin, net margin, revenue growth (`groupby('tic').pct_change()`).
5. **7 visualisations** with matplotlib and seaborn; each chart has a markdown insight block.
6. **Function wrappers** — the whole pipeline is packaged into three reusable functions (`fetch_tech_fundamentals`, `build_financial_panel`, `summary_markdown`) so the same workflow can be re-run next quarter or plugged into a Coze / Dify agent.

## 4. Key Findings

- **The Big Five are not one group — they are three archetypes.** Amazon (asset-heavy retail + cloud), Apple (premium hardware), and the software/ads trio Microsoft / Alphabet / Meta.
- **Ranking by revenue and ranking by profit disagree.** Amazon leads on revenue but Apple and Microsoft lead on net income — direct evidence that the margin structure of the business model dominates scale.
- **2022 is the common trough, 2024 is the common recovery**, but the depth and speed of recovery differ sharply. Meta has the sharpest V-shape; Microsoft's line is the smoothest.
- **Gross margin drives net margin (correlation ≈ 0.65), but revenue size does not.** Pooled correlations show no meaningful link between firm size and profitability within this cohort.
- **Apple's margin is drifting upward** while its top-line growth is flat — a visible mix-shift from hardware to Services.

## 5. How to Run

### Prerequisites

- Python 3.10+
- A **WRDS account** with Compustat North America access (institutional credential)
- Packages in [`requirements.txt`](requirements.txt)

### Setup

```bash
git clone https://github.com/<your-handle>/acc102-data-product.git
cd acc102-data-product
pip install -r requirements.txt
jupyter lab notebook.ipynb
```

The first time you connect, WRDS will prompt for your username and password and offer to cache credentials in `~/.pgpass`. **No credentials are stored in this repository.**

Run the notebook top-to-bottom. All seven charts will be saved into `figures/`.

## 6. Repository Structure

```
acc102-data-product/
├── README.md              ← this file
├── notebook.ipynb         ← main analysis (43 cells, 7 charts)
├── reflection.md          ← 500–800 word reflection report + AI disclosure
├── requirements.txt       ← Python dependencies
└── figures/               ← exported charts (populated after first run)
    ├── 01_revenue_trend.png
    ├── 02_net_income_trend.png
    ├── 03_gross_margin.png
    ├── 04_revenue_growth.png
    ├── 05_net_margin.png
    ├── 06_scale_bar.png
    └── 07_correlation_heatmap.png
```

## 7. Limitations & Next Steps

- **Sample size.** 5 firms × 5 years = 25 observations. Correlation coefficients are directional, not precise.
- **Consolidated data only.** Segment-level detail (Apple Services, AWS, YouTube) is not used — a natural extension via `comp.segments`.
- **Net income includes one-off items** (Amazon's Rivian write-down, Alphabet's EU fines). A cleaner version would use operating income.
- **Pooled correlation mixes cross-section and time-series.** Demean-within-firm would isolate time-series co-movement.
- **Next:** (a) extend to full S&P 500 IT sector; (b) add segment-level breakdowns; (c) layer in CRSP market-cap and valuation multiples; (d) wrap the three functions into a Coze plugin (Track 3).

---

_Submitted for ACC102 Mini Assignment · 2nd Semester 2024-25._
