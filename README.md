# India Tech Career Intelligence (2026) — Exploratory Data Analysis
**Manthan Hackathon — Organized by Calibo AI Academy**  
**Institution:** Dhanekula Institute of Engineering & Technology (DIET) — Path 2  
**Scope:** Complete Descriptive EDA & Workforce Intelligence (Problem Statements 1 to 8)  
**Authors & Team Members:**
1. **Anne Mukesh Balaji** (Data Audit, Cleaning Pipeline & Compensation Modeling)
2. **Kesiboyina Chandrika** (Job Taxonomy, Roles & Employment Mix Analysis)
3. **Chennam Divya** (Skills Extraction, Co-occurrence & Breadth Analysis)
4. **Chinnam Joshitha** (Company Scale, Industry & Geographic Distribution)

---

## Executive Summary & Business Context
Students and early-career tech professionals face an information asymmetry when navigating India's technology hiring landscape. As junior workforce analysts, our goal is to audit, analyze, and translate 100,000 technology job and internship postings into practical, chart-backed career intelligence for college career cells and job seekers.

Per official hackathon guidelines, this project strictly uses **Python, NumPy, Pandas, Matplotlib, and Seaborn**. No predictive models, machine learning, or NLP techniques are applied. All observations reflect empirical patterns within this dataset snapshot and are not presented as a verified national census.

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

## Official Data Cleaning Decision Log

The following table documents every data quality finding, action applied, and analytical justification:

| Column / Component | Audit Finding | Action Applied | Analytical & Business Justification |
| :--- | :--- | :--- | :--- |
| **Raw Data Integrity** | Raw file must not be modified | Created `df_clean = df.copy()` and exported to separate cleaned CSV | Preserves raw provenance and complies with challenge guidelines. |
| **Duplicate Rows** | Evaluated `.duplicated().sum()` | Confirmed `0` duplicates; no rows dropped | Validates that all 100,000 rows are unique listing records. |
| **`probation_period`** | 33,499 missing values (33.5%); values were `"3 Months"` (33.1%) and `"6 Months"` (33.4%) | Imputed with global constant: `'Not Specified'` | Avoids dropping 33,500 rows (data loss) or imputing mode (which fabricates contract terms). Missingness reflects contract or startup roles without formal probation. |
| **String Columns (27 cols)** | Potential leading/trailing whitespace | Applied `.astype(str).str.strip()` across all string columns | Eliminates accidental category mismatches (e.g., `' Remote'` vs `'Remote'`). |
| **`posted_date` & `application_deadline`** | Stored as string text in Day-Month-Year format | Parsed with `pd.to_datetime(format='%d-%m-%Y', errors='coerce')` | Standardizes timestamps for temporal analysis. 0 unparseable values found. |
| **Timeline Ordering** | Deadline must occur on or after posting date | Audited `(deadline < posted_date).sum()` | **0 records violated timeline ordering.** Earliest posted: `01-Jan-2026`, latest: `30-May-2026`. |
| **`career_growth_index`** | Stored as text with percentage symbol (e.g., `'85%'`) | Stripped `'%'` and converted to float `float` (`career_growth_index_pct`) | Enables mathematical summary while documenting provider calculation methodology as unspecified. |
| **Compensation Sanity** | Evaluated minimum, maximum, and stipend bounds | Checked `salary_minimum <= salary_maximum`, non-negativity, and currency | Confirmed 100% currency is `'INR'`. Zero inverted ranges (`min > max = 0`). Minimum salary range: ₹300,004–₹599,999; Maximum salary range: ₹700,013–₹1,799,996; Stipends: ₹15,000–₹44,999. |

---

## Problem Statement Solutions & Analytical Findings

### 1. Problem Statement 1 — Audit and Clean the Career Dataset
* Preserved the raw CSV untouched and executed an end-to-end cleaning pipeline on a working DataFrame.
* Imputed 33,499 missing values in `probation_period` with `'Not Specified'`.
* Standardized text whitespace across all 27 string columns.
* Converted posting and deadline dates to `datetime64[ns]` with zero timeline order violations.
* Cast `career_growth_index` to float and verified compensation bounds (100% INR, 0 inverted salaries, 0 negative values).

