# 🎬 CineScope — Movie Analytics Dashboard

> An end-to-end Power BI project analyzing 5,000+ films from 1916 to 2016 using Power Query, Power Pivot (DAX), and interactive dashboards.

---

## 📊 Project Overview

CineScope transforms a raw IMDB movie dataset into a fully interactive analytics dashboard. The project covers the full data pipeline — from ETL and data modeling to DAX measures and dashboard design.

**Dataset:** 4,923 movies · 14 columns · 66 countries · 100 years (1916–2016)  
**Tools:** Excel · Power Query · Power Pivot · Power BI Desktop

---

## 📁 Repository Structure

```
CineScope/
│
├── data/
│   └── movie_data.csv           # Raw IMDB dataset
│
├── excel/
│   └── Movies1.xlsx             # Power Query + Power Pivot workbook
│
├── powerbi/
│   └── Movie.pbix               # Final Power BI dashboard file
│
├── theme/
│   └── CineScope_EarthTone_Theme.json   # Custom Power BI theme
│
└── README.md
```

---

## ⚙️ Phase 1 — Power Query (ETL)

| Step | Description |
|---|---|
| Split genres | Used Split Column by Delimiter → Rows so each genre gets its own row |
| Clean countries | Verified consistency of country names (USA, UK, etc.) |
| Remove nulls | Filtered rows where `imdb_score` is null |
| Remove duplicates | Checked for duplicate `movie_title` + `title_year` combinations |
| Budget Tier column | Conditional column: `< 10K votes = Indie`, `10K–100K = Mid`, `100K+ = Blockbuster` |
| Trim titles | Stripped trailing whitespace from `movie_title` |

---

## 🧮 Phase 2 — Data Model & DAX (Power Pivot)

### Measures

```dax
Total Movies = COUNTROWS(movie_data)

Avg IMDB Score = AVERAGE(movie_data[imdb_score])

User Engagement = SUM(movie_data[num_voted_users]) / [Total Movies]

Top Director =
CONCATENATEX(
  TOPN(1, VALUES(movie_data[director_name]), [Total Movies], DESC),
  movie_data[director_name]
)

% of Total Movies =
DIVIDE(
  [Total Movies],
  CALCULATE([Total Movies], ALL(movie_data))
)
```

### Calculated Columns

```dax
Decade = LEFT(TEXT([title_year], "0000"), 3) & "0s"

Score Band =
IF([imdb_score] >= 8, "Excellent",
  IF([imdb_score] >= 7, "Good",
    IF([imdb_score] >= 5, "Average", "Poor")))

Budget Tier =
IF([num_voted_users] < 10000, "Indie",
  IF([num_voted_users] < 100000, "Mid", "Blockbuster"))
```

### Date Dimension Table

A separate `DimYear` table (1916–2016) was created and linked to `movie_data[title_year]` to enable proper time-based filtering and DAX time intelligence functions.

---

## 📈 Phase 3 — Dashboard Visuals

| Visual | Chart Type | Fields Used |
|---|---|---|
| KPI cards | Card | Total Movies, Avg IMDB Score, User Engagement, Top Director |
| Top genres | Clustered Bar Chart | genres → Total Movies |
| Movies over time | Line & Column Chart | title_year → Total Movies + Avg IMDB Score |
| Score distribution | Clustered Bar Chart | Score Band → Total Movies |
| Budget tier | Clustered Bar Chart | Budget Tier → Total Movies |
| Director rankings | Matrix | director_name → Total Movies, Avg IMDB Score |
| Country map | Bubble Map | country → Total Movies (size), Avg IMDB Score (color) |
| Slicers | Slicer (Dropdown) | Decade, Genre, Language |

---

## 🎨 Theme

Custom Earth Tone theme applied — warm beige background, terracotta accents, forest green secondary color. Theme JSON file included in `/theme/` folder.

To apply: **View → Themes → Browse for themes** → select `CineScope_EarthTone_Theme.json`

---

## 🔍 Key Insights

- **USA dominates** global production with 75% of all films (3,807 out of 4,923)
- **Drama** is the most common genre with 2,186 films
- **54%** of all films score between 5–7 (Average band)
- **Steven Spielberg** is the most prolific director with 26 films
- Movie production grew dramatically after the **1980s**
- **Blockbuster** films (100K+ votes) make up 38% of the dataset

---

## 🚀 How to Use

1. Clone or download this repository
2. Open `Movies1.xlsx` in Excel to explore the Power Query steps and DAX measures
3. Open `Movie.pbix` in Power BI Desktop
4. Apply the theme from `/theme/CineScope_EarthTone_Theme.json`
5. Use the slicers to filter by Decade, Genre, or Language

---

## 🛠️ Tools & Skills Demonstrated

- Power Query (ETL, data cleaning, column transformations)
- DAX (measures, calculated columns, TOPN, DIVIDE, ALL, CONCATENATEX)
- Data modeling (star schema, dimension tables, relationships)
- Power BI (dashboard design, conditional formatting, Top N filters, map visuals)
- Custom theme design (JSON theme file)

---

## 📸 Dashboard Preview

> *(Add a screenshot of your final dashboard here)*  
> In Power BI: File → Export → Export to PDF or take a screenshot and save as `dashboard_preview.png`

---

## 👤 Author

**Habiba Mohamed**  
Connect on [LinkedIn](#) · [GitHub](#)
