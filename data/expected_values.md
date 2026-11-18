# Expected values (for reconciliation with the Power BI report)

Computed independently from the CSVs with a Python validation script (not included in this repo). Synthetic data, fictional publisher Quelle Academic.

## KPIs by academic year

|                                  |   2024/25 |   2025/26 |
|:---------------------------------|----------:|----------:|
| Total Item Requests              |  147955   |  151799   |
| Unique Title Requests            |   95192   |   97368   |
| Active Institutions              |      27   |      27   |
| Denials                          |    7852   |    8063   |
| No License Denials               |    4469   |    4592   |
| Limit Exceeded Denials           |    3383   |    3471   |
| No License Share %               |      56.9 |      57   |
| Turnaways per 100 Title Requests |       8.2 |       8.3 |

**Unique Title Requests YoY % (2025/26 vs 2024/25): 2.3%**

## Top 10 institutions by denials (2025/26)

|   institution_id | institution_name                         |   No_License |   Limit_Exceeded |   Denials |
|-----------------:|:-----------------------------------------|-------------:|-----------------:|----------:|
|                6 | Hollingford University                   |           22 |             1239 |      1261 |
|               11 | Norrvik University                       |          108 |             1144 |      1252 |
|               27 | Aurenfjord Institute for the Humanities  |          610 |                0 |       610 |
|               17 | Rheinfeld University of Applied Sciences |           39 |              569 |       608 |
|                3 | University of Ostrabruck                 |          460 |                0 |       460 |
|               14 | Westbrook State University               |          411 |               35 |       446 |
|               26 | Institute of Legal History Veldkamp      |          371 |                0 |       371 |
|                9 | Vandermeer University                    |          345 |                0 |       345 |
|               30 | Westbrook Public Library                 |          341 |                0 |       341 |
|               13 | Halvorsen University                     |          265 |                0 |       265 |

## Top 10 titles by denials (2025/26)

|   title_id | title_name                                    | subject             |   Denials |
|-----------:|:----------------------------------------------|:--------------------|----------:|
|         95 | Customary Law: A Comparative Approach         | Law                 |      1746 |
|         73 | Papyrology: Texts and Contexts                | Classics            |       765 |
|         91 | Legal Pluralism: Rethinking the Evidence      | Law                 |       379 |
|          9 | Rural Communities: Perspectives and Debates   | History             |       287 |
|         37 | Reformation Debates: From Manuscript to Print | Theology & Religion |       285 |
|         36 | Church and State: Rethinking the Evidence     | Theology & Religion |       258 |
|         32 | Reformation Debates: Case Studies             | Theology & Religion |       256 |
|         94 | Canon Law: An Introduction                    | Law                 |       240 |
|          2 | Early Print Culture: Rethinking the Evidence  | History             |       220 |
|         74 | Late Antique Letters: A Comparative Approach  | Classics            |       219 |

## Largest declines in Unique Title Requests (2025/26 vs 2024/25)

|   institution_id |   2024/25 |   2025/26 |   change_pct | institution_name                        |
|-----------------:|----------:|----------:|-------------:|:----------------------------------------|
|                2 |      3406 |      1965 |        -42.3 | Eldermark University                    |
|               19 |      2780 |      2243 |        -19.3 | Sonnberg University of Applied Sciences |
|               12 |      4475 |      3667 |        -18.1 | University of Tjelde                    |
|               24 |      2169 |      1904 |        -12.2 | Centre for Ancient Languages Lysander   |
|               20 |      4541 |      4593 |          1.1 | Kestrel College of Arts                 |

## Row counts

|                 |   rows |
|:----------------|-------:|
| dim_date        |   1096 |
| dim_institution |     30 |
| dim_title       |    100 |
| fact_usage      |  30025 |
| fact_denials    |   7913 |

## Validation checks

- PASS: dim_date: unique, contiguous daily dates
- PASS: dim_institution: unique PK
- PASS: dim_title: unique PK
- PASS: fact_usage: month_start is 1st of month
- PASS: fact_usage: within Sep 2024 - Aug 2026
- PASS: fact_usage: month_start found in dim_date
- PASS: fact_usage: institution FK valid
- PASS: fact_usage: title FK valid
- PASS: fact_denials: month_start is 1st of month
- PASS: fact_denials: within Sep 2024 - Aug 2026
- PASS: fact_denials: month_start found in dim_date
- PASS: fact_denials: institution FK valid
- PASS: fact_denials: title FK valid
- PASS: fact_usage: unique grain (month, institution, title)
- PASS: fact_denials: unique grain (month, institution, title, metric_type)
- PASS: fact_denials: metric_type in {No_License, Limit_Exceeded}
- PASS: 1 <= unique_title_requests <= total_item_requests
- PASS: all counts positive
- PASS: usage only on licensed pairs
- PASS: No_License only on unlicensed pairs
- PASS: Limit_Exceeded only on licensed, user-limited pairs
