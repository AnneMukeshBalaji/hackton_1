# India Tech Career Intelligence (2026) — Exploratory Data Analysis
**Manthan Hackathon — Organized by Calibo AI Academy**  
**Institution:** Dhanekula Institute of Engineering & Technology (DIET) — Path 2  
**Scope:** Descriptive EDA & Workforce Intelligence (Python, NumPy, Pandas, Matplotlib, Seaborn)  

---

## Team Task Division & Responsibilities

| Member | Responsibility | Assigned Tasks | Deliverables |
| :--- | :--- | :--- | :--- |
| **Member 1 (Anne Mukesh Balaji)** | **Data Cleaning & Preparation** | **Problem Statement 1** | Dataset ingestion, duplicate audit, missing value imputation, text normalization, datetime standardization, timeline validation, compensation bounds check, cleaned dataset export, and official data cleaning log. |
| **Member 2 (Kesiboyina Chandrika)** | **Job Roles & Skills Analysis** | **Problem Statements 2 & 3** | Category frequency distribution, top 15 job titles, full-time vs. internship contract verification, technical skill extraction, top 20 skills overall, category-specific skill breakdown, and skill bundle analysis. |
| **Member 3 (Chennam Divya)** | **Companies & Location Analysis** | **Problem Statements 4 & 5** | Top 10 employers analysis, enterprise scale & company types, single-industry boundary check, top metropolitan cities & states, and work-mode flexibility (remote/hybrid/on-site) cross-tabulations. |
| **Member 4 (Chinnam Joshitha)** | **Salary, Experience & Final Insights** | **Problem Statements 6, 7 & 8** | Full-time salary distributions & midpoints, independent monthly internship stipend analysis, experience requirements, educational qualifications, applicant volume & career growth indicators, executive dashboard, and evidence-backed career recommendations. |

---

## Executive Summary & Business Context
Students and early-career tech professionals face an information asymmetry when navigating India's dynamic technology employment landscape. As junior workforce intelligence analysts, our mission is to perform an end-to-end, chart-backed exploratory data analysis on 100,000 technology postings across India.

Per official hackathon guidelines, this project strictly uses **Python, NumPy, Pandas, Matplotlib, and Seaborn**. No predictive models, machine learning, or NLP techniques are applied. All observations represent empirical patterns within this dataset snapshot and are not presented as a verified national census.

---

## Dataset Overview (Audit Snapshot)

| Metric | Raw Dataset (`indian_tech_career_intelligence_2026.csv`) | Cleaned Dataset (`indian_tech_career_intelligence_2026_cleaned.csv`) |
| :--- | :--- | :--- |
| **Row Count** | 100,000 listings | 100,000 listings (100% preserved) |
| **Column Count** | 33 attributes | 34 attributes (+1 numeric derived column) |
| **Duplicate Rows** | 0 duplicates | 0 duplicates |
| **Missing Values** | `probation_period`: 33,499 rows (33.5%) | 0 unhandled nulls (imputed with `'Not Specified'`) |
| **Date Columns** | Stored as text (`DD-MM-YYYY`) | Standardized `pd.datetime64[ns]` |
| **Salary Currency** | 100% INR (₹) | 100% INR (₹) |
| **Integrity Checks** | Raw unverified | 0 inverted salaries; 0 negative values; 0 timeline order anomalies |

---

## Member 1 — Data Cleaning & Preparation
**Responsible Member:** **Anne Mukesh Balaji**

### Problem Statement 1 — Audit and Clean the Career Dataset
* **Raw Data Loading:** Preserved the raw CSV untouched (`100,000 rows × 33 columns`) and executed cleaning on a separate working DataFrame.
* **Duplicate Rows Audit:** Audited all 33 columns with `.duplicated().sum()` confirming **0 duplicate listings**.
* **Missing Value Imputation:** Identified that `probation_period` was the only column with nulls (33,499 missing, 33.5%); imputed with global constant `'Not Specified'` to preserve contract authenticity without fabricating terms.
* **Text Normalization:** Stripped leading and trailing whitespace across all 27 string columns using `.str.strip()`.
* **Datetime Parsing & Validation:** Standardized `posted_date` and `application_deadline` to `pd.datetime64[ns]` with format `%d-%m-%Y`. Confirmed zero timeline order violations (`deadline >= posted_date`).
* **Numeric Casting & Bounds Check:** Stripped `'%'` from `career_growth_index` and cast to float `career_growth_index_pct`. Verified compensation bounds (100% INR, 0 inverted salaries, 0 negative values).
* **Cleaned Dataset Sharing:** Exported verified dataset to `indian_tech_career_intelligence_2026_cleaned.csv` for team analysis.

### Member 1: Official Data Cleaning Decision Log

