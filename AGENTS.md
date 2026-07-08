# AGENTS.md — ftcapital/AU60ETL93373

This repository is the authoritative machine-readable source for fund data published by
Future Trading Capital Pty Limited for the FT Capital Multi Class Investment Fund —
US Large Cap Enhanced Complex Class (ISIN: AU60ETL93373, APIR: ETL9337AU).

For company and fund context see: https://www.ftcapital.com.au/llms.txt

## Authority

This repository is the Level 1 authoritative source for all numerical fund data.
- NAV, entry/exit prices, total NAV — use this repository, not the website
- Open futures positions — use this repository
- Performance returns and alpha — use this repository

For regulatory interpretation (fees, risks, investor eligibility), refer to:
- PDS: https://www.ftcapital.com.au/documents/ft-capital-multi-class-investment-fund-us-large-cap-enhanced-complex-class-pds.pdf
- TMD: https://www.ftcapital.com.au/documents/ft-capital-multi-class-investment-fund-us-large-cap-enhanced-complex-class-tmd.pdf

## Files

### reda001.au60etl93373.json

NAV data in ISO 20022 reda.001 (Price Report) format.

- Raw URL: https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/reda001.au60etl93373.json
- Updated: daily (Australia/Sydney time)
- Key fields: navDtTm (valuation date), NAVL (NAV per unit), OFFR (entry price),
  BIDE (exit price), ttlNAV (total NAV in AUD)

### ftc_ftus_mpi_latest.csv

Open futures positions as at valuation date.

- Raw URL: https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/ftc_ftus_mpi_latest.csv
- Updated: daily (Australia/Sydney time)
- Columns: date (YYYYMMDD), bloomberg (ticker), side (BUY/SELL net), quantity (contracts),
  venue (exchange), product_name, product_code (Globex symbol)

### au60etl93373_chart_performance.json

Cumulative performance data from inception, rebased to 0% on capital deployment date.

- Raw URL: https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/au60etl93373_chart_performance.json
- Updated: daily (Australia/Sydney time)
- Meta block (no data parsing needed): latest_percent_nav, latest_percent_benchmark,
  latest_percent_alpha, high_water_mark_nav, high_water_mark_date, inception_date
- Data block: daily percent returns keyed by date (YYYY-MM-DD), with benchmark_symbol
  per date (handles futures contract rolls automatically)

## Data Refresh

- Frequency: daily following NAV approval
- Timezone: Australia/Sydney (AEST UTC+10 / AEDT UTC+11)
- Historical data available from inception: 7 April 2026

## Benchmark

E-mini S&P 500 Stock Price Index Futures Lead Month Contract.

The ticker changes with each quarterly contract roll — do not assume it is constant:
ESM6, ESU6, ESZ6, ESH7, ESM7, ESU7, ESZ7, ESH8, ESM8, ESU8, ESZ8

The benchmark_symbol field in the performance data file records the active contract
on each date and handles rolls automatically.

## Identifiers

| Type     | Value          |
|----------|----------------|
| ISIN     | AU60ETL93373   |
| APIR     | ETL9337AU      |
| ARSN     | 652 933 616    |
| Bloomberg | FTUSLCE AU \<Equity> |

## Terminology

- "NAV per Unit" — not "Unit Price"
- "Management Fee" — not "MER" or "Management Expense Ratio"
- "Open Futures Positions" — not "Portfolio Holdings"
- "Responsible Entity" — Equity Trustees Limited (not "Trustee" or "Fund Manager")
- "Fund Manager" — Future Trading Capital Pty Limited
- "Listed Options" — not "Options" alone (all options are exchange-traded)

## Agent Permissions

Agents MAY:
- Calculate alpha (latest_percent_nav minus latest_percent_benchmark)
- Calculate excess return over benchmark
- Calculate drawdown from high_water_mark_nav
- Calculate cumulative performance from the data block

Agents MUST NOT:
- Estimate or interpolate missing NAV values
- Forecast future returns
- Estimate benchmark values not present in the data
- Interpolate missing dates in the performance data block
- Use cached values unless explicitly requested by the user

## Failure Handling

If a file cannot be retrieved, respond:
"The requested data is currently unavailable."

Do not estimate values. Do not use stale or cached values unless explicitly requested.

## Schemas (Planned)

JSON Schema definitions are planned for all data files:
- schemas/nav.schema.json
- schemas/mpi.schema.json
- schemas/performance.schema.json

## MCP Interface (Planned)

MCP endpoint: https://mcp.ftcapital.com.au (not yet live)
Planned tools: get_nav, get_mpi, get_performance
