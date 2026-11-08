# Code examples

Snippets for the API. Datasets: AtlasCast address forecast (`risk`), point weather (`weather`, with `hours_ahead` for 0 to 48 hours ahead), storm history near an address (`history`) and recent storms near addresses (`storms`). Replace `YOUR_API_KEY` with a real key from
[atlasunited.io/api](https://atlasunited.io/api).

## Price quote (free, no key needed, HTTP 402): recent storms near addresses

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

## Paid pull: recent storms near addresses ($0.05 per address)

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

## AtlasCast address forecast ($0.05 per address)

Add `limit=10` for a 50 cent sample.

```bash
curl "https://data.truefixr.com/v1/data?dataset=risk&state=TX&county=Harris&addresses=true&window=10d" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

## Point weather, 12 hours ahead ($0.0002 per call)

```bash
curl "https://data.truefixr.com/v1/data?dataset=weather&lat=31.07&lon=-97.65&details=true&hours_ahead=12" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

Current conditions are the same call without `hours_ahead`. Both are $0.0002 per call, with or without `details=true`. Without a key the call returns the price quote (HTTP 402).

## Storm history near an address ($0.05 per report line, no minimum)

```bash
curl "https://data.truefixr.com/v1/data?dataset=history&address=1100+Congress+Ave,+Austin,+TX+78701&radius_mi=5"
```

Without a key you get the line count and exact price (HTTP 402). Add `addresses=true` and your key for the full report.

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
    # r = requests.get(url, headers={"PAYMENT-SIGNATURE": signed_payload})  # x402 v2; older v1 clients send X-PAYMENT
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
    "name": "get_weather_data",
    "arguments": { "dataset": "storms", "state": "TX", "county": "Travis" }
  }
}
```
