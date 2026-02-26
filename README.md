# 🚇 Kochi Multimodal Transit Connectivity Analysis

A data analysis project using open GTFS data from **KMRL (Kochi Metro)** and
**Jungle Bus / OpenStreetMap** to evaluate last-mile connectivity across all
25 Kochi Metro stops — measuring how well each metro stop is served by
buses and boats within walking distance.

---

## 🔍 Key Findings

### 1. Last-Mile Connectivity
> **5 out of 25 metro stops are poorly connected** — fewer than 5 bus stops
> within 500m walking distance and zero boat connectivity.

| Connectivity Level | Stops | 
|---|---|
| 🟢 Well Connected (≥15 bus stops within 500m) | 1 |
| 🟡 Moderately Connected (5–14 stops) | 19 |
| 🔴 Poorly Connected (<5 stops) | 5 |

**Poorly connected stops:** Companypady, Ambattukavu, Muttom, Pathadipalam, SN Junction

### 2. The Feeder Gap
All 5 poorly connected stops have **7–9 bus stops within 1km** — meaning
the infrastructure already exists nearby. The fix is not building new
infrastructure but **extending existing bus routes by ~500m** or adding
a dedicated feeder service.

### 3. Service Frequency (Headway)
> KMRL operates with an average headway of **~8.9 minutes** on weekdays —
> above the 7-minute urban benchmark for good frequency service.

| Metric | Value |
|---|---|
| Total metro stops | 25 |
| Weekday trips | 255 |
| Weekend trips | 195 |
| Avg headway (weekday) | 8.9 min |
| Urban benchmark | < 7 min |
| Weekend service drop | 24% fewer trains |

### 4. Phase 1 vs Phase 2 Service Gap
Phase 2 southern stations (Pettah, SN Junction, Elamkulam, Thykoodam,
Vyttila, Tripunithura, Vadakkekotta) operate at a **9.0 min headway**
vs 8.5–8.9 min for Phase 1 stations — confirming newer extensions are
less frequently served.

---

## 🗺️ Interactive Maps

| Map | Description |
|---|---|
| [`output/kochi_metro_headway_map.html`](output/kochi_metro_headway_map.html) | Metro stops colored by headway risk |
| [`output/kochi_connectivity_map.html`](output/kochi_connectivity_map.html) | Full multimodal map — metro + bus + boat + feeder zones |

Open either file in your browser to explore stops with full details in popups.

---

## 📁 Project Structure
```
kochi-transit-analysis/
├── README.md
├── analysis.ipynb               ← main notebook
├── KMRLOpenData/                ← Kochi Metro GTFS (raw)
│   ├── stops.txt
│   ├── routes.txt
│   ├── trips.txt
│   ├── stop_times.txt
│   └── ...
├── KochiTransportData/          ← Bus & Boat GTFS (Jungle Bus / OSM)
│   ├── stops.txt
│   ├── routes.txt
│   ├── trips.txt
│   ├── frequencies.txt
│   └── ...
└── output/
    ├── kochi_metro_headway_map.html       ← headway risk map
    ├── kochi_connectivity_map.html        ← multimodal connectivity map
    ├── all_stops_overview.png             ← static overview plot
    └── kochi_metro_headway_analysis.csv   ← findings table
```

---

## 🛠️ Methodology

### Step 1 — Load GTFS Data
Load stops, routes, trips, stop_times and calendar from both KMRL
and Jungle Bus GTFS feeds. GTFS (General Transit Feed Specification)
is an open standard for publishing public transport schedules and
geographic information.

### Step 2 — Service Frequency Analysis
Filter weekday trips (service_id = `WK`), count unique trips per stop,
and calculate average time between trains:
```
Time between trains = (18hrs × 60min) ÷ (trips in one direction)
```
Classify each stop by how often trains arrive:
- 🔴 **Low Frequency** — trains every >9.0 min
- 🟡 **Medium Frequency** — trains every 7–9 min
- 🟢 **High Frequency** — trains every <7 min

