# Code examples

Real, tested snippets against the live API. Replace `YOUR_API_KEY` with a real key from
[atlasunited.io/api](https://atlasunited.io/api) or [truefixr.com/api](https://truefixr.com/api).

## Free preview (no key needed)

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

## Address-level pull (requires payment)

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

## Forecasted risk with real property values

```bash
curl "https://data.truefixr.com/v1/data?dataset=risk&state=TX&county=Harris&addresses=true&window=16d" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

## Autonomous AI agent payment (x402, no API key)

```python
# Real flow, no account, no signup.
import requests

url = "https://data.truefixr.com/v1/data?dataset=storms&state=TX&county=Travis&addresses=true"
r = requests.get(url)

if r.status_code == 402:
    payment_details = r.json()["for_ai_agents"]
    # payment_details has: pay_to (Base network address), price_usd, network
    # Construct and sign an x402 "exact" scheme payment payload for that amount,
    # then retry with an X-Payment header:
    # r = requests.get(url, headers={"X-Payment": signed_payload})
    print(payment_details)
```

Full machine-readable payment manifest: [`/.well-known/x402.json`](https://truefixr.com/.well-known/x402.json)

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
