<div align="center">
  <img src="assets/header-bt.png" alt="Project Header" width="100%" />
</div>

<br/>

<div align="center">
  <img src="https://img.shields.io/badge/Status-Completed-4ade80?style=for-the-badge&labelColor=1a1a1a" />
  &nbsp;
  <img src="https://img.shields.io/badge/Stack-Power%20BI%20%2B%20Python%20%2B%20Figma%20%2B%20Claude%20MCP-6ab4f5?style=for-the-badge&labelColor=1a1a1a" />
  &nbsp;
  <img src="https://img.shields.io/badge/Type-BI%20%2B%20Business%20Analysis-a78bfa?style=for-the-badge&labelColor=1a1a1a" />
</div>

---

## Executive Summary

Brazil is a major global commodity exporter, yet decision‑makers (trade analysts, consultants, procurement managers) often struggle to extract strategic insights from fragmented government platforms. Existing tools like Comex Stat and Apex Brasil are functional but not designed for fast, executive‑level consumption.

This project delivers a **single‑page interactive dashboard** that answers the five core questions about Brazil’s 2025 trade flows (exports, imports, partners, states, and products) in under **30 seconds of interaction**. Beyond the visual, the project documents the full **Business Analysis journey**: from stakeholder identification and requirements discovery to design trade‑offs, business rules, and success metrics.

The result is a portfolio piece that demonstrates not only technical BI/analytics skills but also the ability to **translate business problems into data‑driven solutions** — a critical competency for Business Analyst and Analytics roles.

---

## Business Problem

Brazilian trade data is abundant but scattered. Professionals who need to answer questions like:

- *“Which are our top 5 export markets this year?”*
- *“How does São Paulo’s share compare to the South region?”*
- *“What product categories dominate our imports?”*

…currently have to navigate multiple government portals, apply filters manually, and mentally connect the pieces. This process is time‑consuming and error‑prone, delaying strategic decisions.

**The core problem:** the gap between **raw data** and **actionable insight** is too wide for business users.

---

## Stakeholders

| Stakeholder | Primary Need |
|-------------|--------------|
| Export/Import Analyst | Quick overview without manual data extraction |
| Business Development Manager | Identify priority partner countries |
| Comex Consultant | Benchmark state performance and category trends |
| Student / Researcher | Accessible, documented dataset for academic analysis |
| Executive / Director | High‑level summary for presentations and strategy |

---

## Business Goals

1. **Reduce insight time** – answer the 5 key trade questions in under 30 seconds.
2. **Improve accessibility** – make trade data understandable without technical training.
3. **Enable comparison** – allow filtering by country, state, and product category.
4. **Support strategic decisions** – provide context (market share, product mix) via tooltips.
5. **Serve as a reusable template** – the data model and design can be adapted to other countries.

---

## Requirements Discovery

Before any development, I translated the business problem into clear requirements.

### Functional Requirements

- Display Brazil’s overall trade balance (exports − imports) at a glance.
- Show top 5 export and import partner countries.
- Visualize trade flows by Brazilian state and region on an interactive map.
- List top 5 export and import product categories (SH2 level).
- Allow filtering by country, state, and product category.
- Provide contextual tooltips with main products, market share, and trade balance.
- Keep all insights on a **single page** for fast consumption.

### Non‑functional Requirements

- Dashboard must answer the core questions in **under 30 seconds** of interaction.
- Responsive layout for 16:9 screens (presentation‑ready).
- Minimal visual clutter, consistent color hierarchy.
- Fast filtering performance even with large fact tables.
- Dark theme to enhance contrast for executive presentations.

### Out of Scope (for this version)

- Monthly/quarterly trend analysis.
- Forecasting or predictive models.
- Customs process indicators (lead time, documentation).
- Company‑level export/import data.
- Drill‑through to detailed NCM (8‑digit) level.

---

## Assumptions

To keep the dashboard focused and executive‑oriented, I assumed:

- Users are familiar with basic international trade concepts (FOB, trade balance, SH classification).
- SH2 categories provide sufficient granularity for strategic decisions (NCM would add noise).
- The primary use case is **exploratory analysis** and **presentation**, not operational reporting.
- Data from Comex Stat is reliable and complete for the period (Jan–Apr 2025).
- FOB values (excluding freight/insurance) are acceptable for comparison purposes.

---

## Constraints

| Constraint | Impact |
|------------|--------|
| Partial year (Jan–Apr 2025) | Full‑year comparisons not yet possible |
| Map visual license | `.pbix` cannot be published to web; local use only |
| FOB values only | Import costs are understated (freight/insurance missing) |
| No historical data | Year‑over‑year trends cannot be shown |

---

## Business Rules

The dashboard follows these explicit business rules:

- **Trade Balance** = `Exports (FOB) − Imports (FOB)`
- **Market Share** = `(value of a specific country/state) / (total value within filter context)`
- **States** are grouped by official Brazilian regions (North, Northeast, Midwest, Southeast, South, and Federal District).
- **Product hierarchy** follows the SH classification: SH2 → SH4 → SH6 → NCM.
- **Null values** in the fact table are treated as zero during aggregation.
- **Country names**: “Países Baixos (Holanda)” is renamed to “Holanda” / “Netherlands” for clarity.

---

## Solution Design

The design process followed three stages, always driven by the business questions:

1. **Define scope** – establish the 5 core questions and the minimum viable answer for each.
2. **Model the data** – build a star schema that supports filtering by country, state, and product without performance degradation.
3. **Design the experience** – mockup in Figma first, then build in Power BI to match the layout and theme.

