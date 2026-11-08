# Datasets

One endpoint (`GET` or `POST https://data.truefixr.com/v1/data`), one `dataset=` parameter per data type. A data call without payment returns the price quote first (HTTP 402: the record count, the exact price, a card pay link and x402 payment details). The price quote is free. The paid pull (`addresses=true`) needs an API key, a card payment or an x402 payment. Billing is per returned record on the per-record datasets and per call on weather and coverage.

Call the endpoint with no params for the current menu. The menu is free.

## The main datasets

### 1. AtlasCast address forecast (`dataset=risk`)

A ranked list of the U.S. addresses in a storm's forecast path, for one county and one window. Ten perils: tornado, hail, wind, flash flood, heavy rain, winter, ice, coastal surge, hurricane, and wildfire by county. Six windows, from the next 24 hours to 10 days (`window`: `24h`, `48h`, `3d`, `5d`, `7d`, `10d`), refreshed 4 times a day. Each window is its own list, and a county's list is capped.

- Example: `GET /v1/data?dataset=risk&county=Dallas&state=TX&window=24h&addresses=true&limit=1`
- You get: `full_address`, `city`, `state`, `zip`, `county`, `lat`, `lon`, `storm_type`, `forecast_time` (UTC), `risk_label`, `facility_name`, `facility_type`, `storm_hits`. Envelope: `window`, `generated`, `data_age_hours`, `reference_id`.
- Price quote (HTTP 402): `total_leads`, `quoted_addresses` and `estimated_cost_usd`. The county's A to F grade and its counts by storm type are a County Summary (`summary=true`, $0.02 per county).
- Price: **$0.05 per address.** `limit` caps the records returned and billed (`limit=10` is a 50 cent sample). `peril=` narrows the list.
- Not: a statement of what will happen, or a prediction of damage to any structure. The A to F grade is a whole-county label, not a property score. Each listed address carries its own risk label.

### 2. Point weather (`dataset=weather`)

Current conditions at a point in the continental U.S., with active severe weather alerts and a short-term rain outlook.

- Example: `GET /v1/data?dataset=weather&lat=31.07&lon=-97.65&details=true`
- You get: temperature, dewpoint, humidity, wind speed, gust and direction, precipitation and type, pressure, visibility, alerts and the rain outlook. `details=true` returns the full set of fields, with upper-air, cloud and radiation detail, at the same price.
- Price: **$0.0002 per call**, with or without `details=true`. A call without payment returns the price quote, not the weather. By card the smallest order is $0.50; x402 pays the exact $0.0002.
- Not: a hazard or damage assessment.

### 3. Point weather, 0 to 48 hours ahead (`dataset=weather&hours_ahead=0-48`)

The same dataset with `hours_ahead` (0 to 48) returns a forecast hour in place of current conditions. The response reports the hour it used in `forecast_hour_used`. Forecast hours carry fewer fields than current conditions.

- Example: `GET /v1/data?dataset=weather&lat=31.07&lon=-97.65&details=true&hours_ahead=12`
- Price: **$0.0002 per call.**

### 4. Storm history near an address (`dataset=history`)

The storm record near one U.S. address back to 2003: the events reported or detected within a chosen radius, in 11 event categories (hail, wind, flood including heavy rain, lightning, tornado, hurricane and tropical, winter storm, extreme heat and cold, drought, wildfire, coastal and surf). Each line gives date, type, magnitude with units where measured, distance in miles and county. Significant thresholds are defined in the report: hail 0.75 inches, wind 50 mph, winter storm 6 inches.

- Example: `GET /v1/data?dataset=history&address=1100 Congress Ave, Austin, TX 78701&radius_mi=5`
- Price quote (HTTP 402): the line count and the total price. Filters: `radius_mi`, `start`/`end` or `years`, `perils`, `max_lines` or `max_usd`.
- Paid: the full report.
- `date_of_loss=YYYY-MM-DD` shows whether an event was reported or detected near the address on that date, and lists the events before and after it. Send the address and the date in a `POST` body to keep them out of URLs.
- Price: **$0.05 per report line, no minimum.** A search that finds nothing is 5 cents.
- Not: a damage assessment. It does not validate a claim.

