
# Enterprise Credit Risk Forecasting

Enterprise-grade credit risk platform implementing:

- Probability of Default (PD)
- Loss Given Default (LGD)
- Exposure at Default (EAD)

using advanced feature engineering and CatBoost machine learning pipelines.

---

# Enterprise Risk Formula

Expected Loss (EL):

EL = PD × LGD × EAD

---

# Model Architecture

| Model | Objective |
|---|---|
| PD | Predict borrower default probability |
| LGD | Predict severity of loss if default occurs |
| EAD | Predict remaining exposure at default |

---


# PD Feature Dictionary

| Feature | Formula Type | Business Meaning |
|---|---|---|
| accounts_per_year | Derived engineered feature | Credit structure and account behavior metric |
| all_util_ratio | Derived engineered feature | Credit utilization and liquidity pressure metric |
| bc_util_ratio | Derived engineered feature | Credit utilization and liquidity pressure metric |
| credit_history_months | Derived engineered feature | Credit History Months |
| credit_hunger | Derived engineered feature | Credit Hunger |
| delinq_intensity | Derived engineered feature | Historical delinquency intensity metric |
| emp_length_years | Derived engineered feature | Emp Length Years |
| emp_title_clean | Derived engineered feature | Emp Title Clean |
| expected_loss | Derived engineered feature | Expected Loss |
| installment_income_ratio | Derived engineered feature | Income affordability and repayment capacity indicator |
| issue_month | Derived engineered feature | Issue Month |
| issue_quarter | Derived engineered feature | Issue Quarter |
| issue_year | Derived engineered feature | Issue Year |
| job_cluster | Derived engineered feature | Job Cluster |
| loan_age_proxy | Derived engineered feature | Loan Age Proxy |
| loan_income_ratio | Derived engineered feature | Income affordability and repayment capacity indicator |
| recovery_rate | Derived engineered feature | Recovery Rate |
| remaining_exposure | Derived engineered feature | Exposure and remaining balance risk metric |
| revol_util_ratio | Derived engineered feature | Credit utilization and liquidity pressure metric |
| stress_score | Derived engineered feature | Financial stress and leverage pressure indicator |
| term_months | Derived engineered feature | Long-term repayment structure metric |
| zip_region | Derived engineered feature | Zip Region |

---


# LGD Feature Dictionary

| Feature | Formula Type | Business Meaning |
|---|---|---|
| account_fragmentation | Derived engineered feature | Credit structure and account behavior metric |
| active_trade_ratio | Derived engineered feature | Credit structure and account behavior metric |
| avg_account_age | Derived engineered feature | Credit structure and account behavior metric |
| bcutil_dti_stress | Derived engineered feature | Financial stress and leverage pressure indicator |
| borrower_complexity | Derived engineered feature | Borrower Complexity |
| credit_fatigue | Derived engineered feature | Credit Fatigue |
| credit_history_months | Derived engineered feature | Credit History Months |
| credit_hunger | Derived engineered feature | Credit Hunger |
| credit_resilience | Derived engineered feature | Credit Resilience |
| credit_velocity | Derived engineered feature | Credit expansion speed metric |
| debt_saturation | Derived engineered feature | Debt Saturation |
| delinq_intensity | Derived engineered feature | Historical delinquency intensity metric |
| derogatory_pressure | Derived engineered feature | Derogatory Pressure |
| dti_per_fico | Derived engineered feature | Credit quality adjusted risk metric |
| emp_length_years | Derived engineered feature | Emp Length Years |
| fico_stress | Derived engineered feature | Financial stress and leverage pressure indicator |
| inq_per_year | Derived engineered feature | Inq Per Year |
| installment_to_income | Derived engineered feature | Income affordability and repayment capacity indicator |
| installment_trade_share | Derived engineered feature | Credit structure and account behavior metric |
| issue_month | Derived engineered feature | Issue Month |
| issue_quarter | Derived engineered feature | Issue Quarter |
| issue_year | Derived engineered feature | Issue Year |
| leverage_acceleration | Derived engineered feature | Leverage Acceleration |
| liquidity_exhaustion | Derived engineered feature | Liquidity Exhaustion |
| loan_per_fico | Derived engineered feature | Credit quality adjusted risk metric |
| loan_to_income | Derived engineered feature | Income affordability and repayment capacity indicator |
| long_term_stress | Derived engineered feature | Financial stress and leverage pressure indicator |
| monthly_income | Derived engineered feature | Income affordability and repayment capacity indicator |
| open_account_ratio | Derived engineered feature | Credit structure and account behavior metric |
| payment_stress_amplification | Derived engineered feature | Financial stress and leverage pressure indicator |
| recent_account_pressure | Derived engineered feature | Credit structure and account behavior metric |
| remaining_bc_capacity | Derived engineered feature | Remaining Bc Capacity |
| remaining_revolving_capacity | Derived engineered feature | Remaining Revolving Capacity |
| revolving_dependency | Derived engineered feature | Borrower leverage dependency indicator |
| revolving_to_income | Derived engineered feature | Income affordability and repayment capacity indicator |
| revolving_trade_share | Derived engineered feature | Credit structure and account behavior metric |
| term_loan_burden | Derived engineered feature | Long-term repayment structure metric |
| term_months | Derived engineered feature | Long-term repayment structure metric |
| trade_density | Derived engineered feature | Credit structure and account behavior metric |
| util_per_account | Derived engineered feature | Credit utilization and liquidity pressure metric |
| utilization_dti_stress | Derived engineered feature | Financial stress and leverage pressure indicator |
| utilization_per_fico | Derived engineered feature | Credit utilization and liquidity pressure metric |
| utilization_shock_risk | Derived engineered feature | Credit utilization and liquidity pressure metric |

