# Early Detection of Agricultural Drought in Kazakhstan

**RU:** Разработка системы раннего обнаружения сельскохозяйственной засухи в Казахстане на основе спутниковых данных и методов машинного обучения
**EN:** Development of a System for Early Detection of Agricultural Drought in Kazakhstan Based on Satellite Data and Machine Learning Methods
**KK:** Спутниктік деректер мен машиналық оқыту әдістері негізінде Қазақстандағы ауыл шаруашылығы құрғақшылығын ерте анықтау жүйесін әзірлеу

## Кратко
Система собирает спутниковые (MODIS) и погодные данные по областям Казахстана, сравнивает текущий сезон с историей и выдаёт раннюю оценку риска засухи и прогноз урожайности яровой пшеницы на разные сроки до уборки, с объяснением результата.

## Проблема, пользователи, ожидаемый результат
См. [docs/01-proposal.md](docs/01-proposal.md).

## Команда и руководитель
| Участник | Роль | GitHub |
|---|---|---|
| <ФИО> | <роль> | <@account> |
| Руководитель: <ФИО> | | |

## Объём и статус
- Регион: богарные зерновые области севера и центра (орошаемый юг вне объёма).
- Статус: этап предзащиты 1 (анализ и проектирование). Код: каркас, выгрузка данных в разработке.

## Технологии (выбранные)
Python (pandas, scikit-learn, LightGBM, SHAP), Google Earth Engine (`earthengine-api`), Parquet/GeoPackage, FastAPI, Streamlit (карта и дашборд). Обоснование: [docs/05-technology-comparison.md](docs/05-technology-comparison.md).

## Ссылки
- YouTrack: <ссылка на проект>
- Документация: [docs/](docs/), чек-лист предзащиты: [docs/00-predefense-checklist.md](docs/00-predefense-checklist.md)

## Запуск и тесты
Будет добавлено, когда появится исполняемый код (`make data`, `make train`, `make app`, `pytest`).
