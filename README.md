# Bank Marketing - Term Deposit Subscription

**Type:** Classification

## Goal
A bank called its clients in a **marketing campaign**. Predict whether the client will **subscribe to a term deposit (1)** or **not (0)**.

**Target:** `subscribed` (1 = yes, 0 = no)

## Data
The data is **synthetic** (fake, created with Python) so it can be big and have all the problems we want to practice on.

- **Generator:** `generate_data.py` (random seed = 123, so you always get the same data)
- **File:** `data/bank_marketing_calls.csv` - about 140,000 rows (+0.5% duplicate rows) and 51 columns
- **Data types in the file:** int, float, text (object) and dates saved as text

Re-create (or change) the data:
```bash
python generate_data.py
```
Change `N_ROWS` (100,000 - 300,000) or `SEED` at the top of `generate_data.py` to get a bigger / different dataset.

### Problems added on purpose
- **ID / personal columns** (IDs, names, e-mail, phone, date of birth)
- **Messy column names** (`Annual Income ($)`, `CreditScore`, `No. of Dependents`, ...)
- **Short codes** that should be full values (`M`/`F`, `Y`/`N`, `T`/`F`, `FT`/`PT`, ...)
- **Numbers saved as text** (`$1,250.00`, `12,500`, `34 yrs`, `12.5 km`, `15%`)
- **Missing values**, also hidden as text (`N/A`, `unknown`, `?`, `-`, a space) and one **mostly empty** column
- **Outliers** (extreme values) and **impossible values** (age = 150, negative mileage, 9999 codes, ...)
- **Dates saved as text** in different formats
- **Messy text** (`New York`, `NEW YORK`, `  new york`)
- **Categories saved as numbers** (`region_code`)
- **Useless columns**: a constant column, an almost-constant column, a duplicate column
- **Duplicate rows** (~0.5%)

### Columns
| Column in the file | Clean name | Problems |
|---|---|---|
| `Client ID` | `client_id` | ID / personal detail -> drop |
| `Full Name` | `full_name` | ID / personal detail -> drop |
| `E-mail` | `email` | ID / personal detail -> drop |
| `Phone #` | `phone_number` | ID / personal detail -> drop |
| `DOB` | `date_of_birth` | ID / personal detail -> drop |
| `Age (Years)` | `age` | number saved as text (`12 yrs`); 2% missing; wrong values: `-1`, `0`, `130` |
| `Gender` | `gender` | short codes: `M`, `F` |
| `Marital Status` | `marital_status` | short codes: `S`, `M`, `D`, `W` |
| `Education` | `education` | short codes: `HS`, `BSC`, `MSC`, `PHD`; 3% missing |
| `City` | `city` | 1% missing; mixed upper / lower case, extra spaces |
| `Region Code` | `region_code` | category saved as a number |
| `Preferred Language` | `preferred_language` | - |
| `Marketing Opt-In` | `marketing_opt_in` | short codes: `Y`, `N` |
| `Agent Id` | `agent_id` | ID / personal detail -> drop |
| `Call Id` | `call_id` | ID / personal detail -> drop |
| `Job` | `job` | 1% missing; mixed upper / lower case, extra spaces |
| `default` | `has_credit_default` | short codes: `Y`, `N` |
| `Avg Yearly Balance` | `avg_yearly_balance` | number saved as text (`$1,250.00`); extreme outliers |
| `housing` | `has_housing_loan` | short codes: `Y`, `N` |
| `loan` | `has_personal_loan` | short codes: `Y`, `N` |
| `Contact Type` | `contact_type` | short codes: `CELL`, `TEL` |
| `Last Contact Date` | `last_contact_date` | date saved as text (`%d/%m/%Y`) |
| `campaign` | `num_contacts_campaign` | wrong values: `99`, `150` |
| `pdays` | `days_since_prev_campaign` | - |
| `previous` | `num_prev_contacts` | - |
| `poutcome` | `prev_campaign_outcome` | - |
| `emp.var.rate` | `employment_variation_rate` | - |
| `cons.price.idx` | `consumer_price_index` | - |
| `cons.conf.idx` | `consumer_confidence_index` | 1% missing |
| `euribor3m` | `euribor_3m` | - |
| `nr.employed` | `num_employees_k` | - |
| `Annual Income` | `annual_income` | number saved as text (`$1,250.00`); 5% missing; extreme outliers |
| `Savings Account Balance` | `savings_account_balance` | number saved as text (`1,250.00`); 6% missing |
| `Num Bank Products` | `num_bank_products` | - |
| `Has Investment Account` | `has_investment_account` | - |
| `Online Banking User` | `online_banking_user` | - |
| `Mobile App Logins Month` | `mobile_app_logins_month` | - |
| `Customer Since Date` | `customer_since_date` | date saved as text (`%Y-%m-%d`) |
| `Credit Card Holder` | `credit_card_holder` | - |
| `Avg Monthly Card Spend` | `avg_monthly_card_spend` | number saved as text (`$1,250.00`); 4% missing |
| `Preferred Contact Time` | `preferred_contact_time` | 3% missing; mixed upper / lower case, extra spaces |
| `Agent Experience Years` | `agent_experience_years` | wrong values: `-5`, `80` |
| `Call Center Region` | `call_center_region` | short codes: `N`, `S`, `E`, `W` |
| `Campaign Channel` | `campaign_channel` | mixed upper / lower case, extra spaces |
| `Notes Code` | `notes_code` | 85% missing (mostly empty) |
| `Call Duration Sec` | `call_duration_sec` | ID / personal detail -> drop; number saved as text (`12 sec`) |
| `Campaign Name` | `campaign_name` | only one value |
| `Bank` | `bank` | only one value |
| `Is Test Call` | `is_test_call` | almost only one value |
| `Products Count` | `products_count` | exact copy of `num_bank_products` |
| `y` | `subscribed` | **TARGET** (text: 'yes', 'no') |

