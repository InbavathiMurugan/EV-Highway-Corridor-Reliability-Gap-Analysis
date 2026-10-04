# EV Highway Corridor Reliability Gap Analysis

### Can an EV driver actually charge along India's major highways, or only inside cities?

A geospatial analytics project that evaluates **fast-charger coverage along six major national highways** and compares charging infrastructure with **EV demand across all 36 Indian states and Union Territories**.

**Author:** Inbavathi  
**Domain:** Data Analytics · Exploratory Data Analysis  
**Geography:** India  
**Tools:** Python · Pandas · GeoPandas · Shapely · SciPy · Scikit-learn · Matplotlib · Seaborn · Jupyter

---

## 1. Project Overview

Most EV-infrastructure analyses focus on the **number of chargers in each state**. However, state-level totals can hide important geographic gaps.

A state may have hundreds of chargers concentrated around major cities while long highway stretches remain poorly served.

This project therefore focuses on a **corridor-level question**:

> **Which highway stretches have no nearby fast charger, and do these gaps overlap with areas experiencing higher EV adoption?**

### State-level view vs. Corridor-level view

| State-level analysis | Corridor-level analysis |
|---|---|
| "Karnataka has 376 mapped chargers." | "A highway stretch has no fast charger within 50 km." |
| State averages can hide local gaps. | Identifies specific uncovered stretches. |
| Limited information for route planning. | Helps identify potential infrastructure priorities. |

---

## 2. Project Objectives

The project aims to:

- Measure fast-charger coverage along major national highways.
- Identify long stretches without nearby fast chargers.
- Compare charger supply with EV registrations.
- Identify states with potential EV infrastructure gaps.
- Evaluate whether charger **count alone** adequately represents highway coverage.
- Test how results change under different assumptions.
- Provide a reproducible geospatial analysis workflow.

---

## 3. Key Findings

| # | Finding |
|---|---|
| 1 | **10,377,820** EVs were registered between 2017 and 2026 in the Vahan data used. Cars and buses account for approximately **7%** of these registrations. |
| 2 | **1,822 operational chargers** were mapped in India, including **1,421 fast chargers** using the project's ≥25 kW definition. |
| 3 | Approximately **98,551 km (60%)** of the mapped road network has no fast charger within 50 km. |
| 4 | Among the six selected highways, **NH-544 (100%)** and **NH-48 (97.7%)** have the highest coverage. |
| 5 | **NH-52 (50.0%)** has the lowest coverage among the six selected highways. |
| 6 | The longest uncovered stretch on a selected highway is approximately **427 km on NH-44**. |
| 7 | **NH-66** has 243 nearby fast chargers but still contains a **376 km uncovered stretch**, demonstrating why charger count alone can hide corridor-level gaps. |
| 8 | Maharashtra has the largest uncovered key-highway length at approximately **891 km**, followed by Madhya Pradesh at **796 km**. |
| 9 | The highest overall state EV Desert Index scores are observed for **Bihar, Chandigarh, Assam, Tripura, and Punjab**. |
| 10 | Highway ratings remain broadly stable under the two primary fast-charger definitions, while the stricter ≥50 kW definition changes some highway classifications. |

> **Important:** Open Charge Map is a crowdsourced source. Areas with low mapped charger counts may partly reflect incomplete or outdated mapping rather than the absence of real-world infrastructure.

---

## 4. Highway Coverage Scorecard

**Fast charger definition:** Operational charger with maximum power ≥25 kW  
**Gap definition:** Segment midpoint is more than 50 km from the nearest fast charger

