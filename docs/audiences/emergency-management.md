# For emergency management

State, county and municipal offices. See [SAMPLE_DATA.md](../SAMPLE_DATA.md) for response shapes.

Public risk tools mostly stop at county or tract level and look backward (annualized risk, past disasters). **ML Storm Forecast by Address** adds a rolling, address-level view of what is forecast for the next 24 hours to 10 days.

- **Resource pre-positioning:** `dataset=risk` with `addresses=true` returns the addresses inside a forecast footprint, with structure replacement value and estimated people at each, so planning can target neighborhoods, not the whole jurisdiction.
- **Numbers for briefings:** `exposure_value_usd` and `exposure_people` for the active forecast window, plus the free `dataset=daily` county digest ranked by exposure.
- **Post-event triage:** **Past Storm Leads by Address: Reported and Radar-Detected, Last 365 Days** shows where events were reported and radar-detected, by address, to help prioritize where to look first.

None of this is a damage certification or a property inspection. A reported or forecast event is a signal worth following up on, not proof of loss. Forecasts describe what could happen, not what will happen.
