# Early Detection of Agricultural Drought in Kazakhstan

**RU:** Разработка системы раннего обнаружения сельскохозяйственной засухи в Казахстане на основе спутниковых данных и методов машинного обучения
**EN:** Development of a System for Early Detection of Agricultural Drought in Kazakhstan Based on Satellite Data and Machine Learning Methods
**KK:** Спутниктік деректер мен машиналық оқыту әдістері негізінде Қазақстандағы ауыл шаруашылығы құрғақшылығын ерте анықтау жүйесін әзірлеу

## Summary
The system collects satellite (MODIS) and weather data for the regions of Kazakhstan, compares the current season with the historical record, and produces an early drought-risk assessment and a spring wheat yield and other agricultures forecast at several lead times before harvest, with an explanation of each result.

## Contributor and supervisor
| Andreyev Alexander | <240848@astanait.edu.kz> |

| supervisor: Bakiyeva Aigerim |

## Links
- YouTrack: https://andronousss.youtrack.cloud/projects/DIP
- 
# 1. Proposal

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

### Related work and differentiation
Wheat yield forecasting in Kazakhstan with remote sensing and ML has been studied, including a 2000-2024 district-level model reaching R2 0.646 under spatial cross-validation, state-level forecasts on GEOGLAM data, and index comparisons for Northern Kazakhstan. A funded national project on ML-based drought early warning for Northern Kazakhstan also exists. This project differs by (a) treating drought detection as the target, with lead time and false alarms measured on named drought years, (b) benchmarking against classical drought-index rules and trend baselines, (c) validating by year and by region, (d) giving a feature-level explanation of each alert, and (e) delivering results in a dashboard with a retrospective view. It makes no claim of being the first system of its kind.

# 3. Literature review

**Status: source log (search stage).** No review or synthesis is written yet. This file records what was searched, which sources were found, and which are worth keeping. The review itself (table and critical synthesis) is written after the sources are actually read.

Requirements: at least 12 sources, at least 8 of them scientific; most from the last 5 years, plus foundational works.

### Search protocol
- Date of search: 22 October 2026.
- Databases and sites: Google Scholar, MDPI, Copernicus (EGU, ESSD), Frontiers, DOAJ, AGRIS, arXiv.
- Queries used:
  - `wheat yield forecasting Kazakhstan MODIS NDVI machine learning`
  - `agricultural drought monitoring Kazakhstan remote sensing VCI SPI wheat`
  - `spatial cross-validation data leakage crop yield prediction machine learning leave-one-year-out`
  - `SHAP interpretable machine learning wheat yield forecast satellite climate features lead time before harvest`
  - `CY-Bench benchmark dataset subnational crop yield forecasting`
  - `FAO ASIS Agricultural Stress Index System`
- Inclusion: a described data set, method and evaluation; relevant to Kazakhstan or semi-arid wheat, drought indices, validation, or interpretability.
- Exclusion: marketing material; works without a validation description.

### Themes
1. Drought indices and vegetation response in Kazakhstan and Central Asia
2. Wheat yield prediction in Kazakhstan with remote sensing and ML
3. Validation: random versus temporal and spatial cross-validation
4. Interpretability (SHAP) and lead time in yield forecasting
5. Benchmarks and operational systems

### Source log

### Theme 1. Drought indices (Kazakhstan, Central Asia)
- Ryssaliyeva L., Salnikov V., Lin Z., Raimbekova Z. (2025). Seasonal Sensitivity of Drought Indices in Northern Kazakhstan: A Comparative Evaluation and Selection of Optimal Indicators. *Sustainability* 17(21), 9413. DOI 10.3390/su17219413. Read: full text. Why: compares SPI, SPEI, HTC, MCZI with VCI/TCI/VHI for the target region; mentions a related funded early-warning project (grant AP25796409).
- Aitekeyeva N. et al. (2020). Drought Risk Assessment in Cultivated Areas of Central Asia Using MODIS Time-Series Data. *Water* 12(6), 1738. Why: MODIS-based drought risk on cultivated land, a classical-index reference.
- Comparison and Evaluation of Remote Sensing Indices for Agricultural Drought Monitoring over Kazakhstan. *ISPRS Archives* XLIII-B3-2020, 899. [verify authors]. Why: index comparison for Kazakhstan.

