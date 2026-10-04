# For reinsurance

See [SAMPLE_DATA.md](../SAMPLE_DATA.md) for response shapes.

- **ML Storm Forecast by Address** returns `exposure_value_usd` and `exposure_people` for the addresses inside a forecast footprint, plus county totals. Useful for accumulation checks ahead of an active weather event, not just after-the-fact loss reporting.
- The free `dataset=daily` digest ranks counties nationwide by exposure value, people, new leads or unique addresses. Exposure is structure replacement cost, not market value.

None of this is a damage certification or a property inspection. A reported or forecast event is a signal worth following up on, not proof of loss. Forecasts describe what could happen, not what will happen.
