# India Tech Career Intelligence (2026) — Exploratory Data Analysis
**Manthan Hackathon — Organized by Calibo AI Academy**  
**Institution:** Dhanekula Institute of Engineering & Technology (DIET) — Path 2  
**Date:** Saturday, 31 October 2026  
**Scope:** Descriptive EDA & Business Intelligence (No ML)

---

## 👥 Team Members
1. **Anne Mukesh Balaji**
2. **Kesiboyina Chandrika**
3. **Chennam Divya**
4. **Chinnam Joshitha**

---

## 📌 Executive Summary & Business Context
Students and early-career tech professionals need reliable, evidence-based insights into India's technology hiring landscape. As junior workforce analysts, our goal is to audit, analyze, and translate 100,000 technology job and internship postings across India into actionable career guidance for college career cells and job seekers.

Per hackathon guidelines, this project strictly uses **Python, NumPy, Pandas, Matplotlib, and Seaborn**. No predictive models, NLP, or machine learning are used. All observations reflect patterns present within this dataset snapshot.

---

## ⏱️ Execution Timeline: Phase 1 — Data Audit & Cleaning
**Recorded Window:** `09:50 AM — 10:45 AM (IST)`  
**Status:** Completed & Validated

During this initial 55-minute window, our team executed a rigorous data audit and cleaning pipeline for **Problem Statement 1**, ensuring raw data integrity while creating an analysis-ready dataset.

```
09:50 AM ── Environment Setup (uv venv, dependencies installed)
09:55 AM ── Raw Dataset Loaded (100,000 rows × 33 columns audited)
10:05 AM ── Duplicate & Missingness Audit (0 duplicates; probation_period analyzed)
10:18 AM ── Categorical Imputation Decision (Global constant 'Not Specified')
10:25 AM ── Text Normalization (Whitespace stripped across 27 object columns)
10:32 AM ── Datetime Parsing (DD-MM-YYYY format parsed; timeline validated)
10:40 AM ── Numeric Feature Conversion ('career_growth_index' to float)
10:45 AM ── Compensation Sanity Checks & Cleaned CSV Export
```

---

## 🗂️ Dataset Overview (Audit Snapshot)

| Metric | Raw Dataset (`indian_tech_career_intelligence_2026.csv`) | Cleaned Dataset (`indian_tech_career_intelligence_2026_cleaned.csv`) |
| :--- | :--- | :--- |
| **Row Count** | 100,000 rows | 100,000 rows (100% preserved) |
| **Column Count** | 33 columns | 34 columns (+1 numeric derived column) |
| **Duplicate Rows** | 0 duplicates | 0 duplicates |
| **Missing Values** | `probation_period`: 33,499 rows (33.5%) | 0 unhandled nulls (imputed with `'Not Specified'`) |
| **Date Columns** | Stored as text (`DD-MM-YYYY`) | Standardized `pd.datetime64[ns]` |
| **Salary Currency** | 100% INR (₹) | 100% INR (₹) |
| **Integrity Checks** | Raw unverified | 0 inverted salaries; 0 negative values; 0 deadline anomalies |

---

## 📝 Problem Statement 1: Data Cleaning Decision Log

The following table documents every audit finding, action taken, and analytical justification applied between 09:50 AM and 10:45 AM:

| Column / Component | Audit Finding | Action Applied | Analytical & Business Justification |
| :--- | :--- | :--- | :--- |
| **Raw Data Integrity** | Raw file must not be modified | Created `df_clean = df.copy()` and exported to a separate CSV | Complies with guideline: *"Keep an unchanged copy of the raw CSV; perform cleaning on a separate DataFrame."* |
| **Duplicate Rows** | Evaluated `.duplicated().sum()` | Confirmed `0` duplicates; no rows dropped | Validates that all 100,000 rows are unique listing records. |
| **`probation_period`** | 33,499 missing values (33.5%); values were `"3 Months"` (33.1%) and `"6 Months"` (33.4%) | Imputed with global constant: `'Not Specified'` | **Crucial Decision:** We did NOT drop the column (explicitly required in PS 7), nor drop 33,500 rows (data loss), nor impute with the mode `"6 Months"` (would fabricate data and violate transparency rules). Missingness reflects contract or startup roles without formal probation. |
| **String Columns (27 cols)** | Potential leading/trailing whitespace | Applied `.astype(str).str.strip()` across all string columns | Eliminates accidental category mismatches (e.g., `' Remote '` vs `'Remote'`). |
| **`posted_date` & `application_deadline`** | Stored as string text in Day-Month-Year format | Parsed with `pd.to_datetime(format='%d-%m-%Y', errors='coerce')` | Standardizes timestamps for temporal analysis. 0 unparseable values found. |
| **Timeline Sanity Check** | Deadline must occur on or after posting date | Audited `(deadline < posted_date).sum()` | **Result: 0 records violated timeline ordering.** Earliest posted: `01-Jan-2026`, latest: `30-May-2026`. |
| **`career_growth_index`** | Stored as text with percentage symbol (e.g., `'85%'`) | Stripped `'%'` and converted to float `float` (`career_growth_index_pct`) | Enables mathematical summary (mean: `81.51%`, median: `82.0%`, range: `65%` to `98%`). Documented as descriptive metadata since provider methodology is unspecified. |
| **Compensation Sanity** | Evaluated minimum, maximum, and stipend bounds | Checked `salary_minimum <= salary_maximum`, non-negativity, and currency | Confirmed 100% currency is `'INR'`. Zero inverted ranges (`min > max = 0`). Minimum salary range: ₹300,004–₹599,999; Maximum salary range: ₹700,013–₹1,799,996; Stipends: ₹15,000–₹44,999. |

---

## 🛠️ Environment Setup & Reproducibility

This project was built using the ultra-fast Python package manager **`uv`**:

```bash
# 1. Clone repository & navigate to folder
cd ~/Downloads/hackton_1

# 2. Create and activate virtual environment
uv venv
source .venv/bin/activate

# 3. Install required libraries (pure EDA stack)
uv pip install pandas numpy matplotlib seaborn jupyter ipykernel

# 4. Launch Jupyter Notebook
jupyter notebook main.ipynb
```

---

## 📂 Project Repository Structure
```
├── README.md                                      # Project documentation & cleaning log
├── hackathon_guide.txt                            # Official challenge guidelines & rubric
├── indian_tech_career_intelligence_2026.csv       # Preserved Raw Dataset (100k rows)
├── indian_tech_career_intelligence_2026_cleaned.csv# Cleaned Working Dataset (Exported at 10:45 AM)
└── main.ipynb                                     # Master Jupyter Notebook
```

---

## 📋 Next Hackathon Milestone
* **Phase 2 (10:45 AM – 12:30 PM):** 
  * Problem Statement 2: Understand Job Types, Categories, and Roles.
  * Problem Statement 3: Skills Extraction, Frequency, and Co-occurrence Analysis.
  * Problem Statement 4 & 5: Company, Geography, and Work-Mode Analysis.
