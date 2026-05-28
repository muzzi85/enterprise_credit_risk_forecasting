
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
| loan_to_income | `loan_amnt / annual_inc` | Measures borrower leverage relative to annual income. | `20000 / 100000 = 0.20` |
| installment_income_ratio | `installment / monthly_income` | Measures monthly repayment affordability pressure. | `500 / 5000 = 0.10` |
| revol_util_ratio | `revol_bal / total_rev_hi_lim` | Measures revolving credit utilization intensity. | `9000 / 10000 = 0.90` |
| accounts_per_year | `total_acc / credit_history_years` | Measures speed of credit expansion. | `20 / 10 = 2 accounts/year` |
| credit_hunger | `inq_last_6mths + acc_open_past_24mths` | Captures aggressive borrowing behavior. | `4 + 6 = 10` |
| dti_per_fico | `dti / fico_score` | Measures debt burden adjusted for credit quality. | `20 / 650 = 0.031` |
| long_term_stress | `term_months × installment_income_ratio` | Captures prolonged affordability stress. | `60 × 0.10 = 6.0` |
| payment_stress_amplification | `installment_income_ratio × dti` | Measures compounded repayment pressure. | `0.10 × 20 = 2.0` |

---

# Loss Given Default (LGD)

## Business Objective

Predict the percentage financial loss incurred if borrower defaults.

## LGD Feature Dictionary

| Feature | Formula | Business Meaning | Customer Example |
|---|---|---|---|
| liquidity_exhaustion | `revol_bal / total_rev_hi_lim` | Measures exhaustion of available liquidity. | `9500 / 10000 = 0.95` |
| debt_saturation | `total_bal_ex_mort / annual_inc` | Measures total debt pressure relative to income. | `70000 / 100000 = 0.70` |
| revolving_dependency | `revol_bal / total_bal_ex_mort` | Measures dependence on revolving debt. | `20000 / 50000 = 0.40` |
| fico_stress | `(revol_util × dti) / fico` | Measures financial stress adjusted for borrower quality. | `(90 × 20) / 650 = 2.77` |
| borrower_complexity | `open_acc + num_rev_accts + num_il_tl` | Measures complexity of debt structure. | `12 + 8 + 5 = 25` |
| credit_fatigue | `num_actv_rev_tl / credit_history_years` | Measures chronic leverage dependency. | `10 / 8 = 1.25` |

---

# Exposure at Default (EAD)

## Business Objective

Predict remaining exposure when borrower defaults.

## EAD Feature Dictionary

| Feature | Formula | Business Meaning | Customer Example |
|---|---|---|---|
| EAD_ratio | `out_prncp / funded_amnt` | Measures remaining normalized exposure at default. | `8000 / 10000 = 0.80` |
| installment_per_loan | `installment / loan_amnt` | Measures repayment velocity. | `300 / 10000 = 0.03` |
| exposure_pressure | `loan_amnt × revol_util` | Measures leveraged exposure intensity. | `10000 × 0.90 = 9000` |
| exposure_income_stress | `(loan_amnt × dti) / annual_inc` | Measures affordability-adjusted exposure. | `(10000 × 20) / 100000 = 2.0` |
| term_installment_pressure | `term_months × installment` | Measures cumulative repayment burden. | `60 × 300 = 18000` |
| leverage_acceleration | `revol_util × inquiries × recent_accounts` | Measures accelerating debt expansion. | `90 × 4 × 3 = 1080` |

---

# Technology Stack

- Python
- Pandas
- NumPy
- CatBoost
- Scikit-Learn
- Jupyter

---

# Validation Methodology

- Stratified K-Fold Cross Validation
- Out-of-Fold Predictions
- ROC-AUC
- PR-AUC
- RMSE
- MAE
- R²

---

# Future Roadmap

- Expected Loss Engine
- Pricing Optimization
- Behavioral Risk Monitoring
- IFRS9 Lifetime PD
- Stress Testing
- Collections Optimization

---

# Author

Mustafa Alhamdi
