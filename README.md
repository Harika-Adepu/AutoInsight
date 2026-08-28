# AutoInsight

**AI-Augmented Data Cleaning & Reporting Pipeline**

AutoInsight is a fully generic, end-to-end data pipeline that cleans, validates, warehouses, visualizes, and AI-summarizes **any tabular CSV dataset** — not just one hardcoded file. Point it at a CSV URL or a local file, and it automatically adapts its cleaning, validation, and dashboard logic to that dataset's own columns and data types, no code changes required.

## 🚀 What it Does

- **Universal CSV Ingestion** — Accepts any CSV via URL *or* local file path, with automatic UTF-8 → Latin-1 encoding fallback.
- **Dynamic Schema Validation** — Builds a Pydantic model at runtime from the dataset's actual columns and dtypes, instead of relying on a hardcoded schema.
- **Intelligent Cleaning** — Auto-detects and drops junk index columns, deduplicates rows (on a chosen subset or all columns), coerces text-that-looks-numeric into real numbers, and imputes missing values (median for numeric columns, `"Unknown"` for text).
- **Smart Auto-Dashboarding** — Automatically picks meaningful metric and label columns (while excluding ID/index-like columns) to render a ranked bar chart and a correlation heatmap in Plotly — the chart adapts to whatever dataset you give it.
- **AI Executive Summaries** — Uses the Google Gemini API to generate a concise, natural-language executive summary of each pipeline run's key metrics.
- **SQL Warehousing** — Persists every cleaned dataset into a SQLite warehouse via SQLAlchemy, keyed by table name, so multiple datasets coexist without overwriting each other.

## 🛠️ Technical Highlights

- Runtime Pydantic schema generation from live DataFrame dtypes — no dataset-specific model definitions.
- Heuristic identifier detection (name-token matching + contiguous-sequence check) to reliably tell real metrics apart from ID/index columns during column selection.
- Column-selection logic for the dashboard that adapts to each dataset's actual shape — no assumptions about fixed column names.
- **Stack:** Python (Pandas, NumPy), Pydantic, SQLAlchemy, SQLite, Plotly, Google Gemini API

## 💡 Benefits of this Project

- **Reusability** — One pipeline that works across arbitrarily different domains (movies, restaurant tips, sports data, etc.) with zero code changes between runs.
- **Data Quality Assurance** — Built-in validation and cleaning rules automatically catch and correct common CSV data-quality issues.
- **Faster Insights** — Goes from a raw CSV to an interactive dashboard plus an AI-written summary with a single function call.
- **Extensible Design** — Clear separation between loading, cleaning, validating, warehousing, and visualizing stages makes it straightforward to swap in new logic for any single step.

## 📋 Dataset Compatibility

AutoInsight is built for standard, single-file tabular CSVs:

- **Format:** Comma-delimited `.csv`, single header row
- **Encoding:** UTF-8 or Latin-1
- **Structure:** Flat tabular data (no nested/multi-file joins)
- **Size:** Best suited for datasets that comfortably fit in memory

Works reliably across mixed numeric/text columns and adapts automatically to each dataset's own schema.

## Usage

```python
run_data_cleaning_pipeline(
    source="your_dataset.csv",      
    dedup_subset=["COL_A", "COL_B"],  # optional
    table_name="cleaned_data"
)
```

Run every cell top to bottom, then call `run_data_cleaning_pipeline()` with any CSV — the pipeline handles the rest.


Code File: AutoInsight.ipynb

Local Datasets: MSFT.csv and ball_by_ball_ipl.csv (link: https://www.kaggle.com/datasets/jamiewelsh2/ball-by-ball-ipl)

Output Files: ai_summary_cache.json and interactive_report.html
