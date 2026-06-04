# AU60ETL93373 — FTC Future Trading Capital Fund (FTUS)

ISIN: AU60ETL93373 | APIR: ETL9337AU | Bloomberg: FTUSLCE AU

This repository contains machine-readable fund data files for AU60ETL93373, published by Future Trading Capital Pty Limited. Files are updated following each business day NAV calculation and approval.

---

## Files

### `ftc_ftus_mpi_latest.csv`

Daily MPI (Market Price Information) file. Most recent settlement date. Format: ASX MPI CSV.

Updated: daily post-settlement.

---

### `reda001.au60etl93373.json`

ISO 20022 `reda.001` NAV price report. Contains unit prices (NAV, entry, exit), total NAV, and rolling return statistics.

**Schema:**

```json
{
  "pricValtnDtls": {
    "navDtTm": "YYYY-MM-DD",
    "finInstrmDtls": { "isin": "AU60ETL93373" },
    "ttlNAV": { "amt": 0.000000, "ccy": "AUD" },
    "pricDtls": [
      { "tp": "NAVL", "val": { "amt": 0.000000, "ccy": "AUD" } },
      { "tp": "OFFR", "val": { "amt": 0.000000, "ccy": "AUD" } },
      { "tp": "BIDE", "val": { "amt": 0.000000, "ccy": "AUD" } }
    ],
    "valtnSttstcs": {
      "byUsrDfndTmPrd": [
        {
          "prd": { "frDt": "YYYY-MM-DD", "toDt": "YYYY-MM-DD" },
          "pricChng": 0.000000
        }
      ]
    }
  }
}
```

`NAVL` = NAV per unit. `OFFR` = entry price. `BIDE` = exit price. `pricChng` = return over period (decimal, not percent).

Updated: daily post-NAV approval.

---

### `chart_performance.au60etl93373.json`

Performance chart data. Percentage returns from inception, rebased to 0% on the capital deployment date. Benchmark is the E-mini S&P 500 futures front contract (currently ESM6).

**Schema:**

```json
{
  "meta": {
    "fund": "FTC Future Trading Capital Fund",
    "class": "FTUS",
    "isin": "AU60ETL93373",
    "apir": "ETL9337AU",
    "currency": "AUD",
    "inception_date": "YYYY-MM-DD",
    "benchmark_name": "E-mini S&P 500",
    "benchmark_code": "ESM6",
    "high_water_mark_nav": 0.000000,
    "high_water_mark_date": "YYYY-MM-DD",
    "latest_date": "YYYY-MM-DD",
    "latest_percent_nav": 0.0000,
    "latest_percent_benchmark": 0.0000,
    "latest_percent_alpha": 0.0000,
    "generated": "YYYY-MM-DDTHH:MM:SSZ",
    "schema_version": "1"
  },
  "data": {
    "YYYY-MM-DD": {
      "percent_nav": 0.0000,
      "percent_benchmark": 0.0000,
      "benchmark_symbol": "ESM6"
    }
  }
}
```

`meta` is first for MCP/agent consumption — current return, HWM, and alpha are available without parsing `data`. All `percent_*` values are percentage returns from `inception_date` (e.g. `5.45` = 5.45%). Alpha = `percent_nav - percent_benchmark`. `benchmark_symbol` per data point handles futures contract rolls automatically. `schema_version` increments on breaking changes.

Updated: daily post-NAV approval.

---

## Contact

Future Trading Capital Pty Limited  
mboard@ftcapital.com.au