| Highway | Length (km) | Fast Chargers Within 5 km | Chargers / 100 km | Covered (%) | Uncovered (km) | Longest Gap (km) | Rating |
|---|---:|---:|---:|---:|---:|---:|---|
| NH-544 | 330.6 | 101 | 30.6 | 100.0 | 0.0 | 0.0 | Reliable |
| NH-48 | 2,542.5 | 191 | 7.5 | 97.7 | 58.2 | 27.1 | Reliable |
| NH-66 | 1,646.0 | 243 | 14.8 | 70.1 | 491.8 | 376.1 | Patchy |
| NH-16 | 1,711.8 | 38 | 2.2 | 64.4 | 609.0 | 126.2 | Poor |
| NH-44 | 3,558.1 | 186 | 5.2 | 56.0 | 1,566.8 | 426.6 | Poor |
| NH-52 | 2,228.7 | 19 | 0.9 | 50.0 | 1,114.7 | 173.4 | Poor |

### Rating Criteria

- **Reliable:** >90% covered
- **Patchy:** 70–90% covered
- **Poor:** ≤70% covered

---

## 5. Visualizations

The project includes maps and charts showing highway coverage, state-level infrastructure gaps, and EV-demand relationships.

### Key Visuals

- [Fast-charging coverage on six highways](outputs/figures/10_gap_map_6_highways.png)
- [Highway scorecard](outputs/figures/09_highway_scorecard.png)
- [EV Desert quadrant](outputs/figures/06_ev_desert_quadrant.png)
- [State gap score map](outputs/figures/18_gap_score_map.png)

Additional charts and maps are available in:

```text
outputs/figures/
```

---

## 6. Data Sources

