<p align="center">
  <img src="assets/header-bt.png" alt="Brazil Trading with the World, 2025" width="100%">
</p>

<p align="center">
  <img alt="Power BI" src="https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black">
  <img alt="Python" src="https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white">
  <img alt="pandas" src="https://img.shields.io/badge/pandas-150458?style=flat&logo=pandas&logoColor=white">
  <img alt="License" src="https://img.shields.io/badge/license-MIT-lightgrey">
</p>

# Brazil Trading with the World, 2025

A star-schema dashboard on Brazil's 2025 export/import data (Jan-Apr, Comex Stat), built from raw government CSVs through a Python cleaning pipeline into a Power BI model. Answers 5 trade questions (balance, top partners, top products, state performance, market share) in under 30 seconds and 1-2 clicks.

<p align="center">
  <img src="assets/demo.gif" alt="Dashboard demo" width="720">
</p>

## Pipeline

Comex Stat ships raw export/import transactions and 4 separate dimension lookups, in Latin-1, semicolon-delimited, no shared key naming. `scripts/data_cleaning.py` turns that into a single analysis-ready star schema:

```mermaid
flowchart LR
    A["EXP_2025.csv\nIMP_2025.csv"] --> B["aggregate by\nNCM x state x country"]
    B --> C["rename FOB columns\nper trade direction"]
    C --> D["outer join\nexports + imports"]
    D --> E["fillna(0)\nstandardize country names"]
    E --> F[("f_trading.csv")]
    G["PAIS / UF / NCM /\nNCM_SH lookups"] --> H["select + translate\nPT/EN descriptions"]
    H --> I[("d_country, d_state,\nd_ncm, d_sh")]
    F --> J["Power BI\nstar schema"]
    I --> J
```

- Aggregates raw transactions to (NCM code x state x country) before joining, so the fact table doesn't carry line-item noise into the model
- Outer-joins exports and imports on the same grain instead of stacking them, so trade balance is a single subtraction, not a cross-table calculation
- Fixes a real data quality issue at the source ("Países Baixos (Holanda)" vs "Holanda" inconsistency) rather than patching it downstream in DAX
- Re-encodes to UTF-8-sig with full quoting, the raw government exports aren't safe to load as-is
- Output: one 17MB fact table + 4 dimension tables, `data/final tables/`

## Data model

```mermaid
erDiagram
    f_trading }o--|| d_country : country_code
    f_trading }o--|| d_state : state_code
    f_trading }o--|| d_ncm : ncm_code
    d_ncm }o--|| d_sh : sh_code

    f_trading {
        string country_code
        string state_code
        string ncm_code
        float vl_fob_expo
        float vl_fob_impo
    }
```

Grain is SH2 (2-digit Harmonized System), not the full 8-digit NCM. NCM has ~13,000 codes, unreadable in any chart; SH2 collapses to ~99 product groups while keeping the drill path to NCM available underneath for anyone who needs it.

## Design decisions

| Decision | Choice | Why |
|---|---|---|
| Layout | Single page over multi-page | Every question answerable without a page switch; kept it to a 16:9 executive-presentation frame |
| Product grain | SH2 over 8-digit NCM | ~99 readable categories vs ~13,000 unchartable codes |
| Labels | Claude MCP-assisted rename | 99 category labels, PT + EN, by hand would've been the slowest part of the build |
| Theme | Dark neutral over flag-colored | Flag colors as a palette fight the data itself for attention |

## Repo structure

```
brazil-trading-with-the-world-2025/
├── brazil-trading.pbix
├── data/final tables/        6 CSVs: 1 fact, 4 dimensions
├── design/                   Figma-exported theme + background
├── scripts/
│   └── data_cleaning.py      raw Comex Stat -> star schema
└── assets/                   header, demo GIFs, data model GIF
```

## Run it yourself

```bash
git clone https://github.com/devletstahl/brazil-trading-with-the-world-2025.git
cd brazil-trading-with-the-world-2025
python scripts/data_cleaning.py   # rebuilds data/final tables/*.csv from raw Comex Stat exports
```

Then open `brazil-trading.pbix` in Power BI Desktop and point it at `data/final tables/`.

## Results

- Time to insight: ~20s (target was 30s)
- Interactions to answer: 1-2 clicks
- Load time: under 3s
- Known limits: Jan-Apr 2025 only (partial year), FOB values only (freight/insurance excluded), map visual license blocks web publishing

## License

[MIT](LICENSE)
