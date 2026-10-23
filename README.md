# Early Detection of Agricultural Drought in Kazakhstan

**RU:** Разработка системы раннего обнаружения сельскохозяйственной засухи в Казахстане на основе спутниковых данных и методов машинного обучения
**EN:** Development of a System for Early Detection of Agricultural Drought in Kazakhstan Based on Satellite Data and Machine Learning Methods
**KK:** Спутниктік деректер мен машиналық оқыту әдістері негізінде Қазақстандағы ауыл шаруашылығы құрғақшылығын ерте анықтау жүйесін әзірлеу

## Summary
The system collects satellite (MODIS) and weather data for the regions of Kazakhstan, compares the current season with the historical record, and produces an early drought-risk assessment and a spring wheat yield and other agricultures forecast at several lead times before harvest, with an explanation of each result.

## Team and supervisor
| Andreyev Alexander | <240848@astanait.edu.kz> |

| supervisor: Bakiyeva Aigerim |


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
Wheat yield forecasting in Kazakhstan with remote sensing and ML has been studied, including a 2000-2024 district-level model reaching R2 0.646 under spatial cross-validation, state-level forecasts on GEOGLAM data, and index comparisons for Northern Kazakhstan (see [docs/03-literature-review.md](03-literature-review.md)). A funded national project on ML-based drought early warning for Northern Kazakhstan also exists. This project differs by (a) treating drought detection as the target, with lead time and false alarms measured on named drought years, (b) benchmarking against classical drought-index rules and trend baselines, (c) validating by year and by region, (d) giving a feature-level explanation of each alert, and (e) delivering results in a dashboard with a retrospective view. It makes no claim of being the first system of its kind.

# 3. Literature review

Requirements: at least 12 sources, at least 8 scientific; most from the last 5 years, plus foundational works.
Status (22 Oct 2026): 19 sources collected: 14 journal articles or dataset papers (rows 1, 2, 4, 5, 6, 8, 9, 10, 11, 14, 15, 16, 17, 19), 4 conference or workshop items (rows 3, 7, 12, 13) and 1 agency paper (row 18). At least 10 were published in 2021 or later (rows 1, 6, 7, 9, 10, 12, 13, 15, 17, 19); the years of rows 5, 8, 11, 14 and 16 are still to be verified. **Reading depth is stated per row**: "abstract" means only the abstract and visible parts were checked via search; "full text" means the whole article was read. Rows marked [verify] contain a field that was not visible in the search result. Before final submission, open each publication and confirm authors, year, venue and the numbers quoted below.

### Search protocol
- Databases: Google Scholar, MDPI, Copernicus (EGU, ESSD), Frontiers, DOAJ, AGRIS, arXiv; searches run on 22 October 2026.
- Queries used: `wheat yield forecasting Kazakhstan MODIS NDVI machine learning`; `agricultural drought monitoring Kazakhstan remote sensing VCI SPI wheat`; `spatial cross-validation data leakage crop yield prediction machine learning leave-one-year-out`; `SHAP interpretable machine learning wheat yield forecast satellite climate features lead time before harvest`; `CY-Bench benchmark dataset subnational crop yield forecasting`; `FAO ASIS Agricultural Stress Index System`.
- Inclusion: works with a described data set, method and evaluation, relevant to Kazakhstan/semi-arid wheat, drought indices, validation or interpretability. Exclusion: marketing material and works without a validation description.

### Themes
1. Drought indices and vegetation response in Kazakhstan
2. Wheat yield prediction in Kazakhstan with remote sensing and ML
3. Validation: random versus temporal/spatial cross-validation
4. Interpretability (SHAP) and lead time in yield forecasting
5. Benchmarks and operational systems

