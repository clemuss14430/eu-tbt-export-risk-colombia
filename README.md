# eu-tbt-export-risk-colombia

**Latent risk and regulatory persistence: European environmental TBTs and Colombia's manufacturing export vulnerability, 2019–2026**

> Data pipeline and analysis scripts for the working paper by Carlos Iván Lemus Serna  
> Universidad EAN · Semillero Inglomark · 2026

[![Dashboard](https://img.shields.io/badge/Power%20BI-Live%20Dashboard-F2C811?style=flat&logo=powerbi&logoColor=black)](https://app.powerbi.com/view?r=eyJrIjoiNDkzZGVlYjMtZjZiNi00ODFmLWE4MDEtMzRhMmRlODRlMTk3IiwidCI6ImMwNmZiNTU5LTFiNjgtNGI4NC1hMTRmLTQ3ZDBkODM3YTVhYiIsImMiOjR9&pageName=3f6bdce00eefc57e3076)
[![License](https://img.shields.io/badge/License-Academic%20Use-blue?style=flat)](README.md#license)
[![Python](https://img.shields.io/badge/Python-3.12+-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)

---

## What this repository contains

This repository provides the complete data pipeline used to construct the **Risk Persistence Index (RPI)** — a custom indicator that classifies 72 Colombian manufacturing export lines (HS Chapters 39–96) by their simultaneous exposure to:

- **Historical regulatory pressure** — EU environmental Technical Barriers to Trade (TBT) notifications registered in the WTO I-TIP Goods database (2019–2023)
- **Prospective regulatory threat** — active EU TBT alerts published on the WTO ePing platform (2024–2026)

The pipeline integrates three international databases at the HS-4 digit level and produces a master file (`Maestro_Riesgo_TBT_ePing.xlsx`) used for the interactive Power BI dashboard and the quantitative analysis of the working paper.

---

## Interactive dashboard

The full analytical results are available as a public, interactive Power BI dashboard with three synchronized views:

| Panel | What it shows |
|---|---|
| **Historical Vulnerability Matrix** | EU TBT notifications by HS chapter (I-TIP, 2019–2023) |
| **Prospective Risk Radar** | Active ePing alerts by chapter (2024–2026) |
| **Master Cross** | Bubble chart: export value × regulatory pressure × RPI category |

👉 **[Open the live dashboard](https://app.powerbi.com/view?r=eyJrIjoiNDkzZGVlYjMtZjZiNi00ODFmLWE4MDEtMzRhMmRlODRlMTk3IiwidCI6ImMwNmZiNTU5LTFiNjgtNGI4NC1hMTRmLTQ3ZDBkODM3YTVhYiIsImMiOjR9&pageName=3f6bdce00eefc57e3076)**

> *Filters are synchronized across all three panels — any chapter or risk-category selection updates the entire dashboard simultaneously.*

---

## Key findings

| Finding | Value |
|---|---|
| Export lines classified as **Extreme Risk** | **59 of 72 (81.9%)** |
| FOB export value under simultaneous historical + prospective pressure | **> USD 99.2 million** |
| Chapter with highest historical pressure | **Ch. 85** (electrical equipment) — 22 I-TIP measures |
| Chapter with highest prospective pressure | **Ch. 85** — 10 active ePing alerts |
| Largest export growth despite Extreme Risk classification | **Ch. 87** (vehicles) — +562% between periods |
| Central paradox | Several high-risk lines continue growing, suggesting exporters are not yet incorporating regulatory signals into their decisions |

---

## Data sources

| Source | Description | Coverage in this study |
|---|---|---|
| [WTO I-TIP Goods](https://i-tip.wto.org/goods) | Environmental TBT notifications in preparation, issued by the EU | 2019–2023 · 88 notifications · 18 HS chapters |
| [WTO ePing](https://epingalert.org) | Active regulatory alerts before entry into force | 2024–2026 · 75 relevant alerts · 103 processed records |
| [ODEB](https://observatorio.desarrolloeconomico.gov.co/bases-de-datos-exportaciones/) | Colombian exports to the EU by HS-4 code | 2019–2025 · 6,129 records · 72 active tariff headings |

> **Note on raw data:** Raw source files are not included in this repository due to size and licensing constraints. All three sources are publicly accessible via the links above. The processed master file (`Maestro_Riesgo_TBT_ePing.xlsx`) is provided directly in this repository.

---

## Repository structure

```
eu-tbt-export-risk-colombia/
│
├── 01 Unification of Colombian exports to the EU.py   # Script 1 — export data pipeline
├── 02 Homologation and organization of ePing.py       # Script 2 — alert normalization
├── 03 Master file - Data unification.py               # Script 3 — RPI construction
│
├── Maestro_Riesgo_TBT_ePing.xlsx                      # Master output file (72 tariff headings)
│
└── README.md
```

---

## Master output file — column reference

The file `Maestro_Riesgo_TBT_ePing.xlsx` contains two sheets.

### Sheet 1: `Maestro` — 72 tariff headings × 24 variables

| Column | Type | Description |
|---|---|---|
| `HS_Key` | Text | HS-4 tariff heading code |
| `Heading name` | Text | Full WCO description of the heading |
| `Chapter code` | Integer | HS-2 chapter number |
| `Chapter name` | Text | HS-2 chapter description |
| `2019`–`2025` | Float (USD) | Annual FOB export value to the EU (7 columns) |
| `Total_2019_2025` | Float (USD) | Cumulative FOB export value, full period |
| `Var_2019_2023` | Float (USD) | Absolute variation in FOB value, historical period |
| `Var_2024_2025` | Float (USD) | Absolute variation in FOB value, prospective period |
| `Trend_Post_TBT` | Text | Export trend in 2024–2025: `Growing` / `Falling` / `Stable` |
| `TBT_Count` | Integer | Number of EU environmental TBT measures in preparation (I-TIP, 2019–2023) |
| `TBT_Date_Min` | Date | Date of earliest I-TIP notification affecting this heading |
| `TBT_Date_Max` | Date | Date of most recent I-TIP notification affecting this heading |
| `TBT_Keywords` | Text | Regulatory objective declared in I-TIP |
| `ePing_Alerts` | Integer | Number of active EU ePing alerts affecting this chapter (2024–2026) |
| `ePing_Date_Min` | Date | Date of earliest ePing alert affecting this heading |
| `ePing_Date_Max` | Date | Date of most recent ePing alert affecting this heading |
| `ePing_Keys` | Text | HS chapter or heading keys used to link ePing alerts |
| `Risk_Persistence_Index` | Text | RPI category label with emoji flag (see table below) |
| `Risk_Score` | Integer | Numeric RPI score: 4 = Extreme Risk, 2 = Chronic Risk |
| `Risk_Narrative` | Text | Auto-generated plain-language description of the heading's risk profile |

### RPI classification logic

| Category | Condition | Score | Flag |
|---|---|---|---|
| **Extreme Risk** | Present in I-TIP **and** ePing | 4 | 🔴 |
| **Chronic Risk** | Present in I-TIP only | 3 | 🟠 |
| **Emerging Risk** | Present in ePing only | 2 | 🟡 |
| **No Identified Risk** | Absent from both sources | 1 | ⚪ |

> In this dataset, all 72 active export lines show at least Chronic Risk. 59 (81.9%) are classified as Extreme Risk.

---

### Sheet 2: `Tabla_Puente_ePing` — bridge table linking ePing alerts to HS chapters

| Column | Description |
|---|---|
| `Cross_Key` | ICS or HS range used to map the alert to a chapter |
| `HS_Chapter` | Resolved HS-2 chapter number |
| `Document_symbol` | WTO notification identifier (e.g., `G/TBT/N/EU/1132`) |
| `Distribution_date` | Date the alert was published on ePing |
| `Title_ePing` | Full regulatory title of the notified measure |
| `Link_ePing` | Direct URL to the WTO notification document |

> This table is the traceability layer of the pipeline: every alert in the master file can be traced back to its original WTO document via `Document_symbol` and `Link_ePing`.

---

## Pipeline — how the scripts work

Run the three scripts **in order**: `01 → 02 → 03`. Each produces the input required by the next.

### Script 01 — Export data unification
**Input:** Annual ODEB export files (CSV or Excel), one per year (2019–2025)  
**What it does:**
- Concatenates all annual files into a single dataset
- Filters records to HS Chapters 39–96 (manufactured goods and diversified consumer products)
- Retains only trade flows destined for the 27 EU member states

**Output:** Unified export base (2019–2025), filtered to EU destinations and HS 39–96

---

### Script 02 — ePing alert normalization
**Input:** Raw ePing export from [epingalert.org](https://epingalert.org) (158 EU notifications, 2024–2026)  
**What it does:**
- Filters notifications to those with an explicit environmental objective
- Separates multiple product codes per row into individual records
- Prioritizes HS codes over ICS codes when both are present
- Applies an ICS-to-HS equivalence dictionary for notifications without a direct tariff code
- Removes unidentifiable entries (ENV-General category)

**Output:** Clean dataset of 103 records, no null values, homogeneous HS coding

> **ICS → HS conversion note:** 83.5% of processed notifications (86 of 103) relied on the ICS-to-HS dictionary rather than a direct HS code. This introduces a potential classification bias, most pronounced in the 35 records corresponding to Chapters 28–38, where ICS equivalences cover broad, heterogeneous product groups. This limitation is discussed in the working paper (§5.3).

---

### Script 03 — Master file and RPI construction
**Input:** Outputs of Scripts 01 and 02 + I-TIP Goods export (88 notifications, 2019–2023)  
**What it does:**
- Integrates all three sources at the HS-4 digit level
- Builds the bridge table (`Tabla_Puente_ePing`) mapping ePing chapter ranges to specific headings with verified trade flows
- Calculates annual export trends (2024–2025 vs. historical baseline)
- Assigns the **Risk Persistence Index (RPI)** to each of the 72 active export lines
- Generates the `Risk_Narrative` field with a plain-language description per heading

**Output:** `Maestro_Riesgo_TBT_ePing.xlsx` with two sheets (`Maestro` and `Tabla_Puente_ePing`)

---

## Requirements and installation

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

No additional configuration is required. The scripts use only standard file paths and do not depend on environment variables or external APIs.

---

## Methodological notes

**Why a descriptive-classificatory design, not a gravitational model?**  
Standard gravity models require bilateral trade flow data with variance in the regulatory variable across partner countries. This study focuses on a single exporter (Colombia) facing a single integrated bloc (the EU), and the available databases (ODEB, ePing) do not include unit prices or substitution elasticities needed for impact estimation. The RPI is therefore designed as a prioritization instrument — identifying *where* risk accumulates before it becomes statistically observable — not as a causal impact estimator.

**What the RPI is and is not:**  
The index assumes equal weighting between historical pressure (I-TIP) and prospective threat (ePing), and uses binary classification (presence/absence) rather than continuous intensity measures. This simplification facilitates application but reduces the index's capacity to distinguish between a heading with one TBT measure and one with fifteen. The RPI should be interpreted as a *relative prioritization tool*, not as an absolute measure of regulatory risk. Weighted and continuous versions are identified as a future research direction in the working paper.

**Coverage and selection bias:**  
The 100% exposure result (all 72 active headings show at least Chronic Risk) partly reflects the study design: selecting HS Chapters 39–96 combined with broadly notified EU environmental TBTs increases the probability that any active export line registers at least one regulatory precedent. Results should be interpreted as evidence of widely extended regulatory pressure, not as proof that no lower-risk sectors exist outside the analyzed portfolio.

---

## Suggested citation

If you use this repository, the master file, or any of its scripts in your research, please cite as:

**Repository:**
> Lemus Serna, C. I. (2026). *eu-tbt-export-risk-colombia* [Source code repository]. GitHub. https://github.com/clemuss14430/eu-tbt-export-risk-colombia

**Dashboard:**
> Lemus, C. (2026). Latent Risk and Regulatory Persistence: OTC-ePing Trade Intelligence Dashboard for Colombian Manufacturing Exports to the European Union [Interactive Dashboard, Microsoft Power BI]. Universidad EAN. * https://app.powerbi.com/view?r=eyJrIjoiNDkzZGVlYjMtZjZiNi00ODFmLWE4MDEtMzRhMmRlODRlMTk3IiwidCI6ImMwNmZiNTU5LTFiNjgtNGI4NC1hMTRmLTQ3ZDBkODM3YTVhYiIsImMiOjR9&pageName=3f6bdce00eefc57e3076

**Working paper:**
> Lemus Serna, C. I. (2026). Latent Risk and Regulatory Persistence: European Environmental TBTs and Colombia’s Manufacturing Export Vulnerability, 2019-2026 *Working paper*. Universidad EAN, Semillero Inglomark.

---

## Author

**Carlos Iván Lemus Serna**  
International Business · Universidad EAN  
Semillero Inglomark
Bogotá, Colombia

---

## License

This repository is shared for academic and research replication purposes.  
Please contact the author before using any component in commercial or policy applications.
