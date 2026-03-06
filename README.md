# 🏏 IPL Analytics Dashboard 2008–2024
### End-to-End Sports Analytics Project | MySQL + Power BI

![Uploading Screenshot 2026-03-06 193704.png…]()



## 📌 Project Overview

This project performs a complete end-to-end analysis of **17 seasons of IPL cricket data (2008–2024)** using MySQL for data storage and querying, and Power BI for interactive dashboard visualization.

The goal is to uncover performance patterns across batters, bowlers, teams, and venues — and answer key strategic questions about what wins IPL matches.

| Detail | Info |
|--------|------|
| **Domain** | Sports Analytics — Cricket |
| **Dataset** | IPL Complete Dataset 2008–2024 (Kaggle) |
| **Tools** | Python · MySQL · Power BI · Pandas |
| **Records** | 1,095 matches · 260,920 deliveries |
| **Seasons** | 17 seasons (2008 → 2024) |
| **GitHub** | github.com/Rushikesh262001/ipl-analytics |

---

## 🎯 Business Questions Answered

- 🏏 Who are the all-time top run scorers in IPL history?
- 🎯 Which bowlers have taken the most wickets?
- 🏆 Which teams have won the most matches?
- 🎲 Does winning the toss actually help win the match?
- 💥 Who hits the most sixes?
- 📍 Which venues produce the highest scoring matches?
- 📈 How has IPL scoring evolved across 17 seasons?
- 🏅 Who wins the most Player of the Match awards?

---

## 🔑 Key Findings

### 🏏 Batting
| Insight | Finding |
|---------|---------|
| All-time top scorer | **V Kohli — 8,014 runs** in 244 matches |
| Highest avg per match | **DA Warner — 35.69** runs/match |
| Six King | **CH Gayle — 359 sixes** in 98 matches |
| Most consistent | Kohli leads #2 by **1,245 runs** |

### 🎯 Bowling
| Insight | Finding |
|---------|---------|
| Most wickets | **YS Chahal — 205 wickets** in 120 matches |
| Best wickets/match | **JJ Bumrah — 1.95** per match |
| Most impactful | Bumrah most dangerous per game despite fewer total wickets |

### 🏆 Teams & Strategy
| Insight | Finding |
|---------|---------|
| Most wins | **Mumbai Indians — 144 wins** |
| Toss advantage | Teams that **field first win 53.86%** of the time |
| Batting first | Only **45.38%** win rate when batting first |
| Best strategy | **Choose to field after winning toss** |

### 📈 Scoring Trends
| Insight | Finding |
|---------|---------|
| Lowest avg season | 2009 — **286.89 runs/match** |
| Highest avg season | **2024 — 365.79 runs/match** |
| Overall growth | IPL scoring up **+28%** from 2008 to 2024 |
| Most explosive year | 2024 — T20 cricket is evolving rapidly |

### 📍 Venues
| Insight | Finding |
|---------|---------|
| Highest scoring | **Dr. YS Reddy ACA-VDCA — 400 avg runs** |
| Most batter-friendly | Chinnaswamy Stadium, Bengaluru |
| Most matches played | Wankhede Stadium, Mumbai — 45 matches |

---

## 🗄️ SQL Queries

### Query 1 — Top 10 Run Scorers
```sql
SELECT 
    batter,
    SUM(batsman_runs) AS total_runs,
    COUNT(DISTINCT match_id) AS matches_played,
    ROUND(SUM(batsman_runs) / COUNT(DISTINCT match_id), 2) AS avg_runs_per_match
FROM deliveries
GROUP BY batter
ORDER BY total_runs DESC
LIMIT 10;
```

### Query 2 — Top 10 Wicket Takers
```sql
SELECT 
    bowler,
    COUNT(*) AS total_wickets,
    COUNT(DISTINCT match_id) AS matches_played,
    ROUND(COUNT(*) / COUNT(DISTINCT match_id), 2) AS wickets_per_match
FROM deliveries
WHERE is_wicket = 1
AND dismissal_kind NOT IN ('run out', 'retired hurt', 'obstructing the field')
GROUP BY bowler
ORDER BY total_wickets DESC
LIMIT 10;
```

### Query 3 — Most Successful Teams
```sql
SELECT 
    winner,
    COUNT(*) AS total_wins,
    ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM matches WHERE winner IS NOT NULL), 2) AS win_percentage
FROM matches
WHERE winner IS NOT NULL
GROUP BY winner
ORDER BY total_wins DESC
LIMIT 10;
```

### Query 4 — Toss Impact Analysis
```sql
SELECT 
    toss_decision,
    COUNT(*) AS total_matches,
    SUM(CASE WHEN toss_winner = winner THEN 1 ELSE 0 END) AS toss_winner_won,
    ROUND(SUM(CASE WHEN toss_winner = winner THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS win_percentage
FROM matches
WHERE winner IS NOT NULL
GROUP BY toss_decision;
```

### Query 5 — Top Six Hitters
```sql
SELECT 
    batter,
    COUNT(*) AS total_sixes,
    COUNT(DISTINCT match_id) AS matches_played
FROM deliveries
WHERE batsman_runs = 6
GROUP BY batter
ORDER BY total_sixes DESC
LIMIT 10;
```

