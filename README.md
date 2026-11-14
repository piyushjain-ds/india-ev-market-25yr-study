# ⚡ Indian Electric Vehicle Ecosystem (2001–2026)
### A 25-Year Macro Study, OEM Consolidation & Infrastructure Analysis in Power BI

[![Power BI](https://img.shields.io/badge/Power_BI-Desktop-F2C811?logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![Data Modeling](https://img.shields.io/badge/Data_Model-Star_Schema-142B3B)](#data-architecture--star-schema)
[![Records Analyzed](https://img.shields.io/badge/Volume-10.62M_EVs-10B981)](#data-reconciliation--audit)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

---

## 📌 Executive Overview

This repository contains an end-to-end Business Intelligence study tracking 25 years of Indian electric vehicle (EV) registrations across vehicle segments, regional geographies, and manufacturing brands.

Using **10.62 million registration records** extracted from the official **MoRTH Vahan Dashboard** and historical archives, this project identifies structural demand shifts, policy sensitivity, market consolidation, and infrastructure bottlenecks across four distinct eras.

- **Author:** Piyush Jain
- **Deliverable:** 7-Page Executive Interactive BI Report
- **Core Stack:** Power BI Desktop, DAX, Power Query (M), Analytical Star Schema Modeling, Python (EDA)

---

## 📊 Key Findings: The 4 Market Eras

| Market Era | Period | Registrations | Annual Average |
|---|---|---:|---:|
| 1. The Silent Years | 2001–2015 | 61,592 | 4.11K |
| 2. The Spark | 2016–2020 | 559,038 | 111.8K |
| 3. The Explosion | 2021–2023 | 2,983,394 | 994.5K |
| 4. The Turf War | 2024–2026 YTD | 7,017,990 | 3.38M Run-Rate |

### 1. The Silent Years (2001–2015)
*Lead-Acid Baseline & Stagnation*

- **Total Registrations:** 61,592 units (4.11K annual average).
- **Segment Dynamic:** Dominated by low-speed 2-wheelers (72.82% share).
- **Key Inflection:** Temporary peak of 12.78K units in 2008, contracting to 2.4K units by 2014.
- **OEM Landscape:** Mainstream automotive conglomerates stayed on the sidelines; Mahindra (Reva) was the sole corporate presence (0.5K units in 2015 baseline).

### 2. The Spark (2016–2020)
*The Commercial E-Rickshaw Wave*

- **Total Registrations:** 559,038 units (~27x expansion to a 111.8K annual run-rate).
- **Segment Dynamic:** Commercial 3-wheelers comprised **83.06%** of all era registrations.
- **Catalyst:** FAME-I incentives enabled operating-cost advantages across high-density Tier-2/3 transit routes.
- **OEM Landscape:** Driven largely by regional kit assemblers (YC Electric: 43K, Saera Electric: 15K).

### 3. The Consumer Explosion (2021–2023)
*The 2W Startup Boom & Subsidy Rush*

- **Total Registrations:** 2,983,394 units (~8.9x expansion over The Spark).
- **Segment Dynamic:** Personal electric 2-wheelers captured **57.91%** of era volume.
- **May 2023 Subsidy Change:** Purchase incentives were reduced from 40% to 15%, effective June 1, 2023. Pre-deadline demand drove a record **165.2K units in May 2023**, followed by a 36% decline in June and recovery to 159K by November 2023.
- **OEM Shift:** Ola Electric ranked #1 (394K) and Ather Energy ranked #3 (184K), while unorganized kit assemblers contracted following localization audits.

### 4. The Turf War (2024–2026 YTD)
*Incumbent Network Consolidation*

- **Volume Scale:** 7,017,990 units, representing **66% of all EVs registered in Indian history**.
- **Legacy Overtake:** Bajaj Auto and TVS Motor Company expanded their combined market share from **23.8% (2024) to 35.6% (2026 YTD)**.
- **Startup Divergence:** Ola Electric contracted from 429K (2024 peak) to 112K YTD, while Ather Energy expanded from 137K in 2024 to 271K in 2026 YTD.
- **Geographic Core:** Uttar Pradesh leads commercial 3W adoption, while Maharashtra, Karnataka, and Tamil Nadu anchor consumer 2W registrations.

---

## 🏗️ Data Architecture & Star Schema

The model uses an **Analytical Star Schema** with single-direction 1-to-many relationships to manage multiple fact-table grains.

```text
                       ┌──────────────────────┐
                       │       Dim_Date       │
                       └──────────┬───────────┘
                                  │ 1
        ┌─────────────────────────┼─────────────────────────┐
        │ *                       │ *                       │ *
┌───────┴────────────────┐ ┌──────┴──────────────┐ ┌────────┴─────────────┐
│Fact_VehicleClass_Monthly│ │ Fact_Maker_Monthly │ │Fact_Maker_State_Yearly│
└───────┬────────────────┘ └──────┬──────────────┘ └────────┬─────────────┘
        │ *                       │ *                       │ *
        │ 1                       │ 1                       │ 1
┌───────┴───────────────┐ ┌──────┴─────────────┐ ┌──────────┴────────────┐
│   Dim_VehicleClass    │ │    Dim_Maker      │ │      Dim_State        │
└───────────────────────┘ └────────────────────┘ └───────────────────────┘
```

### Table Grain & Responsibilities

| Table Name | Type | Grain | Key Columns |
|---|---|---|---|
| `Fact_VehicleClass_Monthly` | Fact | 1 row per Vehicle Class per Month | `Month_Year`, `Vehicle_Class`, `Units_Sold` |
| `Fact_Maker_Monthly` | Fact | 1 row per Manufacturer per Month | `Month_Year`, `Maker`, `Units_Sold` |
| `Fact_Maker_State_Yearly` | Fact | 1 row per Maker per State per Year | `Year`, `State`, `Maker`, `Units_Sold` |
| `Dim_Date` | Dimension | 1 row per Month/Date | `Date`, `Year`, `MonthName`, `MonthNumber`, `IsCompleteMonth` |
| `Dim_VehicleClass` | Dimension | 1 row per Vehicle Class | `Vehicle_Class`, `Vehicle_Group` |
| `Dim_Maker` | Dimension | 1 row per Cleaned OEM Brand | `Maker`, `OEM_Category` |
| `Dim_State` | Dimension | 1 row per State/UT | `State`, `State_Code`, `Region` |

---

## 🔍 Data Reconciliation & Audit

| Time Horizon | Authoritative Source | Verified Registrations |
|---|---|---:|
| 2001–2015 | Kaggle Historical | 61,592 |
| 2016–2020 | Kaggle Historical | 559,038 |
| 2021–2023 | MoRTH Vahan Monthly | 2,983,394 |
| 2024–2026 YTD | MoRTH Vahan Monthly | 7,017,990 |
| **Cumulative Total** | **Reconciled Model** | **10,622,014** |

### ETL & Data Integrity Safeguards

1. **Summary Row Elimination:** Native source extracts contain `Total` summary records across vehicle classes. These were filtered before unpivoting to prevent double counting.
2. **Temporal Flagging (`IsCompleteMonth`):** October 2026 contains partial-month figures (98,511 units). The `IsCompleteMonth` column isolates incomplete periods to prevent annualized calculations and projections from being skewed.
3. **Geographic Standardization:** Regional naming variations were standardized across all 36 Indian administrative jurisdictions.

---

## 🧮 Core DAX Measures

### 1. Total Registrations

```dax
Total Registrations =
SUM('Fact_VehicleClass_Monthly'[Units_Sold])
```

### 2. 2026 Annualized Run-Rate

```dax
2026 Annual Run-Rate =
VAR CompleteMonthsVolume =
    CALCULATE(
        [Total Registrations],
        'Dim_Date'[Year] = 2026,
        'Dim_Date'[IsCompleteMonth] = TRUE()
    )
RETURN
    DIVIDE(CompleteMonthsVolume, 9) * 12
```

### 3. Dynamic Vehicle Segment Share

```dax
Segment Market Share % =
DIVIDE(
    [Total Registrations],
    CALCULATE(
        [Total Registrations],
        ALLSELECTED('Dim_VehicleClass')
    )
)
```

### 4. Combined Legacy Incumbent Share

```dax
Bajaj_TVS_Combined_Share =
VAR CombinedVolume =
    CALCULATE(
        [Total Registrations],
        'Dim_Maker'[Maker] IN {
            "BAJAJ AUTO LTD",
            "TVS MOTOR COMPANY LTD"
        }
    )
RETURN
    DIVIDE(CombinedVolume, [Total Registrations])
```

---

## ⚡ Infrastructure Bottlenecks & 2030 Projections

- **Public Charger Availability (2024):** 12,146 operational public charging stations nationwide.
- **Regional Concentration:** Maharashtra (3,079), Delhi (1,886), and Karnataka (1,041) host **49.4%** of India's operational public charging infrastructure.

### National Penetration vs. 2030 Benchmarks

| Vehicle Segment | Baseline | 2030 Aspiration |
|---|---:|---:|
| Commercial 3W | 58.0% | 70–80% |
| Consumer 2W | 6.5% | 80% |
| Passenger Cars | 2.8% | 30% |

---

## 📁 Repository Directory Structure

```text
├── data/
│   ├── raw/                       # Source extract archives
│   └── processed/                 # Cleaned fact and dimension tables
├── pbix/
│   └── India_EV_Market_Study.pbix
├── exports/
│   └── EV_Report.pdf
├── dax/
│   └── core_measures.dax
├── README.md
└── LICENSE
```

---

## 🚀 How to Run & Interact

1. Clone the repository:

```bash
git clone https://github.com/<your-username>/india-ev-market-25yr-study.git
```

2. Open `pbix/India_EV_Market_Study.pbix` using **Power BI Desktop**.
3. If data source paths change, navigate to **Transform Data → Data Source Settings** and redirect them to the `/data/processed/` folder.

---

## 📄 Scope & Limitations Disclosure

- **Cumulative vs. Active:** Vahan registration figures represent cumulative vehicle registrations, including vehicles subsequently de-registered or scrapped. They do not represent the exact number of active vehicles currently on the road.
- **Cutoff Disclosure:** 2026 data reflects Year-to-Date figures, with October 2026 represented as a partial-month snapshot.
- **Analytical Attribution:** Registration data confirms observed sales trends and OEM rotations. Causal drivers, such as dealer network scale, consumer battery confidence, and subsidy impact, are contextual business explanations rather than independently established causal findings.

---

## 📬 Contact & Connect

**Piyush Jain**

*Aspiring Data Analyst & BI Developer*

- **LinkedIn:** [LinkedIn Profile](https://www.linkedin.com/)
- **GitHub:** [GitHub Profile](https://github.com/)

---

**If you find this project useful, consider giving the repository a ⭐ on GitHub.**
