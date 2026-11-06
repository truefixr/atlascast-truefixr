# For insurance: underwriting and catastrophe risk

See [SAMPLE_DATA.md](../SAMPLE_DATA.md) for response shapes.

- **AtlasCast address forecast** (`dataset=risk`): a ranked list of the addresses in a storm's forecast path for a county and a window, address-level (not ZIP-level), from the next 24 hours to 10 days. The county's A to F grade for the window is a County Summary (`summary=true`).
- **Storm history near an address** (`dataset=history`): the storm record near one address back to 2003, with an optional `date_of_loss` mode. A record of reported and radar-detected events, not a statement that a claim is supported or denied.
- Combine **Recent storms near addresses** (`dataset=storms`: what was reported or radar-detected in the last 365 days) with the forecast (what could happen) for both halves of the picture.

None of this is a damage certification or a property inspection. A reported or forecast event is a signal worth following up on, not proof of loss. Forecasts describe what could happen, not what will happen.
