# E-Commerce-Funnel-Analysis
A end-to-end funnel analysis project tracking 1,000 users across a 4-stage e-commerce purchase journey — from Homepage to Order Confirmation. The project identifies drop-off points, segments conversion rates by gender, device, and traffic source, and visualises findings in an interactive Power BI dashboard.

## 📊 Dashboard Preview
<img width="637" height="373" alt="image" src="https://github.com/user-attachments/assets/f2247051-0096-4ed5-be93-854bb8b89ec0" />


## 🔍 Key Findings
| Metric | Value |
|---|---|
| **Overall Conversion Rate** | 12.1% |
| **Biggest Drop-off** | Home → Search (45%) |
| **Best Traffic Source** | Social (14.7%) |
| **Lowest Traffic Source** | Direct (8.8%) |


## 🗂️ Dataset
Custom-generated dataset simulating real e-commerce user behaviour across 5 tables:

| Table | Rows | Description |
|---|---|---|
| `user_table.csv` | 1,000 | Demographics: gender, age, country, device, traffic source |
| `home_page.csv` | 1,000 | Users who visited the Home Page |
| `search_page.csv` | 550 | Users who reached the Search/Product Page |
| `payment_page.csv` | 220 | Users who reached the Payment Page |
| `confirmation_page.csv` | 121 | Users who completed the purchase |

---

## 🔍 Key Findings

| Metric | Value |
|---|---|
| Overall Conversion Rate | 12.1% |
| Biggest Drop-off | Home → Search (45%) |
| Best Traffic Source | Social (14.7%) |
| Lowest Traffic Source | Direct (8.8%) |
| Best Converting Device | Desktop (12.6%) |
| Gender Difference | Negligible (~12% both) |

### Traffic Source Insights
- **Social (14.7%)** and **Organic (14.2%)** are top performers
- **Paid Ads (9.0%)** underperform despite ad spend — targeting or landing page mismatch
- **Direct (8.8%)** lowest — users likely still in research/browse mode

### Gender x Traffic Source
- Organic Female users convert at **15.49%** — highest segment
- Social Male users convert at **17.60%** — strongest male segment
- Direct traffic consistently underperforms across both genders

---

## 🛠️ Tools Used
- **Excel** — Initial EDA, VLOOKUP for segmentation, PivotTables, Funnel Chart
- **SQL (SQLite via Python/Kaggle)** — Funnel queries, JOIN-based segmentation, drop-off analysis
- **Power BI** — Interactive dashboard with slicers for gender, device, and traffic source
- **Python (Pandas)** — Dataset generation and SQL execution on Kaggle

---

```

---

## 💻 SQL Highlights

### Basic Funnel Count
```sql
SELECT 'Home Page' AS step, COUNT(*) AS users FROM home_page
UNION ALL
SELECT 'Search Page', COUNT(*) FROM search_page
UNION ALL
SELECT 'Payment Page', COUNT(*) FROM payment_page
UNION ALL
SELECT 'Confirmation', COUNT(*) FROM confirmation_page;
```

### Genderwise Conversion with JOINs
```sql
SELECT u.gender,
    COUNT(DISTINCT h.user_id) AS home_users,
    COUNT(DISTINCT c.user_id) AS confirmed_users,
    ROUND(COUNT(DISTINCT c.user_id) * 100.0 / COUNT(DISTINCT h.user_id), 1) AS conv_pct
FROM home_page h
LEFT JOIN confirmation_page c ON h.user_id = c.user_id
JOIN user_table u ON h.user_id = u.user_id
GROUP BY u.gender;
```

---

## 📈 Analysis Steps
1. Loaded 5 CSV tables into SQLite via Python (Kaggle)
2. Built basic funnel count using UNION ALL
3. Calculated step-by-step and overall conversion rates using window functions
4. Segmented by gender using LEFT JOIN + user_table
5. Segmented by device type
6. Segmented by traffic source — identified best and worst performers
7. Cross-segmented gender x traffic source
8. Identified drop-off users using LEFT JOIN + IS NULL
9. Built user-level master funnel table
10. Visualised all findings in Power BI dashboard

---

## 🚀 How to Run
1. Clone the repo
2. Upload CSVs to Kaggle or run locally with Python
3. Run `sql/funnel_queries.ipynb` in your SQL environment
4. Open `dashboard/funnel_dashboard.pbix` in Power BI Desktop