### Theme 2. Wheat yield prediction in Kazakhstan
-  Alpysbay M. et al. (2026) [verify authors]. Crop-Masked Vegetation Indices and TerraClimate for District-Level Wheat Yield Prediction in Kazakhstan: SAVI Advantage, Climate Dominance, and Temporal Transferability Limits. *Agronomy* 16(13), 1264. DOI 10.3390/agronomy16131264. Why: closest related work (district level, spatial and temporal validation); find where its district yield data come from.
-  Improve State-Level Wheat Yield Forecasts in Kazakhstan on GEOGLAM's EO Data by Leveraging a Simple Spatial-Aware Technique (2023). ICLR workshop paper, arXiv 2306.04646 [verify authors]. Why: state-level yield forecasts for Kazakhstan; check its yield data source.
-  Comparative assessment of ML algorithms for forecasting wheat yields using climate and satellite vegetation indices. *Eastern-European Journal of Enterprise Technologies* [verify title, authors, year]. Why: model comparison on MODIS and ERA5 data for Kazakhstan.

### Theme 3. Validation
- Khechba K., Belgiu M., Laamrani A., Dong Q., Stein A., Chehbouni A. (2024). Spatial cross-validation of wheat yield estimations using remote sensing and machine learning. EGU24-22153 (conference abstract). Why: justifies spatial validation.
- Sweet L.-B., Zscheischler J. (2022) [verify authors]. Using interpretable machine learning to identify compound meteorological drivers of crop failure. EGU22-5464 (conference presentation). Why: effect of the validation scheme on skill and interpretation.
- Deployment-oriented evaluation of stage-specific information for rice yield prediction across years, sites and site-year environments in southern China [verify authors, year]. Why: compares several evaluation schemes on one problem.

### Theme 4. Interpretability and lead time
- Joshi A., Pradhan B., Chakraborty S. et al. (2025) [verify full author list]. An explainable Bi-LSTM model for winter wheat yield prediction. *Frontiers in Plant Science*. Why: SHAP/LIME in wheat yield modelling.
- Integrating satellite and climate data to predict wheat yield in Australia using machine learning approaches [verify authors, journal, year]. Why: wheat yield forecast with a stated lead time before maturity.

### Theme 5. Benchmarks and operational systems
-  Paudel D. et al. (2026). CY-Bench: a comprehensive benchmark dataset for sub-national crop yield forecasting. *Earth System Science Data* 18(6), 3997. Why: standard benchmark and protocol; **check whether Kazakhstan is in the country list**.
- Cammalleri C. et al. (2021). A revision of the Combined Drought Indicator (CDI) used in the European Drought Observatory. *Nat. Hazards Earth Syst. Sci.* Why: model for a staged risk-level scheme.
- Rojas O. et al. (2019). Surveillance of agricultural drought worldwide from space using the FAO Agriculture Stress Index System (ASIS). Contributing paper to the Global Assessment Report 2019 (agency paper, not peer-reviewed article). Why: existing-system reference, see `04-existing-systems.md`.


### Questions to answer while reading
1. How is drought or yield loss defined and validated in each Kazakhstan study, and is there any detection-style evaluation (false alarms, lead time)?
2. Which validation schemes are used, and how large is the gap between random and temporal or spatial splits?
3. Which cropland masks and indices (NDVI, EVI, SAVI) are used, and do they matter?
4. Where do the district or state yield series in sources 6 and 7 come from, and are they usable for this project?
5. Does CY-Bench include Kazakhstan?
6. What lead times do yield and drought forecasts reach, and how much of the skill comes from climatology or trend alone?

### Still to do
- Read the full text of the Core sources and replace "abstract" with "full text" in this log.
- Confirm every [verify] field against the publication.
- Decide final inclusion (aim for at least 12 sources, at least 8 scientific, majority from the last 5 years) and then write the analysis table and critical synthesis from what was actually read.
- Add 2-3 non-Kazakhstan ML yield forecasting references if more breadth is needed.

