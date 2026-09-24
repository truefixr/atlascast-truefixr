# Sample data

Real, live API responses, pulled directly from production on 2026-09-24. Nothing here is
fabricated or illustrative — this is exactly what the API returns.

## 1. Reported storm events (TrueFixR) — free preview

```
GET https://data.truefixr.com/v1/data?dataset=storms&state=TX&county=Travis
```

```json
{
  "county": "travis",
  "state": "TX",
  "total_leads": 4019,
  "storm_types": { "THUNDERSTORM_WIND": 1489, "SLEET": 621, "HAIL": 174, "HEAVY_RAIN": 823, "OTHER": 339, "FLASH_FLOOD": 573 },
  "severity_by_type": {
    "HAIL": { "min": 1.48, "max": 1.5, "unit": "in", "with_severity": 174 }
  },
  "estimated_cost_usd": 200.95,
  "what_this_is": "A storm event was reported or radar-detected at or near this address -- it is not a property inspection, damage assessment, or repair estimate."
}
```

## 2. Reported storm events — address-level (paid)

```
GET https://data.truefixr.com/v1/data?dataset=storms&state=TX&county=Travis&peril=HAIL&addresses=true
```

```json
{
  "leads": [
    {
      "full_address": "12720 Tierra Grande Trl, Austin, TX 78732",
      "city": "Austin", "state": "TX", "zip": "78732", "county": "travis",
      "lat": 30.344501495361328, "lon": -97.91567993164062,
      "storm_type": "HAIL", "storm_subtype": "Hail (Radar Estimated)",
      "event_time": "2026-08-27T12:00:00Z",
      "severity": 1.5, "severity_unit": "in",
      "distance_mi": 0.05, "storm_hits": 1, "fema_flood_zone": null
    }
  ]
}
```

## 3. Forecasted risk with real property values (AtlasCast)

This is the layer that answers "how much is actually exposed" — real replacement-cost
values per address, from the National Structure Inventory (NSI), joined to every address in
the forecast footprint. Live example from Harris County, TX, active coastal surge forecast:

```
GET https://data.truefixr.com/v1/data?dataset=risk&state=TX&county=Harris&addresses=true&window=16d
```

```json
{
  "county": "Harris", "state": "TX", "window": "16d",
  "grade": "B", "grade_label": "Moderate risk",
  "total_leads": 532,
  "exposure_value_usd": 246460749958,
  "exposure_people": 1801523,
  "addresses": 736612,
  "addresses_scope": "addresses inside the forecast footprint for this window (not the whole county)",
  "county_addresses": 2707681,
  "residential_addresses": 2421196,
  "county_exposure_value_usd": 781057866542,
  "county_exposure_people": 4774217,
  "value_basis": "NSI replacement cost of structure (not market value)",
  "matched_structure_value_usd": 90032249,
  "leads": [
    {
      "full_address": "4101 Savell Dr, BAYTOWN, TX 77521",
      "city": "BAYTOWN", "state": "TX", "zip": "77521", "county": "Harris",
      "lat": 29.7730655670166, "lon": -94.91851043701172,
      "storm_type": "COASTAL_SURGE",
      "forecast_time": "2026-09-25T06:10:00Z",
      "risk_label": "Extreme",
      "structure_value": 224569,
      "people": 1.1111111640930176,
      "facility_name": null, "facility_type": null
    }
  ]
}
```

**What `structure_value` actually is:** real replacement cost (what it costs to rebuild the
structure), not market price. Market price = rebuild + land + location premium, which is a
different number. Replacement cost is the number that matters for insurance -- insurers don't
insure the land, they insure what it costs to rebuild what's on it.

## 4. One property, full profile

```
GET https://data.truefixr.com/v1/data?dataset=address&address=12720+Tierra+Grande+Trl&state=TX&county=Travis
```

```json
{
  "dataset": "address", "county": "travis", "state": "TX",
  "storm_event_count": 1, "storm_types": { "HAIL": 1 },
  "current_forecast_risk": { "grade": "A", "grade_label": "No current risk" },
  "severity_by_type": { "HAIL": { "min": 1.5, "max": 1.5, "unit": "in", "with_severity": 1 } }
}
```
