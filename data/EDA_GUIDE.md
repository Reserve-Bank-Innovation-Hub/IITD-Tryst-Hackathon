# Exploratory Data Analysis Guide

This guide is meant to help you get started with the dataset. The goal of this EDA phase is to demonstrate your ability to understand banking data, identify meaningful patterns, and think critically about what features could help distinguish mule accounts from legitimate ones.

> This is a 20% representative sample of the full dataset. Distributions and class ratios are preserved. Selected candidates will receive the complete data.

---

## Getting Started

### 1. Load and Inspect the Data

```python
import pandas as pd

customers = pd.read_csv("customers.csv")
accounts = pd.read_csv("accounts.csv")
linkage = pd.read_csv("customer_account_linkage.csv")
products = pd.read_csv("product_details.csv")
labels = pd.read_csv("train_labels.csv")
test = pd.read_csv("test_accounts.csv")

# Transactions are split across 6 files due to GitHub size limits — concatenate them
transactions = pd.concat(
    [pd.read_csv(f"transactions_part_{i}.csv") for i in range(6)],
    ignore_index=True
)
```

Check shapes, data types, and missing values for each table:

```python
for name, df in [("customers", customers), ("accounts", accounts),
                 ("linkage", linkage), ("products", products),
                 ("labels", labels), ("transactions", transactions)]:
    print(f"\n{'='*40}")
    print(f"{name}: {df.shape}")
    print(df.dtypes)
    print(f"\nMissing values:\n{df.isnull().sum()}")
```

### 2. Understand the Target Variable

```python
print(labels["is_mule"].value_counts())
print(f"Mule rate: {labels['is_mule'].mean():.4f}")
```

Note the class imbalance — this will affect your modelling strategy.

### 3. Join the Tables

The data is spread across multiple tables. The key relationships are:

```
customers <--(customer_id)--> linkage <--(account_id)--> accounts
                                                             |
                                                        (account_id)
                                                             |
                                                        transactions

customers <--(customer_id)--> product_details
accounts  <--(account_id)--> train_labels / test_accounts
```

Example join:

```python
# Merge labels with account details
train = labels.merge(accounts, on="account_id", how="left")

# Add customer info
train = train.merge(linkage, on="account_id", how="left")
train = train.merge(customers, on="customer_id", how="left")
```

---

## Suggested EDA Areas

### A. Account-Level Analysis

- How do mule vs. legitimate accounts differ across account attributes?
- Examine distributions of: `avg_balance`, `product_family`, `account_status`, `account_opening_date`, balance metrics
- Are there patterns in when mule accounts were opened?
- How do KYC and nomination flags differ between classes?

### B. Transaction-Level Analysis

- What does the transaction volume and amount distribution look like for mule vs. legitimate accounts?
- Are there differences in channel usage (UPI, NEFT, IMPS, ATM, etc.)?
- How do credit vs. debit patterns differ?
- Look at temporal patterns — time of day, day of week, monthly trends
- Examine counterparty diversity — do mule accounts transact with many or few unique counterparties?

### C. Customer-Level Analysis

- Do mule customers differ in demographics (age, relationship tenure)?
- What about product holdings — loan counts, credit card usage, etc.?
- Do mule customers tend to have multiple accounts?

### D. Temporal Patterns

- How are transactions distributed over the 5-year window (Jul 2020 – Jun 2025)?
- Are there burst patterns — periods of high activity followed by silence?
- Look at transaction velocity — how many transactions per day/week/month?

### E. Network / Relationship Features

- How many unique counterparties does each account interact with?
- Are there shared counterparties across mule accounts?
- Do certain branches have higher concentrations of mule accounts?

### F. Missing Data

- Which columns have missing values? Is the missingness random or does it correlate with the target?
- How should missing values be handled for modelling?

---

## Tips

- **Think about feature engineering** — raw fields alone may not be enough. Consider aggregations (per-account transaction stats), ratios, temporal features, and interaction terms.
- **Be mindful of data leakage** — some columns in the training labels exist only because an account was already identified as a mule. Think carefully about which features would be available at prediction time.
- **Document your findings** — we're looking for clear thinking, not just code. Explain what you observe and why it might matter.
- **Consider the class imbalance** — with a low mule rate, think about how this affects both your analysis and potential modelling approaches.

---

## What We're Looking For

1. **Data understanding** — Can you navigate a multi-table banking dataset?
2. **Analytical rigour** — Are your comparisons statistically sound? Do you account for class imbalance in your visualisations?
3. **Feature intuition** — Can you identify features or patterns that could be predictive of mule behaviour?
4. **Critical thinking** — Do you question the data, notice anomalies, and consider edge cases?
5. **Communication** — Are your findings presented clearly with appropriate visualisations?

Good luck with your exploration.