# 4. Existing system comparison

### Systems and sources
| System | Type | Sources checked |
|---|---|---|
| FAO ASIS | international agricultural drought monitoring | https://agritechobservatory.review.fao.org/en/agricultural-stress-index-system-asis ; https://www.preventionweb.net/publications/view/65847 |
| Copernicus EDO/GDO | European and global drought observatories | https://drought.emergency.copernicus.eu/ ; https://data.jrc.ec.europa.eu/dataset/afa8a5ee-5473-439a-b062-ffdaedc38b2d ; https://cis2.eea.europa.eu/data/406 ; https://eu-space.europa.eu/news/unleashing-power-copernicus-enhancing-decision-making-european-and-global-drought-observatories |
| Kazhydromet agrometeorological service | national service | https://www.kazhydromet.kz/uploads/files_calendar/12487/file/6a82fc8414382prognoz-usloviy-uborki-na-2026-g-okonchatelnyy.pdf ; https://kazhydromet.kz/uploads/files_calendar/5669/file/6662fa31bb71ekratkiy-obzor-za-3-dek-maya-2024-g.pdf |
| EOSDA Crop Monitoring | commercial platform | https://eos.com/wp-content/docs/agriculture-consulting/EOSDA-solutions-for-Agriculture-Consultants.pdf ; https://www.capterra.com.sg/software/195553/eos-crop-monitoring |
| OneSoil | commercial platform | https://onesoil.ai/platform ; https://www.getapp.com.au/software/2113874/onesoil |

### Criteria (8)
1. Coverage of Kazakhstan
2. Spatial level
3. Update frequency
4. Yield forecast
5. Early drought warning
6. Result explanation
7. Openness and cost of access
8. Use of national statistics and local calibration

### Comparison matrix
| Criterion | FAO ASIS | Copernicus EDO/GDO | Kazhydromet | EOSDA | OneSoil |
|---|---|---|---|---|---|
| 1 Coverage of Kazakhstan | global service; Kazakhstan layer to confirm on the map | GDO has global indicators (e.g. SPI, a 10-day persistent-dry indicator); the Combined Drought Indicator is documented for Europe, global availability to confirm | national (state observation network) | global provider; Kazakhstan availability not stated | global app; Kazakhstan availability not stated |
| 2 Spatial level | administrative level, for comparison with national statistics | gridded, about 5 km for the CDI | oblast and zone-level descriptions from station network | field level | field level |
| 3 Update frequency | 10-day | CDI components computed daily; global indicator every 10 days; CDI published weekly (EEA catalogue) | decadal (10-day) reviews and seasonal forecasts | near-real-time per satellite pass | latest cloud-free image per field |
| 4 Yield forecast | not stated | not stated | yield assessment of spring grains is part of observations; harvest-condition forecasts published; numeric yield model not verified | yield prediction mentioned in vendor materials | yield analysis tools in Pro (productivity zones, yield maps); forecast not stated |
| 5 Early drought warning | agricultural drought hotspots from vegetation health index; lead time not stated | yes: Watch, Warning, Alert stages for agricultural drought | crop condition assessment and weather forecasts; formal drought warning lead time not verified | stress detection at field level | NDVI and moisture monitoring, short-range precipitation forecast; no drought warning stated |
| 6 Explanation of result | index-based; feature-level explanation not stated | rule-based cascade (precipitation → soil moisture → vegetation anomaly) | textual descriptions of conditions and crop phases | indices shown (NDVI, NDMI, NDRE); explanation not stated | indices and AI agronomist advice |
| 7 Openness and cost | free | data policy full and open | reviews published as PDFs on the website | free version and paid per-user plans | free core features; paid mobile and Pro plans |
| 8 National statistics / local calibration | compared with administrative statistics; local calibration not stated | European calibration; Kazakhstan not stated | national ground network | not stated | not stated |

Verification date for all cells: 22 October 2026. Cells marked "not stated" or "to confirm" are open items.

