# Sample data

Two kinds of sample, both trimmed.

- **Price quotes** are real answers from the API on 2026-11-06. A data call without payment returns one (HTTP 402), and the price quote is free. The order ID in each card link is shortened here.
- **Paid responses** show the shape of what comes back after payment. In an address record the street text is withheld and the point is rounded to two decimals.

## 1. AtlasCast address forecast (`dataset=risk`)

Price quote:

```
GET https://data.truefixr.com/v1/data?dataset=risk&county=Los+Angeles&state=CA&window=3d
```

```json
{
  "error": "Payment required",
  "quote": {
    "count": 616, "price_usd": 30.8, "card_amount_usd": 30.8,
    "dataset": "risk", "county": "Los Angeles", "state": "CA",
    "total_leads": 616, "quoted_addresses": 616, "estimated_cost_usd": 30.8,
    "sample": {
      "call": "GET /v1/data?dataset=risk&county=Los+Angeles&state=CA&window=3d&addresses=true&limit=10",
      "price_usd": 0.5
    }
  },
  "pay_with_card": {
    "checkout_url": "https://data.truefixr.com/v1/pay/ord_...",
    "amount_usd": 30.8, "exact_price_usd": 30.8,
    "expires_at": "2026-11-06T16:00:33Z"
  },
  "accepts": [
    { "scheme": "exact", "network": "eip155:8453",
      "asset": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913", "amount": "30800000",
      "payTo": "0xFf23cfFf46535E076e88835CD0Bbad8D1033E255" }
  ]
}
```

When no address is in the forecast path, the answer is HTTP 200 at $0.00:

```
GET https://data.truefixr.com/v1/data?dataset=risk&county=Bexar&state=TX&addresses=false
```

```json
{
  "dataset": "risk", "county": "Bexar", "state": "TX",
  "total_leads": 0, "quoted_addresses": 0, "estimated_cost_usd": 0.0
}
```

Paid response, one record (the API's own example for Travis County, TX):

```json
{
  "county": "Travis", "state": "TX", "window": "24h",
  "returned": 1,
  "leads": [
    {
      "full_address": "[street address withheld], Leander, TX 78641",
      "city": "LEANDER", "state": "TX", "zip": "78641", "county": "Travis",
      "lat": 30.56, "lon": -97.91,
      "storm_type": "HEAVY_RAIN",
      "forecast_time": "2026-10-04T12:27:00Z",
      "risk_label": "Extreme",
      "facility_name": null, "facility_type": null,
      "storm_hits": 2
    }
  ]
}
```

Forecasts describe what could happen, not what will happen.

## 2. Point weather (`dataset=weather`)

Price quote. Weather is $0.0002 per call, so by card the smallest order is $0.50:

```
GET https://data.truefixr.com/v1/data?dataset=weather&lat=32.78&lon=-96.80
```

```json
{
  "error": "Payment required",
  "quote": {
    "count": 1, "price_usd": 0.0002, "card_amount_usd": 0.5,
    "dataset": "weather", "estimated_cost_usd": 0.0002,
    "note": "card minimum $0.50; or pay the exact $0.0002 with x402"
  }
}
```

Paid response, 12 hours ahead (a real response from 2026-10-04):

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

## 3. Recent storms near addresses (`dataset=storms`)

Price quote:

```
GET https://data.truefixr.com/v1/data?dataset=storms&state=TX&county=Travis
```

```json
{
  "error": "Payment required",
  "quote": {
    "count": 2000, "price_usd": 100.0, "card_amount_usd": 100.0,
    "dataset": "storms", "county": "travis", "state": "TX",
    "total_leads": 6752, "quoted_addresses": 2000, "estimated_cost_usd": 100.0,
    "sample": {
      "call": "GET /v1/data?dataset=storms&state=TX&county=Travis&addresses=true&limit=10",
      "price_usd": 0.5
    }
  }
}
```

`total_leads` is the count that matches the call. `quoted_addresses` is what this call would return and bill. Add `limit=` to buy fewer.

Paid response, one record (the API's own example for Travis County, TX):

```json
{
  "county": "travis", "state": "TX",
  "returned": 1,
  "sorted_by": "severity desc, then most recent",
  "leads": [
    {
      "full_address": "[street address withheld], Austin, TX 78741",
      "city": "Austin", "state": "TX", "zip": "78741", "county": "travis",
      "lat": 30.24, "lon": -97.73,
      "storm_type": "HEAVY_RAIN",
      "storm_subtype": "Radar-estimated rain",
      "event_time": "2026-10-01 17:42:00",
      "severity": 1.44, "severity_unit": "in",
      "distance_mi": 0.3, "storm_hits": 1
    }
  ]
}
```

A record of reported and radar-detected events, not a damage assessment.

## 4. Storm history near an address (`dataset=history`)

Price quote:

```
GET https://data.truefixr.com/v1/data?dataset=history&address=1100+Congress+Ave%2C+Austin%2C+TX+78701
```

```json
{
  "error": "Payment required",
  "quote": {
    "count": 836, "price_usd": 41.8, "card_amount_usd": 41.8,
    "dataset": "history", "real_line_count": 836, "estimated_cost_usd": 41.8,
    "cost_note": "Report with these filters returns 836 report lines at $0.05 per line ($41.80, no minimum).",
    "options": "Customize with radius_mi (1-25), start/end (YYYY-MM-DD) or years (1-30), perils (comma list: hail, wind, flood, lightning, tornado, hurricane, winter, extreme_temp, drought, wildfire, coastal), max_lines or max_usd (newest lines first).",
    "sample": {
      "call": "GET /v1/data?dataset=history&address=1100+Congress+Ave%2C+Austin%2C+TX+78701&addresses=true&max_lines=10",
      "price_usd": 0.5
    }
  }
}
```

The summary of a property's record is `dataset=hazard_score`. Paid response for one property (a real response from 2026-10-04, address not shown):

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
  }
}
```

The full report is `dataset=history`, $0.05 per report line, no minimum. The summary is $0.50 per property, flat.
