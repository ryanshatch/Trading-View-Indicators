# NFTs vs SOL — Momentum Divergence Radar

### See when NFT activity (proxy volumes) **leads** or **lags** SOL momentum

## Bio

A TradingView indicator that normalizes **SOL momentum** and a **composite NFT proxy volume** into comparable z-scores, then plots their **difference** (divergence). You get zero-line cross signals (“lead/lag”) and a rolling correlation table to judge regime quality.

---

## What it does (concept)

* **SOL side:** Rate of Change of SOL (`ROC(len_mom)`) → standardized over `len_z` → smoothed (`smooth`) → **`sol_z`**.
* **NFT side:** Weighted sum of proxy **volumes** (BLUR, LOOKS, TNSR, MAGIC, APE, optional ME & PENGU) → `log()` to tame skew → standardized over `len_z` → smoothed → **`v_z`**.
* **Divergence:** `div = v_z - sol_z`.

  * `div > 0` → NFT activity is **stronger** than SOL momentum (possible *lead*).
  * `div < 0` → NFT activity **weaker** than SOL momentum (possible *lag*).

---

## On-chart elements

* **Orange histogram:** `div` (NFT volume Z − SOL momentum Z).
* **Purple line:** `v_z` (NFT composite activity).
* **Blue line:** `sol_z` (SOL momentum).
* **Green/Red triangles:** Zero-line crosses of `div` → **lead** (up) / **lag** (down).
* **Top-right table:** Rolling **Pearson correlation** (`len_z`) + list of **active** proxies (non-blank & weight ≠ 0).

---

## How to use (quick start)

1. **Add to chart** on your timeframe (30m–1D are common).
2. **Fill proxy symbols** with exchange prefixes (e.g., `KUCOIN:BLURUSDT`, `GATEIO:LOOKS_USDT`, `MEXC:TNSR_USDT`, `OKX:MAGIC-USDT`). Leave ME/PENGU blank until you have the exact tickers.
3. **Set weights** to reflect relevance/liquidity (start at 1; set 0 to exclude).
4. **Trade the crosses** with context:

   * **Lead (↑0):** NFTs heating up vs SOL → look for long setups if price structure agrees.
   * **Lag (↓0):** NFTs cooling vs SOL → reduce risk/hedge or look for shorts in downtrends.
5. **Consult correlation:**

   * **High (+0.5~1):** Both move together; rely more on **cross timing** than magnitude.
   * **Low/negative:** Decoupling regime; **divergence magnitude** gains importance.

---

## Settings (what they mean)

* **SOL Symbol (`sol_tkr`)** – Default `BINANCE:SOLUSDT`.
* **Proxy symbols (`*_sym`)** – Enter **exchange:pair** strings; blank = ignored.
* **Weights (`w_*`)** – Linear weights before log; emphasize reliable/liquid proxies.
* **Momentum Length (`len_mom`, 14)** – ROC lookback for SOL. Bigger = smoother/slower.
* **Z-Score Length (`len_z`, 50)** – Window for mean/stdev and correlation. Bigger = more stable normalization.
* **Smoothing (`smooth`, 5)** – SMA applied to both z-scores to cut noise.

**Preset ideas**

* **Swing (1D/4H):** `len_mom=14`, `len_z=100`, `smooth=7`; BLUR=1, LOOKS=1, TNSR=1, MAGIC=0.5, APE=0.5.
* **Active (2H/1H):** `len_mom=10`, `len_z=50`, `smooth=5`; same weights; alert on **lead**.

---

## Reading the signals (playbook)

* **Fresh Lead (div crosses ↑0):** Accumulation/participation rising; enter on pullbacks or structure breaks; confirm with volume/HTF trend.
* **Persistent Positive div:** Momentum follow-through likely; trail stops below swing structure.
* **Fresh Lag (div crosses ↓0):** Early cooling; take profits, tighten stops, or rotate.
* **Extreme bars (|div| > ~2):** Outlier conditions; expect mean-reversion or breakout continuation—use price action to decide.
* **Re-cross whipsaws:** Filter with higher `smooth` or require bar close confirmation.

---

## Practical notes & tips

* **Symbol validity:** Always use **exchange prefixes**. If a token is unavailable on your TV plan/exchange, leave it **blank** (script treats it as zero volume).
* **Liquidity bias:** Thin alts can distort the composite; set their weights low or 0.
* **Timeframe consistency:** All `request.security()` calls run at the chart TF—no lower-TF aggregation.
* **Risk management:** Treat crosses as **context**, not standalone buy/sell. Combine with S/R, trend filters, ATR stops, and volume profile.

---

## Alerts (recommended)

* **Lead Cross Up:** `div` crosses above 0.
* **Lag Cross Down:** `div` crosses below 0.
* **Extreme Divergence:** `div` > +2 or `div` < −2 (user threshold).
  *(Add `alertcondition()` lines if you want hard alerts.)*

---

## Troubleshooting

* **“Invalid symbol” popup:** The input contains an unsupported/typo ticker. Use exact **`EXCHANGE:SYMBOL`** (e.g., `OKX:MAGIC-USDT`, not `MAGICUSDT`). Leave fields blank if unsure.
* **Flat/NaN early bars:** Not enough history for `len_z`—normal; resolves as data accumulates.
* **No proxies listed in table:** Ensure at least one proxy is **non-blank** and weight ≠ 0.

---

## Limitations

* Proxy selection matters; if the set doesn’t reflect current NFT flow, signals degrade.
* Z-scoring assumes local stationarity; regime shifts compress/expand readings.
* Exchange symbol formats vary (`-` vs `_`); match TradingView exactly.

---

## Changelog (from header)

* **v1.2 (2025-10-14):** Stability pass, proxy inputs default to blank to avoid symbol errors; refined table; kept v5 compatibility logic.

---

