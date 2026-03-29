# Transformation Output Files — Column Reference

All files are saved under:


This document explains each transformed dataset, the rules used, and the meaning of columns.

---

## 1. transformed_cems.csv — Main Enriched Dataset

**Rules Applied:** T1, T2, T3, T4, T5, T9, T10, T11  

This is the **core analytical dataset** built from cleaned CEMS data.

### Columns

| Column | Source | Description |
|--------|--------|------------|
| Plant_ID, Stack_ID | Original | Factory and stack identifier |
| TS | Original | Timestamp (datetime) |
| PM2.5, SO2, NOx | Original | Pollutant concentrations (µg/m³) |
| Flow_Rate_m3_hr | Original | Stack gas flow rate |
| Unit | Original | Always `ug/m3` after cleaning |
| Status | Original | OK, FAULT, MAINT, UNKNOWN |
| Source_Type | Original | Stack or Ambient |
| Exceedance_Flag | Original | Flag from cleaning (R19) |
| Lat, Lon | Original | Geographic coordinates |

### Derived Features

| Column | Rule | Description |
|--------|------|------------|
| Sector | T0 | Joined from `sensor_master` |
| Exceed_PM25 | T1 | PM2.5 exceeds threshold |
| Exceed_SO2 | T1 | SO2 exceeds threshold |
| Exceed_NOx | T1 | NOx exceeds threshold |
| Any_Exceedance | T1 | Any pollutant exceeds threshold |
| AQI_PM25 | T2 | AQI sub-index |
| AQI_SO2 | T2 | AQI sub-index |
| AQI_NOx | T2 | AQI sub-index |
| AQI_Overall | T2 | Maximum AQI |
| AQI_Category | T2 | AQI classification |
| Load_PM25_kg_day | T3 | Emission load |
| Load_SO2_kg_day | T3 | Emission load |
| Load_NOx_kg_day | T3 | Emission load |
| PM25_24h_avg | T4 | Rolling average |
| SO2_24h_avg | T4 | Rolling average |
| NOx_24h_avg | T4 | Rolling average |
| Wind_Contribution_Score | T5 | Wind influence |
| Health_Risk_Score | T9 | Derived risk score |
| Date | T9 | Extracted date |
| Hour | T10 | Hour of day |
| DayOfWeek | T10 | Day name |
| Wind_Speed_kmh | T11 | Meteorology data |
| Wind_Dir_deg | T11 | Wind direction |
| Temp_C | T11 | Temperature |
| Humidity_RH | T11 | Humidity |

---

## 2. compliance_report.csv — Rule T6

One row per plant.

| Column | Description |
|--------|------------|
| Plant_ID | Factory identifier |
| Total_Readings | Number of readings |
| Exceedances | Number of violations |
| Compliance_Rate_Pct | Compliance percentage |

---

## 3. compliance_report_monthly.csv — Rule T6

Monthly compliance tracking.

| Column | Description |
|--------|------------|
| Plant_ID | Factory |
| Month | Year-Month |
| Total_Readings | Monthly readings |
| Exceedances | Monthly violations |
| Compliance_Rate_Pct | Monthly compliance |

---

## 4. penalty_estimate.csv — Rule T19

Penalty estimation based on violations.

| Column | Description |
|--------|------------|
| Plant_ID | Factory |
| Pollutant | Pollutant |
| Hours | Hours exceeded |
| Pct_Over | % above limit |
| Severity | Severity level |
| Fine_INR | Estimated penalty |

---

## 5. episodes.csv — Rule T7

High pollution episodes.

| Column | Description |
|--------|------------|
| Plant_ID, Stack_ID | Location |
| Pollutant | Pollutant |
| Episode_Start | Start time |
| Episode_End | End time |
| Duration_Hours | Duration |
| Peak_Value | Maximum value |

---

## 6. hotspot_ranking.csv — Rule T17

Ranking of polluted locations.

| Column | Description |
|--------|------------|
| Plant_ID | Location |
| Days | Total days |
| Exc_Days | Exceedance days |
| Avg_PM25 | Average PM2.5 |
| Persist_Pct | Persistence |
| Score | Severity score |
| Rank | Rank |

---

## 7. drift_alarms.csv — Rule T13

Sensor drift detection.

| Column | Description |
|--------|------------|
| Plant_ID, Stack_ID | Sensor |
| Pollutant | Pollutant |
| Alarm_Count | Number of alarms |
| Drift_Score | Drift severity |

---

## 8. spatial_grid.csv — Rule T8

Spatial interpolation output.

| Column | Description |
|--------|------------|
| Grid_Lat | Latitude |
| Grid_Lon | Longitude |
| PM25_IDW | Interpolated PM2.5 |

---

## 9. hourly_profile.csv — Rule T10

Hourly averages.

| Column | Description |
|--------|------------|
| Hour | Hour of day |
| PM2.5 | Avg PM2.5 |
| SO2 | Avg SO2 |
| NOx | Avg NOx |

---

## 10. model_features.csv — Rule T16

Machine Learning dataset.

| Column | Description |
|--------|------------|
| PM25_lag_* | Lag features |
| PM25_roll_* | Rolling statistics |
| IsWeekend | Weekend flag |
| HourSin, HourCos | Cyclical encoding |
| Wind_Speed, Temp_C | Weather features |

---

## 11 & 12. regulatory_report_stack.csv & regulatory_report_ambient.csv — Rule T15

Daily summaries.

| Column | Description |
|--------|------------|
| Plant_ID | Location |
| Date | Day |
| PM25_Avg, SO2_Avg, NOx_Avg | Daily averages |
| Readings | Count |
| Exceedances | Violations |
| AQI_Avg | Average AQI |

---

## 13. emission_factor_comparison.csv — Rule T12

Expected vs actual emissions.

| Column | Description |
|--------|------------|
| Sector | Industry |
| Pollutant | Pollutant |
| EF_kg_per_ton | Expected |
| Avg_Measured_Load | Actual |

---

## 14. control_impact.csv — Rule T18

Impact of control measures.

| Column | Description |
|--------|------------|
| Plant_ID, Stack_ID | Sensor |
| Measure | Control equipment |
| Pollutant | Pollutant |
| Pre_Avg | Before control |
| Post_Avg | After control |
| Reduction_Pct | % reduction |

---

## 15. open_data_extract.csv — Rule T20

Public-safe dataset.

### Changes Applied:
- ❌ `Record_ID` removed  
- ❌ Latitude/Longitude rounded  

### Purpose:
- Safe for public sharing
- No sensitive or identifying data