### Conclusion
Within the sources checked, the surveyed systems fall into three groups: global or European drought monitoring based on indices (ASIS, EDO/GDO), national agrometeorological reporting (Kazhydromet), and field-level commercial platforms (EOSDA, OneSoil). None of the sources found describes a regional-level system for Kazakhstan that combines (a) a yield forecast at several lead dates, (b) a drought risk level validated against national statistics and classical indices (VCI, SPI), and (c) a feature-level explanation of each result. This is the gap the project targets. The claim is limited to the sources checked and is not an exhaustive survey.

# 6. Requirements specification

Priorities: **M** = must, **S** = should, **C** = could.
Numeric quality thresholds (marked `[T]`) are fixed in this file by a commit **after EDA and before experiments begin**, so criteria are not tuned to results.

## Functional requirements

**FR-01 (M). Load satellite and weather data.**
Load MODIS NDVI/EVI and weather data by region for a chosen period.
*Accepted when:* a script reproducibly builds a region × 16-day window × indicator table, gaps are flagged, and the log records date and source.

**FR-02 (M). Load Bureau of Statistics tables.**
Load and parse tables of yield, area and lost crops.
*Accepted when:* the parser reads files of all years, "x" and "-" become missing values, regions are mapped to one scheme, and tests on real files pass.

**FR-03 (M). Compute features.**
Compute season-phase features (NDVI, precipitation, temperature, VCI, SPI).
*Accepted when:* features for year N do not use year-N yield data and a leakage test passes.

**FR-04 (M). Forecast yield.**
Forecast spring wheat yield by region at fixed lead dates before harvest.
*Accepted when:* RMSE and MAE are reported for each lead date and the model beats baseline B1 on RMSE `[T]`.

**FR-05 (M). Estimate drought risk.**
Estimate the drought risk level (normal / watch / risk / critical).
*Accepted when:* the level definition is documented, and recall, false alarms and lead time in weeks are measured `[T]`.

**FR-06 (M). Map and dashboard.**
Show a regional map and dashboard where the user chooses a region and lead date and sees the season versus the norm.
*Accepted when:* a user gets risk and forecast for any region in at most 3 actions.

**FR-07 (M). Explain results.**
Explain each result (SHAP, season comparison with drought years).
*Accepted when:* an explanation of feature contributions is available for every region.

**FR-08 (S). Retrospective view.**
Show a past year at a chosen date.
*Accepted when:* for any year the system shows the forecast that would have been issued on that date.

**FR-09 (C). Model comparison in the interface.**
*Accepted when:* a model metrics table is available on a page.

**FR-10 (C). Regional report export (CSV/PDF).**
*Accepted when:* the file downloads and contains forecast, risk level and sources.

## Non-functional requirements

**NFR-01 (M). Reproducibility.**
Results rebuild from raw data with one command.
*Accepted when:* `make data && make train` in a clean environment yields the same metrics (fixed seed).

**NFR-02 (M). Honest validation.**
Split by years only, and additionally by regions.
*Accepted when:* the splits are described in the verification plan and tests check them.

**NFR-03 (M). Source transparency.**
Every value in the UI shows its source and update date.
*Accepted when:* the UI passes a checklist review.

**NFR-04 (S). Performance.**
The map page loads in `[T]` seconds on precomputed data.
*Accepted when:* measured on a test machine.

**NFR-05 (M). Data.**
Use open aggregated data only, with no personal information.
*Accepted when:* repository review shows `.gitignore` in place and no secrets committed.

**NFR-06 (S). Responsive interface.**
The interface is readable on a phone.
*Accepted when:* checked at 375 px width.

**NFR-07 (M). Documentation.**
README and run instructions are provided.
*Accepted when:* a new person runs the project from the instructions.


## Technologies (selected)
Python (pandas, scikit-learn, LightGBM, SHAP), Google Earth Engine (`earthengine-api`), Parquet/GeoPackage, FastAPI, Streamlit (map and dashboard). Rationale: [docs/05-technology-comparison.md](docs/05-technology-comparison.md). (to be updated)



## Running and testing
To be added once executable code exists (`make data`, `make train`, `make app`, `pytest`).