## Files
| File | Description |
|---|---|
| `generate_data.py` | Creates the dataset |
| `bank_marketing_term_deposit.ipynb` | The full project notebook |
| `data/bank_marketing_calls.csv` | The dataset |
| `data/bank_marketing_calls_metadata.xlsx` | Metadata (data dictionary): description, role, data types, missing values, statistics, codes and problems of every column |
| `models/` | Created when you run the notebook (best model, scaler, selected features) |

## Steps in the notebook
**A. Data cleaning**
- A0 Drop columns that are not useful (IDs, names, ...)
- A1 Rename columns
- A2 Data types: numbers saved as text, messy text, short codes, dates, categories saved as numbers, target to 0/1
- A3 Missing values - % per column, drop columns > 50% missing, fill the rest (mean / mode)
- A4 Outliers - box plots, IQR table, wrong values -> median, extreme values -> capping
- A5 Duplicate rows and duplicate columns
- A6 Columns with only one value

**B. EDA**
- B1 Target count and percentage
- B2 Correlation heatmap and correlation with the target
- B3 Charts for the independent features (and features vs target)

**C. Feature engineering**
- C1 Binning / new features
- C2 Label encoding and one-hot encoding
- C3 Data splitting: 80% train / 15% test / 5% unseen (random_state = 123)
- C4 Feature scaling (StandardScaler)
- C5 Feature selection (SelectKBest + Random Forest importance)
- C6 20,000-row training sample for comparing / tuning the models

**D. Feature extraction**
- D1 PCA
- D2 Factor Analysis
- D3 Cluster analysis (K-Means with elbow and silhouette)

**E. Machine learning (20 models)**
- E1 Define the models
- E2 Evaluation function (all metrics)
- E3 Cross validation (5 folds)
- E4 Training and testing evaluation
- E5 Hyperparameter dictionary
- E6 Grid search tuning
- E7 Compare the models, select the best, re-train it on the full training data
- E8 Save the best model (`joblib`)
- E9 Load the model
- E10 Predict and evaluate on the 5% unseen data

## How to run
```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost joblib jupyter
python generate_data.py
```
Then open `bank_marketing_term_deposit.ipynb` **from this folder** and run all cells (the full notebook takes about 10-20 minutes).
