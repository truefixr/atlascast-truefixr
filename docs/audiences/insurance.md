# For Insurance -- Underwriting & Catastrophe Risk

See [SAMPLE_DATA.md](../SAMPLE_DATA.md) for real response examples.

- Address-level (not ZIP-level) risk scoring at point of quote.
- `structure_value` on every AtlasCast lead is real NSI replacement cost, the number that
  matters for underwriting, not a modeled market estimate.
- Combine `dataset=storms` (what already hit this address) with `dataset=risk` (what's
  forecasted) to price renewal risk with both halves of the picture, not just one.

None of this is a damage certification or a property inspection -- a reported or forecasted
event is a signal worth following up on, not proof of loss.
