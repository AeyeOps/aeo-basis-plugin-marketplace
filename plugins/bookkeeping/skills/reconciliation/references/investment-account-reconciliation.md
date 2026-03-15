---
name: investment-account-reconciliation
description: |
  How to reconcile brokerage investment accounts for C-corps. Teaches how to read
  brokerage statements, distinguish external cash flows from internal activity,
  and avoid the most common error: assuming Schedule L balance increases imply cash deposits.
---

# Investment Account Reconciliation

## Purpose

The brokerage statement is the authoritative record of what happened in the investment
account. It tells a complete, self-contained story each year. The reconciliation must
read that story directly rather than inferring cash movements from Schedule L balance
changes.

**The core error this prevents:** When Schedule L shows an investment increasing
from $X to $Y, the instinct is to ask "how much cash moved to explain the difference?"
The answer is usually: very little or none. Stock portfolios grow through market
appreciation, not cash deposits. A $60K increase on Schedule L might represent $0 in
actual deposits — just stocks going up in value.

## The Conceptual Model

A brokerage account has two types of activity:

**External cash flows** — actual money moving between the bank account and the brokerage:
- Wire transfers in (bank → brokerage)
- Check deposits
- Wire transfers out (brokerage → bank)
- ACH transfers

These are the ONLY transactions that affect the company's cash position and require
journal entries moving money between GL accounts.

**Internal activity** — everything happening inside the brokerage:
- Buying stocks (cash → shares, within the brokerage)
- Selling stocks (shares → cash, within the brokerage)
- Receiving dividends (credited to brokerage cash balance)
- Interest earned on cash balance
- Foreign tax withheld
- Market appreciation/depreciation of held positions

None of these move money in or out of the brokerage. A stock sale doesn't send money
to the bank — it converts shares to cash *within the brokerage*. A dividend doesn't
arrive from outside — it's credited to the brokerage's internal cash balance.

**The reconciliation must isolate external flows from internal activity.** Only external
flows generate cash-movement journal entries.

## Reading Brokerage Statements

Every broker formats statements differently, but they fall into two broad patterns:

### Pattern A: Pre-Separated Summary

Some brokers provide an account summary that explicitly breaks out deposits, withdrawals,
dividends/interest, and market appreciation as separate lines. This is the easiest format
to work with — the deposit total is already isolated for you.

Monthly statements with year-to-date (YTD) columns are especially useful. The December
statement YTD column gives full-year totals in a single line — no transaction scanning
needed.

### Pattern B: Combined Activity Summary

Other brokers provide an activity summary that lumps external deposits together with
internal activity (sale proceeds, purchases) under generic labels like "Other Credits"
and "Other Debits." You CANNOT determine actual deposits from these summary lines alone.

**The solution:** Look past the summary page to the transaction detail — a chronological
log with a running balance. Each transaction has a description or keyword indicating
its type. External cash flow keywords are distinctive (wire transfer, check deposit,
ACH) and separate from internal keywords (bought, sold, dividend, interest, foreign tax).

**Extraction method:** Read every transaction line in the detail section. Filter for
wire/deposit/ACH keywords. Sum those amounts. That's the external deposits number. It
may be zero — many years have no external deposits at all.

### General Principle

Find the document section that shows cash movements with the outside world. It may be
called "Deposits," "Wire Transfers," "Journal Entries," "Cash Activity," or similar.
The key distinction is always: did money cross the boundary between the bank account
and the brokerage, or did it just move around inside the brokerage?

### Self-Reconciliation

Verify the running balance or cash flow equation ties:
`opening_balance + all_transaction_amounts = closing_balance`

If it doesn't tie, transactions are missing or OCR errors exist. The PDF is the
authoritative source, not OCR metadata files.

## How to Think About Schedule L

Schedule L Line 6 (Other Current Assets / Investments) carries whatever number the
CPA chose to report. This might be:

- **Market value** — matches the brokerage EOY account value
- **Cost basis** — the original purchase price of positions still held
- **Some hybrid** — partial positions, or a different methodology entirely

