<div align="center">
  <img src="assets/header-bt.png" alt="Project Header" width="100%" />
</div>
<br/>
<div align="center">
  <img src="https://img.shields.io/badge/Status-Completed-4ade80?style=for-the-badge&labelColor=1a1a1a" />
  &nbsp;
  <img src="https://img.shields.io/badge/Stack-Power%20BI%20%2B%20Python%20%2B%20Figma-6ab4f5?style=for-the-badge&labelColor=1a1a1a" />
</div>

---

When you work with data every day, it's easy to get so focused on work projects that your personal portfolio ends up empty. That's exactly what happened to me. I had built around 5 to 10 dashboards at work this year, but not a single one for myself.

So I decided to change that. I wanted something simple enough to not take forever to build, but still complete, informative, and built with more than one tool.

I also have a background in international business. I used to constantly look up import/export data, consult [Apex Brasil](https://apexbrasil.com.br) dashboards, and build presentations around trade flows. It's a topic I genuinely like. So when I came across a clean, minimalist infographic on Pinterest showing trade data for a country, the idea clicked: let's build a Brazilian version of that in Power BI.

<div align="left">
  <img src="assets/inspo.jpg" alt="Inspiration" width="50%" />
</div>

<br/>

## ✦ Features

- Overview of exports and imports by FOB value (USD)
- Top 5 export and import markets
- Interactive map by state and region
- Top 5 exported and imported products
- Dark theme with a cohesive color palette and clean typography

<div align="left">
  <img src="assets/demo.gif" alt="Demo" width="50%" />
</div>

<br/>

## ✦ Getting the Data

The Brazilian government offers a very user-friendly platform with detailed import and export data: [Comex Stat](http://comexstat.mdic.gov.br/).

The datasets used in this project:

| File | Description |
|------|-------------|
| `EXP_2025.csv` | Export transactions in 2025 |
| `IMP_2025.csv` | Import transactions in 2025 |
| `PAIS.csv` | Country dimension table |
| `UF.csv` | Brazilian states dimension table |
| `NCM.csv` | Product classification (NCM) |
| `NCM_SH.csv` | SH product hierarchy |

<br/>

## ✦ Transforming the Data

Data cleaning and preparation were done in Python with pandas. Here's what was applied:

**Fact table — `f_trading.csv`**
- Exports and imports were aggregated by `CO_NCM`, `SG_UF_NCM` and `CO_PAIS`
- FOB values were summed per group
- Both datasets were merged with an outer join, filling missing values with `0`

**Dimension tables**

| Output file | Columns kept | Notes |
|-------------|-------------|-------|
| `d_country.csv` | `CO_PAIS`, `NO_PAIS`, `NO_PAIS_ING` | "Países Baixos (Holanda)" renamed to "Holanda" / "Netherlands" |
| `d_state.csv` | `SG_UF`, `NO_UF`, `NO_REGIAO` | — |
| `d_ncm.csv` | `CO_NCM`, `CO_SH6`, `NO_NCM_POR`, `NO_NCM_ING` | Exported with full quoting |
| `d_sh.csv` | `CO_SH6`, `NO_SH6_POR`, `NO_SH6_ING`, `CO_SH4`, `NO_SH4_POR`, `NO_SH4_ING`, `CO_SH2`, `NO_SH2_POR`, `NO_SH2_ING` | — |

<div align="left">
  <img src="assets/datamodel.gif" alt="Data Model" width="50%" />
</div>

<br/>

## ✦ Deciding the Theme

Before building anything, I explored color patterns using [Power BI Studio](https://www.powerbistudio.com/), a theme generator that makes it easy to experiment with palettes and export a `.json` file directly to Power BI. I considered blue, green, and yellow, classic Brazilian palette vibes, but ended up going for a dark, neutral palette instead. No need to force Brazilian colors when the data already tells the story.

You can find the theme file at `design/Custom Theme.json` and apply it under **View > Themes > Browse for themes**.

<br/>

## ✦ Creating the Mockup in Figma

Figma was used only to build the background and plan the layout, deciding where each visual would live, following the reference infographic as a starting point. From there, I adapted the layout to fit the theme and the available data.

The original background file is available at `design/background.png`.

<br/>

## ✦ Building the Visuals in Power BI

Visuals were designed following the reference infographic, using built-in Power BI visuals styled with the custom theme.

**Map visual used:**
> **Maps** by Alexander Koch — v1.0.6.7
> [AppSource](https://appsource.microsoft.com) · Support: [relevantvisuals.com](https://www.relevantvisuals.com)

I used a Projection Map for Brazil, with states colored by region: North, Northeast, Midwest, Southeast, South, and Federal District.

<br/>

## ✦ Optimizing Labels with Claude MCP

The SH2 product names were too long to display cleanly in bar and column charts. To fix this, I used the **Claude Modeling MCP** to generate short English versions of each SH2 category, making the visuals easier to read without losing meaning.

<br/>

## ✦ Adding Context with Tooltips

This is where the dashboard goes beyond the original infographic. I added 5 custom tooltips to give more context when hovering over countries and states, showing:

- Main products traded
- Market share
- Trade balance

<div align="left">
  <img src="assets/demo2.gif" alt="Tooltips" width="50%" />
</div>

<br/>

## ✦ Repository Structure

```
brazil-trading-with-the-world-2025/
│
├── assets/                  # Images and GIFs used in this README
├── data/
│   ├── raw/                 # Original files from Comex Stat
│   └── processed/           # Cleaned output files (f_trading, d_country, etc.)
├── design/
│   ├── background.png       # Figma background used in the dashboard
│   └── Custom Theme.json    # Power BI theme file
├── data_cleaning.py         # Python script for data preparation
├── brazil-trading.pbix      # Power BI file
└── README.md
```
<br/>

## ✦ How to Use

Due to the map visual license, this dashboard is not available for web viewing. You can download the `.pbix` file and explore it locally in Power BI Desktop.

1. Clone or download this repository
2. Open `brazil-trading.pbix` in Power BI Desktop
3. Explore the dashboard: hover over countries and states to see the tooltips in action
4. Make it yours: swap the data for another country, adjust the color theme, or extend the visuals however you like

<br/>

## ✦ License

This project is licensed under the [MIT License](LICENSE). Feel free to use it, adapt it, and make it your own.

If you build something inspired by this project, I'd love to see it. Credit is appreciated but not required.


---

<sub>☕︎ Made by <a href="https://github.com/devleticiastahl">Leticia Stahl</a></sub><br><br>
[![UI Kit](https://img.shields.io/badge/UI%20Kit-Emily%20Augusto-purple?style=flat-square&logo=figma&logoColor=white)](https://github.com/emilyaugusto)
