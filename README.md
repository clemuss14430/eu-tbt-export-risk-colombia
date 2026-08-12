# eu-tbt-export-risk-colombia

**Latent risk and regulatory persistence: European environmental TTCs and Colombia's manufacturing export vulnerability, 2019–2026.**

> Data pipeline and analysis scripts for the working paper by Carlos Iván Lemus Serna  
> Universidad EAN · Semillero Inglomark · 2026

---

## Overview

This repository contains the full data pipeline used to construct the **Risk Persistence Index (RPI)**, a custom indicator that classifies 72 Colombian manufacturing export lines (HS Chapters 39–96) according to their simultaneous exposure to:

- **Historical regulatory pressure** — EU environmental Technical Barriers to Trade (TBT) notifications registered in the WTO I-TIP Goods database (2019–2023)
- **Prospective regulatory threat** — active EU TBT alerts published on the WTO ePing platform (2024–2026)

The pipeline integrates three international databases at the HS-4 digit level and produces a master file used for the Power BI dashboard and the quantitative analysis of the working paper.

---

## Files in this Repository

| File | Description |
|------|-------------|
| `01 Unification of Colombian exports to the EU` | Export data unification and filtering (ODEB, 2019–2025) |
| `02 Homologation and organization of ePing notifications` | ePing alert normalization — ICS → HS equivalence mapping |
| `03 Master file - Data unification` | Master file construction + RPI calculation |
| `README.md` | Project documentation |

---

## Data Sources

| Source | Description | Coverage |
|--------|-------------|----------|
| [WTO I-TIP Goods](https://i-tip.wto.org/goods) | Environmental TBT notifications by the EU | 2019–2023 · 88 notifications |
| [WTO ePing](https://epingalert.org) | Active regulatory alerts before entry into force | 2024–2026 · 75 relevant alerts |
| [ODEB](https://observatorio.desarrolloeconomico.gov.co) | Colombian exports to the EU by HS code | 2019–2025 · 6,129 records |

> **Note:** Raw data files are not included in this repository due to size and licensing constraints. All three sources are publicly accessible via the links above.

---

## Execution Order

Run the scripts sequentially: **01 → 02 → 03**. Each script produces the input required by the next.

### Script 01 — `Unification of Colombian exports to the EU`
Unifies annual ODEB export files into a single dataset. Filters HS Chapters 39–96 (manufactured goods and diversified consumer products) and retains only trade flows destined for the 27 EU member states.

**Output:** unified export base (2019–2025), filtered to EU destinations and HS 39–96.

---

### Script 02 — `Homologation and organization of ePing notifications`
Processes the 158 EU ePing notifications (2024–2026). Separates multiple product codes per row into individual records, prioritizes HS codes over ICS codes, and applies an ICS-to-HS equivalence dictionary for notifications without a direct tariff code. Removes unidentifiable entries.

**Output:** clean dataset of 109 records, no null values, homogeneous HS coding.

---

### Script 03 — `Master file - Data unification`
Integrates all three sources at the HS-4 digit level. Builds a bridge table that maps ePing chapter ranges to specific tariff headings with verified trade flows. Calculates the **Risk Persistence Index (RPI)** for each of the 72 export lines.

| RPI Category | Definition | Score |
|---|---|---|
| Extreme Risk | Present in both I-TIP and ePing | 4 |
| Chronic Risk | Present in I-TIP only | 3 |
| Emerging Risk | Present in ePing only | 2 |
| No Identified Risk | Absent from both sources | 1 |

**Output:** master file with 72 tariff headings, annual FOB values (2019–2025), regulatory pressure metrics, and RPI classification.

---

## Requirements

```
Python 3.12+
pandas
openpyxl
numpy
```

Install all dependencies:

```bash
pip install pandas openpyxl numpy
```

---

## Key Findings

- **81.9%** of the export portfolio (59 of 72 tariff headings) is classified as **Extreme Risk**
- Over **USD 99.2 million** in exports face simultaneous historical and prospective regulatory pressure
- **Chapter 85** (electrical and electronic equipment): highest vulnerability — 22 historical measures + 10 active prospective alerts
- **Chapter 87** (vehicles): exports grew **+562%** while accumulating new regulatory alerts
- Central paradox: despite high regulatory pressure, several product lines continue growing, suggesting Colombian exporters are not yet incorporating these regulatory signals into their decisions

---

## Citation

If you use this repository or any of its scripts in your research, please cite as:

> Lemus Serna, C. I. (2026). *eu-tbt-export-risk-colombia* [Source code repository]. GitHub. https://github.com/clemuss14430/eu-tbt-export-risk-colombia

---

## Author

**Carlos Iván Lemus Serna**  
International Business · Universidad EAN  
Semillero Inglomark
Bogotá, Colombia

---

## License

This repository is shared for academic and research replication purposes.  
Please contact the author before using the scripts or data in commercial applications.