### Query 6 — Best Venues by Avg Score
```sql
SELECT 
    m.venue,
    COUNT(DISTINCT m.id) AS matches_played,
    ROUND(SUM(d.total_runs) / COUNT(DISTINCT m.id), 2) AS avg_total_runs
FROM matches m
JOIN deliveries d ON m.id = d.match_id
GROUP BY m.venue
ORDER BY avg_total_runs DESC
LIMIT 10;
```

### Query 7 — Season-wise Scoring Trends
```sql
SELECT 
    m.season,
    SUM(d.total_runs) AS total_runs,
    COUNT(DISTINCT m.id) AS matches_played,
    ROUND(SUM(d.total_runs) / COUNT(DISTINCT m.id), 2) AS avg_runs_per_match
FROM matches m
JOIN deliveries d ON m.id = d.match_id
GROUP BY m.season
ORDER BY m.season;
```

### Query 8 — Player of the Match Leaderboard
```sql
SELECT 
    player_of_match,
    COUNT(*) AS awards,
    COUNT(DISTINCT season) AS seasons_active
FROM matches
WHERE player_of_match IS NOT NULL
GROUP BY player_of_match
ORDER BY awards DESC
LIMIT 10;
```

---

## 📊 Dashboard

The Power BI dashboard includes **8 interactive visuals**:

| Visual | Type | Data Source |
|--------|------|-------------|
| Top Run Scorers | Horizontal Bar | top_run_scorers.csv |
| Top Wicket Takers | Bar Chart | top_wicket_takers.csv |
| Most Successful Teams | Horizontal Bar | team_wins.csv |
| Toss Decision Impact | Pie Chart | toss_impact.csv |
| Top Six Hitters | Horizontal Bar | top_six_hitters.csv |
| Highest Scoring Venues | Horizontal Bar | best_venues.csv |
| Season Scoring Trend | Line Chart | season_stats.csv |
| Player of Match Awards | Horizontal Bar | player_of_match.csv |

**Dashboard Theme:** Dark background `#070810` · Gold accent `#F5A623` · Innovate theme

---

## 🚀 How to Run

### Prerequisites
- Python 3.x
- MySQL Server + MySQL Workbench
- Power BI Desktop (free)
- Kaggle account (for dataset)

### Step 1 — Clone the repo
```bash
git clone https://github.com/Rushikesh262001/ipl-analytics.git
cd ipl-analytics
```

### Step 2 — Install Python dependencies
```bash
pip install pandas sqlalchemy mysql-connector-python
```

### Step 3 — Download the dataset
1. Go to: https://www.kaggle.com/datasets/patrickb1912/ipl-complete-dataset-20082020
2. Download and extract both CSV files
3. Place in `data/` folder:
   - `data/matches.csv`
   - `data/deliveries.csv`

### Step 4 — Set up MySQL database
Open MySQL Workbench and run:
```sql
CREATE DATABASE ipl_analysis;
USE ipl_analysis;
```

### Step 5 — Load data into MySQL
Open `notebooks/01_data_loading.ipynb` and update your MySQL password:
```python
engine = create_engine("mysql+mysqlconnector://root:YOUR_PASSWORD@localhost/ipl_analysis")
```
Run all cells — loads 1,095 matches and 260,920 deliveries.

### Step 6 — Run SQL analysis
Open MySQL Workbench → run all 8 queries from `sql/ipl_analysis_queries.sql`

### Step 7 — Export results
Run `notebooks/02_export_results.ipynb` — saves 8 CSV files to `outputs/`

### Step 8 — Open Power BI Dashboard
1. Open `dashboard/ipl_analytics_dashboard.pbix`
2. Update data source path to your local `outputs/` folder
3. Refresh all visuals

---

## 📁 Project Structure

```
ipl-analytics/
│
├── data/
│   ├── matches.csv
│   └── deliveries.csv
│
├── notebooks/
│   ├── 01_data_loading.ipynb
│   └── 02_export_results.ipynb
│
├── sql/
│   └── ipl_analysis_queries.sql
│
├── outputs/
│   ├── top_run_scorers.csv
│   ├── top_wicket_takers.csv
│   ├── team_wins.csv
│   ├── toss_impact.csv
│   ├── top_six_hitters.csv
│   ├── best_venues.csv
│   ├── season_stats.csv
│   └── player_of_match.csv
│
├── dashboard/
│   └── ipl_analytics_dashboard.pbix
│
├── dashboard_screenshot.png
└── README.md
```

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| **Python** | Data loading and export automation |
| **Pandas** | CSV reading and data manipulation |
| **SQLAlchemy** | Python → MySQL connection |
| **MySQL** | Data storage and SQL analysis |
| **MySQL Workbench** | SQL query execution |
| **Power BI Desktop** | Interactive dashboard |

---

## 👨‍💻 Author

**Rushikesh**
- GitHub: [github.com/Rushikesh262001](https://github.com/Rushikesh262001)
- Project 1: [E-Commerce Churn Analysis](https://github.com/Rushikesh262001/ecommerce-churn-analysis)

---

## 📄 License
This project is open source and available under the [MIT License](LICENSE).

---
⭐ If you found this project helpful, please give it a star!<
