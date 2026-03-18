# Apple Inc. (AAPL) — Equity Valuation Model
### Bloomberg Data → Python → McKinsey-Style Excel | DCF · Comps · ESG · Sensitivity

---

## What this is

A full equity valuation model for Apple Inc. built entirely in Python on Google Colab, using 10 years of raw Bloomberg Terminal data as the input. The output is a formatted Excel workbook — 9 sheets covering everything from the historical financials to a DCF, sensitivity analysis, ratio dashboard, and ESG scorecard.

No templates. No shortcuts. Every number traces back to a Bloomberg field.

---

## The output at a glance

| Sheet | What's in it |
|---|---|
| Cover | Executive snapshot — price, market cap, FCF, intrinsic value vs market |
| Executive Summary | Valuation triangulation, 5-year projection, EV bridge |
| Historical IS | Income Statement FY2016–FY2025, all margins, EPS, 5Y CAGR |
| Historical BS | Balance Sheet FY2016–FY2025, net debt, book value per share |
| Historical CF | Cash flows FY2016–FY2025, FCF build, buybacks + dividends |
| DCF Valuation | WACC build-up, 5-year FCFF projection, terminal value, equity bridge, comps |
| Sensitivity Analysis | 3 heat-map tables: WACC × TGR, Growth × Margin, Bull/Base/Bear |
| Ratio Dashboard | 40+ metrics across 7 categories over 10 years |
| ESG Scorecard | Bloomberg BESG scores, GHG emissions, governance, materiality linkage |

---

## Key findings (as of 18-Mar-2026)

| Metric | Value |
|---|---|
| Current Market Price | $254.23 |
| DCF Intrinsic Value (Base) | $136.25 |
| DCF Intrinsic Value (Bull) | $190.99 |
| WACC | 10.03% |
| Terminal Growth Rate | 3.0% |
| FY2025 Revenue | $416.2B |
| FY2025 FCF | $98.8B |
| FY2025 ROIC | 85.5% |
| ESG Score (Bloomberg BESG) | 5.66/10 |

The gap between the base-case DCF ($136) and market price ($254) is real and worth understanding rather than dismissing. Apple's market premium reflects Services-segment margins (>70% gross), 2B+ active devices, and an AI optionality bet that doesn't show up cleanly in a 5-year FCFF model. The bull case at WACC=8.5% gets to $191 — much closer to where the market is pricing it.

---

## WACC build-up

```
Risk-Free Rate (US 10Y)       4.30%   ← Bloomberg, Mar 2026
Equity Risk Premium           4.80%   ← Damodaran
Beta (5Y Monthly vs S&P 500)  1.24
Cost of Equity (CAPM)        10.25%

Pre-Tax Cost of Debt           3.10%   ← Apple blended coupon
Effective Tax Rate            15.60%   ← FY2025 Bloomberg actual
After-Tax Cost of Debt         2.62%

Equity Weight                 97.14%   ← Based on market cap vs total debt
Debt Weight                    2.86%

WACC                          10.03%
Terminal Growth Rate           3.00%
```

---

## Sensitivity: WACC × Terminal Growth Rate

| WACC ↓ \ TGR → | 2.0% | 2.5% | **3.0%** | 3.5% | 4.0% |
|---|---|---|---|---|---|
| 9.03% | $141 | $150 | $160 | $172 | $186 |
| 9.53% | $131 | $139 | $147 | $157 | $169 |
| **10.03%** | $123 | $129 | **$136** | $145 | $154 |
| 10.53% | $115 | $120 | $127 | $134 | $142 |
| 11.03% | $108 | $113 | $118 | $125 | $132 |

---

## Scenario comparison

| Scenario | Rev CAGR | EBITDA Margin | WACC | Price/Share | vs. Market |
|---|---|---|---|---|---|
| Bear | 4.0% | 32.0% | 11.03% | $80.87 | (68.2%) |
| Base | 8.0% | 35.8% | 10.03% | $136.98 | (46.1%) |
| Bull | 12.0% | 39.0% | 9.03% | $225.16 | (11.4%) |

---

## Data source

