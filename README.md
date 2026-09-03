# AU60ETL93373

NAV price data for FT Capital Multi Class Investment Fund - US Large Cap Enhanced Complex Class.

## Identifiers

| Type | Value |
|------|-------|
| ISIN | AU60ETL93373 |
| FIGI | BBG022MMSXM3 |
| APIR | ETL9337AU |
| ARSN | 652 933 616 |
| LEI | 254900X0N2K4O4JH2510 |
| Bloomberg | FTUSLCE AU \<Equity> |
| Currency | AUD |

## Service Providers

| Role | Entity |
|------|--------|
| Responsible Entity | Equity Trustees Limited |
| Fund Manager | Future Trading Capital Pty Limited |
| Administrator & Custodian | Apex Fund Services Pty Ltd |
| Auditor | Ernst & Young |

Auditor sourced from the RG 240 annual report, year ended 30 June 2026 (see RG 240 Annual Report
below), not independently confirmed elsewhere in this repository.

## Data Format

NAV data is published in ISO 20022 `reda.001.001.05` (Price Report) format, checked
field-by-field against the published message definition.

**File:** `au60etl93373_iso20022_reda001_latest.json`

### Structure

```
Document
└── PricRpt (reda.001.001.05, PriceReportV05)
    ├── MsgId
    │   ├── Id (message identifier)
    │   └── CreDtTm (creation timestamp)
    ├── MsgPgntn, PricRptId, Fctn
    └── PricValtnDtls[]
        ├── NAVDtTm.Dt (valuation date)
        ├── FinInstrmDtls.Id[].ISIN
        ├── TtlNAV[] (total NAV amount + currency)
        ├── ValtnTp, OffclValtnInd, SspdInd
        ├── PricDtls[]
        │   ├── PricTp.Cd = NAVL — ValInInvstmtCcy[].Amt (NAV per unit)
        │   ├── PricTp.Cd = OFFR — ValInInvstmtCcy[].Amt (entry/offer price)
        │   └── PricTp.Cd = BIDE — ValInInvstmtCcy[].Amt (exit/bid price)
        │   (each entry also carries ForExctnInd, CumDvddInd, EstmtdPricInd)
        └── ValtnSttstcs[]
            ├── Ccy, PricTpChngBsis, PricChng (Amt, AmtSgn, Rate)
            ├── ByPrdfndTmPrds.HghstPricVal12Mnths / LwstPricVal12Mnths
            └── ByUsrDfndTmPrd[] (rolling returns, Prd.Dt.FrDt/ToDt + PricChng)
```

## MPI Report

Open futures positions as at valuation date, published in CSV format.

**File:** `au60etl93373_mpi_latest.csv`

| Column | Description |
|--------|-------------|
| date | Valuation date (YYYYMMDD) |
| bloomberg_ticker | Bloomberg ticker |
| side | BUY or SELL (net) |
| contracts | Net open contracts |
| exchange | Listing exchange |
| description | Contract description |
| contract_code | Globex symbol |

Field names above match `schemas/mpi.schema.json`. The file has no header row — columns are positional.

## Performance Chart

Percentage returns from inception, rebased to 0% on the capital deployment date. Benchmark is the E-mini S&P 500 futures front contract. `benchmark_symbol` per data point handles futures contract rolls automatically.

**File:** `au60etl93373_chart_performance_latest.json`

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

## NAV History

Full daily NAV history from inception. Not ISO 20022, no ISO 20022 message type
exists for a raw historical series. Recomputed in full on every publish, not
appended to, so the file always stays internally consistent.

**File:** `au60etl93373_nav_history_latest.json`

### Structure

```
{
  "meta": {
    "fund", "class", "class_code", "isin", "apir", "figi",
    "currency", "earliest_date", "latest_date", "count",
    "generated", "schema_version"
  },
  "data": {
    "YYYY-MM-DD": {
      "nav_unit_price",   // NAV per unit
      "entry_price",      // entry (offer) price
      "exit_price",       // exit (bid) price
      "total_nav",        // total NAV of the fund class in AUD
      "units_on_issue"
    }
  }
}
```