---


# EAD Feature Dictionary

| Feature | Formula Type | Business Meaning |
|---|---|---|
| EAD_ratio | Derived engineered feature | Ead Ratio |
| account_fragmentation | Derived engineered feature | Credit structure and account behavior metric |
| active_trade_ratio | Derived engineered feature | Credit structure and account behavior metric |
| avg_account_age | Derived engineered feature | Credit structure and account behavior metric |
| bcutil_dti_stress | Derived engineered feature | Financial stress and leverage pressure indicator |
| borrower_complexity | Derived engineered feature | Borrower Complexity |
| credit_fatigue | Derived engineered feature | Credit Fatigue |
| credit_history_months | Derived engineered feature | Credit History Months |
| credit_hunger | Derived engineered feature | Credit Hunger |
| credit_resilience | Derived engineered feature | Credit Resilience |
| credit_velocity | Derived engineered feature | Credit expansion speed metric |
| debt_saturation | Derived engineered feature | Debt Saturation |
| delinq_intensity | Derived engineered feature | Historical delinquency intensity metric |
| derogatory_pressure | Derived engineered feature | Derogatory Pressure |
| dti_per_fico | Derived engineered feature | Credit quality adjusted risk metric |
| emp_length_years | Derived engineered feature | Emp Length Years |
| exposure_income_stress | Derived engineered feature | Income affordability and repayment capacity indicator |
| exposure_pressure | Derived engineered feature | Exposure and remaining balance risk metric |
| fico_stress | Derived engineered feature | Financial stress and leverage pressure indicator |
| inq_per_year | Derived engineered feature | Inq Per Year |
| installment_per_loan | Derived engineered feature | Installment Per Loan |
| installment_to_income | Derived engineered feature | Income affordability and repayment capacity indicator |
| installment_trade_share | Derived engineered feature | Credit structure and account behavior metric |
| issue_month | Derived engineered feature | Issue Month |
| issue_quarter | Derived engineered feature | Issue Quarter |
| issue_year | Derived engineered feature | Issue Year |
| leverage_acceleration | Derived engineered feature | Leverage Acceleration |
| liquidity_exhaustion | Derived engineered feature | Liquidity Exhaustion |
| loan_per_fico | Derived engineered feature | Credit quality adjusted risk metric |
| loan_to_income | Derived engineered feature | Income affordability and repayment capacity indicator |
| long_term_stress | Derived engineered feature | Financial stress and leverage pressure indicator |
| monthly_income | Derived engineered feature | Income affordability and repayment capacity indicator |
| open_account_ratio | Derived engineered feature | Credit structure and account behavior metric |
| payment_stress_amplification | Derived engineered feature | Financial stress and leverage pressure indicator |
| recent_account_pressure | Derived engineered feature | Credit structure and account behavior metric |
| remaining_bc_capacity | Derived engineered feature | Remaining Bc Capacity |
| remaining_revolving_capacity | Derived engineered feature | Remaining Revolving Capacity |
| revolving_dependency | Derived engineered feature | Borrower leverage dependency indicator |
| revolving_to_income | Derived engineered feature | Income affordability and repayment capacity indicator |
| revolving_trade_share | Derived engineered feature | Credit structure and account behavior metric |
| term_installment_pressure | Derived engineered feature | Long-term repayment structure metric |
| term_loan_burden | Derived engineered feature | Long-term repayment structure metric |
| term_months | Derived engineered feature | Long-term repayment structure metric |
| trade_density | Derived engineered feature | Credit structure and account behavior metric |
| util_per_account | Derived engineered feature | Credit utilization and liquidity pressure metric |
| utilization_dti_stress | Derived engineered feature | Financial stress and leverage pressure indicator |
| utilization_per_fico | Derived engineered feature | Credit utilization and liquidity pressure metric |
| utilization_shock_risk | Derived engineered feature | Credit utilization and liquidity pressure metric |

---

# Technology Stack

- Python
- Pandas
- NumPy
- CatBoost
- Scikit-Learn
- Jupyter Notebooks

---

# Validation Framework

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
- Stress Testing
- IFRS9 Lifetime Risk
- Collections Optimization

---

# Author

Mustafa Alhamdi