The urban transit benchmark for a good passenger experience is
a train every 7 minutes or less — at this frequency, passengers
don't need to check a timetable.

### Step 3 — Build GeoDataFrames
Convert all stop tables (metro, bus, boat) into spatial GeoDataFrames
using GeoPandas, assigning each stop a point geometry based on its
latitude and longitude coordinates (EPSG:4326).

### Step 4 — 500m Walking Distance Analysis
Re-project all layers to UTM Zone 43N (EPSG:32643) so distances
are measured in metres rather than degrees. Draw a 500m buffer
around each metro stop — representing a comfortable 6–7 minute
walk — and count how many bus and boat stops fall inside.
This measures how easy it is for a passenger to continue their
journey after getting off the metro.

### Step 5 — Connectivity Classification
Classify each metro stop based on the number of bus and boat
stops within 500m walking distance:
- 🟢 **Well Connected** — ≥15 nearby stops (multiple route options)
- 🟡 **Moderately Connected** — 5 to 14 nearby stops (some options)
- 🔴 **Poorly Connected** — fewer than 5 nearby stops (limited options, passenger may struggle to continue journey)

### Step 6 — Feeder Gap Analysis
For poorly connected stops, extend the buffer to 1km and recount
bus stops. If many stops exist in the 500m–1km zone, it means
buses are close but just out of walking range — a short feeder
route extension or auto-rickshaw stand would bridge the gap
at very low cost.


---

## 💡 Recommendations

Based on the analysis, the following interventions would significantly
improve last-mile connectivity at poorly connected stops:

| Stop | Bus Stops (500m) | Bus Stops (1km) | Recommendation |
|---|---|---|---|
| Companypady | 2 | ~9 | Extend nearest bus route by 500m |
| Ambattukavu | 2 | ~9 | Extend nearest bus route by 500m |
| Muttom | 4 | ~11 | Add auto-rickshaw/feeder stand |
| Pathadipalam | 4 | ~11 | Add auto-rickshaw/feeder stand |
| SN Junction | 4 | ~11 | Extend Phase 2 bus feeder |

---

## 📦 Requirements
```bash
pip install pandas geopandas shapely folium requests
```

## ▶️ How to Run
```bash
git clone https://github.com/yourusername/kochi-transit-analysis.git
cd kochi-transit-analysis
jupyter notebook analysis.ipynb
```

Run all cells in order. The notebook will automatically download the
Jungle Bus GTFS data on first run.

---

## 🗃️ Data Sources

| Dataset | Source | License |
|---|---|---|
| KMRL Metro GTFS | [kochimetro.org](https://kochimetro.org/open-data/) | KMRL Open Data |
| Bus & Boat GTFS | [Jungle Bus / OpenStreetMap](https://jungle-bus.github.io/KochiTransport/) | ODbL |

---

## 🌍 Context

Kochi Metro (KMRL) became the **first metro in India to publish open GTFS
data** in 2018. Despite good infrastructure, last-mile connectivity remains
a challenge — particularly for newer Phase 2 stations. This project uses
open data to identify specific gaps and propose targeted, low-cost solutions
that planners and policymakers can act on.

---

## 🔮 Future Work

- Peak vs off-peak headway analysis using `stop_times.txt`
- Fare zone map using `fare_rules.txt`
- Water Metro integration (KMRL Kochi-1 app data)
- Amenity analysis using OpenStreetMap (hospitals, IT parks, schools near stops)
- Compare weekday vs weekend connectivity gaps

> ⚠️ Note: Bus/boat stop data (Jungle Bus) was last updated in 2022.
> Connectivity figures may differ from current ground reality.
> Metro GTFS data is from KMRL Open Data (2024).
---

*Built with Python, GeoPandas, Folium and open transit data.*
*Jungle Bus © OpenStreetMap contributors*