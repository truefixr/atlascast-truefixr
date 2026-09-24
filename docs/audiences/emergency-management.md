# For Emergency Management

State, county, and municipal EM offices. See [SAMPLE_DATA.md](../SAMPLE_DATA.md) for real
response examples.

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

None of this is a damage certification or a property inspection -- a reported or forecasted
event is a signal worth following up on, not proof of loss.
