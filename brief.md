
# BRIEF — Gold Majors Flow & Fundamentals Study (NEM / ABX / AEM)

## Objective

Determine where the first wave of money lands among the three gold majors if gold surges toward US$8,000/oz, by decomposing the 2022–2026 cycle into: (1) ETF/passive flow transmission, (2) active stock-selection money, and (3) the fundamental scorecard active money was pricing. Produce a two-wave playbook (flow-mechanical wave vs. proof-driven wave) with per-company rankings.

## Data available (verify before starting; stop and ask if anything is missing)

- **No market data exists in the workspace.** All prices (gold, NEM, ABX, AEM, GDX, GDXJ, S&P 500, CADUSD) are fetched via yfinance for 2022-10-02 → 2026-02-23, and GDX/GDXJ fund flows are web-fetched, per FETCH_PROMPT.md. Run the fetch session FIRST; this brief's Phase 0 then reads and validates what it produced (data/COVERAGE.md is the entry point).
- Financial statements on disk: all three companies 2024–2026Q1; AEM additionally from 2022.
- S&P 500 passive channel: NEM is an S&P 500 member at roughly 0.15% index weight (verify current weight and log it). Every dollar of passive S&P 500 inflow mechanically buys ~$0.0015 of NEM regardless of any gold view. AEM and ABX have no equivalent channel.
- Flow-series discipline: Studies 1 and 3 must state which flow series each regression uses. Miner-ETF flows (GDX/GDXJ — the equity-demand signal) and any bullion-ETF flows (gold-demand signal) are different variables and must never be pooled.
- To be added manually if not on disk: GDX portfolio weights (VanEck factsheet), float and ADV per stock.

## Ground rules

- Phase 0 (full file read) is a hard gate: no study starts, no chart is drawn, no number is quoted until Phase 0 is complete and logged.
- Every computed number goes into `_ledger.md` with source file, date range, and formula.
- Flag any figure derived from incomplete data as APPROX in all outputs.
- No point price targets. Outputs are rankings, lags, betas, capture ratios, and residual series.
- Every study ends with a one-sentence decision-relevant verdict, not description.
- Write the pre-registered predictions (Section H) to the ledger BEFORE running Studies 1–5, so results can falsify them.

## Phase 0 — MANDATORY file inventory and full read (do this before anything else)

1. List every file in the workspace recursively (`inputs/`, `data/`, `financials/`, and root). Write the complete inventory to `_ledger.md`: filename, type, date range covered, row count for CSVs, page/section count for statements.
2. READ every file fully before any analysis begins. No skimming, no sampling:
   - Every fetched price CSV (from the FETCH session): confirm ticker, currency, frequency, start/end dates, gaps, adjustment basis. Log all of it. Start from data/COVERAGE.md and verify its claims against the actual files.
   - The GDX/GDXJ flow series: confirm units, frequency, source (web-fetched vs. constructed rows), and sign convention. If units or coverage are ambiguous, STOP and ask before using them — a misread flow series invalidates Studies 1 and 3.
   - Every financial statement and MD&A: extract the quarterly figures needed for Study 4 (realized price, AISC, production, ounces sold, diluted shares, OCF, FCF, dividends, buybacks, net debt, guidance) into one extraction table per company, with a page/section citation for every number.
3. Build a data-coverage matrix: rows = studies 1–6, columns = required inputs, cells = filename or MISSING. Any MISSING cell: state it, propose the workaround or the fetch, and get confirmation before proceeding.
4. Nothing in the report may cite a number that does not trace to a file read in this phase or to an explicitly logged fetch/assumption. If a study section cannot be supported by the files on disk, the section says so instead of filling the gap from general knowledge.
5. Only after the inventory, the extraction tables, and the coverage matrix are written to the ledger does analysis begin.

## Study 1 — Flow transmission (ETF flows × prices)

1. Plot cumulative ETF flows 2022–2026. Mark every inflection: sustained outflow→inflow turns and acceleration episodes (define sustained = 4+ consecutive weeks same sign; log the definition).
2. For each inflection episode, per stock: return at +20/+60/+120 trading days; beta to gold inside inflow windows vs. outflow windows; days until prior high broken.
3. Regress weekly stock returns AND weekly miner/gold ratio changes on flows at lags 0–12 weeks. Report the significant lag structure per company.
4. Output: table of "return per $1B inflow" and "response lag in weeks" per stock.
5. Verdict sentence: which stock receives flow money first and at what multiplier.

## Study 2 — Plumbing map (mechanical first-wave ranking)

1. GDX weights for NEM, ABX, AEM (latest + history if available). A dollar into GDX buys in these proportions.
2. Index membership table: S&P 500 (NEM only — log its current index weight, ~0.15%), TSX 60, GDX/GDXJ, major ESG/sector indices if quickly checkable.
3. Float and ADV per stock; compute hypothetical price impact of a $1B sector inflow allocated by GDX weight ÷ ADV.
4. S&P 500 channel sizing: using ^GSPC-tracking fund flow estimates (or a stated assumption, logged), estimate the monthly passive dollar bid for NEM = S&P passive inflow × NEM weight. Compare its magnitude to NEM's share of GDX flows over the same months — which pipe is bigger for NEM, the gold pipe or the index pipe?
5. Verdict sentence: mechanical wave-1 ranking, independent of fundamentals.

## Study 3 — Passive vs. active decomposition (the core question)

Run all three methods; reconcile at the end.

