---
authority_level: secondary
effective_from: evergreen
last_verified: 2026-03-18
jurisdiction: N/A
---

# QBO API Account Management — Knowledge Base

Research date: 2026-02-24. Sources cited inline throughout.

---

## 1. Account Entity Overview

The `Account` object represents a component of the Chart of Accounts. It is used to record monetary amounts allocated against a specific use. All transaction posting ultimately references an Account.

The QBO API does **not** support permanent delete of accounts. "Delete" is achieved by setting `Active = false` on the account object, which hides it from the UI but preserves all transaction history. The record remains queryable via API with `WHERE Active IN (true, false)`.

Source: QBO Account API reference (all-entities), per the python-quickbooks SDK Account class

---

## 2. Required Fields for Account Creation

Minimum required fields for a `POST /v3/company/{realmId}/account` create operation:

- `Name` — Account display name. Max 100 characters. No colons allowed. Must be unique within the same level (see uniqueness rules below).
- `AccountType` — One of the valid AccountType enum values (see Section 4). Required for all product types.

`AccountSubType` is technically optional at the API level but is strongly recommended — if omitted, QBO assigns a default subtype based on AccountType. When provided, it must be a valid subtype for the given AccountType.

Source: [QBO Account API reference — most-commonly-used](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/account), [Apideck QBO exploration](https://www.apideck.com/blog/exploring-the-quickbooks-online-accounting-api)

---

## 3. All Account Entity Fields (python-quickbooks Account class)

```python
class Account(QuickbooksManagedObject, QuickbooksTransactionEntity):

    # System-managed (readonly once created or computed)
    Id                              # str — assigned by QBO on create
    SyncToken                       # str — version counter, required for updates
    MetaData                        # CreateTime, LastUpdatedTime
    FullyQualifiedName              # str — computed: "Parent:Child" hierarchy (readonly)
    CurrentBalance                  # Decimal — readonly, sum for this account
    CurrentBalanceWithSubAccounts   # Decimal — readonly, includes sub-accounts

    # Writable fields
    Name                            # str — required. Max 100 chars. No colons.
    AccountType                     # str — required. See valid values below.
    AccountSubType                  # str — optional but recommended.
    AcctNum                         # str — user-defined account number. Max 100 chars (QBO).
    Description                     # str — description of account purpose.
    Active                          # bool — True (default) = active; False = inactive/hidden.
    Classification                  # str — Asset, Liability, Equity, Revenue, Expense (computed from AccountType).
    SubAccount                      # bool — True if this is a child of another account.
    ParentRef                       # Ref — Id of parent account if SubAccount = True.
    CurrencyRef                     # Ref — currency; defaults to company home currency.
    TaxCodeRef                      # Ref — for tax tracking.
```

Source: per the python-quickbooks SDK Account class, AccountSubTypeEnum Java SDK documentation

---

## 4. Valid AccountType Values

The AccountType determines the financial classification. QBO (Online) does **not** support the `NonPosting` type — that is QBW (Desktop) only.

| AccountType | Classification |
|---|---|
| Bank | Asset |
| AccountsReceivable | Asset |
| OtherCurrentAsset | Asset |
| FixedAsset | Asset |
| OtherAsset | Asset |
| AccountsPayable | Liability |
| CreditCard | Liability |
| OtherCurrentLiability | Liability |
| LongTermLiability | Liability |
| Equity | Equity |
| Income | Revenue |
| CostofGoodsSold | Expense |
| Expense | Expense |
| OtherIncome | Revenue |
| OtherExpense | Expense |

Note: String casing in API requests must match exactly (e.g., `"CostofGoodsSold"` not `"CostOfGoodsSold"`).

Source: [AccountTypeEnum .NET SDK documentation](https://static.developer.intuit.com/sdkdocs/qbv3doc/ippdotnetdevkitv3/html/fe5d46d7-7629-9de7-1011-2b2c2156cc6b.htm)

---

## 5. AccountType to AccountSubType Mapping (Complete)

This is the full mapping. API values use CamelCase with no spaces or special characters. The UI display names differ from API names — always use the API names listed here.

### Asset Classification

**AccountType: Bank**
- CashAndCashEquivalents
- CashOnHand
- Checking
- MoneyMarket
- OtherEarMarkedBankAccounts
- RentsHeldInTrust
- Savings
- TrustAccounts

**AccountType: OtherCurrentAsset**
- AllowanceForBadDebts
- AssetsAvailableForSale
- BalWithGovtAuthorities
- CalledUpShareCapitalNotPaid
- DevelopmentCosts
- EmployeeCashAdvances
- ExpenditureAuthorisationsAndLettersOfCredit
- GlobalTaxDeferred
- GlobalTaxRefund
- InternalTransfers
- Inventory
- Investment_MortgageRealEstateLoans
- Investment_Other
- Investment_TaxExemptSecurities
- Investment_USGovernmentObligations
- LoansToOfficers
- LoansToOthers
- LoansToStockholders
- OtherConsumables
- OtherCurrentAssets
- PrepaidExpenses
- ProvisionsCurrentAssets
- Retainage
- ShortTermInvestmentsInRelatedParties
- ShortTermLoansAndAdvancesToRelatedParties
- TradeAndOtherReceivables
- UndepositedFunds

**AccountType: FixedAsset**
- AccumulatedAmortization
- AccumulatedDepletion
- AccumulatedDepreciation
- AssetsInCourseOfConstruction
- Buildings
- CapitalWip
- CumulativeDepreciationOnIntangibleAssets
- DepletableAssets
- FixedAssetComputers
- FixedAssetCopiers
- FixedAssetFurniture
- FixedAssetPhone
- FixedAssetPhotoVideo
- FixedAssetSoftware
- FixedAssetOtherToolsEquipment
- FurnitureAndFixtures
- IntangibleAssets
- IntangibleAssetsUnderDevelopment
- Land
- LandAsset
- LeaseholdImprovements
- MachineryAndEquipment
- NonCurrentAssets
- OtherFixedAssets
- ParticipatingInterests
- ProvisionsFixedAssets
- Vehicles

**AccountType: OtherAsset**
- AccumulatedAmortizationOfOtherAssets
- AssetsHeldForSale
- AvailableForSaleFinancialAssets
- DeferredTax
- Goodwill
- Investments
- LeaseBuyout
- Licenses
- LongTermInvestments
- LongTermLoansAndAdvancesToRelatedParties
- OrganizationalCosts
- OtherIntangibleAssets
- OtherLongTermAssets
- OtherLongTermInvestments
- OtherLongTermLoansAndAdvances
- PrepaymentsAndAccruedIncome
- ProvisionsNonCurrentAssets
- SecurityDeposits

**AccountType: AccountsReceivable**
- AccountsReceivable

### Liability Classification

**AccountType: AccountsPayable**
- AccountsPayable
- OutstandingDuesMicroSmallEnterprise
- OutstandingDuesOtherThanMicroSmallEnterprise

**AccountType: CreditCard**
- CreditCard

**AccountType: OtherCurrentLiability**
- AccruedLiabilities
- CurrentLiabilities
- CurrentPortionEmployeeBenefitsObligations
- CurrentPortionOfObligationsUnderFinanceLeases
- CurrentTaxLiability
- DirectDepositPayable
- DividendsPayable
- DutiesAndTaxes
- FederalIncomeTaxPayable
- InsurancePayable
- InterestPayables
- LineOfCredit
- LoanPayable
- GlobalTaxPayable
- GlobalTaxSuspense
- OtherCurrentLiabilities
- PayrollClearing
- PayrollTaxPayable
- PrepaidExpensesPayable
- ProvisionForWarrantyObligations
- ProvisionsCurrentLiabilities
- RentsInTrustLiability
- SalesTaxPayable
- ShortTermBorrowings
- SocialSecurityAgencies
- StaffAndRelatedLiabilityAccounts
- StateLocalIncomeTaxPayable
- SundryDebtorsAndCreditors
- TradeAndOtherPayables
- TrustAccountsLiabilities

**AccountType: LongTermLiability**
- AccrualsAndDeferredIncome
- AccruedLongLermLiabilities
- AccruedVacationPayable
- BankLoans
- DebtsRelatedToParticipatingInterests
- DeferredTaxLiabilities
- GovernmentAndOtherPublicAuthorities
- GroupAndAssociates
- LiabilitiesRelatedToAssetsHeldForSale
- LongTermBorrowings
- LongTermDebit
- LongTermEmployeeBenefitObligationsNotesPayable
- NotesPayable
- ObligationsUnderFinanceLeases
- OtherLongTermLiabilities
- OtherLongTermProvisions
- ProvisionForLiabilities
- ProvisionsNonCurrentLiabilities
- ShareholderNotesPayable
- StaffAndRelatedLongTermLiabilityAccounts

### Equity Classification

**AccountType: Equity**
- AccumulatedAdjustment
- AccumulatedOtherComprehensiveIncome
- CalledUpShareCapital
- CapitalReserves
- CommonStock
- DividendDisbursed
- EquityInEarningsOfSubsiduaries
- EstimatedTaxes
- Funds
- Healthcare
- InvestmentGrants
- MoneyReceivedAgainstShareWarrants
- OpeningBalanceEquity
- OtherFreeReserves
- OwnersEquity
- PaidInCapitalOrSurplus
- PartnerContributions
- PartnerDistributions
- PartnersEquity
- PreferredStock
- PersonalExpense
- PersonalIncome
- RetainedEarnings
- ShareApplicationMoneyPendingAllotment
- ShareCapital
- TreasuryStock

### Revenue Classification

**AccountType: Income**
- CashReceiptIncome
- DiscountsRefundsGiven
- NonProfitIncome
- OperatingGrants
- OtherCurrentOperatingIncome
- OtherPrimaryIncome
- OwnWorkCapitalized
- RevenueGeneral
- SalesOfProductIncome
- SalesRetail
- SalesWholesale
- SavingsByTaxScheme
- ServiceFeeIncome
- UnappliedCashPaymentIncome

**AccountType: OtherIncome**
- DividendIncome
- GainLossOnSaleOfFixedAssets
- GainLossOnSaleOfInvestments
- InterestEarned
- LossOnDisposalOfAssets
- OtherInvestmentIncome
- OtherMiscellaneousIncome
- OtherOperatingIncome
- TaxExemptInterest
- UnrealisedLossOnSecuritiesNetOfTax

### Expense Classification

**AccountType: Expense**
- AdvertisingPromotional
- AmortizationExpense
- AppropriationsToDepreciation
- Auto
- BadDebts
- BankCharges
- BorrowingCost
- CharitableContributions
- CommissionsAndFees
- CostOfLabor
- DistributionCosts
- DuesSubscriptions
- Entertainment
- EntertainmentMeals
- EquipmentRental
- ExternalServices
- ExtraordinaryCharges
- FinanceCosts
- GlobalTaxExpense
- IncomeTaxExpense
- Insurance
- InterestPaid
- LegalProfessionalFees
- LossOnDiscontinuedOperationsNetOfTax
- ManagementCompensation
- OtherCurrentOperatingCharges
- OfficeExpenses
- OfficeGeneralAdministrativeExpenses
- OtherBusinessExpenses
- OtherExternalServices
- OtherMiscellaneousServiceCost
- OtherRentalCosts
- OtherSellingExpenses
- PayrollExpenses
- ProjectStudiesSurveysAssessments
- PromotionalMeals
- PurchasesRebates
- RentOrLeaseOfBuildings
- RepairMaintenance
- ShippingAndDeliveryExpense
- ShippingFreightDelivery
- StaffCosts
- Sundry
- SuppliesMaterials
- TaxesPaid
- Travel
- TravelExpensesGeneralAndAdminExpenses
- TravelExpensesSellingExpense
- TravelMeals
- UnappliedCashBillPaymentExpense
- Utilities

**AccountType: OtherExpense**
- Amortization
- DeferredTaxExpense
- Depletion
- Depreciation
- ExceptionalItems
- ExchangeGainOrLoss
- ExtraordinaryItems
- GasAndFuel
- HomeOffice
- HomeOwnerRentalInsurance
- IncomeTaxOtherExpense
- MatCredit
- MortgageInterest
- OtherHomeOfficeExpenses
- OtherMiscellaneousExpense
- OtherVehicleExpenses
- ParkingAndTolls
- PenaltiesSettlements
- PriorPeriodItems
- RentAndLease
- RepairsAndMaintenance
- TaxRoundoffGainOrLoss
- Vehicle
- VehicleInsurance
- VehicleLease
- VehicleLoanInterest
- VehicleLoan
- VehicleRegistration
- VehicleRepairs
- WashAndRoadServices

**AccountType: CostofGoodsSold**
- CostOfLaborCos
- CostOfSales
- EquipmentRentalCos
- FreightAndDeliveryCost
- OtherCostsOfServiceCos
- ShippingFreightDeliveryCos
- SuppliesMaterialsCogs

Source: [TIBCO Scribe QBO Connector docs](https://help.scribesoft.com/scribe/en/sol/conn/qbonline.htm), [AccountSubTypeEnum Java SDK](https://static.developer.intuit.com/sdkdocs/qbv3doc/ipp-v3-java-devkit-javadoc/com/intuit/ipp/data/AccountSubTypeEnum.html)

---

## 6. Creating Accounts via API

### Endpoint

```
POST https://quickbooks.api.intuit.com/v3/company/{realmId}/account
Content-Type: application/json
Authorization: Bearer {access_token}
```

### Minimal Create Request

```json
{
  "Name": "Office Supplies",
  "AccountType": "Expense",
  "AccountSubType": "OfficeExpenses"
}
```

### Full Create Request Example

```json
{
  "Name": "Office Supplies",
  "AccountType": "Expense",
  "AccountSubType": "OfficeExpenses",
  "AcctNum": "6200",
  "Description": "General office supply purchases",
  "Active": true
}
```

### python-quickbooks: Create Account

```python
from quickbooks.objects.account import Account

account = Account()
account.Name = "Office Supplies"
account.AccountType = "Expense"
account.AccountSubType = "OfficeExpenses"
account.AcctNum = "6200"
account.Description = "General office supply purchases"
account.save(qb=client)

# Or from JSON dict:
account = Account.from_json({
    "Name": "Office Supplies",
    "AccountType": "Expense",
    "AccountSubType": "OfficeExpenses",
    "AcctNum": "6200"
})
account.save(qb=client)
```

### Sub-Account Create

```python
from quickbooks.objects.base import Ref

parent_account = Account.get(parent_id, qb=client)

child = Account()
child.Name = "Office Supplies - Stationery"
child.AccountType = "Expense"
child.AccountSubType = "OfficeExpenses"
child.SubAccount = True
child.ParentRef = parent_account.to_ref()
child.save(qb=client)
```

### Name Uniqueness Rules

- Account names must be unique within the same parent level. A top-level "Travel" and a child-of-"Operations" "Travel" can coexist.
- Error `6240 — Duplicate Name Exists` is returned if the name conflicts. This error is shared across Customers, Vendors, Employees, and Accounts when names overlap in some contexts.
- Names cannot contain colons (`:`). Colons are used by QBO internally for the `FullyQualifiedName` hierarchy separator.

Source: [Intuit 6240 Duplicate Name error](https://help.developer.intuit.com/s/article/6240-Duplicate-Name-Exists-Error), [QBO character limitations](https://quickbooks.intuit.com/learn-support/en-ca/other-questions/character-limitations-for-fields-in-quickbooks-online/00/898984)

---

## 7. Renaming Accounts via API

**Yes — renaming is fully supported via API.** To rename an account, perform a sparse or full update with the new `Name` value. You must include `Id` and `SyncToken`.

### Sparse Update (recommended)

```python
account = Account.get(account_id, qb=client)
account.Name = "New Account Name"
account.save(qb=client)
```

The python-quickbooks `save()` method performs a full update (not sparse by default), so the complete object is sent. For explicit sparse updates via raw HTTP:

```json
POST /v3/company/{realmId}/account?operation=update
{
  "sparse": true,
  "Id": "42",
  "SyncToken": "3",
  "Name": "New Account Name"
}
```

### Sparse Update Behavior

- `sparse: true` updates only the fields included in the request body; all other fields are left unchanged.
- Without `sparse: true`, any writable field omitted from the body is set to NULL.
- **Always include `Id` and `SyncToken` for any update.** SyncToken mismatch returns a 4xx conflict error.

Source: QBO API sparse update discussion (Intuit Developer Community), per the python-quickbooks SDK documentation

---

## 8. Making Accounts Inactive via API

**Yes — supported via API.** Set `Active = false` on the account object. This is a soft delete; the account is hidden from the UI but all transaction history is preserved and queryable.

### python-quickbooks: Deactivate Account

```python
account = Account.get(account_id, qb=client)
account.Active = False
account.save(qb=client)
```

### Reactivate

```python
account = Account.get(account_id, qb=client)  # must query with Active=false filter
account.Active = True
account.save(qb=client)
```

### Query Inactive Accounts

```python
# Via SQL-like query
accounts = Account.where("Active = false", qb=client)

# Or include all
accounts = Account.where("Active IN (true, false)", qb=client)
```

### Important Constraints on Inactive Accounts

- Accounts with active sub-accounts cannot be made inactive until sub-accounts are also deactivated.
- System accounts (e.g., Accounts Receivable, Accounts Payable, Retained Earnings, Opening Balance Equity) generally cannot be made inactive.
- Accounts connected to online banking feeds typically cannot be made inactive or deleted until the bank connection is disconnected.

Source: QBO making accounts inactive (Intuit Community), per the python-quickbooks SDK Account class

---

## 9. Merging Accounts via API

**Account merge is NOT supported via the QBO API.** There is no merge endpoint or operation in the Accounting REST API v3.

Merge is a UI-only operation available in the accountant view of QBO.

### UI Merge Requirements (for reference)

The UI merge works by renaming one account to exactly match another existing account:
1. Both accounts must have identical AccountType and AccountSubType (Detail Type).
2. Edit the account to be absorbed: change its Name to exactly match the target account's name.
3. QBO detects the duplicate name and prompts "Yes, merge accounts."
4. All transactions from the absorbed account move to the target account. The operation is permanent and irreversible.

### API Workaround for Merge

There is no true API-level merge. If you need to consolidate accounts programmatically:
1. Query all transactions referencing the source account.
2. Update each transaction to reference the target account.
3. Make the source account inactive (`Active = false`).

This is complex and transaction-type-specific — journal entries, invoices, bills, etc. each require separate update operations.

### Merge Blockers (even in UI)

- Accounts connected to online banking feeds cannot be merged.
- Accounts with differing AccountType or AccountSubType cannot be merged.
- Having an account number (`AcctNum`) on the subordinate account sometimes blocks merge — remove it first.

Source: [How to merge accounts QBO community](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/how-to-merge-accounts/00/559122), [How do I merge accounts in current version](https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/how-do-i-merge-accounts-in-current-version-of-quickbooks-online/00/1394632)

---

## 10. AcctNum Field (Account Numbers)

### Field Specification

- **Field name**: `AcctNum`
- **Type**: String
- **Max length**: 100 characters in QBO (31 characters in QuickBooks Desktop)
- **Uniqueness**: Not enforced by API — QBO does not require AcctNum to be unique across accounts, though it is best practice.
- **Optional**: AcctNum is never required for create or update operations.

### Account Number Feature Flag in QBO

Account numbers are controlled by a company-level setting in QBO:
- Navigate to: Gear icon → Account and Settings → Advanced → Chart of Accounts → Enable account numbers.
- Optionally enable "Show account numbers" to display them in reports and transactions.

**Critical API behavior**: The `AcctNum` field is stored and returned by the API regardless of whether the account numbers feature is enabled in the QBO settings. The UI setting only controls display; the API field is always readable and writable. If you set `AcctNum` via API without enabling the UI feature, the number is stored but not displayed in the UI.

### Does Enabling Account Numbers Change API Matching Behavior?

No — enabling account numbers in the UI does not change API matching or lookup behavior. The API always matches accounts by `Id` (primary key) or by `Name`. Account numbers are informational metadata. There is no API endpoint that looks up accounts by `AcctNum` directly (though you can query: `SELECT * FROM Account WHERE AcctNum = '1000'`).

### python-quickbooks Usage

```python
account = Account()
account.Name = "Checking - Main"
account.AccountType = "Bank"
account.AccountSubType = "Checking"
account.AcctNum = "1010"
account.save(qb=client)

# Query by account number
accounts = Account.where("AcctNum = '1010'", qb=client)
```

Source: [AcctNum field documentation](https://help.developer.intuit.com/s/topic/0TOG00000004qzCOAQ/account-number), [How to enable account numbers](https://gentlefrog.com/how-to-enable-account-numbers/), [QBO account numbers UI](https://www.wizxpert.com/use-account-numbers-in-charts-of-accounts-in-qbo/)

---

## 11. Rate Limits and Batch Considerations

### Standard Rate Limits (Production and Sandbox)

- **500 requests per minute** per realm ID (company), per app (rolling window).
- **10 concurrent requests** maximum per company per app.
- **HTTP 429** (Too Many Requests) is returned when limits are exceeded.
- Sandbox and production environments enforce identical rate limits.
- Implement exponential backoff when receiving 429 responses.

### Batch Operations

The QBO API supports batch requests at:
```
POST /v3/company/{realmId}/batch
```

Key batch constraints:
- **Maximum 30 items per batch request** (enforced by both the API and python-quickbooks BatchManager).
- Batch requests count as **1 request** against the rate limit regardless of how many items are in the batch.
- Within a batch, operations execute **serially** (not in parallel).
- Supported operations: `create`, `update`, `delete` (deactivate).

### python-quickbooks Batch Operations

```python
from quickbooks.batch import batch_create, batch_update, batch_delete

# Create multiple accounts at once
accounts_to_create = []
for name, acct_type, subtype, num in account_list:
    a = Account()
    a.Name = name
    a.AccountType = acct_type
    a.AccountSubType = subtype
    a.AcctNum = num
    accounts_to_create.append(a)

results = batch_create(accounts_to_create, qb=client)

# Check results
for obj in results.successes:
    print(f"Created: {obj.Name} (Id={obj.Id})")

for fault in results.faults:
    print(f"Failed: {fault.original_object.Name}")
    for error in fault.Error:
        print(f"  Error: {error.Message}")
```

The `batch_create`/`batch_update`/`batch_delete` functions in python-quickbooks automatically chunk lists larger than 30 into sequential batch requests.

```python
# Batch update — make multiple accounts inactive
accounts = Account.where("Active = true AND AccountType = 'Expense'", qb=client)
for a in accounts:
    a.Active = False
results = batch_update(accounts, qb=client)
```

### Throttling Guidance for Account Creation

When creating a large Chart of Accounts (50–200+ accounts):
- Use batch_create with lists of up to 30 accounts per call.
- Each batch of 30 accounts = 1 API call against the 500/min limit.
- At 30 accounts per batch, you can create 15,000 accounts/minute before hitting limits.
- Practically, a full CoA (100–150 accounts) can be created in 3–5 batch calls with no throttling concern.

Source: Intuit Developer — API call limits and throttling, per the python-quickbooks SDK batch module, Coefficient QBO rate limits guide, Satva QBO API limitations

---

## 12. Update Pattern Summary

All updates require `Id` and `SyncToken`. The SyncToken is a version counter that increments on every save. Stale SyncToken = conflict error.

```python
# Safe update pattern
account = Account.get(account_id, qb=client)  # gets current SyncToken
account.Name = "New Name"
account.AcctNum = "5000"
account.save(qb=client)  # uses the SyncToken from the get
```

Never construct an update object from scratch without fetching first — you will not have the current SyncToken and the update will fail.

For sparse updates via raw HTTP, set `"sparse": true` in the body to avoid nulling out fields you did not include.

---

## 13. Common Errors

| Error Code | Meaning | Fix |
|---|---|---|
| 6240 | Duplicate Name Exists | Use a different, unique account name |
| 6430 | Invalid Account Type | Check AccountType value is exact enum string |
| 400 | Bad Request — invalid AccountSubType for given AccountType | Verify subtype is valid for the AccountType |
| 4xx (conflict) | SyncToken mismatch | Re-fetch account to get current SyncToken before updating |
| 429 | Too Many Requests | Back off, retry with exponential delay |

Source: [6240 Duplicate Name Exists error](https://help.developer.intuit.com/s/article/6240-Duplicate-Name-Exists-Error), [6430 Invalid account type](https://help.developer.intuit.com/s/article/6430-Invalid-account-type-used)

---

## 14. Known Limitations and Constraints

- **No merge via API** — UI only, and only in accountant view.
- **No true delete** — deactivation (`Active = false`) is the only removal mechanism.
- **System accounts cannot be deactivated** — Accounts Receivable, Accounts Payable, Retained Earnings, Opening Balance Equity, Undeposited Funds are protected.
- **Banking-connected accounts cannot be deactivated or merged** until the bank connection is removed.
- **NonPosting AccountType** is not supported in QBO (Desktop only).
- **FixedAsset + AccumulatedDepreciation/AccumulatedAmortization/AccumulatedDepletion** cannot be set as parent accounts (QBO restriction).
- **Tags** cannot be created, updated, or deleted via API (read-only via API). This is separate from accounts but affects transaction categorization.
- **Custom Fields** have limited API support — read/write of first 3 string fields on Advanced tier only.

Source: [Top 5 QBO API limitations](https://satvasolutions.com/blog/top-5-quickbooks-api-limitations-to-know-before-developing-qbo-app), [QBO Account API reference](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/account)

---

## 15. Key Source URLs

- QBO Account API reference (all-entities): https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/account
- QBO Account API reference (most-commonly-used): https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/account
- AccountTypeEnum .NET SDK: https://static.developer.intuit.com/sdkdocs/qbv3doc/ippdotnetdevkitv3/html/fe5d46d7-7629-9de7-1011-2b2c2156cc6b.htm
- AccountSubTypeEnum Java SDK: https://static.developer.intuit.com/sdkdocs/qbv3doc/ipp-v3-java-devkit-javadoc/com/intuit/ipp/data/AccountSubTypeEnum.html
- python-quickbooks SDK (Account class, batch module, README)
- API call limits and throttling: https://help.developer.intuit.com/s/article/API-call-limits-and-throttling
- QBO API batch operations: https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/batch
- 6240 Duplicate Name Exists error: https://help.developer.intuit.com/s/article/6240-Duplicate-Name-Exists-Error
- 6430 Invalid account type: https://help.developer.intuit.com/s/article/6430-Invalid-account-type-used
- QBO sparse update discussion: https://help.developer.intuit.com/s/question/0D5TR00000oMwF30AK/qbo-api-sparse-update
- How to merge accounts (UI): https://quickbooks.intuit.com/learn-support/en-us/reports-and-accounting/how-to-merge-accounts/00/559122
- How to enable account numbers: https://gentlefrog.com/how-to-enable-account-numbers/
- QBO making accounts inactive: https://quickbooks.intuit.com/learn-support/en-us/account-management/qbo-making-accounts-inactive/00/1126703
- Coefficient rate limits guide: https://coefficient.io/quickbooks-api/quickbooks-api-rate-limits
- TIBCO Scribe AccountType/SubType mapping: https://help.scribesoft.com/scribe/en/sol/conn/qbonline.htm
- Top 5 QBO API limitations: https://satvasolutions.com/blog/top-5-quickbooks-api-limitations-to-know-before-developing-qbo-app
- Apideck QBO API exploration: https://www.apideck.com/blog/exploring-the-quickbooks-online-accounting-api
