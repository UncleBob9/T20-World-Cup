# 🏏 T20 World Cup — Best XI Analytics (Project Sportan)

> *"We don't know the strengths and weaknesses of our opponents, but give me the best 11 from this planet."*

A data-driven **Power BI + Python** analytics project that uses historical T20 World Cup cricket data and objective KPIs to select the best XI players across all roles.

---

## 📌 Project Overview

**Project Sportan** answers a simple but powerful question: *Who are the best 11 cricketers in the world right now, based purely on data?*

The project defines two core performance targets:
- 🎯 The team should be able to **score at least 180 runs** on average
- 🛡️ The team should be able to **defend 150 runs** on average

Rather than relying on intuition or reputation, every player is evaluated against role-specific KPIs using real T20 World Cup match data — scraped from ESPN Cricinfo, cleaned with Python, and visualized in Power BI.

---

## 🗂️ Repository Structure

```
T20-World-Cup/
│
├── web_scrapping_codes/          # Python scripts to scrape match data from ESPN Cricinfo
│   └── web_scrapping_codes/
│
├── t20_json_files/               # Raw scraped data in JSON format
│   └── t20_json_files/
│
├── t20_data_preprocessing/       # Jupyter notebooks for data cleaning & transformation
│
├── t20_csv_files/                # Cleaned, processed data ready for Power BI
│
├── T2011.pbix                    # Power BI dashboard file (Best XI selector)
├── DAX_Measures.docx             # All DAX measures with formulas & descriptions
└── Project_Sportan_Paramaeter_Scoping.docx  # Role criteria & KPI definitions
```

---

## 🔄 Data Pipeline

```
ESPN Cricinfo  →  Web Scraping (Python)  →  JSON Files  →  Data Preprocessing  →  CSV Files  →  Power BI
```

### Step 1 — Web Scraping
Python scripts scrape T20 World Cup match data directly from ESPN Cricinfo, capturing batting and bowling scorecards for every match.

### Step 2 — Data Preprocessing
Jupyter notebooks clean and transform the raw JSON into structured CSV files, including:
- Parsing innings-level batting stats (runs, balls, fours, sixes, batting position)
- Parsing bowling stats (wickets, overs, runs conceded, dot balls)
- Flattening nested JSON into flat tabular format

### Step 3 — Power BI Dashboard
The cleaned CSVs feed directly into Power BI where DAX measures compute KPIs and an interactive Best XI selector is built.

---

## 📊 Power BI Dashboard (`T2011.pbix`)

The dashboard allows you to interactively explore player stats and select the best XI based on role-specific criteria. It includes:
- Role-based player filters (Openers, Middle Order, Finishers, All-Rounders, Bowlers)
- KPI scorecards with batting average, strike rate, boundary %, economy, and more
- Dynamic Best XI builder — click any player to see their combined or individual stats
- Comparison views across players within the same role

---

## 📐 Player Selection Criteria

Players are evaluated by role, each with its own set of KPIs:

### 🟢 Openers
| Parameter | Criteria |
|-----------|----------|
| Batting Average | > 30 |
| Strike Rate | > 140 |
| Innings Batted | > 3 |
| Boundary % | > 50% |
| Batting Position | < 4 |

### 🟡 Anchors / Middle Order
| Parameter | Criteria |
|-----------|----------|
| Batting Average | > 40 |
| Strike Rate | > 125 |
| Innings Batted | > 3 |
| Avg. Balls Faced | > 20 |
| Batting Position | > 2 |

### 🔵 Finisher / Lower Order Anchor
| Parameter | Criteria |
|-----------|----------|
| Batting Average | > 25 |
| Strike Rate | > 130 |
| Innings Batted | > 3 |
| Avg. Balls Faced | > 12 |
| Batting Position | > 4 |
| Innings Bowled | > 1 |

### 🟠 All-Rounders / Lower Order
| Parameter | Criteria |
|-----------|----------|
| Batting Average | > 15 |
| Strike Rate | > 140 |
| Innings Batted | > 2 |
| Batting Position | > 4 |
| Innings Bowled | > 2 |
| Bowling Economy | < 7 |
| Bowling Strike Rate | < 20 |

