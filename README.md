# Atlas United API: AtlasCast U.S. Storm Forecasts & Storm History Since 2003

Before a storm arrives, someone has to decide what to bind, who to warn and where to send crews. Atlas United shows which U.S. addresses are in a storm's forecast path, up to ten days ahead. It grades each county A to F and ranks the addresses across ten perils, refreshed four times a day, built on AEPM, our patent-pending hazard model. The same API returns the storm record near a U.S. address back to 2003.

## Datasets & Pricing

- **Menu (free, HTTP 200):** `GET https://data.truefixr.com/v1/data`
- **Price quote (free, HTTP 402):** Call a dataset without credentials to receive the exact record count, exact USD price, a Stripe card link ($0.50 processor minimum), and x402 payment parameters (USDC on Base).
- **Prepaid API key:** Get a key at https://atlasunited.io/api and pass `Authorization: Bearer <key>`.
- **Remote MCP server:** `https://mcp.atlasunited.io/mcp`

| Dataset | Parameter / Route | What Returns | Price |
|---|---|---|---|
| **AtlasCast County Summary** | `dataset=risk&summary=true` | County A-to-F forecast grade and peril counts (24h to 10d) | `$0.02` per county |
| **AtlasCast Address Forecast** | `dataset=risk` | Ranked U.S. addresses in a storm's forecast path (24h to 10d, ten perils; county list capped) | `$0.05` per address |
| **Portfolio Risk Check** | `POST /v1/portfolio/risk` | Up to 500 U.S. coordinates per call, each with the A-to-F grade of the county it sits in | `$0.05` per location |
| **Schools & Businesses in Path** | `dataset=at_risk` | Schools or businesses inside the forecast path (`kind=schools` or `businesses`) | `$0.05` per location |
| **Storm History Near an Address** | `dataset=history` | Storm events near a U.S. address back to 2003, with an optional `date_of_loss` check | `$0.05` per report line, no minimum |
| **Storm History Summary** | `dataset=hazard_score` | Summary by event category of the storm history near one U.S. property since 2003 | `$0.50` per property |
| **Facilities in the Forecast Path** | `dataset=facilities` | Facilities inside an active forecast window | `$0.05` per record |
| **Wildfire Risk by County** | `dataset=wildfire` | Counties at wildfire risk in the coming days (county-level) | `$0.05` per county |
| **Daily Forecast Digest** | `dataset=daily` | County-level digest of the day's forecast | `$0.02` per county |
| **365-Day Storm Records** | `dataset=storms` / `dataset=address` | Addresses where storms were reported or radar-detected nearby in the last 365 days | `$0.05` per address |
| **Continental U.S. Weather (0 to 48h)** | `dataset=weather` | Point weather and active NWS alerts (`hours_ahead=0..48`, continental U.S. only) | `$0.0002` per call |

## Documentation

- [Datasets](docs/DATASETS.md): every dataset, its parameters and its price.
- [Sample data](docs/SAMPLE_DATA.md): real price quotes and example records.
- [Code examples](docs/CODE_EXAMPLES.md): curl, Python, x402 and MCP.
- [Who is this for](docs/WHO_IS_THIS_FOR.md): the audiences, one page each.

## Disclaimers & License

Forecasts show what could happen, not what will. Storm reports show where a storm was reported or radar-detected near an address, not a damage assessment. Hurricane historical coverage is partial, and wildfire is county-level. Not affiliated with NOAA or the National Weather Service.

Code snippets in this repository are MIT-licensed. All API and MCP data responses are governed by https://atlasunited.io/terms (for your own internal use only; no resale, redistribution, or AI/ML model training without a separate written license).
