# Final-Capstone-Project
# Labour Force, Disability and Child Labour in Bangladesh, 2022

**Presenter:** Mst. Najnin Sultana · **ID:** 25868400021
**Course:** Special Topics in Data Analytics · **Course code:** 826818

A descriptive data-analytics project built on three official Bangladesh Bureau of Statistics (BBS) 2022 surveys:

| Survey | Part used | What it covers |
|---|---|---|
| NCLS 2022 (National Child Labour Survey) | Chapters 4, 5, 6 | Working children, child labour, hazardous work, schooling, hours, income, health |
| LFS 2022 (Labour Force Survey) | Chapter 8 | Unemployment by age, education, sex, area; 2010-2022 trend |
| DILFS 2022 (Disability Insights of LFS) | Chapter 4 | Labour force, employment and unemployment of persons with disabilities |

---

## 1. Project description (problem, data, method, outcome)

**Problem.** Official labour statistics are published as many separate tables. Policy makers cannot easily see
(a) how disability changes labour-market participation, (b) who is unemployed (age, sex, education, area), and
(c) how child work competes with schooling. This project merges the tables into one dataset and presents them
as interactive 2D/3D charts, tests, and a written report.

**Objectives**
1. Merge NCLS, LFS and DILFS tables into one tidy long-format file and document its quality.
2. Compare the same numbers as 2D and 3D charts (3D pie, bar, cylinder; 2D pie, bar, horizontal, stacked).
3. Test whether male-female unemployment gaps are statistically significant (paired t-test, Wilcoxon).
4. Deliver a dashboard (Streamlit), an ERP-style web dashboard (HTML/CSS/JS + PHP), a notebook, a report and slides.

**Method.** Load -> clean (missing values, duplicates, IQR outliers) -> explore -> chart -> test -> interpret.
Data are published aggregates, so the analysis is descriptive with small-sample tests, not predictive modelling.

**Main findings**
- Unemployment is low and nearly equal by sex (3.5% men, 3.6% women) but concentrated in ages 15-24 (10.97%).
- Labour-force participation of persons with disabilities is 22.5% vs 62.3% for others (11.3% for women with disabilities).
- Unemployment rises with education (1.07% none -> 12.0% tertiary).
- 3.54 million children work; 99% informally; school attendance falls from 86.6% (5-11) to 51.4% (14-17).

---

## 2. Files in this folder

| File | Type | Purpose |
|---|---|---|
| `streamlit_app.py` | Python | Main dashboard. Sections: Dashboard, Labour force & disability, Child labour, Literacy & training, Machine learning, Statistics & data quality, Chart studio |
| `labour_core.py` | Python | Shared helpers: table readers (`tbl`, `wide`, `dil`, `lfs`), question builders `q1_...q10_...`, statistics (`male_female_tests`, `trend_regression`, `data_quality`) and all chart functions (`pie3d`, `bars3d`, `bars2d`, `pie2d`, `make_charts`) |
| `lfs_core.py` | Python | Literacy / vocational-training loader and the machine-learning pipeline (`ml_frame`, `design`, `run_ml`) |
| `labour_force_analysis.ipynb` | Notebook | Same analysis and charts as the dashboard, step by step (54 cells) |
| `CLFS_2022_DILFS_2022_LFS_2022.csv` | Data | 1,869 values from 52 tables/figures, long format (survey, chapter, table_id, row/column labels, unit, value) |
| `requirements.txt` | Config | Python packages |
| `labour_erp_dashboard.html` | Web | Static ERP-style dashboard: left sidebar, dropdowns, results table, Chart.js graph, benchmark input |
| `php_version/index.php` + `data.json` | PHP | Same dashboard with a server-side POST form (PHP computes statistics and table) |
| `Labour_Force_Report_Merged.docx` / `.doc` | Report | Cover page, abstract, theory chapters (1-3), two analysis chapters (4-5), 2D/3D comparisons, conclusions, references on the last page. The `.doc` is the same report in the older Word format |
| `Labour_Force_Presentation.pptx` / `.ppt` | Slides | 14 slides with speaker notes: presenter information, objectives, method, key findings, three 2D vs 3D comparisons, statistical tests, limitations, conclusions, references. The `.ppt` is the older PowerPoint format |
| `DILFS_2022_LFS_2022.csv` | Data (**not included**) | Literacy/training tables. Place it here, or upload it in the app sidebar, to enable the Literacy & training and Machine learning sections |

---

**Before submitting:** replace the bracketed supervisor name and department on the report cover page, and insert the university logo if your format requires it.

---

## 3. How to run

**Streamlit dashboard and notebook**
```bash
pip install -r requirements.txt
streamlit run streamlit_app.py
jupyter notebook labour_force_analysis.ipynb
```

**PHP ERP dashboard** (needs PHP 7.4+)
```bash
cd php_version
php -S localhost:8000
# open http://localhost:8000
```

**Static ERP dashboard:** open `labour_erp_dashboard.html` in any browser (needs internet for Chart.js from cdnjs).

---

## 4. Code overview

1. `labour_core.load_data(src)` reads the CSV (UTF-8 with BOM) without turning the education label "None" into a blank.
2. `tbl / wide / dil / lfs` select a table by survey and ID and pivot it wide.
3. Question builders (`q1_emp_unemp`, `q2_age_area`, `q3_outside_lf`, `q4_unemp_rate`, `q4_participation`, `q5_sector`, `q6_status`, `q8_unemp_by_education`, `q9_child_industry`, `q10_school`) return chart-ready tables.
4. `make_charts(df, style)` draws any of seven styles; `streamlit_app.show()` adds a style switch; `two_d_three_d()` draws the same data twice.
5. `lfs_core.run_ml()` runs the machine-learning comparison when the literacy/training CSV is present.

## 5. Limitations
No age x disability, training or literacy variables exist in the published tables; the analysis states this instead of estimating.
Source quirks (for example 11,522.72 vs 11,552.72 for urban earnings) are kept as printed.

## 6. Data source
Bangladesh Bureau of Statistics (BBS): NCLS 2022, LFS 2022, DILFS 2022.
