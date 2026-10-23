# Early Detection of Agricultural Drought in Kazakhstan

**RU:** Разработка системы раннего обнаружения сельскохозяйственной засухи в Казахстане на основе спутниковых данных и методов машинного обучения
**EN:** Development of a System for Early Detection of Agricultural Drought in Kazakhstan Based on Satellite Data and Machine Learning Methods
**KK:** Спутниктік деректер мен машиналық оқыту әдістері негізінде Қазақстандағы ауыл шаруашылығы құрғақшылығын ерте анықтау жүйесін әзірлеу

## Summary
The system collects satellite (MODIS) and weather data for the regions of Kazakhstan, compares the current season with the historical record, and produces an early drought-risk assessment and a spring wheat yield forecast at several lead times before harvest, with an explanation of each result.

## Problem, users, expected outcome
См. [docs/01-proposal.md](docs/01-proposal.md).

## Team and supervisor
| Andreyev Alexander | <240848@astanait.edu.kz> |

| supervisor: Bakiyeva Aigerim |

## Scope and status
- Region: rain-fed grain regions of northern and central Kazakhstan (irrigated south is out of scope).
- Status: pre-defense 1 stage (analysis and design). Code: skeleton in place, data ingestion in progress.

## Technologies (selected)
Python (pandas, scikit-learn, LightGBM, SHAP), Google Earth Engine (`earthengine-api`), Parquet/GeoPackage, FastAPI, Streamlit (map and dashboard). Rationale: [docs/05-technology-comparison.md](docs/05-technology-comparison.md). (to be updated)

## Links
- YouTrack: https://andronousss.youtrack.cloud/projects/DIP
- Documentation: [docs/](docs/), pre-defense checklist: [docs/00-predefense-checklist.md](docs/00-predefense-checklist.md)

## Running and testing
To be added once executable code exists (`make data`, `make train`, `make app`, `pytest`).