### Analysis table
| # | Source | Sci.? | Year | Read? | Data | Method | Metrics / result | Limitations | Take into project |
|---|---|---|---|---|---|---|---|---|---|
| **Theme 1. Drought indices and vegetation response (Kazakhstan, Central Asia)** | | | | | | | | | |
| 1 | Ryssaliyeva L., Salnikov V., Lin Z., Raimbekova Z. Seasonal Sensitivity of Drought Indices in Northern Kazakhstan: A Comparative Evaluation and Selection of Optimal Indicators. Sustainability 17(21), 9413. DOI 10.3390/su17219413 | yes | 2025 | full text | 11 Kazhydromet stations 1990-2024; MODIS MOD13A2/MOD11A2 and Landsat 8 via Google Earth Engine | SPI, SPEI, HTC, MCZI vs VCI, TCI, VHI; Pearson correlation by season and timescale; Modified Mann-Kendall trends | Summer VHI correlates with SPI/MCZI up to r about 0.82 (peak near 9-month scale); SPEI weakly related to vegetation; VHI minimum in 2010 and low again in 2022 | Correlation only, no yield and no prediction model; 11 stations in four regions; authors note transfer needs validation | Use SPI, HTC and VHI/VCI as baseline B3 and for label D3; severe drought years 2010 and 2022 are useful checks; related funded ML early-warning project exists (grant AP25796409), so differentiate |
| 2 | Aitekeyeva N., Li X., Guo H., Wu W., Shirazi Z., Ilyas S., Yegizbayeva A., Hategekimana Y. Drought Risk Assessment in Cultivated Areas of Central Asia Using MODIS Time-Series Data. Water 12(6), 1738 | yes | 2020 | abstract | MODIS 2000-2015, cultivated areas of Central Asia | VCI, TCI, VHI; drought probability and risk mapping | Drought risk map for cultivated land (see paper for quantitative results) | Pre-ML, indices only, ends 2015 | Baseline B3 design; cultivated-area masking |
| 3 | Comparison and Evaluation of Remote Sensing Indices for Agricultural Drought Monitoring over Kazakhstan. ISPRS Archives XLIII-B3-2020, 899 [verify authors] | yes (conference) | 2020 | abstract | Remote sensing drought indices vs soil moisture and 2004-2018 yield | Index comparison | Indices agree only partly with each other and with ground data (see paper) | Index validation only | Supports using several drought labels instead of one |
| 4 | Kogan F., Gitelson A., Zakarin E., Spivak L., Lebed L. AVHRR-Based Spectral Vegetation Index for Quantitative Assessment of Vegetation State and Productivity. Photogramm. Eng. Remote Sens. 69(8), 899-906 | yes | 2003 | abstract | AVHRR, Kazakhstan wheat | VCI versus wheat condition | VCI tracks wheat state (see paper) | Old sensor, coarse | Foundational reference for VCI |
| 5 | Vitkovskaya I., Batyrbayeva M., Asylbekov Z., Mombekova D. [verify title]. Integral vegetation condition index and HTC for drought and grain yield analysis, Kazakhstan. EKTU journal | yes | [verify] | abstract | Kazakh grain regions | IVCI, HTC | See paper | [verify] | Regional index practice |
| **Theme 2. Wheat yield prediction in Kazakhstan** | | | | | | | | | |
| 6 | Alpysbay M., Nurakynov S., Gapparov A., Kaldybayev A. [verify authors]. Crop-Masked Vegetation Indices and TerraClimate for District-Level Wheat Yield Prediction in Kazakhstan: SAVI Advantage, Climate Dominance, and Temporal Transferability Limits. Agronomy 16(13), 1264. DOI 10.3390/agronomy16131264 | yes | 2026 | abstract + intro | 149 districts, 2000-2024, 2,378 district-years, about 390 features: Sentinel-2/Landsat-7, Sentinel-1, TerraClimate, station, soil, terrain | HistGradientBoosting; spatial (GroupKFold) and temporal (expanding window) CV; 10 m cropland masking | Best R2 0.646, RMSE 0.349 t/ha under spatial CV; masking improved index-yield correlation; SAVI beats NDVI from June; SAR adds nothing; Jan-Mar climate gives about 91% of full-year accuracy; temporal transfer is limited | Yield regression only, no drought-event detection, lead time of risk or explanation reported in abstract; district data source to identify | Closest related work: use crop masking, SAVI, both CV types; differentiate by drought detection with lead time, baselines, explanation, dashboard; find where district yields came from |
| 7 | Improve State-Level Wheat Yield Forecasts in Kazakhstan on GEOGLAM's EO Data by Leveraging a Simple Spatial-Aware Technique (ICLR 2023 workshop; arXiv 2306.04646) [verify authors] | yes (workshop) | 2023 | abstract + methods excerpt | GEOGLAM EO data 2001-2020: MODIS NDVI and LAI, temperature, precipitation, SMAP soil moisture, evaporative stress index, wheat crop mask; May-September; 75 samples per province | ML models plus a state-wise additive bias; previous yield and 5-year mean as features | Spatial-aware bias improves state-level forecasts (see paper) | Small sample per province; yield source to identify | Regional bias idea; feature set; check yield data source |
| 8 | Comparative assessment of ML algorithms for forecasting wheat yields using climate indicators and satellite vegetation indices. Eastern-European J. Enterprise Technologies [verify title, authors, year] | yes | [verify] | abstract | Observations 2001-2023; MODIS indices, ERA5 climate; Kazakhstan | Random forest, SVM, CNN and others | Vegetation indices (NDVI, EVI) decisive; combining climate and VI improves ML skill | [verify validation scheme] | Model comparison design |
| 9 | Sadenova M.A., Beisekenov N.A., Varbanov P.S., Kulenova N.A., Abitaev F., Kamenev Y. Digitalization of Crop Production for Transition to Climate-optimized Agriculture Using Spring Wheat in East Kazakhstan as an Example. Chem. Eng. Transactions 96, 415-420. DOI 10.3303/CET2296070 | yes | 2022 | abstract | Nine Sentinel-2/MODIS indices, 5 seasons, one experimental farm (Glubokoe district) | Regression of indices on yield | MAE 3.2-3.7; best R2 0.53 with NDWI, VSDI, NDVI | One farm, five years; very small sample | Illustrates small-sample risk |
| 10 | Sadenova M., Beisekenov N., Varbanov P.S., Pan T. Application of Machine Learning and Neural Networks to Predict the Yield of Cereals, Legumes, Oilseeds and Forage Crops in Kazakhstan [verify venue and year] | yes | 2023 | abstract | East Kazakhstan | ML and neural networks | [verify] | [verify] | Local ML precedent |
| 11 | Machine-learning yield modelling for winter wheat at a farm in East Kazakhstan with NDVI and weather. Chem. Eng. Transactions 88 [verify authors and year] | yes | [verify] | abstract | One farm | SVM and other algorithms | [verify] | Field level | Contrast with regional approach |
| **Theme 3. Validation** | | | | | | | | | |
| 12 | Khechba K., Belgiu M., Laamrani A., Dong Q., Stein A., Chehbouni A. Spatial cross-validation of wheat yield estimations using remote sensing and machine learning. EGU24-22153 | yes (conference abstract) | 2024 | abstract | Wheat yield, province-level and grid blocks (10 and 20 km) | Random CV vs spatial CV at several scales | Spatial CV needed to avoid over-optimistic performance | Conference abstract | Justifies spatial blocks for NFR-02 |
| 13 | Sweet L.-B., Zscheischler J. Using interpretable machine learning to identify compound meteorological drivers of crop failure. EGU22-5464 [verify authors] | yes (conference) | 2022 | presentation | Global reanalysis climate, simulated maize yields | Compares CV strategies and interpretation | Random k-fold overestimates skill; spatial plus temporal CV more representative; CV choice affects interpretations | Simulated yields, maize | Supports leave-one-year-out plus spatial blocks and caution in SHAP interpretation |
| 14 | Deployment-oriented evaluation of stage-specific information for rice yield prediction across years, sites and site-year environments in southern China [verify authors, year] | yes | [verify] | abstract | Multi-year multi-site rice trials | Random 5-fold, leave-one-year, forward-year, leave-one-site, leave-one-site-year | Random record-level CV can overestimate; R2 fell from 0.359 (forward-year) to 0.290 (leave-site) to 0.131 (leave-site-year) | Rice field trials | Evidence that evaluation scheme changes conclusions |
| **Theme 4. Interpretability and lead time** | | | | | | | | | |
| 15 | Joshi A., Pradhan B., Chakraborty S., et al. An explainable Bi-LSTM model for winter wheat yield prediction. Frontiers in Plant Science, Jan 2025 [verify full author list] | yes | 2025 | abstract | Regional winter wheat, multi-source time series | 1D-CNN, LSTM, Bi-LSTM with LIME, Integrated Gradients and SHAP | Bi-LSTM best, R2 up to 0.88; EVI, temperature and precipitation at later stages most important | Not Kazakhstan; DL on larger data | SHAP usage; compare with simple models on small data |
| 16 | Integrating satellite and climate data to predict wheat yield in Australia using machine learning approaches [verify authors, journal, year] | yes | [verify] | abstract | Satellite and climate data, statistical-division level | LASSO benchmark vs SVM, random forest, neural network | ML beats LASSO; R2 about 0.75; climate adds unique information across the season; best skill about two months before maturity | Australia, different climate | Lead-time benchmark and benchmark-model design |
| **Theme 5. Benchmarks and operational systems** | | | | | | | | | |
| 17 | Paudel D., Kallenberg M., Ofori-Ampofo S., Baja H., van Bree R., et al. CY-Bench: a comprehensive benchmark dataset for sub-national crop yield forecasting. Earth System Science Data 18(6), 3997 | yes | 2026 | abstract | Maize (38 countries) and wheat (29 countries), subnational statistics, open (Zenodo, GitHub) | Dataset and benchmark for pre-harvest forecasting | Standardized framework for comparing models | Kazakhstan coverage not verified | Check country list for Kazakhstan; adopt its leakage-controlled protocol |
| 18 | Rojas O. et al. Surveillance of agricultural drought worldwide from space using the FAO Agriculture Stress Index System (ASIS). GAR 2019 contributing paper | semi (agency) | 2019 | abstract | Global VHI-based system | Vegetation Health Index at administrative level | Operational global drought monitoring | Index-based | Existing-system reference (see 04) |
| 19 | Cammalleri C. et al. A revision of the Combined Drought Indicator (CDI) used in the European Drought Observatory. Nat. Hazards Earth Syst. Sci. (2021) | yes | 2021 | abstract | European drought events, 20 years | Cascade Watch-Warning-Alert from precipitation, soil moisture and vegetation anomaly | Revised CDI reproduces major droughts better, especially long events | Europe | Design of the risk-level scheme (FR-05) |

