# Cross-Market Analysis

A Streamlit dashboard that pulls live cryptocurrency (CoinGecko), commodity
(WTI oil, via the `datasets/oil-prices` GitHub CSV), and equity index
(S&P 500, NASDAQ, NIFTY 50, via `yfinance`) data into a local SQLite
database (`datascienceproject.db`) and lets you explore it with charts,
a SQL query console, and a per-coin deep-dive page.

This repo is a cleaned-up, self-contained version of the original
`Cross_Market_Analysis.ipynb` Colab notebook — the notebook mixed data
exploration, a Colab/Cloudflare tunnel setup, and several rewritten drafts
of `app.py`. This repo keeps only the final, working app.

## Features

- **ETL on first run** — `init_db()` creates the SQLite schema and
  `populate_data()` fetches and caches crypto, oil, and stock data so the
  app only re-fetches when the tables are empty.
- **Cross-market dashboard** — compare crypto, oil, and equity indices
  on one page.
- **SQL console** — run predefined or custom SQL queries against the
  database and download results as CSV.
- **Top cryptocurrency analysis** — per-coin price trend chart, summary
  stats, and a downloadable price table.

## Project structure

```
.
├── app.py              # Streamlit application (ETL + dashboard)
├── requirements.txt    # Python dependencies
└── README.md
```

## Setup

```bash
git clone <your-repo-url>
cd <your-repo-name>
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

## Run locally

```bash
streamlit run app.py
```

The first run will take roughly 30–60 seconds while it fetches crypto,
oil, and stock data and writes them into `datascienceproject.db`
(created in the working directory, ignored by `.gitignore`). Subsequent
runs reuse the cached database.

## Data sources

| Data          | Source                                                                 |
|---------------|-------------------------------------------------------------------------|
| Cryptocurrency | [CoinGecko API](https://www.coingecko.com/en/api) — top 250 coins by market cap, 365 days of history |
| Oil prices     | [`datasets/oil-prices`](https://github.com/datasets/oil-prices) WTI daily CSV |
| Equity indices | [`yfinance`](https://pypi.org/project/yfinance/) — `^GSPC` (S&P 500), `^IXIC` (NASDAQ), `^NSEI` (NIFTY 50) |

## Notes

- The database file and any tunnel/log artifacts from the original Colab
  workflow (`nohup.out`, `logs.txt`, `cloudflared-linux-amd64`) are not
  needed to run this locally and are excluded via `.gitignore`.
- CoinGecko's free tier is rate-limited; fetching 250 coins with 365 days
  of history each can take a while and may occasionally hit rate limits.
  If that happens, rerun the app — `populate_data()` skips tables that
  already have data.
