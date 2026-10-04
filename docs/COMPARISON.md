# How this compares to other storm and property-risk data providers

Providers checked 2026-09-24. Every alternative found does one half of what this API does, at one resolution, and the insurance-grade ones need an enterprise sales process before you see a row of data.

| Provider | Historical events? | Forecast risk? | Address-level? | Self-serve? |
|---|---|---|---|---|
| **This API (ML Storm Forecast by Address plus Storm History by Address)** | **Yes** | **Yes** | **Yes** | **Yes, $0.05 per address** |
| Meteomatics | No | Yes | Grid, not address | No, enterprise contract |
| Reask | No | Yes (cat risk) | No | No, enterprise |
| KatRisk | No | Yes (modeled) | No | No, enterprise |
| The Weather Company | Some | Yes | No | No, enterprise |
| Verisk (Respond) | Yes | Limited | Varies | No, enterprise |
| ZestyAI (Z-STORM) | No | Yes (modeled) | Property-level (via other data) | No, enterprise |
| Swath API | Yes, historical only | No | Yes | Yes, API |
| PerilIQ | Yes, historical only | No | Address-level | Yes, API |
| Opterrix | Yes | Limited | Address-specific reports | API, unclear self-serve |
| HailTrace / HailWatch / THOR | Yes, contractor-focused | No | Varies | Subscription, $65-119 per month |
| PlainHazard | Yes | Limited (annualized) | County-level only | Free |
| Public county-level risk indexes | No | Yes, annualized | Tract or county | Free |

## The gap this fills

Reported historical events and a rolling forward forecast, at address-level resolution, buyable without an enterprise sales cycle.

## What this API does not claim

- Not a damage certification or property inspection.
- Not a climate attribution model.
- Forecasts describe what could happen, not what will happen.
- `structure_value` is replacement cost, not market value.
