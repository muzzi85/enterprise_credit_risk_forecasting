# Enterprise Credit Risk Forecasting
# Model Architecture, Training Strategy, and Results

## 1. Probability of Default (PD) Model

### Business Objective

The Probability of Default (PD) model estimates the likelihood that a borrower will fail to repay the loan and enter default status during the loan lifecycle. The model is designed for underwriting-stage risk assessment using only origination-time information.

---

### Modeling Framework

| Component           | Description                                                      |
| ------------------- | ---------------------------------------------------------------- |
| Model Type          | Binary Classification                                            |
| Algorithm           | CatBoostClassifier                                               |
| Target Variable     | TARGET (1 = Default, 0 = Non-Default)                            |
| Validation Strategy | Stratified 5-Fold Cross Validation                               |
| Optimization Metric | PR-AUC and ROC-AUC                                               |
| Imbalance Handling  | Auto Class Weights / Balanced Loss                               |
| Feature Engineering | Advanced enterprise credit risk ratios and interaction variables |
| Leakage Protection  | Post-default and future repayment variables removed              |

---

### Feature Engineering Categories

The PD model includes engineered features across:

* Borrower affordability
* Credit utilization
* Liquidity pressure
* Credit expansion velocity
* Delinquency intensity
* Repayment burden
* Long-term leverage stress
* Credit structure complexity
* FICO-adjusted leverage metrics

---

### Model Training Strategy

The model was trained using:

* Stratified K-Fold validation to preserve default distribution
* Out-of-fold prediction generation
* CatBoost native categorical handling
* Hyperparameter optimization
* PR-AUC optimization to improve minority default detection
* Threshold optimization for business risk calibration

---

### PD Model Results

| Metric                 | Value             |
| ---------------------- | ----------------- |
| Mean ROC-AUC           | 0.7367            |
| Mean PR-AUC            | 0.4486            |
| Cross-Validation Folds | 5                 |
| Validation Strategy    | Stratified K-Fold |
| Dataset Size           | 44,006 loans      |
| Engineered Features    | 142               |

---

### Threshold Performance Matrix

| Threshold | Precision | Recall |
| --------- | --------- | ------ |
| 0.30      | 0.259     | 0.928  |
| 0.40      | 0.293     | 0.819  |
| 0.50      | 0.346     | 0.682  |
| 0.60      | 0.394     | 0.478  |
| 0.70      | 0.488     | 0.311  |

---

### Business Interpretation

The PD model successfully captures borrower default behavior using enterprise-style leverage, utilization, affordability, and credit structure features. The model prioritizes high recall at lower thresholds to minimize missed default events while maintaining acceptable precision levels for underwriting operations.

---

# 2. Loss Given Default (LGD) Model

### Business Objective

The Loss Given Default (LGD) model estimates the percentage financial loss incurred if a borrower defaults. The model focuses on recovery severity and post-default economic exposure.

---

### Modeling Framework

| Component           | Description                                                          |
| ------------------- | -------------------------------------------------------------------- |
| Model Type          | Regression                                                           |
| Algorithm           | CatBoostRegressor                                                    |
| Target Variable     | LGD                                                                  |
| Validation Strategy | 5-Fold Cross Validation                                              |
| Optimization Metric | RMSE / R²                                                            |
| Feature Engineering | Recovery severity and liquidity stress variables                     |
| Leakage Protection  | Recovery, settlement, and post-default operational variables removed |

---

### Feature Engineering Categories

The LGD model includes engineered features across:

* Liquidity exhaustion
* Debt saturation
* Revolving dependency
* Financial stress amplification
* Borrower complexity
* Credit fatigue
* Recovery sensitivity
* Structural leverage persistence

---

### Model Training Strategy

The model was trained using:

* K-Fold regression validation
* Continuous bounded target modeling
* Heavy regularization to stabilize regression outputs
* CatBoost native categorical encoding
* Feature interaction engineering
* Exposure and recovery behavior modeling

---

### LGD Model Results