| Field / Attribute | Audit Finding | Action Applied | Analytical & Business Justification |
| :--- | :--- | :--- | :--- |
| **Raw Data Integrity** | Raw file must not be modified | Created `df_clean = df.copy()` and exported to separate cleaned CSV | Preserves raw provenance and complies with challenge guidelines. |
| **Duplicate Records** | Audited `.duplicated().sum()` | Confirmed `0` duplicates; no rows dropped | Validates that all 100,000 rows are unique listing records. |
| **`probation_period`** | 33,499 missing values (33.5%); values were `"3 Months"` (33.1%) and `"6 Months"` (33.4%) | Imputed with global constant: `'Not Specified'` | Avoids dropping 33,500 rows (data loss) or imputing mode (which fabricates contract terms). Missingness reflects contract or startup roles without formal probation. |
| **String Columns (27 cols)** | Potential leading/trailing whitespace | Applied `.astype(str).str.strip()` across all string columns | Eliminates accidental category mismatches (e.g., `' Remote'` vs `'Remote'`). |
| **`posted_date` & `application_deadline`** | Stored as string text in Day-Month-Year format | Parsed with `pd.to_datetime(format='%d-%m-%Y', errors='coerce')` | Standardizes timestamps for temporal analysis. 0 unparseable values found. |
| **Timeline Ordering** | Deadline must occur on or after posting date | Audited `(deadline < posted_date).sum()` | **0 records violated timeline ordering.** Earliest posted: `01-Jan-2026`, latest: `30-May-2026`. |
| **`career_growth_index`** | Stored as text with percentage symbol (e.g., `'85%'`) | Stripped `'%'` and converted to float `float` (`career_growth_index_pct`) | Enables mathematical summary while documenting provider methodology as unspecified. |
| **Compensation Sanity** | Evaluated minimum, maximum, and stipend bounds | Checked `salary_minimum <= salary_maximum`, non-negativity, and currency | Confirmed 100% currency is `'INR'`. Zero inverted ranges (`min > max = 0`). Minimum salary range: ₹300,004–₹599,999; Maximum salary range: ₹700,013–₹1,799,996; Stipends: ₹15,000–₹44,999. |

---

## Member 2 — Job Roles & Skills Analysis
**Responsible Member:** **Kesiboyina Chandrika**

### Problem Statement 2 — Understand Job Types, Categories and Roles
* **Technology Job Categories:** Evaluated 6 balanced technology categories (~16.4% to 16.9% each, n ≈ 16,600). Web Development leads slightly with 16,872 listings (16.9%), followed by AI / ML Engineering (16,787, 16.8%) and Cloud Architecture (16,782, 16.8%).
* **Technology Job Titles:** Identified 24 distinct titles led by Full Stack Developer (4,321, 4.3%), Machine Learning Engineer (4,286, 4.3%), and Cloud Architect (4,258, 4.3%).
* **Opportunity Mix (Full-Time vs. Internship):** Exactly **59,871 (59.9%)** of listings are Full-time (Permanent), while **40,129 (40.1%)** are Internships (Contractual). Every job category maintains this identical ~60:40 ratio. Job titles appear identically across both contracts; employment type cannot be inferred from job title alone.

### Problem Statement 3 — Identify Skills Requested by Employers
* **Top Technical Skills Overall:** Extracted 684,285 total mentions. Python leads with 33,212 mentions (spanning Data Science and AI/ML), followed by Linux with 33,134 mentions (spanning DevOps and Cybersecurity). Foundational web tools (HTML, CSS, React) and Cloud/DevOps tools (AWS, Docker) each record ~16,400 to 16,800 mentions.
* **Category-Specific Skills:** Mapped specialized stacks for each domain (AI/ML: PyTorch, TensorFlow, Scikit-Learn; DevOps: Docker, Kubernetes, Jenkins, CI/CD; Cloud: AWS, Azure, CloudFormation, EC2).
* **Skill Bundles & Consistency Validation:** Verified 100% concordance between pre-calculated `skill_count` and parsed skill counts using NumPy (`np.where` mismatch = 0). Identified that exactly 6 unique skill bundles exist, mapping 1-to-1 to each category (mean skills = 6.84, median = 7.0).

---

## Member 3 — Companies & Location Analysis
**Responsible Member:** **Chennam Divya**

### Problem Statement 4 — Analyse Companies and Industry Representation
* **Top Employers Analysis:** 12 major employers contribute evenly between 8,160 and 8,517 postings (~8.2% to 8.5% share), led by HCLTech (8,517), Tech Mahindra (8,467), and Cognizant (8,445). Listings reflect platform postings, not total employer headcount.
* **Enterprise Size & Company Types:** Top MNCs represent 50.1% (50,138 listings), Tech Startups represent 33.3% (33,280), and Fintech Unicorns represent 16.6% (16,582). Large organizations (10,000+ employees) account for 50.1% of listings.
* **Industry Representation & Scope Boundary:** Documented that 100% of records belong to `'Information Technology'`. Cross-industry comparisons are methodologically unsupported and explicitly disclosed.

