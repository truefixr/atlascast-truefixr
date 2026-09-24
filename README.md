# AtlasCast + TrueFixR

Nationwide API for **reported severe weather events** (hail, wind, flood) and **forecasted
property-level storm risk**, delivered at the **address level**, self-serve.

- **TrueFixR** — reported and radar-detected storm events, up to 365 days back, refreshed
  every 15 minutes.
- **AtlasCast** — forecasted property risk, up to 10 days ahead, refreshed 4x daily, built on
  NOAA's calibrated outlook data plus our own trained models for perils NOAA doesn't cover.

One nationwide dataset, one API, address-level resolution — not county shapes.

## Quick facts

- 282,904,029 real US addresses covered
- Address-level, not county/ZIP-level
- Both historical (what already hit) and forecast (what might hit next) in one place
- Self-serve: $0.05/address, $25 minimum, no contract, no sales call
- Live in minutes

## Access

| Method | Best for |
|---|---|
| [REST API](https://atlasunited.io/api) | Prepaid API key, `Authorization: Bearer <key>` |
| [MCP server](https://mcp.atlasunited.io/mcp) | Claude, ChatGPT, and other MCP-compatible AI clients |
| [x402](https://truefixr.com/.well-known/x402.json) | Autonomous AI agents — pay per request in USDC on Base network, no account needed |

## For AI agents

This API is **x402-payable**. An agent with no API key and no human in the loop can:

1. Call `GET https://data.truefixr.com/v1/data?dataset=storms&addresses=true&...`
2. Receive an HTTP 402 with a real payment manifest in the `payment-required` header
3. Pay the listed USDC amount on Base network to the listed address
4. Retry the same request with an `X-Payment` header
5. Receive the real data — no signup, no email, no waiting

Machine-readable payment manifest: [`/.well-known/x402.json`](https://truefixr.com/.well-known/x402.json)
Full agent-facing docs: [`llms.txt`](https://truefixr.com/llms.txt)

Free previews (`addresses=false`) require no payment or key at all.

## Endpoint

```
GET https://data.truefixr.com/v1/data
```

| Param | Description |
|---|---|
| `dataset` | `storms` (reported), `risk` (forecast), `address`, `coverage`, `facilities`, `wildfire`, `at_risk`, `daily` |
| `state` / `county` | 2-letter state code / county name |
| `peril` | `HAIL`, `THUNDERSTORM_WIND`, `FLASH_FLOOD`, `HEAVY_RAIN`, `FLOOD`, `TORNADO` |
| `min_severity` / `max_severity` | e.g. `1.5` = 1.5in hail |
| `addresses` | `false` = free preview (counts, severities, cost estimate). `true` = real address-level records, requires payment. |
| `limit` | Max records returned |
| `format` | `json` or `csv` |

## Data honesty

A storm event being reported or radar-detected near an address is **not** a property
inspection, damage assessment, or repair estimate. Forecast risk is probabilistic, not a
certainty. Treat this as lead/exposure data — a signal worth following up on, not a
certification of loss.

## Who this is for

Insurance underwriting, catastrophe risk, claims, MGAs, reinsurance, emergency management,
disaster response, property research, restoration, and any AI agent building its own
workflow around real-time or forecast property risk.

## Links

- API docs & signup: https://atlasunited.io/api
- Demo map: https://atlasunited.io/atlascast/
- MCP server: https://mcp.atlasunited.io
- Company: https://truefixr.com