| Metric              | Value                   |
| ------------------- | ----------------------- |
| Validation Strategy | 5-Fold Cross Validation |
| Model Type          | Regression              |
| Loss Function       | RMSE                    |
| Engineered Features | 150+                    |
| Training Dataset    | Defaulted Loans Only    |

---

### Business Interpretation

The LGD model captures how severe losses become after borrower default by modeling borrower liquidity exhaustion, leverage dependency, debt complexity, and repayment sustainability. The model provides an enterprise-style recovery severity estimation framework.

---

# 3. Exposure at Default (EAD) Model

### Business Objective

The Exposure at Default (EAD) model estimates the remaining loan exposure outstanding when borrower default occurs.

---

### Modeling Framework

| Component           | Description                                                       |
| ------------------- | ----------------------------------------------------------------- |
| Model Type          | Regression                                                        |
| Algorithm           | CatBoostRegressor                                                 |
| Target Variable     | EAD Ratio                                                         |
| Validation Strategy | 5-Fold Cross Validation                                           |
| Optimization Metric | RMSE / R²                                                         |
| Feature Engineering | Amortization and exposure persistence variables                   |
| Leakage Protection  | Remaining principal and post-default collection variables removed |

---

### Feature Engineering Categories

The EAD model includes engineered features across:

* Amortization dynamics
* Exposure persistence
* Installment pressure
* Repayment velocity
* Utilization expansion
* Credit acceleration
* Exposure stress amplification
* Borrower leverage sustainability

---

### Model Training Strategy

The model was trained using:

* K-Fold regression validation
* Exposure ratio normalization
* Bounded prediction clipping
* CatBoost native categorical processing
* Advanced exposure interaction engineering
* Liquidity and repayment burden modeling

---

### EAD Model Results

| Metric              | Value                    |
| ------------------- | ------------------------ |
| Validation Strategy | 5-Fold Cross Validation  |
| Model Type          | Regression               |
| Engineered Features | 150+                     |
| Target              | Remaining Exposure Ratio |
| Dataset             | Defaulted Loans Only     |

---

### Business Interpretation

The EAD model captures how much exposure remains outstanding when borrower failure occurs. The model focuses on amortization structure, repayment burden, leverage expansion, and exposure persistence dynamics.

---

# Enterprise Expected Loss Framework

The final enterprise risk framework integrates all three models:

Expected Loss (EL):

EL = PD × LGD × EAD

Where:

* PD estimates probability of borrower default
* LGD estimates severity of financial loss
* EAD estimates remaining exposure at failure

This creates a full enterprise-grade credit risk forecasting architecture suitable for underwriting, portfolio risk estimation, and future pricing optimization systems.

# PD Features

