# For Developers -- Build Your Own App or Product on This Data

You don't have to be an insurer or EM office to use this. This is raw, real, address-level
data, cheap enough ($0.05/address) to build a product on top of and resell.

## What you could actually build

- **A storm alert app** -- notify homeowners/property managers when their address enters a
  forecast risk window (`dataset=risk`), subscription model.
- **A property risk overlay for real estate sites** -- pull `structure_value` and current risk
  grade for any address a buyer is looking at.
- **A parametric insurance quoting tool** -- real launched products (Vortex Weather Insurance's
  HailSafe, Renewable Guard's parametric hail coverage) already use address-level hail data as
  a payout trigger. Build the quoting layer, use this as the data layer underneath it.
- **A contractor/restoration lead tool** -- `dataset=storms` with `distance_mi` and severity
  gives you exactly what tools like HailTrace and HailWatch already charge $65-119/month for,
  except you'd own the product instead of paying someone else's subscription.
- **A Discord/Slack bot** that pings a channel when a tracked address enters risk -- a weekend
  build, real data underneath it.
- **An AI agent skill or MCP tool** that other people's agents can call through yours, marking
  up the $0.05/address wholesale price into your own paid product.

## The actual economics

- Wholesale: $0.05/address, $25 minimum to start.
- You control the markup. Sell a monthly subscription, a per-report fee, a SaaS tier -- your
  call, this is just the data layer underneath whatever you charge for.
- No contract, no minimum commitment, no enterprise sales call to get started.
- Free previews (`addresses=false`) let you prototype and validate an idea before spending
  anything.

## Real precedent this isn't hypothetical

Small, fast-moving companies are already doing exactly this in the real market -- see
[INDUSTRY_EVIDENCE.md](../INDUSTRY_EVIDENCE.md) for dated, sourced examples: Vortex Weather
Insurance launched a real parametric hail product in March 2026, Renewable Guard built a real
niche parametric product for solar farms, Understory built one specifically for auto dealer
lots. None of these are massive companies -- they found a specific angle and built on top of
address-level storm data.

## Start here

- [Code examples](../CODE_EXAMPLES.md) -- real curl/Python/JS to start pulling data today
- [Sample data](../SAMPLE_DATA.md) -- see the actual response shape before you build anything
- Get a key: [atlasunited.io/api](https://atlasunited.io/api) or [truefixr.com/api](https://truefixr.com/api)

None of this is a damage certification or a property inspection -- a reported or forecasted
event is a signal worth following up on, not proof of loss.
