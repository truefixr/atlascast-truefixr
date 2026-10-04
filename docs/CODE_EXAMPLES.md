# Code examples

Snippets for the live API. Products: ML Storm Forecast by Address (`risk`), Model-Based Weather by Address (`weather`), Model-Based Hourly Forecast by Address (`weather` with `hours_ahead`), Storm History by Address (`history`) and Past Storm Leads by Address (`storms`). Replace `YOUR_API_KEY` with a real key from
[atlasunited.io/api](https://atlasunited.io/api) or [truefixr.com/api](https://truefixr.com/api).

## Free preview (no key needed): Past Storm Leads by Address

### curl

```bash
curl "https://data.truefixr.com/v1/data?dataset=storms&state=TX&county=Travis"
```

### Python

```python
import requests

r = requests.get(
    "https://data.truefixr.com/v1/data",
    params={"dataset": "storms", "state": "TX", "county": "Travis"},
)
print(r.json())
```

### JavaScript

```javascript
const res = await fetch(
  "https://data.truefixr.com/v1/data?dataset=storms&state=TX&county=Travis"
);
const data = await res.json();
console.log(data);
```

## Paid pull: Past Storm Leads by Address ($0.05 per address)

### curl

```bash
curl "https://data.truefixr.com/v1/data?dataset=storms&state=TX&county=Travis&peril=HAIL&addresses=true" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### Python

```python
import requests

r = requests.get(
    "https://data.truefixr.com/v1/data",
    params={"dataset": "storms", "state": "TX", "county": "Travis", "peril": "HAIL", "addresses": "true"},
    headers={"Authorization": "Bearer YOUR_API_KEY"},
)
print(r.json())
```

### JavaScript

```javascript
const res = await fetch(
  "https://data.truefixr.com/v1/data?dataset=storms&state=TX&county=Travis&peril=HAIL&addresses=true",
  { headers: { Authorization: "Bearer YOUR_API_KEY" } }
);
const data = await res.json();
console.log(data);
```

## ML Storm Forecast by Address ($0.05 per address)

Each record carries `structure_value` (replacement cost, not market value) and `people`. Add `limit=10` for a 50 cent sample.

```bash
curl "https://data.truefixr.com/v1/data?dataset=risk&state=TX&county=Harris&addresses=true&window=10d" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

## Model-Based Hourly Forecast by Address ($0.0002 per call)

```bash
curl "https://data.truefixr.com/v1/data?dataset=weather&lat=31.07&lon=-97.65&details=true&hours_ahead=12" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

Model-Based Weather by Address (current conditions) is the same call without `hours_ahead`. The lean response is free.

## Storm History by Address ($0.05 per event line, $10 minimum)

```bash
curl "https://data.truefixr.com/v1/data?dataset=history&address=891+Lake+Hollow+Blvd+SW,+Marietta,+GA+30064&radius_mi=5"
```

The free preview returns the line count and exact price. Add `addresses=true` and your key for the full report.

## Autonomous AI agent payment (x402, no API key)

```python
# No account, no signup.
import requests

url = "https://data.truefixr.com/v1/data?dataset=storms&state=TX&county=Travis&addresses=true"
r = requests.get(url)

if r.status_code == 402:
    quote = r.json()  # accepts[]: scheme, network (eip155:8453), asset, amount, payTo
    # Sign an "exact" scheme USDC payment on Base for that amount,
    # then retry with a payment header:
    # r = requests.get(url, headers={"X-PAYMENT": signed_payload})
    print(quote)
```

Full machine-readable payment manifest: [`/.well-known/x402.json`](https://data.truefixr.com/.well-known/x402.json)

## MCP (Claude, ChatGPT, other MCP clients)

Add as a connector: `https://mcp.atlasunited.io/mcp`

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/call",
  "params": {
    "name": "query_weather_data",
    "arguments": { "dataset": "storms", "state": "TX", "county": "Travis" }
  }
}
```
