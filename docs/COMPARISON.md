# How this compares to other storm/property-risk data providers

Real competitors found and checked (2026-09-24), not a cherry-picked list. Every real
alternative found does ONE half of what this API does, at ONE resolution, and every
insurance-grade one requires an enterprise sales process before you see a row of data.

| Provider | Historical events? | Forecast risk? | Address-level? | Self-serve? |
|---|---|---|---|---|
| **This API (TrueFixR + AtlasCast)** | **Yes** | **Yes** | **Yes** | **Yes, $0.05/address** |
| Meteomatics | No | Yes | Grid, not address | No -- enterprise contract |
| Reask | No | Yes (cat risk) | No | No -- enterprise |
| KatRisk | No | Yes (modeled) | No | No -- enterprise |
| The Weather Company | Some | Yes | No | No -- enterprise |
| Verisk (Respond) | Yes | Limited | Varies | No -- enterprise |
| ZestyAI (Z-STORM) | No | Yes (modeled) | Property-level (via other data) | No -- enterprise |
| Swath API | Yes, historical only | No | Yes | Yes, API |
| PerilIQ | Yes, historical only | No | Address-level | Yes, API |
| Opterrix | Yes | Limited | Address-specific reports | API, unclear self-serve |
| HailTrace / HailWatch / THOR | Yes, contractor-focused | No | Varies | Subscription, $65-119/mo |
| PlainHazard | Yes | Limited (annualized) | County-level only | Free/public data |
| FEMA National Risk Index | No | Yes, annualized | Census tract/county | Free/public |

## The real gap this fills

Nobody in the list above combines reported historical events **and** a rolling forward
forecast, at address-level resolution, buyable without an enterprise sales cycle. That's not
a marketing claim -- it's the actual result of checking every real alternative found.

## What this API does NOT claim

- Not a damage certification or property inspection.
- Not a climate attribution model.
- Two of the ten peril models (FLASH_FLOOD, HEAVY_RAIN) have self-reported, not independently
  reverified accuracy -- see [METHODOLOGY.md](METHODOLOGY.md).
- `structure_value` is replacement cost, not market price.
