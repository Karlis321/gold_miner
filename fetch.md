
# FETCH_PROMPT — market data pull for the gold flow study

Copy-paste this into a Claude Code session in the study workspace. This session is fetch-and-validate ONLY — no analysis.

No price data exists in the workspace. Every market series below must be fetched fresh.

## Part 1 — Prices via yfinance

Fetch daily data for **2022-10-02 to 2026-02-23** and save each as CSV under `data/`:

| Ticker   | What it is                     | Output file           |
| -------- | ------------------------------ | --------------------- |
| GC=F     | Gold futures (gold spot proxy) | data/GOLD_daily.csv   |
| NEM      | Newmont (NYSE, USD)            | data/NEM_daily.csv    |
| ABX.TO   | Barrick (TSX, CAD)             | data/ABX_daily.csv    |
| AEM.TO   | Agnico Eagle (TSX, CAD)        | data/AEM_daily.csv    |
| GDX      | VanEck Gold Miners ETF         | data/GDX_daily.csv    |
| GDXJ     | VanEck Junior Gold Miners ETF  | data/GDXJ_daily.csv   |
| ^GSPC    | S&P 500 index                  | data/SP500_daily.csv  |
| CADUSD=X | CAD/USD rate                   | data/CADUSD_daily.csv |

Requirements:

1. Use `auto_adjust=False`; save Close, Adj Close, and Volume. Returns are computed on Adj Close; log this choice in `_ledger.md`.
2. After each download, print and log: first date, last date, row count, missing trading days vs. the exchange calendar, any zero/negative prices. If GC=F is gappy, fall back to `XAUUSD=X` and log the substitution.
3. Currency discipline: ABX.TO and AEM.TO are CAD; everything else USD. Use CADUSD_daily.csv to express all series in USD wherever cross-compared. Never mix currencies silently.
4. Listing check: decide and log the canonical Barrick listing for the study (ABX.TO in CAD vs. GOLD/B on NYSE in USD). Use one consistently.
5. Spot-check plausibility against known anchors and log the result: gold ~$1,650–1,700 in early Oct 2022; gold above $4,000 by mid-2026 is beyond this window's end — the Feb 23, 2026 close should be in the high-$3,000s/low-$4,000s region. If any series is wildly inconsistent with these anchors, stop and investigate before saving.

## Part 2 — GDX and GDXJ fund flows via web fetch

Miner-ETF creation/redemption flows are the equity-demand signal for Studies 1 and 3. Fetch them from the web (WebFetch tool), with a constructed fallback.

1. **Primary — fetch published flow data.** Try, in order, and log which source succeeded:
   - VanEck's fund pages for GDX and GDXJ (fund flows / historical data sections)
   - etf.com fund-flows pages for GDX and GDXJ
   - etfdb.com or similar aggregator flow tables
     Capture: daily or weekly net flows (US$), as far back toward 2022-10-02 as the source allows. Save raw captures to data/raw/ and cleaned series to data/GDX_flows.csv and data/GDXJ_flows.csv (columns: date, net_flow_usd, cum_flow_usd, source, fetch_date).
2. **Fallback — construct from shares outstanding.** If web sources cover only part of the window, fill the remainder: pull `yf.Ticker("GDX").get_shares_full(start="2022-10-02", end="2026-02-23")` (same for GDXJ); daily est. flow = Δ shares outstanding × Adj Close. Mark every constructed row with source = "constructed". Where web and constructed series overlap, correlate monthly aggregates; if correlation < 0.8, stop and report before merging.
3. **Sanity checks, all logged:** shares outstanding moves in creation-unit blocks (50k-share multiples); flag single-day changes >5% of shares outstanding; computed AUM (shares × price) at the last date must be within ~25% of the published fund AUM or stop and investigate.
4. If the workspace contains any pre-existing ETF flow file, do NOT assume it covers GDX/GDXJ — determine what funds it actually covers (it may be bullion ETFs such as GLD). Log the conclusion. Bullion-ETF flows (gold-demand signal) and miner-ETF flows (equity-demand signal) are different variables and must never be pooled.

## Part 3 — Close-out

1. Write every file created — row count, date range, source, and any gaps or substitutions — to `_ledger.md` under "FETCH 2026-07".
2. Produce data/COVERAGE.md: one table, rows = required series, columns = file, start, end, gaps, source. Any series not fully covered 2022-10-02 → 2026-02-23 is listed with the exact missing segment.
3. Stop. Analysis begins only in a subsequent session against the main BRIEF, and only after Phase 0 of that brief re-reads everything fetched here.
