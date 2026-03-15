# Industry Financial Benchmarking

**Purpose:** Methodology for comparing client financial performance to industry peers using standardized data sources and ratio analysis.

**Tier:** 3 (evergreen, no year indexing needed)

**Skills that reference this:** `fp-industry-analysis`, `fp-kpi-dashboard`, `fp-board-reporting`, `fp-strategic-advisory`

---

## Why Benchmark

Benchmarking answers the client's most common question: "How are we doing compared to others?" It converts raw financial statements into relative performance context — a 15% net margin means nothing in isolation but signals strong performance in food service and weak performance in SaaS. Benchmarking also identifies operational improvement targets, supports loan applications (lenders compare applicants to industry norms), and provides objective grounding for board presentations.

---

## Data Source Hierarchy

### Tier 1: RMA Annual Statement Studies (Gold Standard)

- Published by the Risk Management Association (formerly Robert Morris Associates)
- Compiled from 260,000+ financial statements submitted by commercial bank lending officers
- Organized by NAICS code with revenue-size breakdowns (under $250K through $25M+)
- Provides 16 common-size ratios with quartile distributions (upper quartile, median, lower quartile)
- Includes common-size balance sheet and income statement percentages
- Subscription required (~$300+/year for digital access)
- Strength: real financial data from audited/reviewed statements, not self-reported surveys
- Limitation: skews toward borrowing companies (companies with bank relationships); may not represent non-borrowing firms

### Tier 2: Commercial Data Services

- **BizMiner** — Industry financial profiles segmented by revenue size, geography (state/metro), and legal form. Stronger small-business coverage than RMA. Subscription-based, pay-per-report options available
- **IBISWorld** — Industry reports with financial benchmarks, market structure, and trend analysis. Better for market context than pure ratio comparison. Institutional subscription

### Tier 3: Free Government Sources

- **IRS Statistics of Income (SOI)** — Aggregate corporate return data by industry and asset size. Available free at irs.gov. Limitations: 2-3 year lag, broad industry groupings, includes loss companies that skew averages, no quartile distributions
- **Census Bureau Annual Business Survey** — Broad economic data (revenue, payroll, employment) by NAICS and geography. Useful for sizing markets, less useful for ratio analysis
- **BLS Quarterly Census of Employment and Wages** — Payroll and employment data by industry and area. Supports compensation-to-revenue benchmarking

### Tier 4: Public Company Comparables

- **SEC EDGAR** — Free access to 10-K, 10-Q filings for publicly traded companies. Useful when the client's industry has public comparables
- Limitation: public companies are typically larger, more complex, and subject to different reporting requirements than private clients
- Best used as a directional reference, not a direct peer comparison for small/mid-market private companies

---

## Peer Group Selection

Peer group quality determines benchmarking validity. A poorly constructed peer group produces misleading conclusions.

### Selection Criteria (in priority order)

1. **NAICS code** — Use the most specific level available (6-digit preferred). A general contractor (NAICS 236220) should not be benchmarked against a highway paving company (NAICS 237310) even though both are "construction"
2. **Revenue size band** — A $2M company operates differently from a $50M company in the same industry. Match the revenue bracket as closely as possible
3. **Geographic market** — Labor costs, real estate costs, and competitive dynamics vary by region. National data is acceptable when regional data is unavailable, but note the limitation
4. **Business maturity** — Startups (under 3 years) have different financial profiles than mature businesses. Exclude early-stage companies from benchmarks for established clients, and vice versa
5. **Legal form** — C-corps, S-corps, and partnerships have structurally different financial statements (officer compensation dynamics, tax treatment of income). Match legal form when possible

---

## Financial Ratio Categories

### Liquidity Ratios

- **Current ratio** — Current assets / current liabilities. Measures short-term obligation coverage. Industry median varies widely (1.2x for retailers, 2.0x+ for manufacturers with inventory)
- **Quick ratio** — (Cash + receivables) / current liabilities. Strips out inventory; more conservative liquidity test
- **Cash ratio** — Cash and equivalents / current liabilities. Most conservative; useful for businesses with slow-converting receivables

### Leverage Ratios

- **Debt-to-equity** — Total liabilities / total equity. Measures financial risk and capital structure
- **Debt-to-assets** — Total liabilities / total assets. Alternative leverage measure; bounded between 0 and 1
- **Interest coverage** — EBIT / interest expense. Measures ability to service debt; below 1.5x is a warning signal

### Efficiency Ratios

