# CommentScope & DS391 Big Data Analytics Pipeline

A dual-component platform combining a full-stack **Node.js YouTube Comment Scraper** with a modular **Python Big Data Analytics & Scalability Benchmarking Pipeline** for DS391.

---

## Architecture & Project Overview

The project is structured into two complementary subsystems:

1. **Python Big Data Analytics Pipeline (DS391)**:
   - Ingests and audits multi-video YouTube comment exports.
   - Cleans, standardizes, and combines datasets with source attribution (`video_name`).
   - Performs parametric and non-parametric engagement analytics (Pareto/Gini inequality, skewness, kurtosis, correlation).
   - Analyzes 10-year longitudinal engagement trends (2016 launch vs. 2025–2026 resurgence) and multilingual lexical patterns.
   - Conducts **Synthetic Scalability Stress Testing** across 1x (2,349 rows), 10x (23,490 rows), and 100x (234,900 rows) workloads to evaluate algorithmic runtime and memory scaling.
   - Generates publication-ready figures, structured JSON metrics, CSV benchmark logs, and the formal academic markdown report.

2. **CommentScope Web Scraper (Node.js / Express)**:
   - Interactive local web dashboard for harvesting public comments via YouTube Data API v3.
   - Search by channel handle or video title, select pagination limits (up to 5,000 comments), and toggle reply inclusion.
   - Export scraped comments directly to CSV or JSON formats.
   - API keys are securely encapsulated in the server environment and never exposed to client browsers.

---

## Repository Structure

```text
YouTube_Comment_Scrapper_clean/
├── data/
│   ├── raw/                               # Authentic, immutable scraped datasets
│   │   ├── high-heels-te-nachche-*.csv    # Video 1: Ki & Ka (1,166 rows, 6 cols)
│   │   └── gali-gali-full-video-*.csv     # Video 2: KGF (1,183 rows, 6 cols)
│   └── processed/                         # Standardized & combined analytical datasets
│       ├── cleaned_comments.csv           # Cleaned combined corpus (2,349 rows, 7 cols)
│       └── combined_comments.csv          # Canonical dual-video export
├── outputs/
│   ├── charts/                            # Publication-quality figures (300 DPI)
│   │   ├── likes_replies_distribution.png # Log-KDE density and log-log correlation
│   │   ├── temporal_activity_trends.png   # 10-year volume & cumulative timeline
│   │   ├── top_keyword_frequency.png      # Lexical keywords & thematic entity mentions
│   │   └── scalability_benchmarks.png     # Runtime & memory scaling curves
│   ├── summary_metrics.json               # Full statistical, temporal, and text metrics
│   ├── benchmark_results.json             # Tri-trial scalability benchmark metrics
│   └── benchmark_results.csv              # Workload scaling summary table
├── report/
│   ├── Project_Report.md                  # Primary DS391 academic project report
│   └── ANALYTICS_REPORT.md                # Synchronized comprehensive report mirror
├── src/                                   # Core Python analytics package
│   ├── __init__.py                        # Package initializer
│   ├── loader.py                          # Automated dataset discovery & loader
│   ├── cleaner.py                         # Data cleaning & dual-dataset union logic
│   ├── inspection.py                      # Schema auditing & duplicate detection
│   ├── statistics.py                      # Parametric/non-parametric engagement metrics
│   ├── text_temporal.py                   # Timeline extraction & lexical NLP parsing
│   ├── visualizer.py                      # Matplotlib / Seaborn visualization engine
│   ├── benchmark.py                       # 1x, 10x, 100x scalability stress tester
│   └── report_gen.py                      # Academic markdown & JSON report compiler
├── public/                                # Web Scraper frontend assets
│   ├── index.html                         # Scraper dashboard UI
│   ├── styles.css                         # Modern responsive styling (dark/light mode)
│   └── app.js                             # Client-side UI interaction controller
├── .env.example                           # Safe environment variable template
├── .gitignore                             # Secure Git exclusion rules
├── package.json                           # Node.js project manifest & scripts
├── package-lock.json                      # Node.js deterministic dependency lockfile
├── requirements.txt                       # Python dependencies
├── run_pipeline.py                        # Master end-to-end analytics pipeline runner
├── run_cleaning.py                        # Standalone data cleaning & combination runner
├── run_benchmark.py                       # Standalone scalability benchmark runner
├── server.js                              # Express backend for YouTube comment scraper
└── README.md                              # Project documentation
```

---

## Getting Started & Installation

### Prerequisites

- **Python**: 3.10 or higher (tested on Python 3.12).
- **Node.js**: 18.x or higher (for the web scraper).
- **Git**: For version control.

### 1. Environment & API Key Setup

1. Copy the example environment template to create your local `.env`:
   ```bash
   # On macOS/Linux:
   cp .env.example .env

   # On Windows PowerShell:
   Copy-Item .env.example .env
   ```
