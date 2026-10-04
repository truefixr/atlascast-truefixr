# Atlas United storm data API: ML forecasts and storm history for every U.S. address

Atlas United builds risk models for every U.S. address (latitude/longitude), with all the information inside each location. This repository documents the API: address-level storm forecasts for the next 24 hours to 10 days, plus every reported storm at that address back to 2003. Pay per address, no contract.

AtlasCast is Atlas United's forecasting model suite: probabilistic, address-level forecasts for 10 perils, refreshed 4 times a day. TrueFixR is the storm-data side: reported and radar-detected storm history by address. Not affiliated with NOAA or the NWS.

## The five products

| # | Product | `dataset=` | Price |
|---|---|---|---|
| 1 | **ML Storm Forecast by Address** | `risk` | $0.05 per address |
| 2 | **Model-Based Weather by Address** | `weather` | Lean response free, $0.0002 per call with `details=true` |
| 3 | **Model-Based Hourly Forecast by Address** | `weather` with `hours_ahead` (0 to 48) | $0.0002 per call |
| 4 | **Storm History by Address: Reported and Radar-Detected Events Since 2003** | `history` | $0.05 per event line, $10 minimum per report |
| 5 | **Past Storm Leads by Address: Reported and Radar-Detected, Last 365 Days** | `storms` | $0.05 per address |

Products 1 to 3 are model outputs. Products 4 and 5 are recorded events matched to addresses.

### 1. ML Storm Forecast by Address

Probabilistic, address-level forecasts for 10 perils (tornado, hail, wind, flash flood, heavy rain, winter, ice, coastal surge, hurricane and wildfire), 24 hours to 10 days ahead, refreshed 4 times a day. AtlasCast uses a separate machine-learning model for each peril. Models are trained and validated chronologically on held-out reported events, and inputs are restricted to information available at forecast issue time.

Each address record carries the structure replacement value and the estimated people at that address, plus storm type, forecast time, a risk label and a past-hit count. Replacement value is the cost to rebuild the structure, not market value. Windows: `24h`, `48h`, `3d`, `5d`, `7d`, `10d`. Filter by county, peril and window. A free preview shows counts, per-peril totals, a county-level grade as a teaser, and the exact cost first. Forecasts describe what could happen, not what will happen.

### 2. Model-Based Weather by Address

Point weather for any U.S. coordinate: current conditions, active severe weather alerts and a short-term rain outlook. The lean response is free with no account. Add `details=true` for the full pull of over 170 variables at $0.0002 per call.

### 3. Model-Based Hourly Forecast by Address

Add `hours_ahead` (0 to 48) to the weather dataset for forecast hours instead of current conditions. The response reports the hour actually used in `forecast_hour_used`. Forecast hours carry a smaller core set of fields. $0.0002 per call.

### 4. Storm History by Address: Reported and Radar-Detected Events Since 2003

One property's long record: every storm event reported or radar-detected within a chosen radius, 2003 to the present, across 11 categories (hail, wind, flood, lightning, tornado, hurricane, winter storm, extreme heat and cold, drought, wildfire, coastal). Each line has event date, type, magnitude where measured, distance in miles and county. Citable, with a `reference_id`. An optional `date_of_loss` mode shows whether an event was reported or radar-detected near the address on a date, and lists events before and after it. A record of reported and radar-detected events, not a damage assessment.

### 5. Past Storm Leads by Address: Reported and Radar-Detected, Last 365 Days

Address-level records of storm events reported or radar-detected at or near each address over the trailing 365 days, filterable by county, peril and severity. Each record has the event time (UTC), type, measured or radar-estimated severity with its unit, distance and a repeat-hit count. A free preview shows counts, severity ranges and the exact cost. Not a damage assessment.

## Access

| Method | Best for |
|---|---|
| [REST API](https://atlasunited.io/api) | Prepaid API key (from $25), `Authorization: Bearer <key>` |
| [MCP server](https://mcp.atlasunited.io/mcp) | Claude, ChatGPT and other MCP-compatible AI clients |
| [x402](https://data.truefixr.com/.well-known/x402.json) | Autonomous AI agents: pay per request in USDC on Base, no account |

Same prices either way. Every dataset has a free preview that shows counts and the exact price before you pay. Every paid response carries a `reference_id` receipt, and an identical paid query within 24 hours is not billed twice.

## Endpoint

```
GET  https://data.truefixr.com/v1/data
POST https://data.truefixr.com/v1/data   (same params as a JSON body, keeps addresses out of URLs)
```

| Param | Description |
|---|---|
| `dataset` | `risk`, `weather`, `history`, `storms`, plus `address`, `hazard_score`, `facilities`, `wildfire` (county-level), `at_risk`, `daily`, `coverage` |
| `state` / `county` | 2-letter state code / county name |
| `peril` | Narrow by peril, for example `HAIL`, `FLASH_FLOOD`, `HEAVY_RAIN`, `TORNADO` |
| `window` | `risk` only: `24h`, `48h`, `3d`, `5d`, `7d`, `10d` |
| `hours_ahead` | `weather` only: 0 to 48 |
| `addresses` | Omit for the free preview. `true` for the paid pull (`weather` uses `details=true`) |
| `limit` | Max records returned (`limit=10` on a $0.05 dataset is a 50 cent sample) |
| `format` | `json` or `csv` |

Call the endpoint with no params for the live machine-readable menu. Full detail per dataset: [Datasets](docs/DATASETS.md).

## Batch endpoints

| Endpoint | Max locations | Price |
|---|---|---|
| `POST /v1/weather/batch` | 500 | $0.0002 per location |
| `POST /v1/portfolio/risk` | 500 | $0.05 per location (county forecast status, not address-level) |

## For AI agents

An agent with no API key and no human in the loop can pay per call with x402:

1. Request a paid pull, for example `GET https://data.truefixr.com/v1/data?dataset=storms&county=Dallas&state=TX&addresses=true&limit=1`.
2. Receive HTTP 402 with the exact price.
3. Sign a USDC payment on Base (`eip155:8453`) and retry with the payment header.
4. Receive the data. No signup, no key.

Manifest: [`/.well-known/x402.json`](https://data.truefixr.com/.well-known/x402.json). Agent docs: [`llms.txt`](https://data.truefixr.com/llms.txt).

## Data honesty

- Forecasts describe what could happen, not what will happen.
- A storm reported or radar-detected near an address is not a property inspection, damage assessment or repair estimate.
- `structure_value` is replacement cost (what it costs to rebuild), not market value.
- Not affiliated with NOAA or the NWS.

## Who this is for

Insurers, adjusters, emergency management, property managers, restoration, solar, and data and AI-agent builders.

## Documentation

- [Datasets](docs/DATASETS.md): every dataset, price, free preview and example
- [Sample data](docs/SAMPLE_DATA.md): trimmed real responses
- [Who this is for](docs/WHO_IS_THIS_FOR.md): use cases by audience
- [What you get](docs/METHODOLOGY.md): the two views of every address, and data honesty
- [Comparison](docs/COMPARISON.md)
- [Code examples](docs/CODE_EXAMPLES.md): curl, Python, JavaScript, x402 and MCP
- [Industry evidence](docs/INDUSTRY_EVIDENCE.md)

## Links

- API docs and signup: https://atlasunited.io/api and https://truefixr.com/api
- MCP server: https://mcp.atlasunited.io/mcp
- Company: https://truefixr.com