All financial data comes from the Bloomberg Terminal — Income Statement, Balance Sheet, Cash Flow Statement, CAPEX & Depreciation, Profitability, Growth, Credit, Liquidity, Working Capital, DuPont, Yield Analysis, and ESG modules (BESG, Environmental, Social, Governance).

The raw Bloomberg export (`Apple.xlsx`) is the single input. The code reads it directly — no manual data entry anywhere.

---

## How to run it

**Requirements:** Google Colab (free tier works), `openpyxl`, `pandas`, `numpy`

```
Step 1  →  Upload Apple.xlsx to Colab
Step 2  →  Run Step1_Setup.ipynb  (installs libs, loads + parses all Bloomberg data)
Step 3  →  Run Step2_DCF.ipynb    (WACC, FCFF projections, terminal value, sensitivity tables)
Step 4  →  Run Step3_Excel.ipynb  (builds 9-sheet workbook: Cover through ESG)
Step 5  →  Run Step4_Final.ipynb  (DCF sheet, sensitivity, ratio dashboard, ESG, saves file)
```

Output: `Apple_McKinsey_Valuation_v2.xlsx` — downloads automatically from Colab.

---

## Model structure

```
Bloomberg Export (Apple.xlsx)
        │
        ▼
   Step 1: Data Loading
   ├── 20 Bloomberg sheets parsed
   ├── 10 years of data (FY2016–FY2025)
   └── Revenue reconstructed where Bloomberg marks "—"
        │
        ▼
   Step 2: DCF Engine
   ├── WACC (CAPM + capital structure weights)
   ├── 5-year FCFF projections
   ├── Terminal value (Gordon Growth Model)
   ├── EV → Equity → Per Share bridge
   └── 2D sensitivity tables (WACC×TGR, Growth×Margin)
        │
        ▼
   Steps 3+4: Excel Writer
   ├── openpyxl — formatting, colors, borders, merged cells
   ├── McKinsey color convention (Blue=input, Black=formula, Navy=total)
   └── 9 sheets, 40+ ratio metrics, full ESG scorecard
```

---

## Key metrics — FY2025 snapshot

**Profitability**
- Gross Margin: 46.9% (up from 39.1% in FY2016 — Services mix shift)
- EBIT Margin: 32.0%
- Net Margin: 26.9%
- ROIC: 85.5%

**Capital returns**
- FCF: $98.8B
- Dividends + Buybacks: $106.1B (returned more cash than FCF — funded by balance sheet)
- Buyback + Div / CFO: 95.2%

**Balance sheet**
- Total Debt: $112.4B
- Cash + ST Securities: $54.7B
- Net Debt: $57.7B (very manageable vs. $144.7B EBITDA)

**ESG (Bloomberg BESG)**
- Overall: 5.66/10 (Above Average)
- Governance: 7.71/10 (Leadership tier)
- GHG Scope 3: down 50% since FY2016 (30,784 → 15,228 thousand tonnes CO₂e)
- Renewable energy share: ~86% of total consumption

---

## Limitations worth knowing

This is a 5-year FCFF model. It works well for mature, stable businesses. For Apple specifically, it likely undervalues:

- The Services segment, which has SaaS-level economics ($100B+ revenue, >70% gross margin) and should arguably be valued on a separate multiple
- Any AI/Apple Intelligence monetisation that isn't in the current revenue run rate
- The option value in markets where Apple is still underpenetrated (India, Southeast Asia)

A sum-of-the-parts valuation separating Hardware and Services would give a materially higher number. That's a logical next step.

---

## Files in this repo

```
├── Apple.xlsx                          ← Bloomberg Terminal export (raw input)
├── Apple_McKinsey_Valuation_v2.xlsx    ← Final output workbook
├── Step1_Setup.ipynb
├── Step2_DCF.ipynb
├── Step3_Excel.ipynb
├── Step4_Final.ipynb
└── README.md
```

---

## Disclaimer

This model is for learning and portfolio purposes only. Not investment advice. All data from Bloomberg Terminal. Projections are estimates based on analyst assumptions and should not be used as the basis for any investment decision.

---

*Built by Divyansh Jain — MBA Finance | Bloomberg Terminal | Python*
