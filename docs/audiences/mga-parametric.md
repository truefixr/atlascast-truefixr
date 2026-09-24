# For MGAs & Parametric Insurance

See [SAMPLE_DATA.md](../SAMPLE_DATA.md) for real response examples.

- `dataset=risk` returns a real probabilistic grade (A-F) and forecast window per address --
  usable as a trigger input for parametric products, the same way real launched products
  (HailSafe, Renewable Guard's parametric hail coverage) already use address-level hail/storm
  data as a payout trigger.
- Self-serve, pay-per-address pricing means a small MGA can integrate this without an
  enterprise sales cycle.

None of this is a damage certification or a property inspection -- a reported or forecasted
event is a signal worth following up on, not proof of loss.