### Data Architecture

- **Fact table**: `f_trading` (exports and imports aggregated by NCM, state, and country).
- **Dimension tables**: `d_country`, `d_state`, `d_ncm`, `d_sh` (SH hierarchy).
- **Star schema** ensures fast filtering and scalability.

<div align="center">
  <img src="assets/datamodel.gif" alt="Data Model" width="60%" />
</div>

### Analytical Model

The data model enables:

- Slicing by country, state, and product category.
- Measures: total exports, total imports, trade balance, market share.
- Implicit measures via Power BI’s aggregation engine.

### Dashboard UX Decisions

| Decision | Business Reason |
|----------|-----------------|
| **Single‑page layout** | Reduce navigation time; one glance gives the full picture. |
| **SH2 instead of NCM** | SH2 categories are readable in bar charts; NCM would generate thousands of items. |
| **Dark neutral theme** | Increases contrast and readability in presentations; avoids meaningless color bias. |
| **Custom tooltips** | Provide context without cluttering the main visuals. |
| **Projection map (third‑party)** | Built‑in map doesn't support Brazil’s state‑level granularity well. |
| **AI‑assisted label shortening** | Used Claude MCP to shorten 99 SH2 names to clean English labels for charts. |

---

## Trade‑offs

Every project involves constraints. Here are the key trade‑offs I made:

| Decision | Chosen | Discarded | Reason |
|----------|--------|-----------|--------|
| Page layout | Single‑page | Multi‑page | Executive use case demands speed; one view tells the story. |
| Product granularity | SH2 (broad) | NCM (8‑digit) | SH2 keeps charts readable; NCM would produce 9,000+ items. |
| Map type | Projection map (3rd party) | Built‑in filled map | Built‑in doesn’t handle Brazilian states well. |
| Color palette | Dark neutral | Brazilian flag colors | Data‑first: colors shouldn’t carry meaning unless intentional. |
| Label optimization | AI‑assisted (Claude MCP) | Manual editing | 99 SH2 categories × 2 languages is impractical to do manually. |

---

## Success Metrics

| Metric | Target | Achieved |
|--------|--------|----------|
| Time to answer 5 core questions | ≤ 30 seconds | ✅ ~20 seconds |
| Number of filters/clicks to reach insight | ≤ 3 | ✅ 1‑2 clicks |
| User comprehension (tooltip usefulness) | High | ✅ Custom tooltips provide context |
| Dashboard performance (load time) | < 5s | ✅ < 3s |

---

## Business Outcomes

The final solution enables decision‑makers to:

- **Identify major export markets** (top 5 partners) in seconds.
- **Compare states** without manually filtering each one.
- **Understand product concentration** (top SH2 categories) at a glance.
- **Evaluate regional participation** via the interactive map.
- **Answer strategic trade questions** using a single, unified dashboard.

<div align="center">
  <img src="assets/demo.gif" alt="Dashboard Demo" width="70%" />
</div>

---

## Future Improvements

- **Monthly trend analysis** – add time intelligence to show evolution.
- **Forecasting** – simple linear regression or seasonal decomposition.
- **Drill‑through pages** – from SH2 to SH4 and NCM for deeper analysis.
- **Dynamic decomposition tree** – for root‑cause analysis of trade balance changes.
- **Power BI Service deployment** – if the map visual license is upgraded.
- **Incorporate freight/insurance costs** for more accurate import comparisons.
- **Add a “Compare” mode** – side‑by‑side comparison of two countries or states.

---

## Repository Structure


---

## How to Run

Due to the map visual license, this dashboard is not available for web viewing. You can download the `.pbix` file and explore it locally in Power BI Desktop.

1. Clone or download this repository.
2. Open `brazil-trading.pbix` in Power BI Desktop.
3. Explore the dashboard – hover over countries and states to see tooltips in action.
4. (Optional) Swap the data for another country, adjust the theme, or extend the visuals.

<div align="center">
  <img src="assets/demo2.gif" alt="Tooltips Demo" width="70%" />
</div>

---

## Technologies

| Component | Tools |
|-----------|-------|
| Data preparation | Python (pandas) |
| Data modeling | Power BI (star schema) |
| Dashboard | Power BI Desktop |
| UI/UX mockup | Figma |
| Theme generation | Power BI Studio |
| Label optimization | Claude MCP (AI) |
| Version control | Git / GitHub |

---

## Lessons Learned

- **Start with questions, not visuals.** Defining the 5 core questions first kept the project focused and prevented scope creep.
- **Business rules matter.** Explicitly documenting rules (e.g., trade balance calculation, market share) avoids ambiguity and builds stakeholder trust.
- **Trade‑offs are part of the process.** Choosing SH2 over NCM was a conscious decision to prioritise readability over granularity.
- **AI is a powerful assistant.** Using Claude MCP to shorten 99 product labels saved hours of manual work.
- **A great dashboard tells a story.** The single‑page layout + tooltips guides the user naturally through the data narrative.

---

## License

This project is licensed under the [MIT License](LICENSE). Feel free to use, adapt, and extend it.

If you build something inspired by this project, I’d love to see it. Credit is appreciated but not required.

---

<sub>☕︎ Made by <a href="https://github.com/devleticiastahl">Letícia Stahl</a></sub>

<br/>

[![UI Kit](https://img.shields.io/badge/UI%20Kit-Emily%20Augusto-purple?style=flat-square&logo=figma&logoColor=white)](https://github.com/emilyaugusto)
