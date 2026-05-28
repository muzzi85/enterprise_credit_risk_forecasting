
# Enterprise Credit Risk Forecasting

Enterprise-grade credit risk modeling framework implementing:

- Probability of Default (PD)
- Loss Given Default (LGD)
- Exposure at Default (EAD)

using advanced feature engineering and CatBoost-based machine learning pipelines.

---

# Enterprise Credit Risk Architecture

<div align="center">

| Component | Business Question | Output |
|---|---|---|
| PD | Will the borrower default? | Default Probability |
| LGD | If default occurs, how severe is the loss? | Loss Severity |
| EAD | How much exposure remains at default? | Remaining Exposure |

</div>

---

# Expected Loss Framework

EL = PD × LGD × EAD

---

# 1. Probability of Default (PD)

## Business Objective

Predict the probability that a borrower will default at loan origination.

---

## PD Feature Engineering Dictionary

<table>
<tr>
<th>Feature</th>
<th>Mathematical Formula</th>
<th>Business Meaning</th>
<th>Example</th>
</tr>

<tr>
<td><b>loan_to_income</b></td>
<td>loan_amnt / annual_inc</td>
<td>Measures borrower leverage relative to annual income.</td>
<td>£20k loan / £100k income = 0.20</td>
</tr>

<tr>
<td><b>installment_to_income</b></td>
<td>installment / monthly_income</td>
<td>Measures monthly repayment burden.</td>
<td>£500 installment / £5000 income = 10%</td>
</tr>

<tr>
<td><b>dti_per_fico</b></td>
<td>dti / fico_score</td>
<td>Measures debt burden adjusted for credit quality.</td>
<td>High DTI + weak FICO = elevated risk</td>
</tr>

<tr>
<td><b>credit_hunger</b></td>
<td>inquiries + recent_accounts</td>
<td>Captures aggressive borrowing behavior.</td>
<td>Many recent credit applications</td>
</tr>

<tr>
<td><b>utilization_shock_risk</b></td>
<td>revol_util × recent_trades</td>
<td>Captures rapid leverage expansion.</td>
<td>90% utilization + multiple new trades</td>
</tr>

<tr>
<td><b>long_term_stress</b></td>
<td>term_months × installment_to_income</td>
<td>Measures long-duration repayment stress.</td>
<td>Long-term high payment burden</td>
</tr>

<tr>
<td><b>payment_stress_amplification</b></td>
<td>installment_to_income × dti</td>
<td>Captures combined affordability pressure.</td>
<td>High installment burden + high DTI</td>
</tr>

</table>

---

# 2. Loss Given Default (LGD)

## Business Objective

Predict the percentage financial loss incurred if default occurs.

---

## LGD Feature Engineering Dictionary

<table>
<tr>
<th>Feature</th>
<th>Mathematical Formula</th>
<th>Business Meaning</th>
<th>Example</th>
</tr>

<tr>
<td><b>liquidity_exhaustion</b></td>
<td>revol_bal / total_rev_hi_lim</td>
<td>Measures exhaustion of available revolving liquidity.</td>
<td>£9k used from £10k limit = 90%</td>
</tr>

<tr>
<td><b>debt_saturation</b></td>
<td>total_bal_ex_mort / annual_inc</td>
<td>Measures total debt pressure relative to income.</td>
<td>Heavy debt relative to salary</td>
</tr>

<tr>
<td><b>revolving_dependency</b></td>
<td>revol_bal / total_bal_ex_mort</td>
<td>Measures dependence on revolving credit.</td>
<td>Borrower mainly relies on credit cards</td>
</tr>

<tr>
<td><b>fico_stress</b></td>
<td>revol_util × dti / fico</td>
<td>Measures stress adjusted for borrower quality.</td>
<td>Weak FICO + high utilization</td>
</tr>

<tr>
<td><b>borrower_complexity</b></td>
<td>open_acc + rev_accounts + installment_accounts</td>
<td>Measures structural complexity of debt portfolio.</td>
<td>Many fragmented debt products</td>
</tr>

<tr>
<td><b>credit_fatigue</b></td>
<td>active_revolving_trades / history_years</td>
<td>Measures chronic long-term leverage usage.</td>
<td>Persistent revolving debt behavior</td>
</tr>

</table>

---

# 3. Exposure at Default (EAD)

## Business Objective

Predict remaining loan exposure when default occurs.

---

## EAD Feature Engineering Dictionary

<table>
<tr>
<th>Feature</th>
<th>Mathematical Formula</th>
<th>Business Meaning</th>
<th>Example</th>
</tr>

<tr>
<td><b>EAD_ratio</b></td>
<td>out_prncp / funded_amnt</td>
<td>Normalized remaining exposure at default.</td>
<td>£8k remaining / £10k funded = 80%</td>
</tr>

<tr>
<td><b>installment_per_loan</b></td>
<td>installment / loan_amnt</td>
<td>Measures repayment velocity.</td>
<td>Higher payments reduce exposure faster</td>
</tr>

<tr>
<td><b>exposure_pressure</b></td>
<td>loan_amnt × revol_util</td>
<td>Measures leveraged exposure intensity.</td>
<td>Large loan combined with high utilization</td>
</tr>

<tr>
<td><b>exposure_income_stress</b></td>
<td>loan_amnt × dti / income</td>
<td>Measures affordability-adjusted exposure.</td>
<td>High debt relative to repayment ability</td>
</tr>

<tr>
<td><b>term_installment_pressure</b></td>
<td>term_months × installment</td>
<td>Captures cumulative repayment burden.</td>
<td>Long-term repayment pressure</td>
</tr>

<tr>
<td><b>leverage_acceleration</b></td>
<td>utilization × inquiries × recent_accounts</td>
<td>Captures accelerating debt expansion behavior.</td>
<td>Rapid debt accumulation</td>
</tr>

</table>

---

# Technology Stack

- Python
- Pandas
- NumPy
- CatBoost
- Scikit-Learn
- Jupyter Notebooks

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
- Behavioral Monitoring Models
- Stress Testing
- IFRS9 Lifetime Risk
- Collections Optimization

---

# Author

Mustafa Alhamdi
