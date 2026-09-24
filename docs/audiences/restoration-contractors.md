# For Restoration & Contract Field Workers

Restoration companies, claims response teams, independent adjusters, and contract crews
doing field work after a storm. See [SAMPLE_DATA.md](../SAMPLE_DATA.md) for real response
examples.

- `distance_mi` and `storm_subtype` on every `dataset=storms` lead show exactly how close a
  reported event was to a specific address and what kind of event it was (radar-estimated
  hail, confirmed wind report, etc.) -- for prioritizing which addresses to actually visit
  first after a storm.
- Address-level data means crews can be routed to the highest-severity addresses first,
  instead of canvassing a whole neighborhood blind.

None of this is a damage certification or a property inspection -- a reported or forecasted
event is a signal worth following up on, not proof of loss.