- **Method A — direct ETF share count.** Quarterly snapshots 2022–2026: GDX shares held of each stock = fund shares outstanding × weight ÷ stock price. Add major passive index funds for NEM. Output: % of float held passively, per quarter, per stock. The delta series = mechanical flow.
- **Method B — 13F ownership deltas.** From IR ownership summaries or 13F aggregates: index vs. active long-only vs. hedge fund share counts by quarter. Identify divergence quarters (active adding while flows flat = pure stock selection). Caveat for ledger: 13F covers US institutions only; treat TSX-listed AEM/ABX splits as directional.
- **Method C — return residual decomposition (runnable from price CSVs alone; do this first).** Regress each stock's weekly returns on GDX returns over rolling 52-week windows. For NEM, run a second specification adding ^GSPC returns as a factor — NEM carries broad-market beta through its S&P membership that AEM/ABX largely do not, and omitting it mislabels index-driven moves as stock selection. Cumulate residuals 2022–2026 per stock. Overlay event dates: Mali seizure timeline (Nov 2024 detentions, Jan 2025 export block/suspension, Jun 2025 provisional administration), NEM Q3-2023 Newcrest close, NEM Feb-2024 dividend cut and guidance reset, AEM quarterly results dates, NEM 2025 divestiture completions.
- Reconciliation: do the three methods agree on when active money led and when passive amplified? State agreement/disagreement explicitly.
- Verdict sentence: what share of each stock's 2022–26 return was sector flow vs. stock selection.

## Study 4 — Fundamentals panel (the scorecard active money read)

Per company, per quarter, from the statements on disk:

1. **Margin capture:** realized gold price − AISC; and capture ratio = Δ(operating cash flow or EBITDA) ÷ (Δ realized price × ounces sold). AEM computable from 2022; NEM/ABX from 2024.
2. **Ounces per share:** production ÷ diluted shares, indexed to first available quarter.
3. **FCF per share** and allocation split: dividends / buybacks / debt paydown / growth capex.
4. **Net debt (cash)** trajectory.
5. **Guidance vs. delivery:** production and cost guidance against actuals for every quarter available.

Cross-check: correlate each stock's Method-C residual series against quarterly changes in these five metrics. Identify which metric best explains the residual (hypothesis: guidance delivery and FCF/share, not gold price).

## Study 5 — Earnings event study

Tag all earnings dates 2024–2026Q1 (AEM from 2022). Compute 1-day and 5-day abnormal returns vs. gold and vs. GDX around each. Test: do NEM's largest moves cluster on earnings dates (episodic proof-driven money) while AEM's returns accrue between them (continuous flow-driven bid)?

## Study 6 — Asset base and valuation (which of the three to pick)

Per company, from latest annual reserve/resource statements (fetch from filings on disk; if absent, flag and request upload):

1. **Reserve life:** P+P reserves ÷ current annual production, in years. Note the gold price assumption behind each company's reserve statement — they differ, and a reserve stated at $1,400 is not comparable to one at $1,700 without adjustment.
2. **Grade profile:** average reserve grade per major asset; flag assets where grade is declining year over year.
3. **Price-optionality ounces:** M&I resources NOT in reserves, per company. At $8,000/oz, cutoff grades fall and this material converts. Rank the three by (resources ex-reserves) ÷ reserves — the biggest ratio has the most free ounces from a price surge. Note known low-grade halos qualitatively (e.g., Detour for AEM, Nevada JV for NEM/ABX).
4. **EV/oz two ways:** EV ÷ P+P reserves, and EV ÷ (P+P + M&I). Compute EV as market cap + net debt − net cash from Study 4.4 quarterly panel, so the ratio is time-consistent. The spread between the two EV/oz figures per company = what the market charges for optionality; compare across the three.
5. **Copper adjustment:** for NEM and ABX, compute gold-only EV/oz by deducting an estimated copper-asset value (state assumption in ledger; sensitivity ±50%). Without this, ABX looks artificially cheap per gold ounce.
6. Verdict sentence: which company is cheapest per ounce of $8k-activated optionality, after copper and jurisdiction adjustment.

## Synthesis deliverable

One Word report + one summary matrix:

- **Wave 1 map (flow-mechanical, first weeks–months of an $8k surge):** ranking from Studies 1–2 with the measured lags and multipliers.
- **Wave 2 map (proof-driven, following quarters):** ranking from Studies 3–5 by capture ratio, earnings-surprise potential, and residual momentum.
- **Torque overlay:** note from the historical record that panic-price episodes eventually reward the highest-torque laggard (2016 precedent); state the ABX-if-Mali-resolves conditional explicitly.
- **Pick matrix:** one row per company, columns = wave-1 rank (Studies 1–2), wave-2 rank (Studies 3–5), reserve life, optionality ratio, copper-adjusted EV/oz (Study 6). The final pick argues from this matrix, not from any single metric.
- **CIO decision line** at the end: one sentence, positionable.

## H — Pre-registered predictions (write to ledger before running)

1. Active money (Method C residual) turned positive for AEM in 2023, before the 2024 flow inflection.
2. Passive flows 2024–25 amplified all three roughly by GDX weight, indiscriminately.
3. Final 2022–26 return divergence ≈ cumulative active residual, not flow share.
4. NEM's returns cluster on earnings dates; AEM's do not.
5. ABX's residual is persistently negative and event-driven (Mali dates), not flow-driven.

If any prediction fails, the failure is the finding — write it up, do not rationalize it.

## Formatting standards

Plain English, sentences under 30 words, no jargon ("re-rate", "the drill bit" etc. banned). Charts: Goldman-minimalist — no gridlines, light axis spines, matplotlib with escaped \$. Every chart title states the conclusion, not the contents.
