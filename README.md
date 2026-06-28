# live-project
# 💰 Bangalore Tech Salary Decoder
### 1,000 Bengaluru tech salaries. 2 hours. Real patterns.

> *"I analysed 1,000 Bangalore tech salaries in 2 hours. Here's what I found."*

Built during **The Unlox Academy — Weeks 1+2 Live Project (2-Hour Industry Sprint)**  
Stack: Python · Pandas · NumPy · No ML. No APIs. Just clean analytics on messy data.

---

## What it does

Reads a synthetic but realistically messy HR dataset of 1,015 Bengaluru tech professionals, cleans it end-to-end, and produces a formatted salary-intelligence report covering role-wise pay, experience curves, skill premiums, company-type gaps, and the most underpaid professionals in the dataset.

**Sample output:**

```
============================================================
 BANGALORE TECH SALARY DECODER
 Built by M. Ganesh Nayak | The Unlox Academy | 2-Hour Live Project
============================================================
 Dataset : 1,000 Bengaluru tech professionals (synthetic)
 Period  : 2024 employment snapshot

 ---- MEDIAN CTC BY ROLE (in LPA) ----
 Product Manager    ████████████████████  XX.X
 Data Scientist     ████████████████      XX.X
 SDE Full-Stack     ███████████████       XX.X
 DevOps Engineer    █████████████         XX.X
 SDE Backend        █████████████         XX.X
 Business Analyst   ██████████            XX.X
 UI/UX Designer     █████████             XX.X
 Data Analyst       █████████             XX.X

 ---- SDE BACKEND CTC BY EXPERIENCE BAND ----
 0 to 1 years  : XX.X LPA median
 2 to 3 years  : XX.X LPA median  (+XX% growth)
 4 to 5 years  : XX.X LPA median  (+XX% growth)
 6+ years      : XX.X LPA median  (+XX% growth)

 ---- SKILL PREMIUM FOR SDEs (median CTC) ----
 Skill          With Skill   Without   Premium
 AWS            XX.X LPA     XX.X      +XX%
 ML             XX.X LPA     XX.X      +XX%
 System Design  XX.X LPA     XX.X      +XX%
 Kubernetes     XX.X LPA     XX.X      +XX%

 ---- COMPANY-TYPE PREMIUM (SDE Backend, same role) ----
 Unicorn        XX.X LPA  <- baseline ceiling
 MNC            XX.X LPA  (-XX% vs Unicorn)
 Mid-size       XX.X LPA  (-XX% vs Unicorn)
 Early-stage    XX.X LPA  (-XX% vs Unicorn)

 ---- TOP 5 MOST-UNDERPAID PROFESSIONALS ----
 BLR0042   SDE Backend    MNC   3 yrs   gap: -X.X LPA
 BLR0119   Data Analyst   MNC   2 yrs   gap: -X.X LPA
 ...
============================================================
 KEY INSIGHTS
 1. <data-specific insight with number>
 2. <data-specific insight with number>
 3. <data-specific insight with number>
============================================================
```

---

## The 6 Tasks — What was built

| # | Task | What it involved |
|---|------|-----------------|
| 1 | **Load & Inspect** | `.info()`, `.shape`, `.dtypes`, `.isnull().sum()`, `.duplicated()` — 5-line dataset quality summary |
| 2 | **Clean** | Role variant standardisation (mapping dict), CTC string-to-float parsing across 4 formats, education tier + company type normalisation, duplicate removal |
| 3 | **Five Business Questions** | CTC by role · Experience curve · Skill premium · Company-type gap · Underpaid professionals |
| 4 | **Printed Report** | ASCII-art formatted salary intelligence report — the LinkedIn screenshot |
| 5 | **Three Insights** | Data-specific, number-backed, action-implied findings from the actual data |
| 6 | **Code Review Pass** | Comments on every cell, clean variable names, notebook runs top-to-bottom |

---

## The real challenge — messy data

This wasn't a clean CSV. Real-world HR data never is.

**What had to be handled:**

| Problem | Example | Fix |
|---------|---------|-----|
| CTC in 4 string formats | `'15.5 LPA'`, `'15.5'`, `'15,50,000'`, `'₹15.5 LPA'` | Custom `parse_ctc()` with magnitude detection |
| Role name variants | `'SDE Backend'`, `'Backend Engineer'`, `'BE'`, `'Backend Dev'` | Manual mapping dict after inspecting `df['Role'].unique()` |
| Company type casing | `'unicorn'`, `'UNICORN'`, `'Unicorn'` | `.str.strip().str.title()` |
| Education tier variants | `'Tier 1'`, `'T1'`, `'1'`, `'Tier-1'` | Dict-based `.replace()` |
| NaN in Previous_CTC | ~20% of rows (freshers / career changers) | Left as NaN — NOT filled with 0 to avoid skewing growth calc |
| Groups too small for reliable median | Task 3.5 underpaid analysis | `groupby + transform + filter` — groups with < 10 members excluded |

The hardest trap: `'15,50,000'` looks like 1.5 million but is actually ₹15.5L. The parser checks for magnitude, not just format.

---

## Constraint list

**Used:**
- `pandas` — `read_csv`, `groupby`, `transform`, `apply`, `pd.cut`, `str.contains`, `value_counts`
- `numpy` — aggregations
- Python built-ins — f-strings, dicts, loops, functions

**Not used (by design):**
- `matplotlib` / `seaborn` / `plotly` — all visualisation is text-art bar charts using `█`
- `regex` — all string parsing done with `.str.replace`, `.str.strip`, `.str.contains`
- `collections.Counter`, `defaultdict` — standard dict used throughout
- Any external salary API or dataset

---

## How to run

### Google Colab (recommended)

1. Open the notebook link below.
2. Upload `bangalore_tech_salaries.csv` via the left sidebar.
3. `Runtime → Run all`.

**Colab:** [LiveProject_Salary_Decoder — Ganesh Nayak](https://colab.research.google.com/drive/1OTMErs3L_Kwo1mhXc-6aAR21N9ofYG98?usp=sharing)

### Local Jupyter

```bash
git clone https://github.com/3132Ganesh/live-project.git
cd live-project
pip install pandas numpy
jupyter notebook LiveProject_Salary_Decoder_Ganesh.ipynb
```

Place `bangalore_tech_salaries.csv` in the same directory.

---

## Dataset

| Property | Value |
|----------|-------|
| File | `bangalore_tech_salaries.csv` |
| Rows | 1,015 (synthetic, realistic patterns) |
| Snapshot | Bengaluru tech, 2024 |
| Roles | SDE Backend, Data Scientist, PM, DevOps, Full-Stack, BA, UI/UX, Data Analyst |
| Company types | Unicorn, MNC, Mid-size, Early-stage |
| Education tiers | Tier 1 / Tier 2 / Tier 3 |

Synthetic data — generated by The Unlox Academy around real Indian tech compensation patterns. Every number in the report comes from running the code, not from guessing.

---

## Project structure

```
live-project/
├── LiveProject_Salary_Decoder_Ganesh.ipynb   # Main notebook
├── bangalore_tech_salaries.csv               # Dataset
└── README.md                                 # This file
```

---

## Built by

**M. Ganesh Nayak**  
B.Tech Data Science, GRIET Hyderabad — Batch of 2026  
Roll No: 23241A67A1

Built in a 2-hour live sprint as part of The Unlox Academy Weeks 1+2 Industry Project.

---

*Bangalore Tech Salary Decoder • Built with Python + Pandas + NumPy*
