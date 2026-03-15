# Debt Covenant Analysis

**Purpose:** Procedures for analyzing, calculating, and monitoring debt covenant compliance for borrower clients.

**Tier**: 3 (evergreen, no year indexing needed)

**Skills**: `fp-debt-analysis`, `fp-covenant-monitoring`, `lender-reporting`

---

## Overview

Debt covenants are legally binding conditions in loan agreements requiring borrowers to meet financial or operational standards. CPA firms serve borrower clients by extracting financials, calculating covenant ratios, preparing compliance certificates, and providing early warning when metrics approach breach thresholds.

---

## Covenant Types

### Affirmative Covenants (Positive Obligations)

Actions the borrower must take: maintain minimum cash/working capital, submit financial statements by deadlines, carry specified insurance, provide compliance certificates, maintain corporate existence and good standing, pay taxes when due, permit lender inspection of books, notify lender of material adverse changes or litigation.

### Negative Covenants (Restrictions)

Actions the borrower must not take without lender consent: incur additional debt above threshold, pay dividends/distributions exceeding cap, sell major assets, enter mergers/acquisitions, make capex above annual limits, change business nature or management, create liens on collateral, make related-party loans, change accounting methods or fiscal year-end.

### Financial Covenants (Quantitative Metrics)

Ratio-based thresholds tested periodically (usually quarterly): DSCR, leverage/debt-to-EBITDA, current ratio, tangible net worth minimum, fixed charge coverage, interest coverage, maximum capital expenditures.

---

## Common Financial Covenants — Calculations

### Debt Service Coverage Ratio (DSCR)

- **Formula:** Net Operating Income / Total Debt Service
- **NOI:** Gross revenue minus operating expenses (excluding interest, depreciation, amortization, and taxes); alternatively EBITDA depending on agreement definition
- **Total Debt Service:** All principal and interest payments due in the measurement period
- **Typical minimum:** 1.20x to 1.50x
- **DSCR = 1.0** means break-even; no margin for error
- **DSCR < 1.0** means operating income cannot cover debt payments
- **Variations:** Some agreements add back non-cash charges (depreciation, amortization) to NOI; some exclude unfunded capex; real estate deals may use gross rental income instead of NOI
- **Real estate DSCR:** May include PITIA (Principal, Interest, Taxes, Insurance, Association dues) in the denominator

### Leverage Ratio (Debt-to-EBITDA)

- **Formula:** Total Funded Debt / Trailing 12-Month EBITDA
- **Typical maximum:** 3.0x to 5.0x depending on industry and risk
- **Net variation:** (Total Debt - Unrestricted Cash) / EBITDA
- **Watch for:** Agreement-specific EBITDA adjustments (add-backs for one-time charges, stock compensation, management fees)

### Current Ratio

- **Formula:** Current Assets / Current Liabilities
- **Typical minimum:** 1.0x to 1.5x
- **Measures:** Short-term liquidity and ability to meet near-term obligations
- **Watch for:** Classification of current portion of long-term debt; revolving credit facility treatment; exclusion of certain current assets (restricted cash)

### Tangible Net Worth

- **Formula:** Total Equity - Intangible Assets (goodwill, patents, trademarks, organizational costs)
- **Typical minimum:** Fixed dollar floor, or a ratchet (prior period TNW + percentage of net income, never decreasing)
- **Watch for:** Treatment of deferred tax assets, capitalized software costs, and right-of-use assets

### Fixed Charge Coverage Ratio (FCCR)

- **Formula:** (EBITDA - Unfinanced CapEx - Cash Taxes - Distributions) / (Interest + Scheduled Principal + Lease Payments)
- **Typical minimum:** 1.10x to 1.25x
- **Broader than DSCR:** Captures lease obligations, taxes, and distributions in addition to debt service
- **Watch for:** Whether the agreement defines "fixed charges" to include or exclude specific items

### Interest Coverage Ratio

- **Formula:** EBIT / Interest Expense
- **Typical minimum:** 2.0x to 3.0x
- **Simpler than FCCR:** Only measures ability to cover interest, not principal

---

## Measurement Periods

- **Trailing twelve months (TTM):** Most common for income-based ratios; rolling four quarters
- **Year-to-date annualized:** Less common; can distort seasonal businesses
- **Point-in-time:** Balance sheet ratios (current ratio, TNW) measured at period end
- **Testing frequency:** Quarterly is standard; monthly for higher-risk credits
- **Reporting deadline:** Typically 30-45 days after quarter-end for internal financials; 90-120 days for audited annual statements

---

## Compliance Monitoring Procedures

### Quarterly Compliance Cycle

