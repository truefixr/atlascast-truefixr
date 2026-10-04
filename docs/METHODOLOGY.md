# What you get

## Two views of every address

- **Storm record (products 4 and 5).** Storm History by Address: Reported and Radar-Detected Events Since 2003, and Past Storm Leads by Address: Reported and Radar-Detected, Last 365 Days. Events that already happened near an address.
- **Model outputs (products 1 to 3).** ML Storm Forecast by Address (10 perils, 24 hours to 10 days, refreshed 4 times a day), Model-Based Weather by Address, and Model-Based Hourly Forecast by Address (1 to 48 hours).

Each forecast address record carries the structure replacement value and the estimated people at that address, a risk label, a forecast time and a storm type. Call `GET https://data.truefixr.com/v1/data` with no parameters to see the live list of datasets, fields and prices.

## How the forecast models are built

AtlasCast uses a separate machine-learning model for each of 10 perils. Models are trained and validated chronologically on held-out reported events, so they are scored only on periods they never saw in training. Inputs are restricted to information available at forecast issue time, so no future information leaks into a forecast. Forecasts refresh 4 times a day, are timestamped, versioned and reproducible, and every paid response carries a `reference_id` receipt.

## Data honesty

- Forecasts describe what could happen, not what will happen. A forecast can be wrong.
- A storm reported and radar-detected near an address is not a property inspection, damage assessment or repair estimate.
- `structure_value` is replacement cost (what it costs to rebuild), not market value.
- Not affiliated with NOAA or the NWS.
