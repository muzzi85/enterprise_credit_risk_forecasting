
# Enterprise Credit Risk Forecasting

Enterprise-grade credit risk platform implementing:

- Probability of Default (PD)
- Loss Given Default (LGD)
- Exposure at Default (EAD)

---

# Enterprise Risk Architecture

| Model | Objective |
|---|---|
| PD | Predict borrower default probability |
| LGD | Predict financial loss severity |
| EAD | Predict remaining exposure at default |

---

# Expected Loss Formula

Expected Loss (EL):

EL = PD × LGD × EAD

---

# Probability of Default (PD)

## Business Objective

Predict the probability that a borrower defaults during the loan lifecycle.

## PD Feature Dictionary

| Feature | Formula | Business Meaning | Customer Example |
|---|---|---|---|
| loan_to_income | `loan_amnt / annual_inc` | loan_amnt represents the total loan requested by the borrower, while annual_inc represents the borrower annual income before taxes. This feature measures how large the requested loan is relative to the borrower earning capacity. Higher values indicate elevated leverage and greater repayment pressure. | loan_amnt = 20,000 ; annual_inc = 100,000 ; loan_to_income = 20,000 / 100,000 = 0.20 |
| installment_income_ratio | `installment / monthly_income` | installment represents the required monthly repayment amount, while monthly_income represents the borrower monthly earnings. This feature measures the percentage of monthly income consumed by loan repayments and captures affordability stress. | installment = 500 ; monthly_income = 5,000 ; installment_income_ratio = 500 / 5,000 = 0.10 |
| revol_util_ratio | `revol_bal / total_rev_hi_lim` | revol_bal represents the currently utilized revolving debt balance, while total_rev_hi_lim represents the maximum available revolving credit limit. This feature measures revolving credit utilization intensity and liquidity exhaustion. | revol_bal = 9,000 ; total_rev_hi_lim = 10,000 ; revol_util_ratio = 9,000 / 10,000 = 0.90 |

---

# Loss Given Default (LGD)

## Business Objective

Predict the percentage financial loss incurred if borrower defaults.

## LGD Feature Dictionary

| Feature | Formula | Business Meaning | Customer Example |
|---|---|---|---|
| liquidity_exhaustion | `revol_bal / total_rev_hi_lim` | revol_bal represents utilized revolving credit, while total_rev_hi_lim represents the borrower total revolving credit capacity. This feature measures how exhausted borrower liquidity is before default and is strongly linked to recovery severity. | revol_bal = 9,500 ; total_rev_hi_lim = 10,000 ; liquidity_exhaustion = 9,500 / 10,000 = 0.95 |
| debt_saturation | `total_bal_ex_mort / annual_inc` | total_bal_ex_mort represents total outstanding debt excluding mortgage balances, while annual_inc represents borrower annual income. This feature measures how saturated the borrower balance sheet is relative to repayment capacity. | total_bal_ex_mort = 70,000 ; annual_inc = 100,000 ; debt_saturation = 70,000 / 100,000 = 0.70 |

---

# Exposure at Default (EAD)

## Business Objective

Predict remaining exposure when borrower defaults.

## EAD Feature Dictionary

| Feature | Formula | Business Meaning | Customer Example |
|---|---|---|---|
| EAD_ratio | `out_prncp / funded_amnt` | out_prncp represents the remaining unpaid principal balance at default, while funded_amnt represents the original funded loan amount. This feature measures the percentage exposure still outstanding when default occurs. | out_prncp = 8,000 ; funded_amnt = 10,000 ; EAD_ratio = 8,000 / 10,000 = 0.80 |
| installment_per_loan | `installment / loan_amnt` | installment represents monthly repayment amount, while loan_amnt represents original loan size. This feature measures repayment velocity and amortization speed of the loan structure. | installment = 300 ; loan_amnt = 10,000 ; installment_per_loan = 300 / 10,000 = 0.03 |

---

# Technology Stack

- Python
- Pandas
- NumPy
- CatBoost
- Scikit-Learn
- Jupyter

---

# Author

Mustafa Alhamdi
