# AU60ETL93373

NAV price data for FT Capital Multi Class Investment Fund - US Large Cap Enhanced Complex Class.

## Identifiers

| Type | Value |
|------|-------|
| ISIN | AU60ETL93373 |
| APIR | ETL9337AU |
| ARSN | 652 933 616 |

## Data Format

NAV data is published in ISO 20022 `reda.001` (Price Report) format.

**File:** `reda001.au60etl93373.json`

### Structure

```
PriceReport (reda.001)
├── msgId
│   ├── id (message identifier)
│   └── creDtTm (creation timestamp)
└── pricValtnDtls
    ├── navDtTm (valuation date)
    ├── finInstrmDtls.isin
    ├── ttlNAV (total NAV amount + currency)
    ├── pricDtls[]
    │   ├── NAVL (NAV per unit)
    │   ├── OFFR (entry/offer price)
    │   └── BIDE (exit/bid price)
    └── valtnSttstcs
        ├── hghstPricVal12Mnths
        ├── lwstPricVal12Mnths
        └── byUsrDfndTmPrd[] (rolling returns)
```

Fetch the latest NAV data:

```
https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/reda001.au60etl93373.json
```

## MPI Report

Open futures positions as at valuation date, published in CSV format.

**File:** `ftc_ftus_mpi_latest.csv`

| Column | Description |
|--------|-------------|
| date | Valuation date (YYYYMMDD) |
| bloomberg | Bloomberg ticker |
| side | BUY or SELL (net) |
| quantity | Net open contracts |
| venue | Listing exchange |
| product_name | Contract description |
| product_code | Globex symbol |

Fetch the latest MPI data:

```
https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/ftc_ftus_mpi_latest.csv
```

## Performance Chart

Percentage returns from inception, rebased to 0% on the capital deployment date. Benchmark is the E-mini S&P 500 futures front contract. `benchmark_symbol` per data point handles futures contract rolls automatically.

**File:** `chart_performance.au60etl93373.json`

### Structure

```
{
  "meta": {
    "fund", "class", "isin", "apir", "currency",
    "inception_date",
    "benchmark_name", "benchmark_code",
    "high_water_mark_nav", "high_water_mark_date",
    "latest_date",
    "latest_percent_nav", "latest_percent_benchmark", "latest_percent_alpha",
    "generated", "schema_version"
  },
  "data": {
    "YYYY-MM-DD": {
      "percent_nav",        // % return from inception
      "percent_benchmark",  // % return from inception
      "benchmark_symbol"    // futures symbol on that date (e.g. ESM6)
    }
  }
}
```

All `percent_*` values are percentage returns from `inception_date` (e.g. `5.45` = 5.45%). Alpha is `latest_percent_nav - latest_percent_benchmark`. The `meta` block is first for programmatic consumption — current return, HWM, and alpha are available without parsing `data`.

Fetch the latest performance chart data:

```
https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/chart_performance.au60etl93373.json
```

## Usage

Fetch the latest NAV data:

```
https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/reda001.au60etl93373.json
```

## Links

- Website: [www.ftcapital.com.au](https://www.ftcapital.com.au)
- PDS: [Product Disclosure Statement](https://edge.sitecorecloud.io/eqtservicesd49b-equity3a10-prod1271-d16d/media/equitytrustees/files/instofunds/future-trading-capital-pty-limited/ft-capital-multi-class-investment-fund-us-large-cap-enhanced-complex-class-pds.pdf)
- TMD: [Target Market Determination](https://swift.zeidlerlegalservices.com/tmds/ETL9337AU)
