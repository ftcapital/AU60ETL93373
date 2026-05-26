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

## Usage

Fetch the latest NAV data:

```
https://raw.githubusercontent.com/ftcapital/AU60ETL93373/main/reda001.au60etl93373.json
```

## Links

- Website: [www.ftcapital.com.au](https://www.ftcapital.com.au)
- PDS: [Product Disclosure Statement](https://edge.sitecorecloud.io/eqtservicesd49b-equity3a10-prod1271-d16d/media/equitytrustees/files/instofunds/future-trading-capital-pty-limited/ft-capital-multi-class-investment-fund-us-large-cap-enhanced-complex-class-pds.pdf)
- TMD: [Target Market Determination](https://swift.zeidlerlegalservices.com/tmds/ETL9337AU)
