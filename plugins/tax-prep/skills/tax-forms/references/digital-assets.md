---
authority_level: primary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: federal
---

# Cryptocurrency and Digital Asset Tax Compliance — C-Corporations

**Purpose:** Tax classification, reporting requirements, and accounting treatment for digital assets held or transacted by C-corporations.

**Tier:** 1 (year-indexed — rules are rapidly evolving; verify current-year guidance before applying)

**Skills that reference this:** `tx-digital-assets`, `tx-compliance-checklist`, `bk-transaction-classification`

---

## IRS Classification

Digital assets (cryptocurrency, NFTs, stablecoins, tokenized securities) are treated as **property** under Notice 2014-21, NOT as currency. This classification drives all downstream tax consequences:

- Every disposition is a realization event
- Gain or loss is computed as proceeds minus adjusted basis
- Character (ordinary vs. capital) depends on the taxpayer's use of the asset
- For C-corporations, the distinction between short-term and long-term capital gains is irrelevant — corporations do not receive a preferential capital gains rate. All income is taxed at the flat 21% rate (post-TCJA)

---

## Taxable Events

### Events That Trigger Gain/Loss Recognition

- **Sale for fiat currency** — gain or loss = fiat received minus adjusted basis of the digital asset sold
- **Exchange for another digital asset** — treated as a sale of the asset given and a purchase of the asset received. Gain or loss recognized on the asset given up at its FMV at the time of exchange
- **Payment for goods or services** — treated as a sale at FMV. The business deducts the expense (if deductible) at the FMV of the crypto transferred
- **Receipt as payment for goods/services** — ordinary income at FMV on date of receipt. Basis in the received asset = FMV at recognition
- **Mining rewards** — ordinary income at FMV when the taxpayer gains dominion and control (typically when tokens are credited to the wallet). Cost of mining operations (electricity, equipment depreciation) are deductible business expenses
- **Staking rewards** — IRS position: ordinary income at FMV when received. This remains contested (see *Jarrett v. United States*, where the government conceded on refund but did not issue formal guidance). Conservative approach: report as income when received
- **Hard forks with airdrop** — if the fork results in a new asset the taxpayer can access and control, ordinary income at FMV of the new asset when dominion and control is established. Rev. Rul. 2019-24
- **Airdrops** — ordinary income at FMV on date of receipt, provided the taxpayer has dominion and control

### Events That Are NOT Taxable

