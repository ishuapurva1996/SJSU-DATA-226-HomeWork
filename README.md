# DATA 226 Homework Assignments

Homework assignments for DATA 226 at San Jose State University. Each assignment builds an Apache Airflow ETL pipeline that extracts weather data from the [Open-Meteo API](https://open-meteo.com/) and loads it into Snowflake.

## Assignments

### HW5 — Batch Weather ETL (`weather_api_hw5.py`)

A full-refresh ETL pipeline that loads 60 days of historical weather data into Snowflake.

**Pipeline:** `WeatherData_ETL_HW5`
**Schedule:** Daily at 6:10 AM (`10 6 * * *`)
**Target table:** `RAW.Weather_ETL_HW5`

| Task | Description |
|------|-------------|
| `extract` | Fetches 60 days of historical weather data from Open-Meteo for San Jose, CA |
| `transform` | Structures API response into records (lat, lon, date, max/min temp, weather code, city) |
| `load` | Deletes existing records and inserts fresh data into Snowflake with transaction management |

---

### HW6 — Incremental Weather ETL (`hw6_placeholder.py`)

An incremental ETL pipeline that processes one day at a time using Snowflake temporary stages for efficient bulk loading.

**Pipeline:** `weather_ETL_incremental`
**Schedule:** Daily at 3:30 AM (`30 3 * * *`)
**Target table:** `USER_DB_BADGER.raw.weather_data_HW6_inc`

| Task | Description |
|------|-------------|
| `extract` | Fetches weather data for a single day using the Airflow logical date |
| `load` | Saves data to a temp CSV, uploads to a Snowflake stage, and uses `COPY INTO` for bulk loading |

Key improvement over HW5: uses incremental deletes (only removes the date being reloaded) instead of full table refreshes.

---

## Tech Stack

| Layer | Tools |
|-------|-------|
| Orchestration | Apache Airflow |
| Data Warehouse | Snowflake |
| Data Source | Open-Meteo API |
| Language | Python 3 |
| Libraries | `requests`, `pandas`, `snowflake-connector-python` |

## Data Fields

Both pipelines capture the following weather metrics for San Jose, CA:

- `latitude`, `longitude` — location coordinates
- `date` — observation date
- `temperature_2m_max` / `temperature_2m_min` — daily max/min temperature (°C)
- `weather_code` — standardized WMO weather condition code
- `city` — city name label
