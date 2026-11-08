# For emergency management

State, county and municipal offices. See [SAMPLE_DATA.md](../SAMPLE_DATA.md) for response shapes.

Public risk tools mostly stop at county or tract level and look backward (annualized risk, past disasters). The **AtlasCast address forecast** (`dataset=risk`) adds a rolling, address-level view of what is forecast for the next 24 hours to 10 days.

- **Resource pre-positioning:** `dataset=risk` with `addresses=true` returns the addresses in the forecast path for a county and a window, so planning can target neighborhoods, not the whole jurisdiction.
- **Schools and businesses:** `dataset=at_risk` lists the schools and businesses inside the forecast area, by peril and lead time.
- **Numbers for briefings:** the county's A to F grade and its counts by storm type for the window (County Summary, `summary=true`, $0.02 per county), plus the `dataset=daily` county digest ($0.02 per county).
- **Post-event triage:** **Recent storms near addresses** (`dataset=storms`) shows where events were reported or radar-detected in the last 365 days, by address, to help prioritize where to look first.

None of this is a damage certification or a property inspection. A reported or forecast event is a signal worth following up on, not proof of loss. Forecasts describe what could happen, not what will happen.