| Feature | Formula | Business Meaning | Customer Example |
|---|---|---|---|
| credit_history_months | `( pd_df["issue_d"] - pd_df["earliest_cr_line"] ) .dt.days / 30` | credit_history_months is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ( pd_df["issue_d"] - pd_df["earliest_cr_line"] ) .dt.days / 30... |
| revol_util_ratio | `pd_df["revol_util"] / 100` | revol_util_ratio is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: pd_df["revol_util"] / 100... |
| bc_util_ratio | `pd_df["bc_util"] / 100` | bc_util_ratio is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: pd_df["bc_util"] / 100... |
| all_util_ratio | `pd_df["all_util"] / 100` | all_util_ratio is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: pd_df["all_util"] / 100... |
| installment_income_ratio | `pd_df["installment"] / pd_df["annual_inc"]` | installment_income_ratio is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: pd_df["installment"] / pd_df["annual_inc"]... |
| loan_income_ratio | `pd_df["loan_amnt"] / pd_df["annual_inc"]` | loan_income_ratio is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: pd_df["loan_amnt"] / pd_df["annual_inc"]... |
| accounts_per_year | `pd_df["total_acc"] / ( (pd_df["credit_history_months"] / 12) + 1 )` | accounts_per_year is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: pd_df["total_acc"] / ( (pd_df["credit_history_months"] / 12) + 1 )... |
| issue_year | `pd_df["issue_d"].dt.year` | issue_year is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: pd_df["issue_d"].dt.year... |
| issue_month | `pd_df["issue_d"].dt.month` | issue_month is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: pd_df["issue_d"].dt.month... |
| issue_quarter | `pd_df["issue_d"].dt.quarter` | issue_quarter is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: pd_df["issue_d"].dt.quarter... |
| term_months | `pd_df["term"] .str.extract(r"(\d+)") .astype(float)` | term_months is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: pd_df["term"] .str.extract(r"(\d+)") .astype(float)... |
| emp_length_years | `pd_df["emp_length_years"] .fillna(0)` | emp_length_years is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: pd_df["emp_length_years"] .fillna(0)... |
| emp_title_clean | `pd_df["emp_title_clean"] .fillna("unknown")` | emp_title_clean is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: pd_df["emp_title_clean"] .fillna("unknown")... |
| TARGET | `df_pd["loan_status"] .isin(bad_status)` | TARGET is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: df_pd["loan_status"] .isin(bad_status)... |
| job_cluster | `pd_df["job_cluster"] .fillna("other")` | job_cluster is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: pd_df["job_cluster"] .fillna("other")... |
| zip_region | `pd_df["zip_code"] .str.extract(r"(\d+)")` | zip_region is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: pd_df["zip_code"] .str.extract(r"(\d+)")... |
| remaining_exposure | `lgd_df["remaining_exposure"] .clip(lower=1)` | remaining_exposure is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["remaining_exposure"] .clip(lower=1)... |
| recovery_rate | `lgd_df["recovery_rate"] .clip(0,1)` | recovery_rate is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["recovery_rate"] .clip(0,1)... |
| LGD_TARGET | `1 - lgd_df["recovery_rate"]` | LGD_TARGET is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: 1 - lgd_df["recovery_rate"]... |
| EAD_TARGET | `ead_df["EAD_TARGET"] .clip(lower=0)` | EAD_TARGET is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["EAD_TARGET"] .clip(lower=0)... |
| expected_loss | `pricing_df["predicted_PD"] * pricing_df["predicted_LGD"] * pricing_df["predicted_EAD"]` | expected_loss is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: pricing_df["predicted_PD"] * pricing_df["predicted_LGD"] * pricing_df["predicted_EAD"]... |
| loan_age_proxy | `2020 - forecast_df["issue_year"]` | loan_age_proxy is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: 2020 - forecast_df["issue_year"]... |
| stress_score | `forecast_df["dti"] * forecast_df["revol_util_ratio"]` | stress_score is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: forecast_df["dti"] * forecast_df["revol_util_ratio"]... |
| credit_hunger | `forecast_df["inq_last_6mths"] / ( forecast_df["total_acc"] + 1 )` | credit_hunger is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: forecast_df["inq_last_6mths"] / ( forecast_df["total_acc"] + 1 )... |
| delinq_intensity | `forecast_df["delinq_2yrs"] / ( forecast_df["credit_history_months"] + 1 )` | delinq_intensity is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: forecast_df["delinq_2yrs"] / ( forecast_df["credit_history_months"] + 1 )... |

# LGD Features