### 🔴 Specialist Fast Bowlers
| Parameter | Criteria |
|-----------|----------|
| Innings Bowled | > 4 |
| Bowling Economy | < 7 |
| Bowling Strike Rate | < 16 |
| Bowling Style | contains "Fast" |
| Bowling Average | < 20 |
| Dot Ball % | > 40% |

---

## 🧮 DAX Measures

All KPIs in Power BI are computed using custom DAX measures. Key measures include:

**Batting**
| Measure | DAX Formula |
|---------|-------------|
| Total Runs | `SUM(fact_batting_summary[runs])` |
| Batting Average | `DIVIDE([Total Runs], [Total Innings Dismissed], 0)` |
| Strike Rate | `DIVIDE([Total Runs], [total balls faced], 0) * 100` |
| Boundary % | `DIVIDE(SUM(fact_batting_summary[Boundary runs]), [Total Runs], 0)` |
| Batting Position | `ROUNDUP(AVERAGE(fact_batting_summary[batting_pos]), 0)` |

**Bowling**
| Measure | DAX Formula |
|---------|-------------|
| Wickets | `SUM(fact_bowling_summary[wickets])` |
| Bowling Economy | `DIVIDE([Runs Conceded], ([balls Bowled] / 6), 0)` |
| Bowling Strike Rate | `DIVIDE([balls Bowled], [wickets], 0)` |
| Bowling Average | `DIVIDE([Runs Conceded], [wickets], 0)` |
| Dot Ball % | `DIVIDE(SUM(fact_bowling_summary[zeros]), SUM(fact_bowling_summary[balls]), 0)` |

> See [`DAX_Measures.docx`](./DAX_Measures.docx) for the full list of all 20 measures and 3 calculated columns with complete descriptions.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| **Python** | Web scraping & data preprocessing |
| **BeautifulSoup / Requests** | Scraping ESPN Cricinfo |
| **Pandas** | Data cleaning & transformation |
| **Jupyter Notebook** | Interactive preprocessing workflow |
| **JSON / CSV** | Intermediate data storage formats |
| **Power BI** | Dashboard & KPI visualization |
| **DAX** | Custom KPI calculations in Power BI |

---

## 🚀 Getting Started

### Prerequisites
- Python 3.8+
- Jupyter Notebook
- Power BI Desktop (free from [Microsoft](https://powerbi.microsoft.com/en-us/desktop/))

### 1. Clone the repository
```bash
git clone https://github.com/UncleBob9/T20-World-Cup.git
cd T20-World-Cup
```

### 2. Install Python dependencies
```bash
pip install pandas requests beautifulsoup4 jupyter
```

### 3. (Optional) Re-scrape the data
Run the scripts in `web_scrapping_codes/` to pull fresh data from ESPN Cricinfo. Pre-scraped JSON files are already included in `t20_json_files/`.

### 4. Run data preprocessing
Open and run the Jupyter notebooks in `t20_data_preprocessing/` to regenerate the cleaned CSV files. Pre-processed CSVs are already included in `t20_csv_files/`.

### 5. Open the dashboard
Open `T2011.pbix` in **Power BI Desktop** to explore the interactive Best XI selector.

---

## 📁 Data Model

The Power BI data model is built on two fact tables and one dimension table:

- **`fact_batting_summary`** — One row per player per innings batted (runs, balls, fours, sixes, batting position, out/not-out)
- **`fact_bowling_summary`** — One row per bowler per innings bowled (wickets, overs, runs, dot balls)
- **`dim_player`** — Player dimension with name, team, batting/bowling style, and custom batting order for the final XI

---

## 📄 Documentation

| File | Description |
|------|-------------|
| [`DAX_Measures.docx`](./DAX_Measures.docx) | Complete list of DAX measures & calculated columns with formulas |
| [`Project_Sportan_Paramaeter_Scoping.docx`](./Project_Sportan_Paramaeter_Scoping.docx) | Full KPI criteria for each player role |

---

## 🙌 Acknowledgements

- Match data sourced from **[ESPN Cricinfo](https://www.espncricinfo.com/)**
- Project inspired by real-world sports analytics workflows used in franchise cricket

---
## 📞 Contact
- **Email:** adarshyadav99772@gmail.com
- **LinkedIn:** www.linkedin.com/in/adarsh-yadav-bb2163246

## 📜 License

This project is open source and available under the [MIT License](LICENSE).