### 2. Problem Statement 2 — Understand Job Types, Categories and Roles
* **Job Categories:** 6 balanced technology categories (~16.4% to 16.9% each, n ≈ 16,600). Web Development leads slightly with 16,872 listings (16.9%), followed by AI / ML Engineering (16,787, 16.8%) and Cloud Architecture (16,782, 16.8%).
* **Job Titles:** 24 distinct titles led by Full Stack Developer (4,321, 4.3%), Machine Learning Engineer (4,286, 4.3%), and Cloud Architect (4,258, 4.3%).
* **Employment & Contract Mix:** Exactly **59,871 (59.9%)** of listings are Full-time (Permanent), while **40,129 (40.1%)** are Internships (Contractual). Every job category maintains this identical ~60:40 ratio.
* **Key Principle:** Job titles appear identically across both full-time and internship contracts; employment type cannot be inferred from job title alone.

### 3. Problem Statement 3 — Identify Skills Requested by Employers
* **Top Overall Skills:** Across 684,285 total mentions, Python ranks highest with 33,212 mentions (spanning Data Science and AI/ML), followed by Linux with 33,134 mentions (spanning DevOps and Cybersecurity). Web tools (HTML, CSS, React) and Cloud/DevOps tools (AWS, Docker) each record ~16,400 to 16,800 mentions.
* **Category Specialization:** Each category exhibits distinct skill profiles (e.g. AI/ML: PyTorch, TensorFlow, Scikit-Learn; DevOps: Docker, Kubernetes, Jenkins, CI/CD; Cloud: AWS, Azure, CloudFormation, EC2).
* **NumPy Validation:** Verified 100% concordance between pre-calculated `skill_count` and actual parsed string length (`np.where` mismatch = 0).
* **Analytical Finding:** Exactly 6 complete skill bundles exist, mapping 1-to-1 to each job category (mean skills per job = 6.84, median = 7.0).

### 4. Problem Statement 4 — Analyse Companies and Industry Representation
* **Top Employers:** 12 major employers contribute evenly between 8,160 and 8,517 postings (~8.2% to 8.5% share), led by HCLTech (8,517), Tech Mahindra (8,467), and Cognizant (8,445).
* **Company Type & Scale:** Top MNCs represent 50.1% (50,138 listings), Tech Startups represent 33.3% (33,280), and Fintech Unicorns represent 16.6% (16,582). Organizations with 10,000+ employees account for half of all listings (50.1%).
* **Mandatory Industry Limitation:** 100% of records belong to `'Information Technology'`. Cross-industry comparisons are methodologically unsupported and explicitly disclosed. Listing counts represent platform postings, not total employer headcount.

### 5. Problem Statement 5 — Map Opportunity Locations and Work Modes
* **Geographic Distribution:** Listings are distributed equitably across 8 metropolitan tech hubs (~12.5% each): Chennai (12,636), Noida (12,622), Gurgaon (12,521), Delhi NCR (12,515), Pune (12,452), Mumbai (12,443), Hyderabad (12,439), and Bangalore (12,372). Maharashtra leads state-wise with 24,895 postings (24.9%).
* **Work Mode Flexibility:** **79.7%** of opportunities feature location flexibility (**40.0% Remote, 39.7% Hybrid**), while traditional On-site roles comprise only 20.3% (20,328). This flexibility remains uniform across all six job categories.

### 6. Problem Statement 6 — Analyse Salary Ranges and Internship Stipends
* **Full-Time Salaries (n = 59,871):**
  - Minimum salary range: ₹300,004 to ₹599,996 (Median: ₹450,216).
  - Maximum salary range: ₹700,015 to ₹1,799,992 (Median: ₹1,250,279).
  - Derived Salary Midpoint: Mean = ₹8.50 LPA (₹849,661), Median = ₹8.50 LPA (₹849,819), IQR = ₹2.75 LPA (₹7.12 LPA to ₹9.87 LPA).
* **Internship Stipends (n = 40,129):**
  - Analyzed independently from annual salaries.
  - Stipend range: ₹15,000 to ₹44,999 per month.
  - Central tendency: Mean = ₹29,953/mo, Median = ₹29,959/mo, IQR = [₹22,464, ₹37,392].
* **Methodological Boundary:** Stipends are monthly figures and must not be conflated with annual CTC. Take-home pay cannot be deduced.

### 7. Problem Statement 7 — Examine Experience, Education, Applicants and Career Indicators
* **Experience Mapping:** 100% of internships require 0–1 Years (Freshers / Entry Level); 100% of full-time roles require 2–4 Years (Mid-Senior Level).
* **Educational Qualifications:** Equal eligibility across Bachelor's Degree (33.4%), MCA / MSc (33.3%), and B.E. / B.Tech (33.2%).
* **Applicant Snapshot:** Postings record between 15 and 479 applicants (Median: 247). Documented as platform snapshot metrics rather than candidate selection probability.
* **Career Growth Index:** Ranges from 65% to 98% (Median: 82.0%). Documented as descriptive metadata with unspecified provider calculation methodology.