The CPA may change methodology between years. When they do, the Schedule L delta
includes a **valuation reclassification** that looks like a huge cash movement but isn't.

**Example pattern:** Year 1 Schedule L carries $15K (cost basis or partial).
Year 2 Schedule L carries $75K (switched to market value). The brokerage statement
shows zero deposits in Year 2. The $60K "increase" is a reporting correction, not
cash movement.

**The correct approach:**
1. Determine external deposits/withdrawals from the brokerage statement (the actual number)
2. Determine portfolio market value from the brokerage statement (opening and closing)
3. Compare Schedule L to brokerage market value
4. If they match → Schedule L is at market value, straightforward
5. If they don't match → flag as valuation methodology difference, investigate what the CPA was reporting
6. Never work backwards from Schedule L to infer deposits

## Reconciliation Workflow

When brokerage statements are available:

1. **Identify the statement format** — does it pre-separate deposits (Pattern A) or combine them (Pattern B)?
2. **Find the external cash flow section** — summary line for Pattern A, transaction detail for Pattern B
3. **Extract and classify transactions** — isolate wires/deposits from buys/sells/dividends
4. **Record external deposits and withdrawals** — value may be zero. Zero is valid evidence, not a gap.
5. **Self-reconcile** — verify the running balance or cash flow equation ties
6. **Extract supporting data** — EOY account value, dividend totals, interest, foreign tax paid, sale proceeds, cost basis if available

**If the activity summary shows large credits but transaction detail shows zero deposits:**
That's normal — the credits include sale proceeds. Not a discrepancy, not a gap.

## Determining Total Deposits

**Source priority:**
1. External deposits identified directly from the brokerage statement — best
2. Sum of identified wire/deposit entries from corresponding bank statements — acceptable
3. Derived from Schedule L arithmetic — **NEVER acceptable**

The formula `deposits = EOY_balance - BOY_balance - gains - dividends - unrealized_change`
produces fictional numbers because:
- Schedule L may not be at market value
- The CPA may have changed valuation methodology
- AOCI reported on the return may not match actual market movement
- The formula assumes all unexplained balance change = cash in

**When deposits = $0 (evidence-based):** Record total deposits as 0. Do not manufacture
a deposit to explain Schedule L growth. The growth is market appreciation.

**When brokerage data is unavailable:** Flag as a blocking gap. Do not derive.

**Schedule L comparison:** After determining the investment account balance from brokerage
evidence, compare to Schedule L. Flag discrepancies as valuation differences for
investigation, not as missing cash flows.

## Journal Entries

**External cash transfers (when deposit total is evidence-based and > 0):**
```
DR Brokerage Investments     {deposit amount}
  CR Checking                {deposit amount}
```
Record gross deposits and gross withdrawals as separate entries.

**If deposits = $0 (evidence-based):** No cash transfer JE. Do not create one.

**Dividends, capital gains, foreign tax, interest:** Standard income/expense JEs
within the brokerage accounts. These don't touch the operating cash account.

**Market value adjustment:** DR/CR Investments and AOCI for unrealized gain/loss changes.

**Valuation reclassification (when CPA changed methodology between years):**
This is a balance sheet correction, not a cash transaction. The JE adjusts the
investment balance and equity without touching cash.

## Red Flags

- **Schedule L investment grew but brokerage shows zero deposits:** Normal. Market appreciation. Not a gap.
- **Activity summary credits are large:** Normal. Includes sale proceeds. Check transaction detail for actual deposits.
- **Schedule L BOY ≠ prior year brokerage EOY market value:** Valuation methodology difference. Investigate, don't plug.
- **1099-DIV shows $0 but brokerage shows dividends:** Check whether the 1099-DIV is the "reported to IRS" version vs the broker's income summary (not reported to IRS). Corporate accounts may have dividends on the income summary statement instead.
- **Only annual account summary available (no monthly statements):** Cannot determine deposits from a tax-document-only summary. Request monthly statements or transaction detail.

## References

- UCC Article 8 — Investment Securities
- IRS Publication 550 — Investment Income and Expenses
- ASC 320 — Investments: Debt and Equity Securities
