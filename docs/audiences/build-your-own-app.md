# For developers: build your own app or product on this data

Address-level data, priced per call ($0.05 per address), for your software or your AI agent.

## Datasets you can call

- **AtlasCast address forecast** (`dataset=risk`): a ranked list of the addresses in a storm's forecast path, for 10 perils (wildfire by county), 24 hours to 10 days. $0.05 per address.
- **Point weather** (`dataset=weather`, with `hours_ahead` 0 to 48 for forecast hours): $0.0002 per call.
- **Storm history near an address** (`dataset=history`): $0.05 per report line, no minimum.
- **Recent storms near addresses** (`dataset=storms`): $0.05 per address.

## What you could build

- A storm alert app that notifies owners or property managers when their address enters a forecast window (`dataset=risk`).
- A Discord or Slack bot that pings a channel when a tracked address enters a forecast window.
- An AI agent skill or MCP tool that other agents call through yours.

## The economics

- Pay per call: $0.05 per address or report line, with a prepaid API key, by card, or with x402.
- No contract, no sales call. Every data call shows its exact price first (omit `addresses=true`), and `limit=10` is a 50 cent sample.
- The data is licensed for your own use. Resale, redistribution and AI training need a separate license ([terms](https://atlasunited.io/terms)).

## Start here

- [Code examples](../CODE_EXAMPLES.md)
- [Sample data](../SAMPLE_DATA.md)
- Get a key: [atlasunited.io/api](https://atlasunited.io/api)

None of this is a damage certification or a property inspection. A reported or forecast event is a signal worth following up on, not proof of loss. Forecasts describe what could happen, not what will happen.