### 8. Problem Statement 8 — Final Career Intelligence Report & Executive Dashboard
* Integrated 4-panel executive dashboard summarizing opportunity mix, work mode flexibility, full-time salary midpoints, and internship stipends.
* Synthesized three evidence-backed career insights and a 7-step practical action plan for students and career cells.

---

## Three Evidence-Backed Career Insights

###  Insight 1: Domain-Specific Technical Stacks & Complete Tooling Mastery
* **Supporting Evidence ($n = 100,000$):** Skill co-occurrence analysis reveals six distinct, non-overlapping skill bundles mapped 1-to-1 to job categories. Python leads overall with 33,212 mentions (spanning Data Science and AI/ML), while Linux appears in 33,134 postings (DevOps and Cybersecurity).
* **Analytical Caveat:** Perfect 1-to-1 bundle mapping is an artifact of synthetic benchmarking; organic industry hiring often requires cross-functional skills.
* **Practical Next Step:** Learners should master end-to-end stacks within a single chosen domain (e.g. Python + PyTorch/TensorFlow for AI, or Docker/Kubernetes + Linux for DevOps) rather than acquiring disconnected tools.

###  Insight 2: High Early-Career Flexibility via Remote and Hybrid Adoption
* **Supporting Evidence ($n = 100,000$):** 79.7% of postings support flexible work arrangements (40.0% Remote, 39.7% Hybrid). Opportunities are spread across 8 metropolitan cities with ~12.5% share each.
* **Analytical Caveat:** Postings reflect advertised work modes; organizational policies may require periodic on-site collaboration.
* **Practical Next Step:** College career cells should train students in asynchronous engineering workflows (Git, Docker, Slack, Jira) to access pan-India opportunities regardless of physical campus location.

###  Insight 3: Distinct Compensation Bifurcation (Entry Internships vs Mid-Senior Full-Time)
* **Supporting Evidence ($n = 59,871$ Full-Time, $n = 40,129$ Internships):** Full-time roles require 2–4 years of experience and offer a median midpoint salary of ₹8.50 LPA (IQR: ₹7.12 LPA to ₹9.87 LPA). Internships provide a vital bridge for college freshers (0–1 years), offering competitive median stipends of ₹29,959 per month (IQR: ₹22,464 to ₹37,392).
* **Analytical Caveat:** Advertised compensation reflects target employer ranges; take-home pay cannot be deduced.
* **Practical Next Step:** Early-career graduates should actively target paid internships as primary conversion conduits to full-time engineering roles.

---

## Action Plan for College Career Cells & Job Seekers

1. **Curriculum Alignment with Domain Bundles:** Design career training around complete technology stacks (Full-Stack MERN, Cloud Architecture on AWS/Azure, AI/ML pipelines).
2. **Prioritize Paid Internships as Conversion Conduits:** With 40.1% of opportunities structured as paid internships (median stipend ₹30,000/mo), career cells should formalize pre-placement internship pipelines.
3. **Equip Students for Asynchronous Remote Work:** Integrate remote collaboration tools (Git, Jira, Docker) into academic projects to leverage the 79.7% flexible job market.
4. **Scrutinize Contract Terms vs Job Titles:** Educate students that titles like 'Data Scientist' or 'Cloud Engineer' appear in both contract internships and permanent roles; contract details must be verified.
5. **Master Universal Core Competencies:** Ensure all graduates master foundational tools (Python, SQL, Git, Linux) that span multiple domain boundaries.
6. **Encourage Non-Circuit Degree Streams:** Postings accept Bachelor's (33.4%), MCA/MSc (33.3%), and B.E./B.Tech (33.2%) equitably; students across all technical streams should apply.
7. **Maintain Methodological Transparency:** Always verify whether job board numbers represent point-in-time snapshots rather than absolute census guarantees.

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
├── README.md                                      # Comprehensive project report & documentation
├── hackathon_guide.txt                            # Official hackathon challenge guidelines & rubric
├── indian_tech_career_intelligence_2026.csv       # Preserved Raw Dataset (100,000 rows × 33 columns)
├── indian_tech_career_intelligence_2026_cleaned.csv# Cleaned Working Dataset (100,000 rows × 34 columns)
└── main.ipynb                                     # Complete Master Notebook (Problem Statements 1 to 8)
```
