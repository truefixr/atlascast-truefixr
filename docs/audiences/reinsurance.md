# For reinsurance

See [SAMPLE_DATA.md](../SAMPLE_DATA.md) for response shapes.

- **AtlasCast address forecast** (`dataset=risk`) returns the ranked list of addresses inside a county's forecast footprint, 24 hours to 10 days ahead, each with a storm type, a forecast time and a risk label, plus the county's A to F grade. Useful for a check of what is on risk ahead of an active weather event, not just after-the-fact loss reporting.
- **`POST /v1/portfolio/risk`** checks up to 500 locations in one call ($0.05 per location), each with the forecast grade of the county it sits in.
- The **daily digest** (`dataset=daily`, $0.02 per county) is a county-level summary of the day's forecast; county aggregates only, no address records.

None of this is a damage certification or a property inspection. A reported or forecast event is a signal worth following up on, not proof of loss. Forecasts describe what could happen, not what will happen.
