# Bitcoin Sentiment vs Hyperliquid Trader Behavior & Performance

This repo analyzes whether **Bitcoin market sentiment (Fear/Greed)** is associated with **Hyperliquid traders' performance and behavior**.

## Data
- `data/fear_greed_index.csv` — daily Fear/Greed classification
- `data/historical_data.csv` — Hyperliquid trades (account/coin/side/size/PnL/timestamps)

## Quickstart
```bash
pip install -r requirements.txt
jupyter notebook notebooks/analysis.ipynb
```

## Outputs
- `figures/` — saved charts (PNG)
- `outputs/` — saved tables (CSV)

## Key Questions Answered
1. Does performance (PnL, win rate, activity) differ between Fear vs Greed days?
2. Do traders change behavior (trade frequency, long/short bias, sizing) by sentiment?
3. Which trader segments stand out (frequent vs infrequent; consistent vs inconsistent)?

See **writeup.md** for a 1-page summary.
