# Datasets

One endpoint (`GET https://data.truefixr.com/v1/data`), one `dataset=` param per real
data type below. Every dataset has a free preview (real counts, no account needed) and a
paid full pull (`addresses=true`, requires an API key or an x402 payment). Two batch
endpoints (separate POST routes) sit at the bottom for real portfolio use.

Call the endpoint with no params to get this same information back live, straight from
the API — this file is a snapshot of it for browsing on GitHub.

## storms — TrueFixR

Storms that already happened. Reported and radar-detected events near an address, up to
365 days back.

- Not a property inspection or damage assessment — it means a storm was detected here,
  not what it did to any specific structure.
- Example: `GET /v1/data?dataset=storms&county=Dallas&state=TX&peril=HAIL`
- Free preview: real event counts, storm types, measured severity ranges, repeat-hit
  addresses.
- Paid: the real address-level event records. **$0.05/address.**
- `min_severity=`/`max_severity=` filter by severity value. Some storm types (e.g.
  `THUNDERSTORM_WIND`) carry severity in more than one real, incompatible unit — check
  `severity_by_type` on a free preview first. If it shows `mixed_units:true`, add
  `severity_unit=` (a key under `by_unit`, e.g. `pct` or `mph`) or filtering returns a
  real 400 instead of a silently wrong blend.

## risk — AtlasCast

Storms that might happen. Forecast risk for future leads over the coming days,
probabilistic, refreshed several times a day from NOAA models.

- Example: `GET /v1/data?dataset=risk&county=Dallas&state=TX`
- Free preview: county risk grade, storm types forecast, total real leads in the window.
- Paid: the real address-level forecast records. **$0.05/address.**

## history — TrueFixR

The deep archive. One property, every peril (hail, wind, flood, lightning, tornado),
2003 to present, in a single report. Full report only — there's no hail-only or
wind-only purchase.

- Example: `GET /v1/data?dataset=history&address=891 Lake Hollow Blvd SW, Marietta, GA 30064&radius_mi=5`
- Free preview: the real line count and total price before you pay anything.
- Paid: the full real report, every matched event, by peril, with a reference ID.
- `date_of_loss=YYYY-MM-DD` anchors the report around one specific date — real yes/no on
  whether weather data supports a loss that day, plus real events immediately before and
  after it. This IS the claims-evidence product: `date_of_loss=` plus `report_markdown`
  (a formatted, citable report with a real `reference_id`) covers hail/wind/flood/
  lightning/tornado evidence for a specific loss date. No separate endpoint needed.
- **Pricing: $0.05/real report line, $10.00 minimum per report.**

## hazard_score — TrueFixR

One property's historical hazard profile, by peril — event frequency, most severe event,
most recent event, years since last event. Same 2003-present archive as `history`, not a
new data source — a scored/summarized cut of it.

- Example: `GET /v1/data?dataset=hazard_score&address=891 Lake Hollow Blvd SW, Marietta, GA 30064`
- Free preview: event counts per peril, no account needed.
- Paid: most recent event date + years-since-last-event for every peril. Most severe
  event size and significant-event counts only for perils that carry a real severity/
  magnitude value (hail, wind) — flood/lightning/tornado are count-only, same real
  limitation `history` already has for those perils (no comparable size measure exists in
  the source data, not withheld).
- **Pricing: $0.50/property, flat** — not per line like `history`.

## address — TrueFixR + AtlasCast combined

One property, one call: its own storm history plus that county's current forecast risk
grade.

- Example: `GET /v1/data?dataset=address&lat=32.78&lon=-79.93`
- Free preview: counts and ranges for both the history side and the forecast side.
- Paid: the real event records. Billed once per address regardless of event count.
  **$0.05/address.**

## coverage — always free

Which states and counties actually have data on file. No paid tier — free-only, always.

- Example: `GET /v1/data?dataset=coverage&state=TX`

## weather — TrackCast

Live point-in-time weather for any US coordinate — 164 real HRRR-based fields, hourly
refresh, plus MRMS precip nowcast and NWS severe alerts.

