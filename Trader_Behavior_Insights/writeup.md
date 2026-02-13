# 1‑Page Write‑Up: Methodology, Insights, Strategy Ideas

## Methodology (What I did)
1. **Loaded & validated datasets**  
   - Sentiment: 2,644 rows × 4 cols; 0 missing; 0 duplicates  
   - Trades: 211,224 rows × 17 cols; 0 missing; 0 duplicates
2. **Aligned to daily level**  
   - Converted sentiment `date` and trade `Timestamp IST` → `date` (daily).  
   - Left-joined sentiment onto trades by `date`.
3. **Built daily metrics**
   - Per **account-day**: `daily_pnl` (sum closed PnL), `win_rate`, `avg_trade_size_usd`, `trades_per_day`, `long_ratio`
   - Also built **market-level** daily aggregates for context.
4. **Segmentation**
   - **Frequent vs infrequent** (median split on `trades_per_day`)
   - **Consistency** per account using mean daily PnL, share of positive days, and volatility proxy (std of daily PnL)

## Evidence‑Backed Findings
Tables: `outputs/` • Charts: `figures/`

### 1) Performance differs by sentiment (Fear > Greed)
From `outputs/fear_vs_greed_summary.csv`:
- Avg daily PnL (per account‑day): **Fear 5328.82** vs **Greed 3318.10**
- Win rate (per account‑day): **Fear 0.364** vs **Greed 0.344**
- Trades/day (per account‑day): **Fear 98.15** vs **Greed 77.63**

Interpretation: Fear regimes appear more exploitable (higher volatility / stronger directional moves), leading to higher realized closed PnL.

### 2) Traders change behavior with sentiment (higher activity in Fear)
Fear days show **higher trades/day** (see `figures/trades_fear_vs_greed.png`).  
This indicates traders are more active during Fear regimes.

### 3) Segments matter: frequent traders earn much more
From `outputs/frequency_segment_summary.csv`:
- Frequent mean daily PnL: **7457.48**
- Infrequent mean daily PnL: **1273.54**

## Strategy Recommendations (Rules of Thumb)
1. **Fear‑Regime “Opportunity Mode”**
   - Allow **higher activity** (more setups) and prioritize volatility/continuation patterns.
   - Best suited for **frequent traders** (segment with much higher mean daily PnL).
2. **Greed‑Regime “Risk Compression”**
   - Reduce marginal trades; tighten risk (smaller sizing / stricter stops).
   - Prefer cleaner, higher‑conviction setups (avoid overtrading when edge appears weaker).

## Notes / Limitations
- The provided trade file does **not** include a leverage field; leverage-based analysis was not performed.
- PnL is based on **Closed PnL** only (open/unrealized not included).


## Bonus (Optional)

**Predictive model:** A simple multinomial Logistic Regression predicts **next-day profitability bucket (Low/Mid/High)** using sentiment + same-day behavior features (win rate, trades/day, avg trade size, long ratio, sentiment value).  
**Result:** Accuracy ≈ 0.392, Macro-F1 ≈ 0.346.  
Key drivers (by coefficient magnitude) are exported in `outputs/bonus_logreg_coefficients.csv`, and visualized in `figures/bonus_feature_importance.png`.

**Clustering:** KMeans (k=4) groups accounts into behavioral archetypes using aggregated behavior + PnL stats. Cluster summaries are exported to `outputs/bonus_cluster_summary.csv` with a supporting chart in `figures/bonus_cluster_avg_pnl.png`.