2. Open `.env` and supply your personal Google YouTube Data API v3 key:
   ```env
   YOUTUBE_API_KEY=your_actual_api_key_here
   PORT=3000
   ```
   > **Security Note**: `.env` is strictly excluded by `.gitignore` and must **never** be committed or shared.

### 2. Python Environment Setup

Install the required Python analytics packages:

```bash
pip install -r requirements.txt
```

*Key Python dependencies*: `pandas>=2.0.0`, `numpy>=1.24.0`, `matplotlib>=3.7.0`, `seaborn>=0.12.0`.

### 3. Node.js Environment Setup

Install the web scraper dependencies:

```bash
npm install
```

---

## Execution Guide

### 1. Run the Complete Analytics Pipeline (Recommended)

To execute data ingestion, schema inspection, descriptive statistics, NLP parsing, scalability benchmarking, chart rendering, and report compilation end-to-end:

```bash
python run_pipeline.py
```

**Execution Pipeline Steps**:
1. Automatically loads `data/processed/cleaned_comments.csv` (or combines raw files from `data/raw/`).
2. Audits schema, identifies missing/blank values, and verifies deduplication ($N = 2,349$).
3. Computes parametric and non-parametric engagement metrics (means, medians, IQR, Gini coefficients).
4. Extracts longitudinal temporal patterns (2016–2026) and token frequencies.
5. Executes synthetic scalability benchmarks across 1x, 10x, and 100x workloads with `tracemalloc`.
6. Regenerates publication charts in `outputs/charts/`.
7. Compiles `outputs/summary_metrics.json`, `outputs/benchmark_results.json`, `outputs/benchmark_results.csv`, and `report/Project_Report.md`.

### 2. Run Standalone Data Cleaning & Combination

To re-clean the raw datasets and regenerate `cleaned_comments.csv` and `combined_comments.csv`:

```bash
python run_cleaning.py
```

### 3. Run Standalone Scalability Benchmarking

To run only the scalability stress test and update benchmark logs:

```bash
python run_benchmark.py
```

### 4. Launch the CommentScope Web Scraper

To run the interactive browser-based YouTube comment collection tool:

```bash
npm start
```

Access the dashboard at `http://localhost:3000`.

---

## Synthetic Scalability Stress Test Explained

To evaluate computational performance under big data workloads without mutating or fabricating original records, the pipeline includes an in-memory synthetic stress-testing harness (`src/benchmark.py`):

1. **Replication Strategy**:
   - The verified ground-truth dataset ($N = 2,349$ rows) serves as the baseline ($1\times$).
   - Synthetic workloads are constructed in-memory by replicating the baseline records:
     - **1x Workload**: 2,349 rows
     - **10x Workload**: 23,490 rows
     - **100x Workload**: 234,900 rows
2. **Measurement Rigor**:
   - Each workload level is executed across 3 independent iterations (`n_iterations=3`).
   - Execution latency is measured using high-precision wall-clock timing (`time.perf_counter()`).
   - Peak memory consumption is captured via Python's built-in memory profiler (`tracemalloc`).
3. **Empirical Findings**:
   - **Runtime**: Scales from ~0.135 seconds ($1\times$) to ~11.42 seconds ($100\times$). Processing 100 times as many rows requires ~84.8 times as long, demonstrating worse-than-linear observed scaling driven by string deduplication hashing and repeated regular expression scans.
   - **Heap Memory**: Scales from 0.94 MB ($1\times$) to 89.57 MB ($100\times$), exhibiting near-linear $O(N)$ spatial growth (~94.8x).
4. **Tool Justification**:
   - **When Pandas is Sufficient**: For in-memory workloads under ~10 GB (millions of comments), Pandas executes in single-node RAM without network shuffle or driver coordination latency.
   - **When Apache Spark is Justified**: For multi-gigabyte or terabyte corpora across thousands of channels, out-of-core distributed processing, partitioned joins, and Kafka streaming ingest require Spark or PySpark clusters.

---

## Security & Best Practices

- **API Credentials**: Never hardcode API keys in source files. Always load credentials through `process.env` or Python `os.environ` via local `.env`.
- **Pre-Commit Secrets Audit**: If an API key was ever accidentally committed in previous revisions, simply removing it in a new commit is insufficient. You must revoke or rotate the key immediately in the [Google Cloud Console](https://console.cloud.google.com/apis/credentials) and purge Git history.
- **Git Tracking Cleanliness**: `.gitignore` is pre-configured to exclude `.env`, `node_modules/`, `__pycache__/`, virtual environments, IDE metadata, and local backup folders.

---

## License & Academic Integrity

Created for the **DS391 Big Data Analytics** coursework. All analysis is reproducible from the provided raw datasets in `data/raw/`. External data was harvested via the official Google YouTube Data API v3 in compliance with YouTube API Terms of Service.
