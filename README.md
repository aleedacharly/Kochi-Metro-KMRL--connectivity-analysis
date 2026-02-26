# 🚇 Kochi Metro (KMRL) Service Frequency Risk Analysis

A data analysis project using **open GTFS data** from Kochi Metro Rail Limited (KMRL)
to evaluate service frequency across all 25 metro stops and assess passenger wait-time risk.

## 🔍 Key Finding

> All 25 KMRL metro stops operate with an average headway of **~8.9 minutes** on weekdays —
> above the **7-minute urban transit benchmark** for good frequency service.
> This classifies every stop as **Medium Risk** for passenger wait time.

| Metric | Value |
|---|---|
| Total metro stops | 25 |
| Weekday trips | 255 |
| Weekend trips | 195 |
| Avg headway (weekday) | 8.9 min |
| Urban benchmark | < 7 min |
| Risk level | Medium Risk (all stops) |

Weekend service is **24% lower** than weekday, worsening headways further on Sundays.

## 🗺️ Interactive Map

Open `output/kochi_metro_headway_map.html` in your browser to explore all 25 stops
with headway and risk data in popups.

## 📁 Project Structure
```
kochi-metro-lastmile-risk/
├── README.md
├── analysis.ipynb          ← main notebook (4 steps)
├── KMRLOpenData/           ← raw GTFS data
│   ├── stops.txt
│   ├── routes.txt
│   ├── trips.txt
│   ├── stop_times.txt
│   └── ...
└── output/
    ├── kochi_metro_headway_map.html     ← interactive map
    └── kochi_metro_headway_analysis.csv ← findings table
```

## 🛠️ Methodology

1. **Load** GTFS files — stops, routes, trips, stop_times, calendar
2. **Filter** weekday trips (service_id = `WK`) and count unique trips per stop
3. **Calculate headway** — `(18 hrs × 60 min) ÷ (trips in one direction)`
4. **Classify risk** — headway > 10 min = High, 7–10 min = Medium, < 7 min = Low
5. **Visualize** — interactive Folium map with popups per stop

## 📦 Requirements
```bash
pip install pandas geopandas shapely folium
```

## 🗃️ Data Source

[KMRL Open Data](https://kochimetro.org) — General Transit Feed Specification (GTFS)
format, covering Kochi Metro Route 1 (Aluva ↔ Tripunithura), 25 stops.

## 💡 Context

KMRL operates Kerala's first metro system. While the network is expanding,
frequency improvements — especially on weekends — would significantly improve
the passenger experience and bring KMRL closer to international urban transit standards.
