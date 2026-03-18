---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# Working Capital Optimization

**Purpose:** Procedures for analyzing, measuring, and optimizing working capital for advisory clients across service and product businesses.

**Tier**: 3 (evergreen, no year indexing needed)

**Skills**: `fp-working-capital`, `fp-cash-management`, `fp-advisory-reporting`

---

## Overview

Working capital is current assets minus current liabilities — the operating liquidity buffer that funds day-to-day business operations. Too little working capital creates liquidity risk and missed obligations; too much signals idle capital that could be deployed for growth or returned to owners. Advisory work centers on measuring the cash conversion cycle, benchmarking against industry norms, and recommending operational changes to free trapped cash.

---

## Cash Conversion Cycle (CCC)

The CCC measures the number of days between cash outflow for inputs and cash inflow from sales. Lower is better — it means the business recycles cash faster.

**Formula:** CCC = DSO + DIO - DPO

### Component Metrics

- **Days Sales Outstanding (DSO):** Accounts Receivable / (Revenue / 365) — measures how quickly customers pay. Lower DSO = faster collections.
- **Days Inventory Outstanding (DIO):** Inventory / (COGS / 365) — measures how long inventory sits before sale. N/A for pure service companies. Lower DIO = faster inventory turns.
- **Days Payable Outstanding (DPO):** Accounts Payable / (COGS / 365) — measures how long the business takes to pay suppliers. Higher DPO = longer use of supplier financing.

### Industry Benchmarks

CCC varies dramatically by industry and business model:

- **Manufacturing:** 60-90 days (heavy inventory, trade receivables)
- **Professional services:** 30-45 days (no inventory, DSO is the primary driver)
- **Retail (pre-paid / cash sales):** Can be negative if customers pay upfront and suppliers extend terms
- **Construction:** 80-120+ days (long project cycles, retention receivables)
- **SaaS / subscription:** Near zero or negative if annual prepayment with monthly expense recognition

Cross-reference `fp/industry-benchmarking.md` for ratio data sources and peer comparison procedures.

---

## Working Capital Ratios

### Current Ratio

- **Formula:** Current Assets / Current Liabilities
- **Target range:** 1.5 to 2.0 for most industries
- **Below 1.0:** Business cannot cover short-term obligations from short-term assets — liquidity stress
- **Above 3.0:** May indicate idle assets or inefficient capital deployment

### Quick Ratio (Acid Test)

- **Formula:** (Cash + Short-Term Investments + Accounts Receivable) / Current Liabilities
- **Target:** 1.0 or above
- **Excludes inventory and prepaid expenses** — tests ability to meet obligations without liquidating inventory

### Cash Ratio

- **Formula:** (Cash + Cash Equivalents) / Current Liabilities
- **Most conservative liquidity measure** — rarely used as a primary metric but useful for stress testing

---

## AR Optimization

- **Invoice promptly:** Every day between delivery and invoice issuance adds a day to DSO
- **Clear payment terms on every invoice:** Net 30 is standard; Net 15 or due-on-receipt for small or new clients
- **Early payment discounts:** 2/10 Net 30 means 2% discount if paid within 10 days. The annualized cost of not taking this discount is approximately 36.7% — (2% / 98%) x (365 / 20). Offer this when the implied return exceeds the client's cost of capital.
- **Aging analysis:** Review AR aging weekly. Buckets: current, 1-30 past due, 31-60, 61-90, 90+. Escalate collections effort as invoices age.
- **Customer credit policies:** Set credit limits for new customers; require credit applications and references above threshold amounts; suspend shipments/services when accounts exceed terms

---

## AP Management

- **Negotiate extended terms:** Moving from Net 30 to Net 45 frees 15 days of cash — material impact on CCC
- **Capture early payment discounts when cash allows:** If offered 2/10 Net 30, taking the discount earns 36.7% annualized — compare to the company's borrowing cost or opportunity cost of capital. If cost of capital is below ~15%, taking the discount is nearly always optimal.
- **Avoid late payment penalties:** Late fees damage vendor relationships and may trigger restrictive terms
- **Vendor relationship management:** Consistent on-time payment builds leverage for term negotiations and priority during supply constraints
- **Payment batching:** Consolidate payment runs to weekly or bi-weekly to reduce processing overhead while staying within terms

---

## Inventory Optimization

Primarily relevant for product businesses; skip for pure service firms.

- **ABC analysis:** Classify inventory by value and velocity. A items (top 20% by value, ~80% of spend) get tight controls; C items (bottom 50% by value) get simpler reorder rules.
- **Economic Order Quantity (EOQ):** Balances ordering costs against holding costs to determine optimal order size
- **Safety stock:** Buffer inventory to absorb demand variability and lead time uncertainty. Size based on service level target and demand standard deviation.
- **Just-in-time (JIT):** Minimize on-hand inventory by timing purchases to production/sales needs. Reduces DIO but increases supply chain risk.
- **Obsolescence review:** Quarterly review of slow-moving and obsolete inventory. Write-down or liquidate items with no realistic sale path.

