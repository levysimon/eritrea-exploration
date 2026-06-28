# Eritrea Metrics Tracker

A Jupyter notebook that aggregates open-source data on Eritrea — one of the most information-restricted countries in the world — into a single, reproducible analysis pipeline.

## What it does

The notebook pulls from three data sources, caches everything locally, and produces charts for each dataset:

| Section | Source | What it tracks |
|---|---|---|
| Port activity | PortWatch (FAO/Oxford) | Ship calls, import/export tonnage at Massawa |
| Humanitarian funding | HDX HAPI | Aid requirements vs. funding received per year |
| Conflict events | HDX HAPI (ACLED) | Events and fatalities by year and type (from 2003) |
| Rainfall | HDX HAPI | Dekad precipitation vs. long-term average, by region |
| Returnees | HDX HAPI | Population returning to Eritrea, by country of origin |
| Refugees | HDX HAPI | Eritreans abroad, by asylum country, gender, and age |


## Project structure

```
.
├── eritrea.ipynb           # Main notebook
├── .env                    # API credentials (never committed)
├── data/                   # Cached CSV files (auto-created)
│   ├── portwatch_eritrea_cache.csv
│   ├── Returnees_Eritrea.csv
│   ├── Conflict_Events_Eritrea.csv
│   ├── Funding_Eritrea.csv
│   ├── Rainfall_Eritrea.csv
│   └── eritrea_refugees.csv
└── figures/                # Output plots (auto-created)
    ├── massawa_dashboard.png
    ├── humanitarian_funding_per_year.png
    ├── conflict_analysis_2003_2026.png
    ├── rainfall_national.png
    ├── rainfall_regional_comparison.png
    ├── returnees_top5.png
    ├── refugees_overview.png
    └── refugees_demographics.png
```

## Setup

**1. Install dependencies**
```bash
pip install pandas matplotlib seaborn requests python-dotenv
```

**2. Create a `.env` file** at the project root:
```
HDX_APP_IDENTIFIER=your_base64_token_here
```
Get your token by registering at [hapi.humdata.org](https://hapi.humdata.org).


**3. Run the notebook** top to bottom. On first run it fetches all data and caches it in `data/`. Subsequent runs load from cache (refreshes after 30 days).

## Caching

All datasets are cached locally for 30 days. If a file is missing or expired, the notebook re-fetches it automatically. The refugees dataset is large (~35 000 rows) and uses a paginated JSON fetch — this may take a minute on first run.

## Notes

- Massawa is Eritrea's only active port, making it a rare proxy for economic activity.
- Conflict data is filtered from 2003 onwards.
- Rainfall uses dekad (10-day) aggregation periods to avoid mixing timescales.