1. **Extract financials** from the accounting system or GL as of measurement date
2. **Map accounts** to covenant definitions (the agreement's definition of EBITDA, debt, etc. controls — not GAAP defaults)
3. **Calculate each ratio** using the exact formulas specified in the credit agreement
4. **Compare results** to required thresholds with a cushion analysis (how far from breach)
5. **Prepare compliance certificate** in the lender's required format
6. **Officer review and signature** before submission
7. **Submit to lender** within the required reporting window

### Early Warning System

Track covenant headroom as a percentage. Flag when any ratio falls within the warning zone:
- **Green:** Ratio exceeds covenant by more than 20% of the threshold
- **Yellow:** Ratio within 10-20% of the threshold — proactive discussion with client
- **Red:** Ratio within 10% of threshold or below — immediate client notification, explore remediation

Trend analysis: plot each ratio quarterly to identify deterioration patterns before they become breaches.

### Common Triggers to Watch

- Revenue decline or seasonality compressing TTM income
- Large capital expenditures reducing cash / increasing debt
- Dividend or distribution payments reducing equity
- Reclassification of debt from long-term to current
- Asset write-downs reducing tangible net worth
- Lease obligations changing under ASC 842

---

## Covenant Breach — Consequences and Remedies

### Consequences of Breach

- **Technical default:** Lender may accelerate the loan (demand immediate full repayment)
- **Cross-default:** Breach in one facility can trigger defaults on all other credit agreements
- **Default interest rate:** Typically +2% to +5% above the contract rate
- **Restricted access:** Revolving lines frozen or reduced; additional collateral demands
- **Enhanced reporting:** Monthly financials, more frequent compliance certificates

### Cure Periods

- **Equity cure:** Some agreements allow shareholders to inject equity to bring ratios back into compliance; typically limited to 2-3 times over the loan term
- **Cure period:** Many agreements provide 15-30 days to cure financial covenant breaches after notice
- **No cure for negative covenants:** Violations of negative covenants (unauthorized asset sales, excess distributions) generally have no cure right

### Amendment and Waiver Process

- **Waiver:** One-time lender forgiveness of a specific breach; does not change the agreement terms going forward; typically documented in a formal waiver letter
- **Amendment:** Permanent change to covenant levels or definitions; requires negotiation and often a fee (0.10% to 0.50% of commitment)
- **Forbearance:** Lender agrees not to exercise default remedies for a defined period while borrower develops a remediation plan
- **Amendment fees and legal costs** are borne by the borrower

---

## Accounting System Integration Notes

### Report Extraction for Covenant Calculations

Delegate report extraction to `qbo-integration:qbo-reporting` (or the equivalent platform integration skill). The key reports needed:

- **Balance Sheet (Standard):** Source for current assets, current liabilities, total equity, intangible assets, total debt — use for current ratio, TNW, leverage
- **Profit and Loss (Standard):** Source for revenue, operating expenses, EBITDA build-up, interest expense — use for DSCR, FCCR, interest coverage
- **Profit and Loss by Month:** Use for TTM calculations — sum trailing 12 months of data
- **Statement of Cash Flows:** Cross-check for actual debt service payments, capex, and distributions
- **Balance Sheet by Month:** Track point-in-time ratios over time for trend analysis

### Common Accounting System Limitations

- Most accounting systems do not natively calculate covenant ratios — extraction and calculation is manual or via external tools
- EBITDA is not a standard report line; must be derived (Net Income + Interest + Taxes + Depreciation + Amortization)
- Debt schedules may not distinguish current vs. long-term portions accurately if not maintained
- Distributions may be recorded in equity or as draws depending on entity setup; verify classification

### Recommended Workflow

1. Run Balance Sheet and P&L as of the covenant measurement date in the accounting system
2. Export to Excel or a covenant tracking spreadsheet
3. Map account balances to agreement-defined line items
4. Calculate ratios using agreement-specific formulas (not textbook formulas)
5. Document any adjustments or add-backs with supporting detail

---

## Decision Points

- **Which EBITDA definition controls?** Always use the credit agreement's definition, not GAAP or textbook EBITDA. Agreements often permit add-backs for non-recurring charges, restructuring costs, or stock compensation.
- **Reclassification impact?** ASC 842 lease capitalization and debt reclassifications can affect both leverage and liquidity ratios. Confirm whether the agreement references "frozen GAAP" (pre-ASC 842 treatment) or current GAAP.
- **Equity cure available?** Check the agreement before advising a client to inject capital. Cure rights are typically limited in number and amount.
- **Pro forma adjustments?** Some agreements allow pro forma treatment of acquisitions or dispositions. Verify whether the lender requires adjustments or uses actual results.
- **Multiple facilities?** Clients with multiple lenders may have different covenant packages. Each must be tracked independently. Cross-default provisions mean a breach in one facility can cascade.

---

## References

- Loan/credit agreement: The governing document; always start here for definitions and thresholds
- FASB ASC 470-10 (Debt — Overall): Classification and disclosure of debt covenants in financial statements
- FASB ASC 842 (Leases): Impact on leverage and fixed charge calculations
- Uniform Commercial Code (UCC): Governs security interests and collateral
- CFI — Debt Covenants: corporatefinanceinstitute.com/resources/commercial-lending/debt-covenants/
- CFI — Financial Covenants: corporatefinanceinstitute.com/resources/accounting/financial-covenants/
