# Methodology

## How risk gets attached to an address

Two mechanisms, both real, neither invented:

1. **NOAA's own calibrated outlook data** covers tornado/hail/wind out to day 3 -- a real,
   government-published probability polygon. We don't invent our own number here; we take
   the calibrated one and do point-in-polygon matching against every real address.
2. **Our own trained models per peril**, scored at radar resolution against every address,
   for every peril beyond day 3 and for perils the government outlook doesn't cover at all
   (flash flood, heavy rain, winter, ice, coastal surge, hurricane, wildfire).

An address doesn't get a private simulation run just for it -- it gets the forecast of
whichever grid cell or polygon its real coordinate physically falls inside. Precision comes
from how fine that grid is: government forecast data starts around 50km resolution; radar
data (used for wind/hail precision) is closer to 1km.

## Real, current model accuracy (AUC), per peril

| Peril | AUC | Verification |
|---|---|---|
| WIND | 0.8103 | Fully reverified, 3-seed mean |
| FLASH_FLOOD | 0.8919 | Self-reported, not yet independently reverified |
| HEAVY_RAIN | 0.8592 | Self-reported, not yet independently reverified |
| TORNADO | 0.6951 | Held-out, corrected twice (label bug, then split bug) |
| HAIL | 0.6998 | Held-out, real MESH radar hail-size features |
| WINTER | 0.8888 | Held-out, neighborhood-boosted synoptic features |
| ICE | 0.8062 | Held-out, neighborhood-boosted synoptic features |
| COASTAL_SURGE | 0.9223 | Held-out, real SLOSH storm-surge grid join, geo-sanity verified |
| HURRICANE | 0.6698 | Held-out -- see the correction story below |
| WILDFIRE | 0.846 | Held-out, full fire-weather feature set |

**Before relying on FLASH_FLOOD or HEAVY_RAIN in a customer-facing claim:** these two are
self-reported by the training pipeline, not independently re-scored on a fresh holdout the
way WIND and HURRICANE were. Worth knowing if you're building something that cites these
numbers directly.

## The hurricane model correction (why you should trust these numbers)

An earlier version of the hurricane model scored 0.8315 -- until a review caught it using
the storm's actual *future* position as a feature, which is leakage (the model was
effectively looking at the answer). It was rebuilt using issue-time-only position data and
now honestly scores 0.6698. That's a real drop of 16 points, published here instead of
quietly kept at the old, inflated number. If a number here is wrong, the fix goes in this
table, not around it.

## Data honesty

A storm event being reported or radar-detected near an address is not a property inspection,
damage assessment, or repair estimate. Forecast risk is probabilistic, not a certainty.
`structure_value` fields are NSI **replacement cost** (what it costs to rebuild), not market
price.
