# Transformation Plan — All 20 Rules

## Overview

Transform cleaned CEMS data into **analysis-ready outputs**.

- **Input:** Cleaned datasets from Phase 2 + reference datasets  
- **Output:** Analytical tables, reports, and ML-ready datasets  
- **Design:** Fully rule-driven transformation pipeline (T1–T20)

---

## New Reference Datasets (No Cleaning Required)

| Node | File | Description |
|------|------|------------|
| 6 | meteorology_data.csv | Hourly weather data |
| 7 | aqi_standards_cpcb.csv | AQI breakpoint table |
| 8 | emission_factors.csv | Emission factors by sector |
| 9 | industry_control_measures.csv | Control equipment logs |
| 10 | compliance_penalty_rules.csv | Penalty rules |

---

## Transformation Rules — Execution Order

### T1: Exceedance Flags
- **Output:**  
  `Exceed_PM25`, `Exceed_SO2`, `Exceed_NOx`, `Any_Exceedance`
- **Logic:**  
  Direct comparison with regulatory thresholds

---

### T4: Rolling 24-Hour Averages
- **Output:**  
  `PM25_24h_avg`, `SO2_24h_avg`, `NOx_24h_avg`
- **Logic:**  
  Rolling window based on 15-minute data

---

### T10: Diurnal Profiles
- **Output:**  
  `hourly_profile.csv`
- **Derived Columns:**  
  `Hour`, `DayOfWeek`
- **Logic:**  
  Aggregation by hour

---

### T2: AQI Computation
- **Output Columns:**
  - `AQI_PM25`
  - `AQI_SO2`
  - `AQI_NOx`
  - `AQI_Overall`
  - `AQI_Category`

---

### T9: Health Risk Index
- **Output Columns:**
  - `Health_Risk_Score`
  - `Date`

---

### T3: Emission Load
- **Output Columns:**
  - `Load_PM25_kg_day`
  - `Load_SO2_kg_day`
  - `Load_NOx_kg_day`

---

### T12: Emission Factor Comparison
- **Output:**  
  `emission_factor_comparison.csv`  
- **Note:**  
  Does NOT modify main dataset

---

### T11: Meteorology Join
- **Output Columns:**
  - `Wind_Speed_kmh`
  - `Wind_Dir_deg`
  - `Temp_C`
  - `Humidity_RH`

---

### T5: Source Contribution
- **Output Column:**
  - `Wind_Contribution_Score`

---

### T6: Compliance Rate
- **Output Files:**
  - `compliance_report.csv`
  - `compliance_report_monthly.csv`

---

### T15: Regulatory Reports
- **Output Files:**
  - `regulatory_report_stack.csv`
  - `regulatory_report_ambient.csv`

---

### T19: Penalty Estimation
- **Output:**
  - `penalty_estimate.csv`

---

### T7: Episode Detection
- **Output:**
  - `episodes.csv`

---

### T13: Drift Detection
- **Output:**
  - `drift_alarms.csv`

---

### T8: Spatial Grid
- **Output:**
  - `spatial_grid.csv`

---

### T17: Hotspot Ranking
- **Output:**
  - `hotspot_ranking.csv`

---

### T16: Model Features
- **Output:**
  - `model_features.csv`

---

### T18: Control Impact
- **Output:**
  - `control_impact.csv`

---

### T14: Gap Analysis
- **Output:**
  - Summary metrics  
- **Note:**  
  Does not depend on GAP_FILLED logic

---

### T20: Open Data Extract
- **Output:**
  - `open_data_extract.csv`

### Data Privacy:
- ❌ Remove `Record_ID`
- ❌ Round Latitude & Longitude

---

## Output Files Summary

| Output File | Rules | Description |
|------------|------|------------|
| transformed_cems.csv | T1–T5, T9–T11 | Main enriched dataset |
| hourly_profile.csv | T10 | Hourly aggregates |
| compliance_report.csv | T6 | Compliance metrics |
| penalty_estimate.csv | T19 | Financial penalties |
| episodes.csv | T7 | Pollution episodes |
| drift_alarms.csv | T13 | Sensor drift |
| spatial_grid.csv | T8 | Spatial interpolation |
| hotspot_ranking.csv | T17 | Ranking of polluted areas |
| model_features.csv | T16 | ML-ready dataset |
| control_impact.csv | T18 | Impact of control measures |
| open_data_extract.csv | T20 | Public-safe dataset |

---

## Summary

This transformation pipeline:

- Converts cleaned data into **analytical outputs**
- Applies **20 structured rules (T1–T20)**
- Produces **compliance, ML, and reporting datasets**
- Ensures **data privacy and regulatory alignment**

---
