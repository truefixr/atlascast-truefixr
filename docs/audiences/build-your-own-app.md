# For developers: build your own app or product on this data

Address-level data, cheap enough ($0.05 per address) to build a product on top of.

## Products you can call

- **ML Storm Forecast by Address** (`dataset=risk`): forecasts for 10 perils, 24 hours to 10 days, with structure replacement value and estimated people on every address record.
- **Model-Based Weather by Address** and **Model-Based Hourly Forecast by Address** (`dataset=weather`, `hours_ahead` 0 to 48): free lean response, $0.0002 per call for the full pull.
- **Storm History by Address: Reported and Radar-Detected Events Since 2003** (`dataset=history`): $0.05 per event line, $10 minimum per report.
- **Past Storm Leads by Address: Reported and Radar-Detected, Last 365 Days** (`dataset=storms`): $0.05 per address.

## What you could build

- A storm alert app that notifies owners or property managers when their address enters a forecast window (`dataset=risk`).
- A property overlay for real estate sites using `structure_value` (replacement cost) and the forecast for any address.
- A parametric quoting tool: address-level storm data as the data layer, you build the quoting layer.
- A restoration or field-team tool: `dataset=storms` with `distance_mi` and severity for prioritizing addresses.
- A Discord or Slack bot that pings a channel when a tracked address enters a forecast window.
- An AI agent skill or MCP tool that other agents call through yours.

## The economics

- Wholesale: $0.05 per address, $25 minimum prepaid top-up.
- You set the markup. Subscription, per-report fee or SaaS tier, your call.
- No contract, no sales call. Free previews (omit `addresses=true`) let you prototype before spending anything.

See [INDUSTRY_EVIDENCE.md](../INDUSTRY_EVIDENCE.md) for dated examples of small companies building parametric and storm products on address-level data.

## Start here

- [Code examples](../CODE_EXAMPLES.md)
- [Sample data](../SAMPLE_DATA.md)
- Get a key: [atlasunited.io/api](https://atlasunited.io/api) or [truefixr.com/api](https://truefixr.com/api)

None of this is a damage certification or a property inspection. A reported or forecast event is a signal worth following up on, not proof of loss. Forecasts describe what could happen, not what will happen.
