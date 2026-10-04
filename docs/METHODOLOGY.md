# What you get

## Two views of every address

- **Reported and radar-detected storms.** Events that already happened near an address, up
  to 365 days back, with a 2003-to-present archive for one-property reports.
- **Forecast risk.** What could happen at an address over the coming days, across ten
  perils: tornado, hail, wind, flash flood, heavy rain, winter, ice, coastal surge,
  hurricane and wildfire. Refreshed several times a day.

Both come back at the address level. Call `GET https://data.truefixr.com/v1/data` with no
parameters to see the live list of datasets, fields and prices.

## Data honesty

- A storm that was reported and radar-detected near an address is not a property
  inspection, damage assessment, or repair estimate.
- Forecast risk shows what could happen, not what will. A forecast can be wrong.
- `structure_value` is replacement cost (what it costs to rebuild), not market price.
- Not affiliated with NOAA or the National Weather Service.