### Critical synthesis
**What is known.** In Northern Kazakhstan, drought is strongly tied to rain-fed wheat, and index comparisons show that precipitation-based indices (SPI, MCZI), HTC and vegetation health indices track each other best in summer, while SPEI links weakly to vegetation (source 1). Different indices disagree, and no single one is accepted as a standard (sources 1, 3). Crop-specific cropland masking and gridded climate data strongly improve yield models in Kazakhstan, SAVI beats NDVI from June, and the district-level model reaches R2 0.646 under spatial cross-validation but transfers poorly across years (source 6). State-level forecasts benefit from a simple spatial-bias term (source 7), and small-sample farm-level studies reach modest skill (sources 9, 11).

**Where approaches disagree.** Deep sequence models can reach R2 up to 0.88 on larger data (source 15), but small regional samples favor simple models; indices disagree among themselves; and reported skill depends heavily on the validation scheme: random cross-validation overestimates skill, while forward-year and leave-site-year evaluation can drop R2 several-fold (sources 12-14).

**What is missing.** The Kazakhstani yield studies found report regression accuracy but, in the parts checked, not (a) drought events as the target with false alarms and lead time, (b) comparison against classical drought-index baselines, (c) retrospective validation on named drought years, or (d) feature-level explanation delivered to the user. This is a gap in the sources checked, not a proof that nothing exists.