- **Inventory turnover** — COGS / average inventory. Higher = faster inventory movement. Compare only within industry (grocery vs. heavy equipment are incomparable)
- **Days sales outstanding (DSO)** — (Receivables / revenue) x 365. Measures collection speed. Rising DSO relative to peers signals collection problems
- **Days payable outstanding (DPO)** — (Payables / COGS) x 365. Measures payment speed to vendors
- **Asset turnover** — Revenue / total assets. Measures asset utilization efficiency

### Profitability Ratios

- **Gross margin** — (Revenue - COGS) / revenue. Measures pricing power and direct cost control
- **Operating margin** — Operating income / revenue. Measures operational efficiency after overhead
- **Net margin** — Net income / revenue. Bottom-line profitability after all costs, interest, and taxes
- **Return on assets (ROA)** — Net income / average total assets. Measures how effectively assets generate profit
- **Return on equity (ROE)** — Net income / average equity. Measures return to owners
- **Return on invested capital (ROIC)** — NOPAT / (total debt + equity - excess cash). Most comprehensive return measure; adjusts for capital structure

---

## Analysis and Presentation

### Trend vs. Point-in-Time

- Point-in-time comparison shows current positioning relative to peers
- Trend analysis (3-5 years) shows trajectory — a company at the 40th percentile but improving year-over-year tells a different story than one at the 60th percentile and declining
- Always present both when data permits

### Quartile Ranking

- Position each ratio within the industry distribution: upper quartile (top 25%), median, lower quartile (bottom 25%)
- Identify ratios where the client is an outlier (above upper quartile or below lower quartile) — these are the discussion points
- A ratio at the median is not necessarily "good" — it means average performance, which may or may not align with the client's goals

### Narrative Context

- Raw ratios without context are meaningless to most clients. Translate: "Your DSO is 52 days vs. an industry median of 38 days" becomes "You're collecting payments 14 days slower than your peers, which ties up approximately $X in working capital"
- Identify actionable insights: what can the client change, and what is structural to their business model
- Acknowledge limitations: "Your NAICS code groups you with companies ranging from $1M to $50M in revenue; your $3M operation may not be directly comparable to the larger firms in this group"

---

## Limitations and Caveats

- **Industry definition mismatch** — NAICS codes may not capture the client's actual business mix; a company doing 60% consulting and 40% product sales may not fit cleanly into either NAICS code
- **Size effects dominate** — Revenue size often matters more than industry for certain ratios (small companies in any industry tend to have lower margins and higher leverage than large ones)
- **Accounting method differences** — Cash-basis companies report different current ratios and margins than accrual-basis companies; ensure the benchmark data matches the client's method
- **Geographic variation** — A company in Manhattan has fundamentally different cost structures than one in rural Florida
- **Survivorship bias** — Benchmark databases exclude failed companies, which inflates average performance metrics
- **Timeliness** — RMA and SOI data lag 1-3 years; economic conditions may have shifted

---

## Accounting System Data Extraction

- Pull **Balance Sheet (Standard)** and **Profit & Loss (Standard)** reports for the analysis period
- Use **custom date ranges** to match the benchmark data's period (calendar year for most sources)
- Export to spreadsheet for ratio computation if the accounting system does not compute financial ratios natively
- For trend analysis, pull comparative periods (current year vs. 2-4 prior years) in a single report run
- Use monthly P&L reports for the granularity needed for seasonality-adjusted comparisons
- Ensure the COA structure separates COGS from operating expenses to compute gross margin correctly
- For platform-specific report extraction, delegate to `qbo-integration:qbo-reporting`

---

## Decision Points

- **Which data source to use:** RMA for bank-quality benchmarking and loan support; IRS SOI when budget is constrained; BizMiner when geographic or size-specific segmentation matters most
- **When benchmarking adds value vs. noise:** Benchmarking is most valuable for established businesses (3+ years) in well-defined industries. For startups, niche businesses, or diversified operations, peer group construction may be too imprecise to be actionable
- **How to handle outlier ratios:** Investigate before presenting. An unusually high current ratio might indicate excess idle cash (inefficiency), not superior liquidity. An unusually low margin might reflect a deliberate low-price strategy, not poor performance
- **Frequency:** Annual benchmarking for most clients; quarterly for clients actively working on performance improvement or preparing for financing

---

## Authoritative References

- Risk Management Association — Annual Statement Studies (rmahq.org)
- IRS Statistics of Income — Corporation Income Tax Returns (irs.gov/statistics)
- U.S. Census Bureau — Annual Business Survey (census.gov)
- BLS — Quarterly Census of Employment and Wages (bls.gov/cew)
- SEC EDGAR — Full-text search of public company filings (sec.gov/edgar)
- Investopedia — Financial Ratio Analysis (reference for ratio definitions and interpretation)
- Corporate Finance Institute — Financial Analysis Fundamentals (corporatefinanceinstitute.com)