---

## Line of Credit Sizing

- **Minimum facility size:** Peak CCC (in days) x average daily operating expenses = baseline borrowing need
- **Seasonal adjustment:** Size the facility to cover the peak working capital month, not the average
- **Utilization target:** Lenders prefer average utilization below 50-60% of commitment — signals adequate headroom
- **Revolver vs. term:** Revolving credit for seasonal working capital swings; term debt for permanent working capital increases from growth

---

## Cash Flow Forecasting Tie-In

Working capital changes are the primary driver of operating cash flow variance from net income. A profitable company can be cash-negative if working capital is growing (e.g., revenue growth outpaces collections).

- **13-week cash flow model:** The standard short-term forecasting tool. Week-by-week projections of cash inflows (collections on AR aging) and outflows (AP payments, payroll, rent, debt service). Cross-reference the 13-week cash flow model concept in advisory engagements.
- **Bridge from net income to cash:** Net income + depreciation/amortization - change in AR - change in inventory + change in AP = approximate operating cash flow. Working capital line items dominate the bridge for most operating businesses.

---

## Debt Covenant Interaction

Many loan agreements specify minimum current ratio or minimum working capital as financial covenants. Aggressive working capital optimization (e.g., stretching AP, drawing down cash) can push ratios toward covenant floors.

- **Always check covenant thresholds** before recommending working capital changes
- **Maintain headroom buffer** of at least 15-20% above covenant minimums
- Cross-reference `fp/debt-covenant-analysis.md` for covenant monitoring procedures and early warning systems

---

## Decision Points

- **Take the early pay discount or preserve cash?** Compare annualized discount rate to cost of capital. If 2/10 Net 30 yields 36.7% annualized and the revolver costs 8%, take the discount.
- **Extend AP terms or protect vendor relationships?** Stretching beyond agreed terms saves cash short-term but risks supply disruption, penalty fees, and loss of future term concessions.
- **Reduce inventory or maintain service levels?** Cutting inventory improves CCC but risks stockouts. Use ABC analysis and demand variability data to cut intelligently.
- **Size the line of credit for average or peak needs?** Size for peak plus a margin — paying commitment fees on unused capacity is cheaper than emergency financing.
- **When does negative working capital signal strength vs. distress?** Negative CCC (collect before you pay) is healthy for subscription and retail models. Negative working capital from inability to pay bills is distress.

---

## Accounting System Report Extraction

### Reports Needed

- **Balance Sheet (Standard):** Pull current assets (cash, AR, inventory, prepaids) and current liabilities (AP, accrued expenses, current portion of debt, deferred revenue) as of the analysis date
- **AR Aging Summary:** DSO calculation and aging bucket analysis — run as of period end to match balance sheet
- **AP Aging Summary:** DPO calculation and payment timing analysis
- **Profit and Loss (Standard):** Revenue and COGS for ratio denominators (DSO, DIO, DPO calculations)
- **Profit and Loss by Month:** Trend analysis of revenue and COGS for rolling CCC calculations

Invoke `qbo-integration:qbo-reporting` to pull these reports from the accounting system.

### Common Limitations

- Most accounting systems do not compute CCC, DSO, DIO, or DPO natively — extract balances and calculate externally
- Inventory tracking in general-purpose accounting systems is limited; businesses with complex inventory may use a separate inventory management system with different values
- AR and AP aging reports may not match balance sheet if transactions are mis-dated or unapplied payments exist — reconcile before calculating ratios
- Deferred revenue classification varies by accounting system setup; confirm it is in current liabilities for working capital calculations

### Recommended Workflow

1. Run Balance Sheet and P&L as of the analysis period end
2. Run AR Aging Summary and AP Aging Summary as of the same date
3. Extract totals into a working capital analysis template
4. Calculate CCC components (DSO, DIO, DPO) and ratios (current, quick, cash)
5. Compare to prior periods and industry benchmarks
6. Identify the largest improvement opportunities (usually AR or inventory)
7. Present findings with dollar impact estimates (e.g., reducing DSO by 10 days frees $X of cash)

---

## References

- Sagner, James. *Essentials of Working Capital Management.* Wiley (2010) — comprehensive practitioner reference
- FASB ASC 210-10 (Balance Sheet — Overall): Classification of current assets and current liabilities
- FASB ASC 310-10 (Receivables — Overall): Recognition and measurement of trade receivables
- CFI — Cash Conversion Cycle: corporatefinanceinstitute.com/resources/accounting/cash-conversion-cycle/
- CFI — Working Capital: corporatefinanceinstitute.com/resources/accounting/working-capital-formula/
- AICPA — Management Advisory Services: Guidance on advisory engagements including working capital analysis