**Limitations the project accounts for.** Small sample (regions × years), 2022 boundary changes, different grain-weight definitions across publication years, irrigated south, and the finding that skill early in the season may come largely from climatology and trend, which requires honest baselines (sources 6, 16).

**How the project differs.** It targets early detection of agricultural drought with measured lead time, benchmarks against VCI/SPI/HTC rules and simple yield baselines, validates by year and by region, explains each alert with SHAP, and exposes the result in a dashboard with retrospective view. A funded related project on ML-based drought early warning for Northern Kazakhstan exists (grant AP25796409 cited in source 1), so the project should state its own scope clearly and avoid claiming first-of-kind.

### Implications for the design (feed into 06 and 09)
1. Use cropland masking (Dynamic World or WorldCover) and add SAVI next to NDVI.
2. Report leave-one-year-out and spatial-block validation separately; add forward-year evaluation.
3. Include classical-index baselines (VCI/VHI, SPI, HTC) and a trend baseline.
4. Report lead time and false-alarm rate, not only R2.
5. Use SHAP with the caution that interpretation depends on the CV scheme (source 13).
6. Check CY-Bench for Kazakhstan, and identify the source of the district-level yield series used in source 6.

### Still to do
Open each [verify] item and confirm bibliographic data; read the full text of sources 1, 6, 7, 12, 13; add 2-3 more non-Kazakhstan machine-learning yield forecasting references if the supervisor requests more breadth.

# 4. Existing system comparison

Requirement: 3-5 systems, 6-10 criteria, claims backed by sources. All values below were checked via web search on **22 October 2026**. Where a source did not state something, the cell says "not stated" (this is not the same as "absent"): confirm directly on the platform before final submission. Sources for EOSDA and OneSoil include third-party catalogues (Capterra, GetApp) in addition to vendor pages; vendor pages are preferred.

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


## Technologies (selected)
Python (pandas, scikit-learn, LightGBM, SHAP), Google Earth Engine (`earthengine-api`), Parquet/GeoPackage, FastAPI, Streamlit (map and dashboard). Rationale: [docs/05-technology-comparison.md](docs/05-technology-comparison.md). (to be updated)

## Links
- YouTrack: https://andronousss.youtrack.cloud/projects/DIP
- Documentation: [docs/](docs/), pre-defense checklist: [docs/00-predefense-checklist.md](docs/00-predefense-checklist.md)

## Running and testing
To be added once executable code exists (`make data`, `make train`, `make app`, `pytest`).
