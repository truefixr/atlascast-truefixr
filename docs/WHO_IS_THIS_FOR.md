# Who this is for

Real use cases, grouped by who's actually asking for this kind of data (see [`docs/SAMPLE_DATA.md`](SAMPLE_DATA.md) for what the responses actually look like).

## Emergency Management (state / county / municipal)

The public tools EM already has -- FEMA's National Risk Index, FEMA RAPT -- stop at the
county or census-tract level, and are mostly backward-looking (annualized risk, historical
disasters). None of them give a rolling, address-level view of what's forecasted to happen
in the next 10 days.

What this API adds for EM specifically:
- **Resource pre-positioning**: `dataset=risk` with `addresses=true` returns the real
  addresses inside a forecast footprint, not just "this county is at risk" -- so response
  planning can target specific neighborhoods, not the whole jurisdiction.
- **Real exposure numbers for briefings**: `county_exposure_value_usd`,
  `county_exposure_people`, and `exposure_people` inside the active forecast window give a
  real dollar and population figure to put in front of a board or a governor's office, not
  a modeled annualized estimate.
- **Post-event assessment**: `dataset=storms` gives reported/radar-detected event history
  by address for damage assessment triage, without waiting on field survey teams to cover
  every block first.

## Insurance -- underwriting & catastrophe risk

- Address-level (not ZIP-level) risk scoring at point of quote.
- `structure_value` on every AtlasCast lead is real NSI replacement cost, the number that
  matters for underwriting, not a modeled market estimate.
- Combine `dataset=storms` (what already hit this address) with `dataset=risk` (what's
  forecasted) to price renewal risk with both halves of the picture, not just one.

## MGAs & parametric insurance

- `dataset=risk` returns a real probabilistic grade (A-F) and forecast window per address --
  usable as a trigger input for parametric products, the same way real launched products
  (HailSafe, Renewable Guard's parametric hail coverage) already use address-level hail/storm
  data as a payout trigger.
- Self-serve, pay-per-address pricing means a small MGA can integrate this without an
  enterprise sales cycle.

## Reinsurance

- `county_exposure_value_usd` and `county_exposure_people` give portfolio-level accumulation
  numbers for a specific forecast window, useful for treaty-level exposure checks ahead of an
  active weather event, not just after-the-fact loss reporting.

## Restoration & claims response

- `distance_mi` and `storm_subtype` on every `dataset=storms` lead show exactly how close a
  reported event was to a specific address and what kind of event it was (radar-estimated
  hail, confirmed wind report, etc.) -- for prioritizing which addresses to actually visit
  first after a storm.

## Research & academia

- Nationwide, address-level, both historical event data and forecast risk in one place --
  most public datasets are one or the other, county-level, or both.

---

None of this is a damage certification or a property inspection. Every response carries the
same honest disclaimer the API itself returns: a reported or forecasted event is a signal
worth following up on, not proof of loss.
