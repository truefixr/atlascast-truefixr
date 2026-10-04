# For insurance: underwriting and catastrophe risk

See [SAMPLE_DATA.md](../SAMPLE_DATA.md) for response shapes.

- **ML Storm Forecast by Address:** address-level (not ZIP-level) forecasts from per-peril machine-learning models. `structure_value` on every record is replacement cost, the number that matters for rebuilding, not market value.
- **Storm History by Address: Reported and Radar-Detected Events Since 2003:** a citable record with a `reference_id`, with an optional `date_of_loss` mode. A record of reported and radar-detected events, not a statement that a claim is supported or denied.
- Combine **Past Storm Leads by Address** (what was reported and radar-detected in the last 365 days) with the forecast (what could happen) for both halves of the picture.

None of this is a damage certification or a property inspection. A reported or forecast event is a signal worth following up on, not proof of loss. Forecasts describe what could happen, not what will happen.
