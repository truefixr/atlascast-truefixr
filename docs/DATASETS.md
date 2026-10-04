# Datasets

One endpoint (`GET` or `POST https://data.truefixr.com/v1/data`), one `dataset=` parameter per data type. Every dataset has a free preview (counts and the exact price, no account) and a paid pull (`addresses=true`, or `details=true` for weather) that needs an API key or an x402 payment. Billing is per real returned record, never per request. Every paid response carries a `reference_id` receipt (format `TFX-YYYYMM-XXXXXXXX`).

Call the endpoint with no params for the live version of this information.

## The five canonical products

### 1. ML Storm Forecast by Address (`dataset=risk`)

Probabilistic, address-level forecasts from per-peril machine-learning models across 10 perils: tornado, hail, wind, flash flood, heavy rain, winter, ice, coastal surge, hurricane and wildfire. 24 hours to 10 days ahead (`window`: `24h`, `48h`, `3d`, `5d`, `7d`, `10d`), refreshed 4 times a day.

- Example: `GET /v1/data?dataset=risk&county=Dallas&state=TX&window=24h&addresses=true&limit=1`
- You get: `structure_value` (USD replacement cost), `people` (estimated occupants), `full_address`, `city`, `state`, `zip`, `county`, `lat`, `lon`, `storm_type`, `forecast_time` (UTC), `risk_label`, `facility_name`, `facility_type`, `storm_hits`. Envelope: exposure totals, `value_basis`, `generated`, `data_age_hours`, `reference_id`.
- Free preview: county, window, a county-level grade (teaser), `total_leads`, per-peril counts, `generated`, `data_age_hours`, `estimated_cost_usd`.
- Price: **$0.05 per address.** `limit` defaults to 2000, max 5000. `peril=` narrows and lowers cost.
- Not: a statement of what will happen, or a prediction of damage to any structure. The county grade is a whole-county label, not a property score.

### 2. Model-Based Weather by Address (`dataset=weather`)

Point-in-time conditions for any U.S. coordinate, with active severe weather alerts and a short-term rain outlook.

- Example: `GET /v1/data?dataset=weather&lat=31.07&lon=-97.65&details=true`
- Free: the lean response (temperature, dewpoint, humidity, wind speed, gust and direction, precipitation and type, pressure, visibility, alerts, rain outlook). No account.
- Paid: `details=true` for the full pull of over 170 variables (hail size, storm rotation, lightning, reflectivity, instability, cloud layers, radiation, winds and temperatures by level, soil, snow, vegetation). **$0.0002 per call.**
- Not: a hazard or damage assessment.

### 3. Model-Based Hourly Forecast by Address (`dataset=weather&hours_ahead=0-48`)

The same dataset with `hours_ahead` (0 to 48) returns forecast hours instead of current conditions. The response reports `forecast_hour_used` and a `forecast_note`. Forecast hours carry about 40 core fields, fewer than current conditions.

- Example: `GET /v1/data?dataset=weather&lat=31.07&lon=-97.65&details=true&hours_ahead=12`
- Price: **$0.0002 per call.**

### 4. Storm History by Address: Reported and Radar-Detected Events Since 2003 (`dataset=history`)

One property's long record: every storm event reported or radar-detected within a chosen radius, 2003 to the present, across 11 categories (hail, wind, flood including heavy rain, lightning, tornado, hurricane and tropical, winter storm, extreme heat and cold, drought, wildfire, coastal and surf). Each line gives date, type, magnitude with units where measured, distance in miles and county. Significant thresholds are defined in the report: hail 0.75 inches, wind 50 mph, winter storm 6 inches.

- Example: `GET /v1/data?dataset=history&address=891 Lake Hollow Blvd SW, Marietta, GA 30064&radius_mi=5`
- Free preview: the line count and total price.
- Paid: the full report with a citable `reference_id`.
- `date_of_loss=YYYY-MM-DD` answers whether an event was reported or radar-detected near the address on that date, and lists events before and after it.
- Price: **$0.05 per event line, $10 minimum per report.**
- Not: a damage assessment. Not proof that a claim is supported or denied.

### 5. Past Storm Leads by Address: Reported and Radar-Detected, Last 365 Days (`dataset=storms`)

Address-level records of storm events reported or radar-detected at or near each address over the trailing 365 days, filterable by county, peril and severity. Sorted by severity, then recency.

- Example: `GET /v1/data?dataset=storms&county=Dallas&state=TX&peril=HEAVY_RAIN&addresses=true&limit=1`
- You get: `full_address`, `city`, `state`, `zip`, `county`, `lat`, `lon`, `storm_type`, `event_time`, `severity`, `severity_unit`, `distance_mi`, `storm_hits`. Envelope: `total_leads`, `returned`, `lead_cap`, `sorted_by`, `reference_id`.
- Free preview: county, state, `total_leads`, per-type counts, `severity_by_type` (min, max, unit), `repeat_addresses`, `estimated_cost_usd`.
- Severity units: some types carry severity in more than one unit. Check `severity_by_type` in the preview. If it shows `mixed_units`, add `severity_unit=` so filtering never blends units.
- Price: **$0.05 per address.**
- Not: a damage assessment, inspection or repair estimate.

## Other datasets

| Dataset | What it is | Price |
|---|---|---|
| `address` | One property, one call: its own reported and radar-detected events over the last 365 days (about 150 m match), with the county forecast label as secondary context | $0.05 per address |
| `hazard_score` | One property's historical hazard profile since 2003 by category: counts, most recent event, years since last event, most severe event for hail, wind and winter. A history summary, not a forecast | $0.50 per property, flat |
| `facilities` | Schools, hospitals and other facilities inside the current forecast footprint, with peril, probability in percent, risk tier and lead days. County and state required | $0.05 per facility |
| `wildfire` | **County-level** wildfire risk, one record per county at risk. Not address-level. A quiet period can return zero counties | $0.05 per county |
| `at_risk` | Schools and businesses in the forecast footprint by peril and lead window (`0-1d`, `1-3d`, `3-7d`, `7-10d`) with probability and change since the last run | $0.05 per record |
| `daily` | County-level daily digest of forecast exposure: counts, storm types, structure replacement cost, people. `rank=` returns the top N counties | Free |
| `coverage` | Which states and counties have data on file | Free |

## How billing works

- Free preview is the default. Add `addresses=true` (weather: `details=true`) for the paid pull.
- Two ways to pay at the same prices: a prepaid API key (`Authorization: Bearer KEY`, minimum top-up $25) or x402 pay-per-call in USDC on Base, no account.
- Per real returned record, never per request. An identical paid query within 24 hours is not re-billed (`receipt_reused: true`).
- `coverage`, `daily` and every free preview are always free.

## Batch endpoints

Separate POST routes. Your location list is sent fresh every call and never stored.

### `POST /v1/weather/batch`

Current conditions and active severe weather alerts for up to 500 locations. **$0.0002 per location.**
Body: `{"locations": [{"lat": 30.27, "lon": -97.74, "label": "optional"}]}`

### `POST /v1/portfolio/risk`

County forecast status for up to 500 locations: each point resolves to its county and returns the county's current forecast grade for the window. This is county-level status, a quick screen of which locations need attention, not an address-level forecast. For address-level records use ML Storm Forecast by Address. **$0.05 per location.**
Body: `{"locations": [{"lat": 30.27, "lon": -97.74, "label": "optional"}], "window": "24h"}`
