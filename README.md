# Banking EDA Risk Analytics Dashboard

A complete, end-to-end banking risk analytics mini-project that covers **Exploratory Data Analysis (EDA)** in Python and an interactive **Power BI dashboard** for lending & deposit risk signals.

---

## 🔎 What this project does

- Cleans and engineers banking client data (engagement days, income bands, processing fee rules).
- Computes core KPIs for risk and growth tracking (clients, loans, deposits, fees).
- Visualizes **Loan Analysis**, **Deposit Analysis**, and a **Summary Dashboard** in Power BI.
- Provides a reproducible Python notebook workflow for EDA.

> The design, KPIs, and dashboard sections follow the attached report and slide deck. See KPIs (e.g., Total Clients, Total Loan, Total Deposit, Total Fees) and pages like **Loan Analysis**, **Deposit Analysis**, and **Summary Dashboard** for guidance.  

## 📦 Dataset snapshot

- **Rows:** 3000
- **Columns:** 30
- **Unique Clients:** 2940
- **Joined Bank date range:** 1995-01-08 → 2021-12-31

### Columns & inferred dtypes
- **Client ID** — object
- **Name** — object
- **Age** — int64
- **Location ID** — int64
- **Joined Bank** — datetime64[ns]
- **Banking Contact** — object
- **Nationality** — object
- **Occupation** — object
- **Fee Structure** — object
- **Loyalty Classification** — object
- **Estimated Income** — float64
- **Superannuation Savings** — float64
- **Amount of Credit Cards** — int64
- **Credit Card Balance** — float64
- **Bank Loans** — float64
- **Bank Deposits** — float64
- **Checking Accounts** — float64
- **Saving Accounts** — float64
- **Foreign Currency Account** — float64
- **Business Lending** — float64
- **Properties Owned** — int64
- **Risk Weighting** — int64
- **BRId** — int64
- **GenderId** — int64
- **IAId** — int64
- **Engagement Days** — int64
- **Income Band** — category
- **Processing Fees** — float64
- **Total Loan** — float64
- **Total Fees** — float64

> Note: Values above are computed directly from `Banking.csv` shipped with the project.

## 🧪 Feature engineering (in code)

- **Engagement Days** = days since `Joined Bank`
- **Income Band** from `Estimated Income` (Low <100k, Mid 100k–300k, High >300k)
- **Processing Fees** from `Fee Structure` → (Low=1%, Medium=3%, High=5%)
- **Total Loan** = `Bank Loans` + `Business Lending` + `Credit Card Balance`
- **Total Fees** = `Total Loan` × `Processing Fees`

These align with the steps described in the report (new columns and bins for income & fees).

## 📊 Key metrics (computed on the provided CSV)

- **Total Loan:** 4383966512.51
- **Total Deposit:** 3766335079.58
- **Total Fees (derived):** 143855885.81

> Numbers here are from the raw CSV; your Power BI cards may show filtered or formatted results depending on model relationships and slicers (e.g., examples in the slides).

## 🖥️ Power BI measures (DAX)

Use these canonical measures when building visuals:

```DAX
-- Clients
Total Clients = DISTINCTCOUNT('Clients - Banking'[Client ID])

-- Loan components
Bank Loan = SUM('Clients - Banking'[Bank Loans])
Business Lending = SUM('Clients - Banking'[Business Lending])
Credit Cards Balance = SUM('Clients - Banking'[Credit Card Balance])

-- Aggregates
Total Loan = [Bank Loan] + [Business Lending] + [Credit Cards Balance]

Bank Deposit = SUM('Clients - Banking'[Bank Deposits])
Checking Accounts = SUM('Clients - Banking'[Checking Accounts])
Savings Account = SUM('Clients - Banking'[Saving Accounts])
Foreign Currency Account = SUM('Clients - Banking'[Foreign Currency Account])
Total Deposit = [Bank Deposit] + [Savings Account] + [Foreign Currency Account] + [Checking Accounts]

-- Fees (processing fee is a column derived from Fee Structure rule)
Total Fees = SUMX('Clients - Banking', [Total Loan] * 'Clients - Banking'[Processing Fees])

-- Engagement
Engagement Days = DATEDIFF('Clients - Banking'[Joined Bank], TODAY(), DAY)
```

## 📈 Suggested visuals

- **Summary Dashboard:** KPI cards (Total Clients, Total Loan, Total Deposit, Total Fees), slicers (Gender, Nationality, Income Band), engagement trend.
- **Loan Analysis:** Loan by Income Band, Loan by Nationality, Loan vs. Risk Weighting, Top borrowers.
- **Deposit Analysis:** Deposits by Account Type (Checking/Saving/Foreign), Bank-wise distribution, Top depositors.

## 🛠️ How to run

### Option A — Python EDA
1. Open `notebooks/Banking_EDA.ipynb` (or create one) in Jupyter/Colab.
2. Load `Banking.csv` and run the feature engineering steps above.
3. Explore distributions and outliers for risk flags (high balance + high risk weighting, negative engagement, etc.).

### Option B — Power BI Dashboard
1. Import `Banking.csv` into Power BI Desktop.
2. Create calculated columns: `Engagement Days`, `Income Band`, `Processing Fees`.
3. Add the DAX measures from this README.
4. Build pages: **Summary**, **Loan Analysis**, **Deposit Analysis**.
5. Publish to Power BI Service and share with stakeholders.

## 🧭 Folder structure (suggested)

```
.
├── data/
│   └── Banking.csv
├── powerbi/
│   └── Banking_Dashboard.pbix        # build here
├── docs/
│   ├── Banking Report.docx
│   └── Banking.pptx
├── notebooks/
│   └── Banking_EDA.ipynb
└── README.md  ← you are here
```

## ✅ Quality checks

- Nulls and date parsing for `Joined Bank`
- Value ranges for income, loans, deposits
- Cross-check totals across measures and visuals
- Slicer interactions (e.g., Income Band, Nationality) update all cards consistently

## 🚀 Future work

- Probability of default (PD) model with logistic regression/gradient boosting
- Early-warning risk signals (trend breaks in engagement and deposits)
- Client segmentation using K-means for pricing & cross-sell
- What-if analysis for fee structure and interest rates

## 📎 Credits

- Project brief, KPIs, and dashboard sections derived from the attached **Banking Report** and **Banking.pptx**.  
- CSV provided with the project for reproducibility.

---

**Author:** Rupesh Gavli 

**Linkedin:** www.linkedin.com/in/rupesh-gavli

**E-mail:** rupeshgavli2003@gmail.com