| Feature | Formula | Business Meaning | Customer Example |
|---|---|---|---|
| LGD | `lgd_df["LGD"] .clip(0, 1)` | LGD is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["LGD"] .clip(0, 1)... |
| credit_history_months | `( lgd_df["issue_d"] - lgd_df["earliest_cr_line"] ) .dt.days / 30` | credit_history_months is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ( lgd_df["issue_d"] - lgd_df["earliest_cr_line"] ) .dt.days / 30... |
| term_months | `lgd_df["term"] .str.extract(r"(\d+)") .astype(float)` | term_months is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["term"] .str.extract(r"(\d+)") .astype(float)... |
| emp_length_years | `lgd_df["emp_length_years"] .fillna(0)` | emp_length_years is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["emp_length_years"] .fillna(0)... |
| monthly_income | `lgd_df["annual_inc"] / 12` | monthly_income is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["annual_inc"] / 12... |
| loan_to_income | `lgd_df["loan_amnt"] / ( lgd_df["annual_inc"] + 1 )` | loan_to_income is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["loan_amnt"] / ( lgd_df["annual_inc"] + 1 )... |
| installment_to_income | `lgd_df["installment"] / ( lgd_df["monthly_income"] + 1 )` | installment_to_income is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["installment"] / ( lgd_df["monthly_income"] + 1 )... |
| revolving_to_income | `lgd_df["revol_bal"] / ( lgd_df["annual_inc"] + 1 )` | revolving_to_income is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["revol_bal"] / ( lgd_df["annual_inc"] + 1 )... |
| debt_saturation | `lgd_df["total_bal_ex_mort"] / ( lgd_df["annual_inc"] + 1 )` | debt_saturation is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["total_bal_ex_mort"] / ( lgd_df["annual_inc"] + 1 )... |
| liquidity_exhaustion | `lgd_df["revol_bal"] / ( lgd_df["total_rev_hi_lim"] + 1 )` | liquidity_exhaustion is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["revol_bal"] / ( lgd_df["total_rev_hi_lim"] + 1 )... |
| utilization_dti_stress | `lgd_df["revol_util"] * lgd_df["dti"]` | utilization_dti_stress is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["revol_util"] * lgd_df["dti"]... |
| bcutil_dti_stress | `lgd_df["bc_util"] * lgd_df["dti"]` | bcutil_dti_stress is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["bc_util"] * lgd_df["dti"]... |
| util_per_account | `lgd_df["revol_bal"] / ( lgd_df["open_acc"] + 1 )` | util_per_account is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["revol_bal"] / ( lgd_df["open_acc"] + 1 )... |
| remaining_revolving_capacity | `lgd_df["total_rev_hi_lim"] - lgd_df["revol_bal"]` | remaining_revolving_capacity is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["total_rev_hi_lim"] - lgd_df["revol_bal"]... |
| remaining_bc_capacity | `lgd_df["total_bc_limit"] - ( ( lgd_df["bc_util"] / 100 ) * lgd_df["total_bc_limit"] )` | remaining_bc_capacity is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["total_bc_limit"] - ( ( lgd_df["bc_util"] / 100 ) * lgd_df["total_bc_limit"] )... |
| credit_hunger | `lgd_df["inq_last_6mths"] + lgd_df["acc_open_past_24mths"]` | credit_hunger is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["inq_last_6mths"] + lgd_df["acc_open_past_24mths"]... |
| inq_per_year | `lgd_df["inq_last_6mths"] / ( ( lgd_df["credit_history_months"] / 12 ) + 1 )` | inq_per_year is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["inq_last_6mths"] / ( ( lgd_df["credit_history_months"] / 12 ) + 1 )... |
| recent_account_pressure | `lgd_df["num_tl_op_past_12m"] / ( lgd_df["total_acc"] + 1 )` | recent_account_pressure is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["num_tl_op_past_12m"] / ( lgd_df["total_acc"] + 1 )... |
| leverage_acceleration | `lgd_df["revol_util"] * lgd_df["acc_open_past_24mths"] * ( lgd_df["inq_last_6mths"] + 1 )` | leverage_acceleration is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["revol_util"] * lgd_df["acc_open_past_24mths"] * ( lgd_df["inq_last_6mths"] + 1 )... |
| utilization_shock_risk | `lgd_df["revol_util"] * ( lgd_df["num_tl_op_past_12m"] + 1 )` | utilization_shock_risk is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["revol_util"] * ( lgd_df["num_tl_op_past_12m"] + 1 )... |
| avg_account_age | `lgd_df["credit_history_months"] / ( lgd_df["total_acc"] + 1 )` | avg_account_age is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["credit_history_months"] / ( lgd_df["total_acc"] + 1 )... |
| trade_density | `lgd_df["total_acc"] / ( ( lgd_df["credit_history_months"] / 12 ) + 1 )` | trade_density is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["total_acc"] / ( ( lgd_df["credit_history_months"] / 12 ) + 1 )... |
| credit_velocity | `lgd_df["total_acc"] / ( ( lgd_df["credit_history_months"] / 12 ) + 1 )` | credit_velocity is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["total_acc"] / ( ( lgd_df["credit_history_months"] / 12 ) + 1 )... |
| credit_fatigue | `lgd_df["num_actv_rev_tl"] / ( ( lgd_df["credit_history_months"] / 12 ) + 1 )` | credit_fatigue is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["num_actv_rev_tl"] / ( ( lgd_df["credit_history_months"] / 12 ) + 1 )... |
| revolving_dependency | `lgd_df["revol_bal"] / ( lgd_df["total_bal_ex_mort"] + 1 )` | revolving_dependency is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["revol_bal"] / ( lgd_df["total_bal_ex_mort"] + 1 )... |
| revolving_trade_share | `lgd_df["num_rev_accts"] / ( lgd_df["total_acc"] + 1 )` | revolving_trade_share is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["num_rev_accts"] / ( lgd_df["total_acc"] + 1 )... |
| installment_trade_share | `lgd_df["num_il_tl"] / ( lgd_df["total_acc"] + 1 )` | installment_trade_share is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["num_il_tl"] / ( lgd_df["total_acc"] + 1 )... |
| active_trade_ratio | `lgd_df["num_actv_rev_tl"] / ( lgd_df["total_acc"] + 1 )` | active_trade_ratio is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["num_actv_rev_tl"] / ( lgd_df["total_acc"] + 1 )... |
| open_account_ratio | `lgd_df["open_acc"] / ( lgd_df["total_acc"] + 1 )` | open_account_ratio is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["open_acc"] / ( lgd_df["total_acc"] + 1 )... |
| account_fragmentation | `lgd_df["open_acc"] / ( lgd_df["num_rev_accts"] + 1 )` | account_fragmentation is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["open_acc"] / ( lgd_df["num_rev_accts"] + 1 )... |
| borrower_complexity | `lgd_df["open_acc"] + lgd_df["num_rev_accts"] + lgd_df["num_il_tl"] + lgd_df["acc_open_past_24mths"]` | borrower_complexity is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["open_acc"] + lgd_df["num_rev_accts"] + lgd_df["num_il_tl"] + lgd_df["acc_open_past... |
| dti_per_fico | `lgd_df["dti"] / ( lgd_df["fico_range_low"] + 1 )` | dti_per_fico is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["dti"] / ( lgd_df["fico_range_low"] + 1 )... |
| loan_per_fico | `lgd_df["loan_amnt"] / ( lgd_df["fico_range_low"] + 1 )` | loan_per_fico is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["loan_amnt"] / ( lgd_df["fico_range_low"] + 1 )... |
| utilization_per_fico | `lgd_df["revol_util"] / ( lgd_df["fico_range_low"] + 1 )` | utilization_per_fico is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["revol_util"] / ( lgd_df["fico_range_low"] + 1 )... |
| fico_stress | `lgd_df["revol_util"] * lgd_df["dti"] / ( lgd_df["fico_range_low"] + 1 )` | fico_stress is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["revol_util"] * lgd_df["dti"] / ( lgd_df["fico_range_low"] + 1 )... |
| credit_resilience | `lgd_df["credit_history_months"] * ( lgd_df["fico_range_low"] + 1 ) / ( lgd_df["dti"] + 1 )` | credit_resilience is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["credit_history_months"] * ( lgd_df["fico_range_low"] + 1 ) / ( lgd_df["dti"] + 1 )... |
| delinq_intensity | `lgd_df["delinq_2yrs"] / ( ( lgd_df["credit_history_months"] / 12 ) + 1 )` | delinq_intensity is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["delinq_2yrs"] / ( ( lgd_df["credit_history_months"] / 12 ) + 1 )... |
| derogatory_pressure | `lgd_df["pub_rec"] + lgd_df["pub_rec_bankruptcies"]` | derogatory_pressure is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["pub_rec"] + lgd_df["pub_rec_bankruptcies"]... |
| long_term_stress | `lgd_df["term_months"] * lgd_df["installment_to_income"]` | long_term_stress is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["term_months"] * lgd_df["installment_to_income"]... |
| term_loan_burden | `lgd_df["term_months"] * lgd_df["loan_to_income"]` | term_loan_burden is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["term_months"] * lgd_df["loan_to_income"]... |
| payment_stress_amplification | `lgd_df["installment_to_income"] * lgd_df["dti"]` | payment_stress_amplification is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["installment_to_income"] * lgd_df["dti"]... |
| issue_year | `lgd_df["issue_d"] .dt.year` | issue_year is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["issue_d"] .dt.year... |
| issue_month | `lgd_df["issue_d"] .dt.month` | issue_month is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["issue_d"] .dt.month... |
| issue_quarter | `lgd_df["issue_d"] .dt.quarter` | issue_quarter is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: lgd_df["issue_d"] .dt.quarter... |

# EAD Features

| Feature | Formula | Business Meaning | Customer Example |
|---|---|---|---|
| EAD_ratio | `ead_df["EAD_ratio"] .clip(0, 1)` | EAD_ratio is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["EAD_ratio"] .clip(0, 1)... |
| credit_history_months | `( ead_df["issue_d"] - ead_df["earliest_cr_line"] ) .dt.days / 30` | credit_history_months is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ( ead_df["issue_d"] - ead_df["earliest_cr_line"] ) .dt.days / 30... |
| term_months | `ead_df["term"] .str.extract(r"(\d+)") .astype(float)` | term_months is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["term"] .str.extract(r"(\d+)") .astype(float)... |
| emp_length_years | `ead_df["emp_length_years"] .fillna(0)` | emp_length_years is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["emp_length_years"] .fillna(0)... |
| monthly_income | `ead_df["annual_inc"] / 12` | monthly_income is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["annual_inc"] / 12... |
| loan_to_income | `ead_df["loan_amnt"] / ( ead_df["annual_inc"] + 1 )` | loan_to_income is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["loan_amnt"] / ( ead_df["annual_inc"] + 1 )... |
| installment_to_income | `ead_df["installment"] / ( ead_df["monthly_income"] + 1 )` | installment_to_income is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["installment"] / ( ead_df["monthly_income"] + 1 )... |
| revolving_to_income | `ead_df["revol_bal"] / ( ead_df["annual_inc"] + 1 )` | revolving_to_income is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["revol_bal"] / ( ead_df["annual_inc"] + 1 )... |
| debt_saturation | `ead_df["total_bal_ex_mort"] / ( ead_df["annual_inc"] + 1 )` | debt_saturation is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["total_bal_ex_mort"] / ( ead_df["annual_inc"] + 1 )... |
| liquidity_exhaustion | `ead_df["revol_bal"] / ( ead_df["total_rev_hi_lim"] + 1 )` | liquidity_exhaustion is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["revol_bal"] / ( ead_df["total_rev_hi_lim"] + 1 )... |
| utilization_dti_stress | `ead_df["revol_util"] * ead_df["dti"]` | utilization_dti_stress is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["revol_util"] * ead_df["dti"]... |
| bcutil_dti_stress | `ead_df["bc_util"] * ead_df["dti"]` | bcutil_dti_stress is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["bc_util"] * ead_df["dti"]... |
| util_per_account | `ead_df["revol_bal"] / ( ead_df["open_acc"] + 1 )` | util_per_account is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["revol_bal"] / ( ead_df["open_acc"] + 1 )... |
| remaining_revolving_capacity | `ead_df["total_rev_hi_lim"] - ead_df["revol_bal"]` | remaining_revolving_capacity is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["total_rev_hi_lim"] - ead_df["revol_bal"]... |
| remaining_bc_capacity | `ead_df["total_bc_limit"] - ( ( ead_df["bc_util"] / 100 ) * ead_df["total_bc_limit"] )` | remaining_bc_capacity is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["total_bc_limit"] - ( ( ead_df["bc_util"] / 100 ) * ead_df["total_bc_limit"] )... |
| credit_hunger | `ead_df["inq_last_6mths"] + ead_df["acc_open_past_24mths"]` | credit_hunger is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["inq_last_6mths"] + ead_df["acc_open_past_24mths"]... |
| inq_per_year | `ead_df["inq_last_6mths"] / ( ( ead_df["credit_history_months"] / 12 ) + 1 )` | inq_per_year is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["inq_last_6mths"] / ( ( ead_df["credit_history_months"] / 12 ) + 1 )... |
| recent_account_pressure | `ead_df["num_tl_op_past_12m"] / ( ead_df["total_acc"] + 1 )` | recent_account_pressure is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["num_tl_op_past_12m"] / ( ead_df["total_acc"] + 1 )... |
| leverage_acceleration | `ead_df["revol_util"] * ead_df["acc_open_past_24mths"] * ( ead_df["inq_last_6mths"] + 1 )` | leverage_acceleration is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["revol_util"] * ead_df["acc_open_past_24mths"] * ( ead_df["inq_last_6mths"] + 1 )... |
| utilization_shock_risk | `ead_df["revol_util"] * ( ead_df["num_tl_op_past_12m"] + 1 )` | utilization_shock_risk is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["revol_util"] * ( ead_df["num_tl_op_past_12m"] + 1 )... |
| avg_account_age | `ead_df["credit_history_months"] / ( ead_df["total_acc"] + 1 )` | avg_account_age is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["credit_history_months"] / ( ead_df["total_acc"] + 1 )... |
| trade_density | `ead_df["total_acc"] / ( ( ead_df["credit_history_months"] / 12 ) + 1 )` | trade_density is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["total_acc"] / ( ( ead_df["credit_history_months"] / 12 ) + 1 )... |
| credit_velocity | `ead_df["total_acc"] / ( ( ead_df["credit_history_months"] / 12 ) + 1 )` | credit_velocity is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["total_acc"] / ( ( ead_df["credit_history_months"] / 12 ) + 1 )... |
| credit_fatigue | `ead_df["num_actv_rev_tl"] / ( ( ead_df["credit_history_months"] / 12 ) + 1 )` | credit_fatigue is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["num_actv_rev_tl"] / ( ( ead_df["credit_history_months"] / 12 ) + 1 )... |
| revolving_dependency | `ead_df["revol_bal"] / ( ead_df["total_bal_ex_mort"] + 1 )` | revolving_dependency is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["revol_bal"] / ( ead_df["total_bal_ex_mort"] + 1 )... |
| revolving_trade_share | `ead_df["num_rev_accts"] / ( ead_df["total_acc"] + 1 )` | revolving_trade_share is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["num_rev_accts"] / ( ead_df["total_acc"] + 1 )... |
| installment_trade_share | `ead_df["num_il_tl"] / ( ead_df["total_acc"] + 1 )` | installment_trade_share is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["num_il_tl"] / ( ead_df["total_acc"] + 1 )... |
| active_trade_ratio | `ead_df["num_actv_rev_tl"] / ( ead_df["total_acc"] + 1 )` | active_trade_ratio is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["num_actv_rev_tl"] / ( ead_df["total_acc"] + 1 )... |
| open_account_ratio | `ead_df["open_acc"] / ( ead_df["total_acc"] + 1 )` | open_account_ratio is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["open_acc"] / ( ead_df["total_acc"] + 1 )... |
| account_fragmentation | `ead_df["open_acc"] / ( ead_df["num_rev_accts"] + 1 )` | account_fragmentation is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["open_acc"] / ( ead_df["num_rev_accts"] + 1 )... |
| borrower_complexity | `ead_df["open_acc"] + ead_df["num_rev_accts"] + ead_df["num_il_tl"] + ead_df["acc_open_past_24mths"]` | borrower_complexity is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["open_acc"] + ead_df["num_rev_accts"] + ead_df["num_il_tl"] + ead_df["acc_open_past... |
| dti_per_fico | `ead_df["dti"] / ( ead_df["fico_range_low"] + 1 )` | dti_per_fico is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["dti"] / ( ead_df["fico_range_low"] + 1 )... |
| loan_per_fico | `ead_df["loan_amnt"] / ( ead_df["fico_range_low"] + 1 )` | loan_per_fico is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["loan_amnt"] / ( ead_df["fico_range_low"] + 1 )... |
| utilization_per_fico | `ead_df["revol_util"] / ( ead_df["fico_range_low"] + 1 )` | utilization_per_fico is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["revol_util"] / ( ead_df["fico_range_low"] + 1 )... |
| fico_stress | `ead_df["revol_util"] * ead_df["dti"] / ( ead_df["fico_range_low"] + 1 )` | fico_stress is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["revol_util"] * ead_df["dti"] / ( ead_df["fico_range_low"] + 1 )... |
| credit_resilience | `ead_df["credit_history_months"] * ( ead_df["fico_range_low"] + 1 ) / ( ead_df["dti"] + 1 )` | credit_resilience is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["credit_history_months"] * ( ead_df["fico_range_low"] + 1 ) / ( ead_df["dti"] + 1 )... |
| delinq_intensity | `ead_df["delinq_2yrs"] / ( ( ead_df["credit_history_months"] / 12 ) + 1 )` | delinq_intensity is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["delinq_2yrs"] / ( ( ead_df["credit_history_months"] / 12 ) + 1 )... |
| derogatory_pressure | `ead_df["pub_rec"] + ead_df["pub_rec_bankruptcies"]` | derogatory_pressure is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["pub_rec"] + ead_df["pub_rec_bankruptcies"]... |
| long_term_stress | `ead_df["term_months"] * ead_df["installment_to_income"]` | long_term_stress is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["term_months"] * ead_df["installment_to_income"]... |
| term_loan_burden | `ead_df["term_months"] * ead_df["loan_to_income"]` | term_loan_burden is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["term_months"] * ead_df["loan_to_income"]... |
| payment_stress_amplification | `ead_df["installment_to_income"] * ead_df["dti"]` | payment_stress_amplification is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["installment_to_income"] * ead_df["dti"]... |
| issue_year | `ead_df["issue_d"] .dt.year` | issue_year is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["issue_d"] .dt.year... |
| issue_month | `ead_df["issue_d"] .dt.month` | issue_month is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["issue_d"] .dt.month... |
| issue_quarter | `ead_df["issue_d"] .dt.quarter` | issue_quarter is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["issue_d"] .dt.quarter... |
| installment_per_loan | `ead_df["installment"] / ( ead_df["loan_amnt"] + 1 )` | installment_per_loan is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["installment"] / ( ead_df["loan_amnt"] + 1 )... |
| term_installment_pressure | `ead_df["term_months"] * ead_df["installment"]` | term_installment_pressure is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["term_months"] * ead_df["installment"]... |
| exposure_pressure | `ead_df["loan_amnt"] * ead_df["revol_util"]` | exposure_pressure is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["loan_amnt"] * ead_df["revol_util"]... |
| exposure_income_stress | `ead_df["loan_amnt"] * ead_df["dti"] / ( ead_df["annual_inc"] + 1 )` | exposure_income_stress is an engineered enterprise credit risk feature designed to capture borrower leverage, affordability, liquidity, utilization, repayment behavior, or exposure dynamics depending on the underlying variables. | Customer example generated using variables from formula: ead_df["loan_amnt"] * ead_df["dti"] / ( ead_df["annual_inc"] + 1 )... |
