# Sample data

Trimmed real responses from the live API (2026-10-04). Free previews need no key.

## 1. ML Storm Forecast by Address (paid, `dataset=risk`)

```
GET https://data.truefixr.com/v1/data?dataset=risk&county=Dallas&state=TX&window=24h&addresses=true&limit=1
```

```json
{
  "county": "Dallas", "state": "TX", "window": "24h",
  "total_leads": 809, "returned": 1, "lead_cap": 1,
  "exposure_value_usd": 204622180150,
  "exposure_people": 1550820,
  "value_basis": "Replacement cost of structure (not market value)",
  "generated": "2026-10-04T03:32:44.962420Z", "data_age_hours": 3.6,
  "leads": [
    {
      "full_address": "405 Evergreen Trail, Cedar Hill, TX 75104",
      "city": "Cedar Hill", "state": "TX", "zip": "75104", "county": "Dallas",
      "lat": 32.5599, "lon": -96.9493,
      "storm_type": "HEAVY_RAIN",
      "forecast_time": "2026-10-04T12:27:00Z",
      "risk_label": "Extreme",
      "structure_value": 300882,
      "people": 3.5,
      "facility_name": null, "facility_type": null,
      "storm_hits": 2
    }
  ],
  "reference_id": "TFX-202610-6A3B8262"
}
```

`structure_value` is the replacement cost of the structure (what it costs to rebuild), not market value. `people` is the estimated occupancy of the address. Forecasts describe what could happen, not what will happen.

## 2. Model-Based Hourly Forecast by Address (`dataset=weather&hours_ahead`)

```
GET https://data.truefixr.com/v1/data?dataset=weather&lat=31.07&lon=-97.65&details=true&hours_ahead=12
```

```json
{
  "location": { "lat": 31.071398, "lon": -97.661621 },
  "forecast_hour_used": 12,
  "valid_time": "2026-10-04T12:00:00+00:00",
  "alerts": [],
  "temperature_f": 71.0761, "dewpoint_f": 69.9834, "humidity_pct": 96.3,
  "wind_gust_mph": 17.8832, "sustained_wind_mph": 9.3999,
  "precip_in": 0.0091, "visibility_mi": 7.5807, "pressure_mb": 989.4,
  "precip_type": "none",
  "dataset": "weather", "billed": true
}
```

## 3. Past Storm Leads by Address (paid, `dataset=storms`)

```
GET https://data.truefixr.com/v1/data?dataset=storms&county=Dallas&state=TX&peril=HEAVY_RAIN&addresses=true&limit=1
```

```json
{
  "county": "dallas", "state": "TX",
  "total_leads": 1229944, "returned": 1,
  "peril_filter_applied": "HEAVY_RAIN",
  "sorted_by": "severity desc, then most recent",
  "leads": [
    {
      "full_address": "2121 Uhl Rd, , TX 75154", "zip": "75154", "county": "dallas",
      "lat": 32.5476, "lon": -96.8457,
      "storm_type": "HEAVY_RAIN",
      "event_time": "2026-10-01 16:12:00",
      "severity": 2.28, "severity_unit": "in",
      "distance_mi": 1.93, "storm_hits": 1
    }
  ],
  "reference_id": "TFX-202610-0116AECD"
}
```

A record of reported and radar-detected events, not a damage assessment.

## 4. Storm History by Address (`dataset=hazard_score` summary of the same record)

```
GET https://data.truefixr.com/v1/data?dataset=hazard_score&address=891 Lake Hollow Blvd SW, Marietta, GA 30064&addresses=true
```

```json
{
  "dataset": "hazard_score",
  "report_period": "2003-01-01 to present",
  "total_events": 362,
  "by_peril": {
    "hail": { "event_count": 65, "significant_event_count": 61, "significant_threshold": 0.75,
              "significant_threshold_unit": "in", "most_recent_event_date": "2025-06-25",
              "years_since_last_event": 1.28,
              "most_severe": { "date": "2007-06-12", "size": 1.75, "unit": "in", "distance_mi": 2.76 } },
    "tornado": { "event_count": 2, "most_recent_event_date": "2006-04-08", "years_since_last_event": 20.49 }
  },
  "reference_id": "TFX-202610-4290E8CE"
}
```

The full report is `dataset=history`, $0.05 per event line with a $10 minimum. Hazard score is $0.50 per property, flat.

## 5. Free daily digest (`dataset=daily`)

```
GET https://data.truefixr.com/v1/data?dataset=daily&rank=exposure_value_usd&limit=1
```

```json
{
  "dataset": "daily", "ranked_by": "exposure_value_usd", "returned": 1,
  "counties": [
    { "state": "TX", "county": "Harris", "new_leads": 3991238, "unique_addresses": 2248054,
      "exposure_value_usd": 705777196148, "exposure_people": 4428498,
      "storms": [ { "type": "HAIL", "count": 205883 }, { "type": "FLASH_FLOOD", "count": 1665073 } ],
      "has_hot": true }
  ]
}
```

County aggregates only. Exposure is structure replacement cost, not market value.
