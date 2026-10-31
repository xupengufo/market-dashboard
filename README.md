# Market Dashboard

**Live Site**: https://xupengufo.github.io/market-dashboard/

Static stock dashboard with daily auto-refresh via GitHub Actions (Yahoo Finance), hosted on GitHub Pages.

**Data Updates**: Mon–Fri at 16:30 US Eastern (20:30 UTC)

> Origin: this repository is a copy of [traderwillhu/market_dashboard](https://gitlab.com/traderwillhu/market_dashboard) (MIT License, Copyright © 2026 Will Hu), restyled with a light, elegant theme. The `.gitlab-ci.yml` from the original was removed; deployment now runs on GitHub Pages + GitHub Actions.

## Features

- **Multi-Category ETF Tracking**: Monitor 180+ ETFs across 7 categories:
  - Indices (SPY, QQQ, DIA, etc.)
  - S&P Style ETFs (Value, Growth, etc.)
  - Select Sector ETFs (XLE, XLF, XLK, etc.)
  - Equal-Weight Sector ETFs
  - Industry ETFs
  - Country ETFs (EEM, EFA, etc.)
  - Leveraged ETFs (TQQQ, SQQQ, DPST, etc.)

- **Advanced Technical Indicators**:
  - **Grade (ABC Rating)**: Trend strength based on EMA10 > EMA20 > SMA50 alignment
  - **ATR%**: Daily ATR as percentage of price
  - **ATRx**: Distance from SMA50 in ATR units
  - **1M-VARS**: Volatility-Adjusted Relative Strength vs SPY (21-day)
  - **VARS**: Relative strength visualization chart
  - **Daily/Intra/5D/20D**: Performance metrics with visual bars

- **Interactive Charts**: TradingView integration with:
  - Single chart view (one symbol at a time)
  - Multi-chart grid view (entire sector at once)
  - SPY overlay comparison
  - MA% Ribbon indicator

- **ETF Holdings**: View top 10 holdings for any ETF with weight percentages

- **Market Data Tools**:
  - Economic Calendar (TradingView widget)
  - Market Breadth overview

- **User Experience**:
  - Keyboard navigation (Arrow keys to browse, H for holdings)
  - Column sorting (click any header to sort)
  - Light theme UI
  - Auto-refresh daily via GitHub Actions

## Build data locally

```bash
cd market-dashboard
pip install -r requirements.txt
python scripts/build_data.py --out-dir data
```

This generates: `data/snapshot.json`, `data/events.json`, `data/meta.json`, and `data/charts/*.png`.

**Local preview (required):** Do not double-click `index.html` - the browser will block loading `data/*.json`. From the `market-dashboard` folder run a local server, then open the URL:

```bash
cd market-dashboard
python -m http.server 8000
```

Then in your browser open: **http://localhost:8000**

## Deploy to GitHub Pages

1. Push this directory to a GitHub repository.
2. **Before first deploy** you need initial data. Either:
   - **Recommended:** In the repo go to **Actions** > **Refresh dashboard data** > **Run workflow**. When it finishes, it will commit `data/` to the repo.
   - Or run locally: `python scripts/build_data.py --out-dir data`, then `git add data/`, commit and push.
3. Enable Pages: **Settings > Pages > Build and deployment > Source: Deploy from a branch**, branch `main`, folder `/` (root).
4. The workflow runs daily at 20:30 UTC (Mon-Fri) to refresh data; you can also run it manually from **Actions**.

Site URL: `https://<your-username>.github.io/<repo-name>/`

## Project structure

```
market-dashboard/
├── .github/workflows/refresh_data.yml  # Daily data refresh (GitHub Actions)
├── scripts/build_data.py               # Fetches data, outputs JSON + charts
├── data/                               # Generated (commit for Pages)
│   ├── snapshot.json
│   ├── events.json
│   ├── meta.json
│   └── charts/*.png
├── index.html                          # Static frontend
├── requirements.txt
└── README.md
```

Data: Yahoo Finance (yfinance), economic calendar and market breadth (TradingView widgets). Charts: TradingView widget.