### 5. Recent storms near addresses (`dataset=storms`)

Addresses where a storm was reported or radar-detected nearby in the last 365 days, filterable by county, peril and severity. Sorted by severity, then recency.

- Example: `GET /v1/data?dataset=storms&county=Dallas&state=TX&peril=HEAVY_RAIN&addresses=true&limit=1`
- You get: `full_address`, `city`, `state`, `zip`, `county`, `lat`, `lon`, `storm_type`, `event_time`, `severity`, `severity_unit`, `distance_mi`, `storm_hits`. Envelope: `total_leads`, `returned`, `lead_cap`, `sorted_by`, `reference_id`.
- Price quote (HTTP 402): `total_leads`, `quoted_addresses` and `estimated_cost_usd`. Counts by storm type are in the County Summary (`summary=true`, $0.02 per county).
- Severity units: some types carry severity in more than one unit. Check `severity_by_type` in the County Summary. If it shows `mixed_units`, add `severity_unit=` so filtering never blends units.
- Price: **$0.05 per address.**
- Not: a damage assessment, inspection or repair estimate.

## Other datasets

| Dataset | What it is | Price |
|---|---|---|
| `risk` or `storms` with `summary=true` | County Summary. On `risk`: the county's A to F forecast grade and counts by storm type for the window. On `storms`: counts by storm type | $0.02 per county |
| `address` | One property, one call: the events reported or radar-detected near it over the last 365 days (about 150 m match), with the county forecast label as secondary context | $0.05 per address |
| `hazard_score` | Summary by event category of the storm history near one property since 2003: counts, most recent event, years since last event, most severe event for hail, wind and winter. A history summary, not a forecast | $0.50 per property, flat |
| `facilities` | Facilities inside an active forecast window, with peril, probability in percent, risk tier and lead days. County and state required | $0.05 per facility |
| `wildfire` | **County-level** wildfire risk, one record per county at risk. Not address-level. A quiet period can return zero counties | $0.05 per county |
| `at_risk` | Schools and businesses in the forecast path by peril and lead window (`0-1d`, `1-3d`, `3-7d`, `7-10d`) with probability and change since the last run | $0.05 per record |
| `daily` | County-level daily digest of the forecast: addresses in the forecast and storm types, by county. `rank=unique_addresses` with `limit=N` returns the top N counties | $0.02 per county |
| `coverage` | Which states and counties have data on file | $0.0002 per call |

## How billing works

- A data call without payment returns the price quote (HTTP 402). The price quote is free. Add `addresses=true` for the paid pull. Weather is a paid call with or without `details=true`.
- Three ways to pay, at the same prices: by card from the link in the quote, with a prepaid API key (`Authorization: Bearer KEY`), or with x402 pay-per-call in USDC on Base, no account.
- Per returned record on the per-record datasets, per call on weather and coverage.
- Free: the menu (`GET /v1/data` with no params) and the price quote. A forecast or recent-storms call that finds nothing answers HTTP 200 at $0.00. A storm history search that finds nothing is 5 cents.
- Card payments under $0.50 are charged $0.50, and the rest can be kept as key credit. x402 pays the exact amount.
- License: for your own use. No resale, redistribution or AI training; bulk, archive and training rights are licensed separately ([terms](https://atlasunited.io/terms)).

## Batch endpoints

Separate POST routes. Your location list is sent fresh every call and never stored.

### `POST /v1/weather/batch`

Current conditions and active severe weather alerts for up to 500 locations. **$0.0002 per location.**
Body: `{"locations": [{"lat": 30.27, "lon": -97.74, "label": "optional"}]}`

### `POST /v1/portfolio/risk`

County forecast status for up to 500 locations: each point resolves to its county and returns the county's current forecast grade for the window. This is county-level status, a quick screen of which locations need attention, not an address-level forecast. For address-level records use the AtlasCast address forecast (`dataset=risk`). **$0.05 per location.**
Body: `{"locations": [{"lat": 30.27, "lon": -97.74, "label": "optional"}], "window": "24h"}`