`figi` in `meta` is present only when the class has a FIGI on file. `count` matches
the number of keys in `data`. `data` has one entry for each valuation date from
`earliest_date` to `latest_date`.

## NAV Approval Audit

Redacted public record of the daily NAV approval workflow — import, approval, and publish steps (Apex NAV import, approval, BDUP/Lipper exports, Reda001 and performance data publish) for the most recently completed run. Not fund valuation data; a process/audit record.

**File:** `au60etl93373_nav_audit_public_latest.adoc`

Always reflects only the most recently completed run, not a historical log — it is overwritten on each run, not appended to. A `.sha256` sidecar (`au60etl93373_nav_audit_public_latest.adoc.sha256`) is published alongside it, generated by the same process that publishes the `.adoc` file — see the integrity note under Settlement Audit below, which applies equally here.

## Settlement Audit

Redacted public record of the daily settlement process — currency fixings, benchmark value, and MPI publish status for the most recently completed run. Not fund valuation data; a process/audit record.

**File:** `au60etl93373_audit_settlement_public_latest.adoc`

Always reflects only the most recently completed run, not a historical log — it is overwritten on each run, not appended to. A `.sha256` sidecar (`au60etl93373_audit_settlement_public_latest.adoc.sha256`) is published alongside it so a copy of the file can be checked for accidental corruption in transit; it is generated by the same process that publishes the `.adoc` file, so it is not independent verification against tampering by anyone with publish access to this repository.

## RG 240 Monthly Update

Monthly RG 240 disclosure update (ASIC Regulatory Guide 240, Benchmark 2). Not fund valuation data; a process/investor disclosure record. Covers net asset value, redemption value per unit, net return, risk profile, strategy, key investment decision-makers, and key service providers (shown only when changed since the last report).

**File:** `au60etl93373_rg240_monthly_latest.md`

Reflects only the most recently published month, overwritten on each publish, not a historical log. This file's own commit history on GitHub preserves each prior month's content. Format is Markdown, with a YAML frontmatter block (`schema`, `schema_version`, `document_id`, `reporting_date`, `generated_at`).

## RG 240 Annual Report

Annual RG 240 disclosure report (ASIC Regulatory Guide 240, Benchmark 2), one file for each real financial year end (30 June). Not fund valuation data; a process/investor disclosure record. Covers asset allocation, liquidity profile, liability maturity profile, gross leverage ratio, derivative counterparties, investment returns since inception, and key service providers.

**File:** `au60etl93373_rg240_annual_20260630.md` (current financial year end — a separate file exists for each year, never overwritten; check this repository's own file listing for the most recent date)

Format is Markdown, with a YAML frontmatter block (`schema`, `schema_version`, `document_id`, `reporting_date`, `generated_at`).

## Usage

Fetch the latest NAV data:

```
https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/au60etl93373_iso20022_reda001_latest.json
```

Fetch the latest MPI data:

```
https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/au60etl93373_mpi_latest.csv
```

Fetch the latest performance chart data:

```
https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/au60etl93373_chart_performance_latest.json
```

Fetch the latest NAV approval audit record (and verify it against its sha256):

```
https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/au60etl93373_nav_audit_public_latest.adoc
https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/au60etl93373_nav_audit_public_latest.adoc.sha256
```

Fetch the latest settlement audit record (and verify it against its sha256):

```
https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/au60etl93373_audit_settlement_public_latest.adoc
https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/au60etl93373_audit_settlement_public_latest.adoc.sha256
```

Fetch the latest RG 240 monthly update:

```
https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/au60etl93373_rg240_monthly_latest.md
```

Fetch the current RG 240 annual report:

```
https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/au60etl93373_rg240_annual_20260630.md
```

## Links

- Website: [www.ftcapital.com.au](https://www.ftcapital.com.au)
- PDS: [Product Disclosure Statement](https://edge.sitecorecloud.io/eqtservicesd49b-equity3a10-prod1271-d16d/media/equitytrustees/files/instofunds/future-trading-capital-pty-limited/ft-capital-multi-class-investment-fund-us-large-cap-enhanced-complex-class-pds.pdf)
- TMD: [Target Market Determination](https://swift.zeidlerlegalservices.com/tmds/ETL9337AU)
