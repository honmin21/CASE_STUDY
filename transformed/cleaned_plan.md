# Data Distribution & Cleaning Strategy

## Overview

This project is based on a real-world data pipeline with:

- **10 Sensors**
- **15-minute intervals**
- **28,800 rows per month**  
  (Calculation: 10 × 4 × 24 × 30 ≈ 28,800)

This ensures a realistic high-volume streaming dataset.

---

## 1. raw_cems_data.csv (Core Input Stream)

### Volume
- ~28,800 rows per month
- Continuous, real-time data stream

### Columns & Data Behavior

#### Record_ID
- Format: `E00001` → `E28800`
- Unique for every row

#### Plant_ID & Stack_ID
- Example:
  - Plant: `PL-01`, `LOC-DEL-01`
  - Stack: `S-01`, `A-01`

#### Flow_Rate_m3_hr
- Range: **1000 – 5000**
- **NaN for ambient sensors**

---

### TS (Timestamp) — Data Quality Issues

| Type | Percentage | Example | Rule |
|------|----------|--------|------|
| Clean | 90% | `14-03-2026 12:15` | — |
| Edge Case | 5% | `12:24:10` | R1 |
| UTC | 5% | `UTC 12:15` | R17 |

---

### Pollutants (PM2.5, SO2, NOx)

| Issue | Percentage | Example | Rule |
|------|----------|--------|------|
| Normal | 85% | 15.5 – 400.0 | — |
| Negative | 5% | -5.0 | R5 |
| BDL / Text | 5% | `<2.0`, `BDL` | R11 |
| Spikes | 3% | 9999.0 | R8 |
| Missing | 2% | NaN | Filled later |

---

### Unit

| Type | Percentage | Example | Rule |
|------|----------|--------|------|
| Clean | 80% | `ug/m3` | — |
| Unicode | 10% | `µg/m³` | R2 |
| Alternative | 10% | `mg/Nm3` | R14 |

---

### Status

| Type | Percentage | Example | Rule |
|------|----------|--------|------|
| Clean | 70% | `OK`, `FAULT`, `MAINT` | — |
| Dirty | 30% | `" ok "`, `offline`, `Error 404` | R3, R12 |

---

### Lat_Lon

| Type | Percentage | Example | Rule |
|------|----------|--------|------|
| Clean | 98% | `13.0827, 80.2707` | — |
| Invalid | 2% | `-999`, malformed values | R4 |

---

## 2. sensor_master.csv

- Total rows: **10**
- Purpose: Reference dataset

### Used in:
- Calibration (R7)
- BDL handling (R11)
- Mapping validation (R13)
- Source tagging (R15)

---

## 3. maintenance_logs.csv

- ~30 rows/month

### Important Behavior

✔ **Rows are NOT deleted**  
✔ Instead:
- Pollutants → `NULL`
- Status → `MAINT`

---

## 4. manual_entries.csv

- ~50 rows/month

### Behavior

✔ Data is **not stopped**  
✔ QC logic applied:

- If difference ≤ 1% → `QC_PASS`
- If difference > 1% → `QC_FAIL`

---

## 5. regulatory_thresholds.csv

- Exactly 6 rows
- Defines legal pollutant limits

---

## Why This Design Matters

This dataset intentionally introduces controlled "messiness" to:

- Trigger all cleaning rules (R1–R19)
- Simulate real-world data problems
- Validate a full data quality pipeline

---

## Summary

This dataset demonstrates:

- High-volume data (~28,800 rows/month)
- Realistic data corruption scenarios
- End-to-end cleaning and validation pipeline
- Strong auditability and traceability

---
