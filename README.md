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

## 📌 Current Dataset Scope & Roadmap

AutoInsight is built to generalize across a wide range of real-world CSVs — flat, comma-delimited, single-header-row tabular data (the shape of most public datasets, like the movies, restaurant-tips, and IPL ball-by-ball datasets it's been tested on). A few boundaries define today's scope, each a clear direction for the next iteration:

- **File format** — Currently reads comma-delimited `.csv` files (URL or local). Extending the loader to support `.tsv`, semicolon-delimited files, and Excel is a natural next step.
- **Encoding coverage** — Handles UTF-8 with automatic Latin-1 fallback, covering the vast majority of public datasets. Broader encoding detection (e.g. `chardet`-based auto-detection) is on the roadmap for fully international datasets.
- **Numeric inference threshold** — Uses an 80% heuristic to decide whether a text column should convert to numeric, tuned to avoid false positives on ID-like columns. Making this threshold configurable per-column is a planned enhancement for edge-case datasets.
- **Imputation strategy** — Fills missing values with median (numeric) / `"Unknown"` (text) as a robust default. Adding pluggable strategies (mode, forward-fill for time series, KNN imputation) would extend it to more specialized use cases.
- **Single flat table** — Designed for one CSV at a time. Support for joining multiple related files is a logical next feature for relational datasets.
- **In-memory processing** — Loads the full dataset into memory, which comfortably handles typical datasets (tested up to hundreds of thousands of rows). Chunked/streaming reads would extend this to multi-GB files.
- **AI summaries** — Require a `GEMINI_API_KEY` environment variable; the pipeline runs fully without one, simply skipping that step.

These aren't hard walls — they reflect where the pipeline is focused today, and each is a concrete direction for future contributions.

## Usage

```python
run_data_cleaning_pipeline(
    source="your_dataset.csv",      
    dedup_subset=["COL_A", "COL_B"],  # optional
    table_name="cleaned_data"
)
```

Run every cell top to bottom, then call `run_data_cleaning_pipeline()` with any CSV — the pipeline handles the rest.
