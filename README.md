# Early Detection of Agricultural Drought in Kazakhstan

**RU:** Разработка системы раннего обнаружения сельскохозяйственной засухи в Казахстане на основе спутниковых данных и методов машинного обучения
**EN:** Development of a System for Early Detection of Agricultural Drought in Kazakhstan Based on Satellite Data and Machine Learning Methods
**KK:** Спутниктік деректер мен машиналық оқыту әдістері негізінде Қазақстандағы ауыл шаруашылығы құрғақшылығын ерте анықтау жүйесін әзірлеу

## Summary
The system collects satellite (MODIS) and weather data for the regions of Kazakhstan, compares the current season with the historical record, and produces an early drought-risk assessment and a spring wheat yield and other agricultures forecast at several lead times before harvest, with an explanation of each result.

## Team and supervisor
| Andreyev Alexander | <240848@astanait.edu.kz> |

| supervisor: Bakiyeva Aigerim |


## Problem, users, expected outcome

### Problem statement
Grain production in Kazakhstan is largely rain-fed and depends on precipitation, so drought directly reduces yield. Final yield and loss figures are published by the Bureau of National Statistics only after harvest (for example, the 2025 publication "Gross harvest of agricultural crops" reports yield and the area of lost crops by region), which is too late for operational decisions. Satellite and weather data are openly available and updated regularly, but they are not combined into a single tool that compares the current season with a region's history, estimates risk in advance, and explains the estimate.

**Verified facts (open data, 2025):** spring wheat yield differs roughly twofold between regions (North Kazakhstan 20.7 c/ha, Kostanay 16.0, Aktobe 10.7, West Kazakhstan 7.6); the area of lost spring crops nationwide was 132,640 ha, of which 42,930 ha in Akmola and 32,862 ha in Kostanay.
**Assumptions (to be tested by interviews and data):** users need an assessment several weeks before harvest; existing tools do not provide a regional forecast with an explanation for Kazakhstan.

### Target users
Analysts in agricultural departments and akimats, agronomy consultants, researchers (experts assessing the method), system administrator.

### Goal
Develop and evaluate a system that uses satellite and weather data to determine drought risk in advance and forecast spring wheat yield by region of Kazakhstan, and show on retrospective data that it outperforms simple baselines.

### Objectives
1. Collect and clean data: MODIS NDVI/EVI, weather (ERA5-Land, CHIRPS), Bureau statistics (yield, area, lost crops) for the longest available period.
2. Define and fix drought labels (yield anomaly relative to trend, share of lost crops, meteorological index).
3. Build season-phase features and baselines (mean, trend, peak-NDVI regression, classical indices VCI/SPI).
4. Train and compare models (regularized regression, gradient boosting, optionally a neural network) with leave-one-year-out validation.
5. Measure forecast quality and detection lead time; analyze errors and interpret results (SHAP).
6. Implement a web dashboard with a regional map, forecast, risk level, explanation and retrospective view.
7. Verify the system and document its limitations.

### Expected results
A reproducible data pipeline; a model comparison against baselines; quantitative estimates of accuracy and lead time; a web dashboard; a repository with documentation and tests; a thesis report.

### Scope
**In scope:** region level (district level if data allow), spring wheat, rain-fed regions of the north and centre, one season per year, open data.
**Out of scope:** individual fields, irrigated regions, other crops, agronomic recommendations, alerts and a mobile app, integration with government systems.

### Engineering complexity
Multi-source data processing (geospatial data, time series, tabular statistics with unstable formats), leakage-free feature construction, model comparison on a small sample with honest validation, explainability, interactive visualization.


## Technologies (selected)
Python (pandas, scikit-learn, LightGBM, SHAP), Google Earth Engine (`earthengine-api`), Parquet/GeoPackage, FastAPI, Streamlit (map and dashboard). Rationale: [docs/05-technology-comparison.md](docs/05-technology-comparison.md). (to be updated)

## Links
- YouTrack: https://andronousss.youtrack.cloud/projects/DIP
- Documentation: [docs/](docs/), pre-defense checklist: [docs/00-predefense-checklist.md](docs/00-predefense-checklist.md)

## Running and testing
To be added once executable code exists (`make data`, `make train`, `make app`, `pytest`).