- Example: `GET /v1/data?dataset=weather&lat=31.07&lon=-97.65`
- Free preview: the full 10-field lean response, no account needed — this one is free by
  default.
- Paid: `details=true` for the full 164-field pull. Billed per real call, not per
  address. **$0.0002/call.**
- `hours_ahead=0-48` swaps in a real HRRR forecast instead of current conditions — snaps
  to the nearest real available forecast hour (not hourly-dense across the full range),
  response's `forecast_hour_used` tells you exactly what you got. Forecast snapshots
  carry fewer real fields than current conditions (core conditions plus CAPE/hail/
  lightning/radar, no upper-air/cloud/radiation detail).

## facilities — AtlasCast

Schools, hospitals, and other real facilities currently under an active risk window —
not a static inventory, a facility only appears here while it's actually at risk.

- Example: `GET /v1/data?dataset=facilities&county=Travis&state=TX`
- Free preview: real counts only (total, returned=0), no facility records.
- Paid: the real facility records — name, category, city/state, coordinates,
  risk_label. **$0.05/facility.**

## wildfire — AtlasCast

County-level wildfire risk in the coming days.

- Example: `GET /v1/data?dataset=wildfire&state=CA`
- Free preview: real county count and risk summary, no per-county detail records.
- Paid: the real per-county wildfire risk records. **$0.05/county.**

## at_risk — AtlasCast

Schools and businesses at risk, broken out by peril and forecast window, for future
leads in the coming days.

- Example: `GET /v1/data?dataset=at_risk&state=TX&kind=schools&peril=TORNADO`
- Free preview: real totals per peril/window, no individual records.
- Paid: the real matched school/business records. **$0.05/record.**

## daily — AtlasCast, always free

The daily merged leads digest across all counties — county-level aggregate only
(counts, storm types, dollar exposure), no individual address records. Always free
regardless of `addresses=` — no address-level tier for this one.

- Example: `GET /v1/data?dataset=daily&rank=exposure_value_usd&limit=20`
- `rank=exposure_value_usd` (or `exposure_people`, `new_leads`, `unique_addresses`)
  returns the real top-N counties nationwide by that metric, flat and sorted, instead of
  the full nested by-state blob (688KB unranked). `limit=` sets N, default 2000.

## How billing works

The free preview is the default — no params needed, no account needed. Real counts and
real price shown before you commit to anything. Most datasets unlock the paid pull with
`addresses=true` (`history` uses the same flag) — requires either your own API key as a
Bearer token, or an x402 crypto payment with no account at all. `coverage` and `daily`
have no paid tier — free-only, always.

## Risk scales

Two separate real scales, not yet unified:

- **1-5 numeric** (`risk_tiers`): 1 Low, 2 Moderate, 3 Elevated, 4 High, 5 Extreme.
- **AtlasCast letter grade** (`grade`/`grade_label` on `dataset=risk` and
  `POST /v1/portfolio/risk`): A (no/low risk) through F (extreme risk), 5 distinct grades
  observed live.

## Batch endpoints

Separate POST routes, not `dataset=` values on `/v1/data` — built for real portfolio use
(many locations in one call). Your location list is sent fresh every call, never stored
server-side.

### `POST /v1/weather/batch` — TrackCast

Current conditions + active NWS alerts for every location in one call.

- Max 500 locations per call.
- Body: `{"locations": [{"lat": 30.27, "lon": -97.74, "label": "optional, your own reference"}]}`
- **Pricing: $0.0002/location**, same rate as a single `dataset=weather` call.

### `POST /v1/portfolio/risk` — AtlasCast

Current AtlasCast forecast risk grade for every location in one call — a quick "which of
my properties need attention" scan across a real portfolio.

- Max 500 locations per call.
- Body: `{"locations": [{"lat": 30.27, "lon": -97.74, "label": "optional"}], "window": "24h"}`
- **Pricing: $0.05/location**, same rate as a single `dataset=risk` address pull.