| Dataset | Source | Purpose |
|---|---|---|
| EV registrations | [Vahan Dashboard](https://vahan.parivahan.gov.in/vahan4dashboard/) | EV demand |
| Charging stations | [Open Charge Map API](https://openchargemap.org/site/develop/api) | Charger supply |
| National highways | MoRTH via PM GatiShakti / [India Geodata](https://yashveeeeeeer.github.io/india-geodata/) | Highway network |
| State & UT boundaries | India Geodata | Spatial assignment and analysis |

### EV Registration Data

The Vahan dataset covers:

- Calendar years: **2017–2026**
- Fuel types: **ELECTRIC(BOV)** and **PURE EV**
- Registration status: **ACTIVE**
- Geography: **All 36 states and Union Territories**

> **Note:** 2026 is a partial year and should not be interpreted as a complete annual total.

### Charging Data

Open Charge Map was queried for India.

- Raw records: **1,979**
- Cleaned records: approximately **1,841**
- Operational chargers used in the final analysis: **1,822**
- Fast chargers under the ≥25 kW definition: **1,421**

Open Charge Map data is licensed under **CC BY 4.0**. Attribution should be provided when reusing the charger data.

---

## 7. Methodology

### Step 1 — Data Cleaning

Each dataset was cleaned and standardized before analysis.

| Dataset | Raw / Original | Cleaned |
|---|---:|---:|
| Charging stations | 1,979 | 1,841 |
| Highway records | 10,317 | 9,507 |

Cleaning included handling:

- Duplicate records
- Invalid geometries
- Missing values
- Coordinate issues
- Inconsistent state names
- Highway attributes
- Charger operational status

---

### Step 2 — Fast Charger Definition

A charger is classified as a **fast charger** when:

```text
Operational AND Maximum Power ≥ 25 kW
```

The dataset's own fast-charge flag disagreed with the power-based definition for **152 stations**.

Therefore, the primary analysis uses **maximum charging power** as the fast-charger definition.

The alternative dataset flag is tested during the robustness analysis.

---

### Step 3 — Highway Segmentation

The highway network is divided into approximately **25 km segments**.

For each segment:

1. Calculate the segment midpoint.
2. Find the nearest operational fast charger.
3. Calculate the straight-line distance to that charger.
4. Classify the segment as covered or uncovered.

A segment is classified as a **gap** when:

```text
Distance to nearest fast charger > 50 km
```

---

### Step 4 — Highway Corridor Analysis

Six national highways were selected for detailed corridor analysis:

```text
NH-44
NH-48
NH-16
NH-66
NH-544
NH-52
```

Only chargers located within a **5 km corridor buffer** of the corresponding highway are counted for the highway scorecard.

This prevents unrelated chargers located elsewhere in the state from artificially improving highway coverage.

---

### Step 5 — Highway Scoring

Each highway is evaluated using:

- Total highway length
- Fast chargers within the corridor
- Chargers per 100 km
- Percentage of highway covered
- Total uncovered length
- Longest uncovered stretch

The primary highway rating is based on percentage of covered highway length.

---

### Step 6 — EV Desert Index

A state-level **EV Desert Index** ranging from 0–100 is calculated using three components:

| Component | Weight |
|---|---:|
| EVs per charger | 40% |
| Share of highway in gap | 35% |
| Charger sparsity per area | 25% |

Each component is **min-max scaled** before applying the weights.

A higher score indicates a potentially larger mismatch between EV demand and charging infrastructure.

> The weights are analytical assumptions rather than an official standard.

---

### Step 7 — Robustness Testing

The analysis tests whether the findings change under different assumptions.

#### Gap-distance sensitivity

```text
25 km
50 km
75 km
100 km
```

#### Fast-charger definitions

```text
≥22 kW
≥25 kW
≥50 kW
Dataset fast-charge flag
```

#### Weighting sensitivity

Five alternative weighting schemes are tested for the EV Desert Index.

This helps determine whether the conclusions depend heavily on a particular threshold or weighting choice.

---

### Step 8 — K-Means Clustering

K-Means clustering with **3 clusters** is used as a supporting analytical view.

The clustering helps group states with similar combinations of:

- EV demand
- Charger availability
- Highway gaps
- Charger density

Clustering is treated as a supporting analysis rather than the primary ranking method.

---

## 8. Coordinate Reference System

All distance and length calculations use:

```text
EPSG:7755
```

This projected coordinate reference system allows geometric measurements to be performed in metres rather than geographic degrees.

---

## 9. Repository Structure

```text
EV_Highway_Corridor_Reliability_Gap_Analysis/
│
├── README.md
│
├── data/
│   ├── Raw datasets/
│   │   └── Source downloads
│   │
│   └── Cleaned datasets/
│       ├── states_clean.parquet
│       ├── highways_clean.geojson
│       ├── Ev_registration_all_cleaned.csv
│       ├── Ev_registration_car_cleaned.csv
│       ├── Ev_registration_bus_cleaned.csv
│       └── Ev_chargers_cleaned.csv
│
├── notebooks/
│   ├── Dataset cleaning notebooks
│   └── Final analysis notebook
│
├── outputs/
│   ├── figures/
│   │   └── PNG charts and maps
│   │
│   └── tables/
│       ├── highway_scorecard.csv
│       ├── state_gap_master.csv
│       ├── state_priority_6_highways.csv
│       ├── highway_rating_by_definition.csv
│       └── GeoJSON analysis outputs
│
├── docs/
│   └── PROJECT_REPORT.md
│
└── requirements.txt
```

---

## 10. Technologies Used

### Programming & Analysis

- Python
- Pandas
- NumPy
- GeoPandas
- Shapely
- SciPy
- Scikit-learn

### Visualization

- Matplotlib
- Seaborn

### Development Environment

- Jupyter Notebook
- Git / GitHub

---

## 11. Reproducibility

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd EV_Highway_Corridor_Reliability_Gap_Analysis
```

### 2. Create a Python environment

```bash
python -m venv .venv
```

Activate it on Windows:

```bash
.venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install pandas numpy geopandas shapely scipy scikit-learn matplotlib seaborn pyarrow requests jupyter
```

Or, if available:

```bash
pip install -r requirements.txt
```

### 4. Configure Open Charge Map API

Create a free Open Charge Map API key.

Keep the API key **outside the source code** using an environment variable or `.env` file.

Make sure `.env` is included in `.gitignore`.

### 5. Run the notebooks

Run the notebooks in the following order:

```text
1. Dataset cleaning notebooks
2. Final analysis notebook
```

The analysis generates outputs in:

```text
outputs/figures/
outputs/tables/
```

---

## 12. Main Analysis Parameters

The primary analysis thresholds are defined at the beginning of the analysis notebook.

```python
CORRIDOR_KM = 5
SEG_KM = 25
GAP_KM = 50
FAST_KW = 25
TARGET_HIGHWAYS = [
    "NH-44",
    "NH-48",
    "NH-16",
    "NH-66",
    "NH-544",
    "NH-52"
]
```

These parameters can be modified to reproduce the sensitivity analysis.

---

## 13. Limitations

The results should be interpreted within the limitations of the available datasets and assumptions.

### 13.1 Crowdsourced charger data

Open Charge Map is crowdsourced and may contain:

- Missing stations
- Outdated records
- Duplicate locations
- Incorrect status or power information

Therefore, a mapped gap does not necessarily mean that no real-world charger exists.

### 13.2 Operational status is a snapshot

A station marked as operational does not guarantee that it is currently available or functioning.

This project measures **mapped infrastructure presence**, not real-time charging reliability.

### 13.3 Straight-line distance

The 50 km gap rule uses straight-line distance from the segment midpoint to the nearest fast charger.

It does not represent actual driving distance along the road network.

### 13.4 Highway dataset age

The highway dataset was updated on **30-06-2022**.

The analysis does not filter the network by current construction or operational status, so some proposed or under-construction road segments may be present.

### 13.5 Analyst-defined thresholds

The following are project assumptions:

- 5 km corridor buffer
- 25 km highway segmentation
- 50 km gap threshold
- ≥25 kW fast-charger definition

These are not official government standards.

### 13.6 EV demand is a proxy

Vahan registration data represents registered vehicles at the state level.

It does not directly measure:

- Highway traffic
- EV travel patterns
- Inter-state trips
- Charging demand at individual locations

### 13.7 EV registration period

The 2026 registration data is year-to-date and therefore should not be compared directly with completed calendar years without considering the partial-year effect.

### 13.8 EV Desert Index weights

The 40/35/25 weighting scheme is a project-specific analytical choice.

State rankings can change when different weights are applied.

---

## 14. Future Work

Potential extensions include:

- Repeat charger data collection over time to measure infrastructure changes.
- Use **road-network driving distance** instead of straight-line distance.
- Add highway traffic volume and vehicle-flow data.
- Expand the analysis beyond the six selected highways.
- Compare Open Charge Map with official state-wise charger counts.
- Estimate the minimum number of new charging locations required to reduce all gaps below a target distance.
- Build an interactive dashboard for highway and state-level exploration.
- Develop route-level lookup functionality for EV drivers.
- Incorporate charger reliability and uptime data when available.

---

## 15. Acknowledgements

This project uses publicly available data and resources from:

- **Open Charge Map contributors** — charging-station data
- **Ministry of Road Transport and Highways** — Vahan registration data and highway information
- **PM GatiShakti / India Geodata** — geospatial highway and state-boundary data
- **India Geodata project** — processed geographic datasets

Open Charge Map data is licensed under **CC BY 4.0**. Please provide appropriate attribution when reusing the charger dataset.

---

## 16. Project Report

Detailed methodology, analysis, results, visualizations, and sensitivity checks are documented in:

```text
docs/PROJECT_REPORT.md
```

---

## 17. Conclusion

This project demonstrates why **charger counts alone are not enough to evaluate EV infrastructure**.

A state may have a large number of charging stations while important highway stretches remain poorly served. By combining **EV registrations, charger locations, highway geometry, spatial segmentation, and gap analysis**, this project provides a more route-oriented view of charging infrastructure.

The analysis therefore shifts the question from:

> **"How many chargers does a state have?"**

to:

> **"Can an EV driver actually travel along the highway and find a fast charger when needed?"**