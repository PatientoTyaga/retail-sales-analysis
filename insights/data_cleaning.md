# Data Cleaning & Quality Assessment

## Dataset Overview

- Original rows: 1,067,371
- Grain: One row represents one product/item within an invoice.
- The dataset contains two years of UK online retail transaction data.

## Missing Values

- Customer ID: 243,007 null values
- Description: 4,382 null values
- All other columns contained no null values.
- Missing Customer IDs were not automatically removed because these records may still be useful for overall sales and product analysis.

## Quantity Investigation

- Minimum quantity: -80,995
- Maximum quantity: 80,995
- Negative quantity rows: 22,950
- Negative quantity with cancellation invoice (`C`): 19,493
- Negative quantity without cancellation invoice: 3,457
- Zero quantity rows: 0

Negative quantities were investigated rather than automatically treated as errors. Cancellation invoices and operational adjustments such as lost, damaged, and short inventory were found in the data.

## Price Investigation

- Minimum price: -53,594.36
- Maximum price: 38,970
- Zero-price rows: 6,202
- Negative-price rows: 5
- All negative-price records were identified as "Adjust bad debt" entries.

These records appear to include accounting and operational adjustments, so unusual price values were not automatically removed.

## Duplicate Investigation

- Original rows: 1,067,371
- Rows after removing exact duplicates: 1,033,036
- Redundant exact duplicate rows removed: 34,335

Exact duplicates were defined as records with identical values across all eight original columns. These duplicates were removed to prevent double-counting in sales and quantity metrics.

## Transaction Classification

Rather than removing unusual transaction records, transaction lines were classified to preserve information for later analysis.

After exact duplicate removal:

- Sale: 1,007,913 rows
- Cancellation: 19,104 rows
- Inventory Adjustment: 3,393 rows
- Zero Price: 2,621 rows
- Accounting Adjustment: 5 rows

Classification logic:

1. Invoice beginning with `C` → Cancellation
2. Negative Price → Accounting Adjustment
3. Negative Quantity → Inventory Adjustment
4. Zero Price → Zero Price
5. Otherwise → Sale

This approach preserves cancellations and operational/accounting adjustments while allowing sales KPIs to be calculated separately from non-sales activity.

## Derived Fields

- `Revenue` = `Quantity × Price`
- `IsCancellation` identifies invoices beginning with `C`.
- `TransactionType` classifies each transaction line for downstream analysis.