### Problem Statement 5 — Map Opportunity Locations and Work Modes
* **Top Metropolitan Cities & States:** Listings are distributed equitably across 8 metropolitan tech hubs (~12.5% each): Chennai (12,636), Noida (12,622), Gurgaon (12,521), Delhi NCR (12,515), Pune (12,452), Mumbai (12,443), Hyderabad (12,439), and Bangalore (12,372). Maharashtra leads state-wise with 24,895 postings (24.9%).
* **Work Mode Flexibility:** **79.7%** of opportunities feature location flexibility (**40.0% Remote, 39.7% Hybrid**), while traditional On-site roles comprise only 20.3% (20,328). Cross-tabulated flexibility across all six job categories.

---

## Member 4 — Salary, Experience & Final Insights
**Responsible Member:** **Chinnam Joshitha**

### Problem Statement 6 — Analyse Salary Ranges and Internship Stipends
* **Full-Time Salary Modeling (n = 59,871):**
  - Minimum salary range: ₹300,004 to ₹599,996 (Median: ₹450,216).
  - Maximum salary range: ₹700,015 to ₹1,799,992 (Median: ₹1,250,279).
  - Derived Salary Midpoint: Mean = ₹8.50 LPA (₹849,661), Median = ₹8.50 LPA (₹849,819), IQR = ₹2.75 LPA (₹7.12 LPA to ₹9.87 LPA).
* **Independent Internship Stipend Analysis (n = 40,129):**
  - Analyzed independently from annual salaries.
  - Stipend range: ₹15,000 to ₹44,999 per month.
  - Central tendency: Mean = ₹29,953/mo, Median = ₹29,959/mo, IQR = [₹22,464, ₹37,392].
  - Stipends represent monthly compensation and must not be conflated with annual CTC.

### Problem Statement 7 — Examine Experience, Education, Applicants and Career Indicators
* **Experience & Education Analysis:** 100% of internships require 0–1 Years (Freshers / Entry Level); 100% of full-time roles require 2–4 Years (Mid-Senior Level). Qualifications are partitioned equitably across Bachelor's Degree (33.4%), MCA / MSc (33.3%), and B.E. / B.Tech (33.2%).
* **Applicant Volume & Career Growth Indicators:** Applicant counts range between 15 and 479 (Median: 247; platform snapshot metric). Career Growth Index ranges between 65% and 98% (Median: 82.0%; undocumented provider calculation methodology).

### Problem Statement 8 — Build the Final Career Intelligence Report
* **Executive Career Dashboard:** Integrated 4-panel dashboard summarizing opportunity mix, work mode flexibility, full-time salary midpoints, and internship stipends.
* **Three Evidence-Backed Career Insights:**
  1. *Domain-Specific Technical Stacks & Complete Tooling Mastery:* Skill co-occurrence reveals 6 non-overlapping bundles mapped 1-to-1 to categories; learners must master complete domain stacks.
  2. *High Early-Career Flexibility via Remote/Hybrid Adoption:* 79.7% of listings offer remote/hybrid work across 8 pan-India hubs; career cells should prepare students for asynchronous workflows.
  3. *Distinct Compensation Bifurcation (Internships vs Full-Time):* Full-time median ₹8.50 LPA (2–4 yrs); internships median ₹29,959/mo (0–1 yrs) serve as primary talent conversion bridges.
* **Action Plan for College Career Cells & Job Seekers:** 7 concrete, data-backed recommendations covering curriculum alignment, paid internship pipelines, asynchronous remote skills, and degree equity.

---

## Methodological Boundaries & Limitations
* Machine learning, NLP, clustering, and predictive modelling were intentionally omitted per challenge scope.
* Analysis is strictly descriptive EDA using NumPy, Pandas, Matplotlib, and Seaborn.
* The raw dataset is 100% preserved; all transformations were conducted on a separate working DataFrame.
* Missing values in `probation_period` were imputed as `'Not Specified'` to preserve data integrity without fabricating terms.
* All findings represent patterns within this 100,000-listing snapshot and are not a complete census of India's labor market.

---

## Environment Setup & Reproducibility

This project is built using Python's standard data science ecosystem and can be executed using `uv` or standard `pip`:

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

## Project Repository Structure
```
├── README.md                                      # Comprehensive project report & team documentation
├── hackathon_guide.txt                            # Official hackathon challenge guidelines & rubric
├── indian_tech_career_intelligence_2026.csv       # Preserved Raw Dataset (100,000 rows × 33 columns)
├── indian_tech_career_intelligence_2026_cleaned.csv# Cleaned Working Dataset (100,000 rows × 34 columns)
└── main.ipynb                                     # Master Notebook (Member 1 to Member 4 with Problem Statements 1–8)
```