- Purchasing digital assets with fiat currency (no realization event)
- Transferring digital assets between the taxpayer's own wallets (no change in ownership)
- Gifting digital assets (but gift tax rules apply; donor's basis carries over to donee)
- Soft forks that do not result in a new asset

---

## Cost Basis Methods

- **Specific identification** — preferred method; requires lot-level tracking (date acquired, amount, cost basis per unit). The taxpayer must be able to identify the specific units disposed of. Requires contemporaneous records — wallet addresses, transaction hashes, exchange records
- **FIFO (First In, First Out)** — default method if specific identification is not established. Oldest units are deemed sold first
- **HIFO (Highest In, First Out)** — a variant of specific identification that selects the highest-cost lots first, minimizing taxable gain. Requires the same record-keeping as specific identification
- **Average cost** — NOT available for property (only permitted for mutual fund shares and certain other regulated investment company shares under Reg. 1.1012-1). Do not use average cost for digital assets

### Holding Period

- **Short-term** — held less than one year (one year or less for property)
- **Long-term** — held more than one year
- For C-corporations this distinction affects only the character (capital gain/loss vs. ordinary) and any capital loss limitation analysis — not the tax rate

---

## Wash Sale Rule

IRC Section 1091 (wash sale) currently does **NOT** apply to digital assets. Digital assets are property, not stock or securities as defined in Section 1091. This means a corporation can sell crypto at a loss and immediately repurchase the same asset, recognizing the loss without deferral.

**Pending legislation** — multiple bills (Lummis-Gillibrand Responsible Financial Innovation Act, Digital Asset Anti-Money Laundering Act) have proposed extending the wash sale rule to digital assets. Monitor each legislative session for changes. If enacted, the effective date may be prospective or retroactive to a specific date.

---

## Reporting Requirements

### Form 1120 Digital Asset Question

Starting with tax year 2023, Form 1120 includes a mandatory yes/no question: *"At any time during the tax year, did the corporation receive, sell, send, exchange, or otherwise acquire any digital assets?"*

Answer **Yes** if the corporation engaged in ANY of the taxable or non-taxable events listed above (including mere receipt or transfer between wallets). The only exception: holding digital assets in a custodial account without any transactions during the year does not require a "Yes" if the corporation took no actions.

### Form 8949 and Schedule D

- Report each digital asset disposition on Form 8949 (Sales and Other Dispositions of Capital Assets)
- Aggregate short-term and long-term totals flow to Schedule D (Form 1120)
- If the number of transactions is large, attach a statement with the same columns as Form 8949 and enter "See attached" on the form

### Form 1099-DA (New — TY2025+)

- Brokers and exchanges must report digital asset sales on Form 1099-DA, similar to Form 1099-B for securities
- Phased implementation: custodial brokers first (TY2025), decentralized and non-custodial platforms later
- Corporations should reconcile 1099-DA data against internal records — discrepancies must be resolved before filing

### Information Reporting for Payments

- Payments of $600+ in digital assets for services must be reported on Form 1099-NEC (or 1099-MISC for rent/other payments)
- The payment amount is the FMV of the digital assets on the date of payment

---

## DeFi Considerations

IRS guidance on decentralized finance (DeFi) activities is limited. Conservative treatment:

- **Liquidity provision** — depositing tokens into a liquidity pool may or may not be a taxable exchange depending on whether the depositor receives a new token (LP token). If an LP token is received, treat as an exchange (taxable). If the deposit is a loan-like arrangement, basis may carry over. No definitive IRS guidance
- **Yield farming** — rewards earned from yield farming are ordinary income at FMV when received, consistent with staking/mining treatment
- **Governance tokens** — if received for free (airdrop), ordinary income at FMV. If purchased, basis = purchase price. Disposition triggers capital gain/loss
- **Wrapped tokens** — wrapping (e.g., ETH to WETH) is likely a taxable exchange under current property treatment rules, though the gain/loss is typically minimal. Some practitioners argue it is analogous to a non-taxable exchange of identical property

---

## Record-Keeping Requirements

Maintain transaction-level records including:

- Date and time of each transaction (UTC preferred for consistency)
- Type of transaction (purchase, sale, exchange, receipt, mining reward, staking reward)
- Amount of digital asset (quantity and denomination)
- Fair market value at the time of the transaction (source the price from CoinMarketCap, CoinGecko, or the exchange's reported price)
- Cost basis of the asset disposed (if applicable)
- Wallet addresses involved (sending and receiving)
- Transaction hash (blockchain confirmation)
- Exchange or platform account statements (download annually and archive — exchanges shut down)
- Counterparty information (if known)

---

## Accounting System Integration Notes

- Create a separate **Other Current Asset** account for each significant crypto holding (e.g., "Digital Assets — BTC", "Digital Assets — ETH")
- Record purchases at cost (debit Digital Assets, credit bank/cash)
- Record sales with gain/loss: debit bank for proceeds, credit Digital Assets for basis, debit/credit Gain/Loss on Digital Assets for the difference
- Mining/staking income: debit Digital Assets at FMV, credit "Digital Asset Mining Income" or "Digital Asset Staking Income" (Other Income or Revenue, depending on whether mining is a core business activity)
- Use memo fields to record transaction hashes and lot identification for audit trail
- At year-end, reconcile digital asset account balances to wallet balances and exchange statements
- Most accounting systems do not natively support lot-level tracking for non-security assets — maintain a supplemental spreadsheet or use specialized crypto tax software and reconcile totals to the GL

For platform-specific account setup and journal entry creation, invoke the appropriate `qbo-integration` skill.

---

## Year-by-Year Regulatory Timeline

- **2014** — Notice 2014-21: IRS classifies virtual currency as property. Establishes basic tax treatment framework
- **2014-2018** — Form 8949/Schedule D reporting for dispositions. No specific digital asset question on returns
- **2019** — Rev. Rul. 2019-24: hard forks and airdrops guidance. Form 1040 adds digital asset question (individual returns)
- **2020** — IRS sends 10,000+ compliance letters (Letter 6173, 6174, 6174-A) to crypto holders identified through exchange records
- **2021** — Infrastructure Investment and Jobs Act expands "broker" definition to include digital asset platforms (effective 2024+). Section 6050I reporting for cash transactions extended to digital assets (>$10K)
- **2022** — Continued enforcement focus. IRS Criminal Investigation prioritizes crypto tax fraud
- **2023** — Form 1120 adds the digital asset yes/no question. Proposed regulations on broker reporting (REG-122793-19)
- **2024** — Final broker reporting regulations issued (T.D. 9989). Custodial broker reporting begins for TY2025
- **2025** — Form 1099-DA effective for custodial brokers. Corporations receiving 1099-DAs must reconcile against internal records. FIFO becomes default for broker-reported basis if taxpayer does not make a specific identification election with the broker
- **Future** — Watch for: wash sale extension to digital assets, staking income clarification, DeFi reporting requirements, OECD Crypto-Asset Reporting Framework (CARF) implementation

---

## Decision Points

- **Mining as trade or business vs. investment activity** — if mining is a core business activity (significant equipment, dedicated operations), expenses are deductible under Section 162. If incidental, expenses may be limited. Document the business purpose and operational scale
- **Specific identification vs. FIFO** — use specific identification when the corporation has good lot-level records and wants to optimize gain/loss recognition. Default to FIFO if records are incomplete — switching methods mid-year creates audit risk
- **When to engage crypto tax software** — any corporation with more than 50 transactions per year or activity across multiple wallets/exchanges should use dedicated crypto tax software for basis tracking and Form 8949 generation
- **Valuation for illiquid tokens** — tokens without active market pricing require a reasonable fair value determination. Document the methodology (comparable sales, last known price, zero if truly worthless). Consider a Section 165 worthlessness deduction if the token has become valueless

---

## Authoritative References

- Notice 2014-21 — IRS guidance on virtual currency as property
- Rev. Rul. 2019-24 — Tax treatment of hard forks and airdrops
- IRC Section 1001 — Determination of gain or loss on disposition
- IRC Section 1091 — Wash sale rule (currently inapplicable to digital assets)
- IRC Section 6045 — Broker reporting requirements (as amended by IIJA 2021)
- IRC Section 6050I — Cash transaction reporting (extended to digital assets)
- T.D. 9989 — Final regulations on digital asset broker reporting (2024)
- REG-122793-19 — Proposed regulations on digital asset broker reporting
- Form 8949 Instructions — Reporting sales and dispositions of capital assets
- OECD Crypto-Asset Reporting Framework (CARF) — International reporting standard
