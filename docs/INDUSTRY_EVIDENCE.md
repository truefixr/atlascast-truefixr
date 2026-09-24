# Industry evidence

Real, dated, sourced evidence that the problem this API solves is a live industry concern --
not a claim, a list of things that actually happened, checked 2026-09-24.

## Companies already doing this kind of thing

- **Vortex Weather Insurance -- HailSafe** (parametric hail insurance). Launched direct sales
  **March 24, 2026**. Uses CoreLogic as the hail-verification data provider.
  [Insurance Journal, 3/24/2026](https://www.insurancejournal.com/news/midwest/2026/03/24/863245.htm) ·
  [Yahoo Finance, 3/24/2026](https://finance.yahoo.com/sectors/technology/articles/vortex-weather-insurance-launches-direct-125600799.html)

- **CoreLogic's hail-verification model** confirmed as the real trigger mechanism behind a
  parametric hail product, cited in **Maryland state legislative testimony** -- address-level
  hail verification data is load-bearing enough to show up in state insurance regulatory
  hearings. [Maryland General Assembly testimony PDF](https://mgaleg.maryland.gov/cmte_testimony/2026/fin/1T87dF371H1KhlyifDHONNddBTVP1XuqR.pdf)

- **Renewable Guard** -- parametric hail insurance for renewable energy projects. *"Adding
  parametric hail gives the existing project-level insurance program an extra and important
  dimension of coverage"* -- Michael Cosgrave, Principal.
  [PR Newswire](https://www.prnewswire.com/news-releases/renewable-guard-announces-new-parametric-hail-insurance-coverage-for-renewable-energy-projects-301048773.html)

- **Understory / MSI / GuaranteedWeather** -- sensor-triggered parametric hail insurance for
  auto dealer lots. [Artemis.bm](https://www.artemis.bm/news/parametric-hail-insurance-with-sensors-launched-by-understory-msi-guaranteedweather)

- **Restoration & Remediation Magazine**, two dated 2026 pieces aimed at their own
  restoration-contractor readership:
  - **May 11, 2026**: ["How Weather Data Helps Improve Hurricane Response for Restoration Contractors"](https://www.randrmagonline.com/articles/91953-how-weather-data-helps-improve-hurricane-response-for-restoration-contractors)
  - **May 26, 2026**: ["How Restoration Companies Can Track Equipment and Improve Hurricane Response"](https://www.randrmagonline.com/articles/91995-how-restoration-companies-can-track-equipment-and-improve-hurricane-response)

- **reworked.ai** industry blog: *"verified impact -- real-time storm reports mapped to
  actual affected addresses, not a county-wide blur"* described as modern best practice, by a
  third party with no relationship to this project.
  [reworked.ai](https://www.blog.reworked.ai/insurance-restoration-marketing)

- **RoofPredict** (**April 1, 2026**) names the real operational constraint: State Farm
  requires claims filed within 72 hours of damage -- real-time address-level data is what
  makes that window achievable.
  [RoofPredict blog](https://roofpredict.com/blog/storm-response-workflow-alert-to-contract-in-72-hours)

- **NOAA NCEI 2026 Industry Partnerships Summit**, **Sept 1-3, 2026** -- ~400 industry leaders,
  panel titled *"The Future of Coverage: Weather Inputs to Parametric Insurance"*, with senior
  catastrophe-research leadership from a top-10 US insurer in attendance.
  [NOAA NCEI summit page](https://www.ncei.noaa.gov/2026-industry-partnerships-summit)

- **Concirrus + Applied Underwriters -- SkyMiner**, announced **June 29, 2026**: an
  "insurance-native data and context layer" product, explicitly positioned against generic AI
  tools with "no model of insurance at all."
  [Press release](https://www.auw.com/about-us/news/article/concirrus-and-applied-underwriters-announce-skyminer)

- **Verisk**: **5/29/2026** *"Roofing Reality Check: Risk Is Rising Even in Quiet Storm
  Years"* (hail volatility, aging roof stock); **5/5/2026** Verisk analytics shipped directly
  into Anthropic's Claude via MCP connectors -- confirmation that insurance-data vendors are
  already building for AI-agent consumption, same as this project's own MCP server.
  [Verisk newsroom](https://www.verisk.com/company/newsroom/tags/underwriting)

- **Insurity 2026 AI in Insurance Report** (**May 21, 2026**): 51% of consumers say they'd be
  comfortable with their insurer using AI to monitor severe weather and deliver real-time
  alerts, up from 45% in 2025.
  [Insurity press release](https://insurity.com/press-release/consumer-comfort-with-ai-for-severe-weather-monitoring-climbs-to-51-in-2026-insurity-survey-finds)

- **Texas Division of Emergency Management** (**March 31, 2026**): state activated emergency
  response resources ahead of severe weather -- a real state EM agency acting on
  forward-looking weather risk.
  [TDEM press release](https://tdem.texas.gov/press-release/3-31-26)

- **LA County Office of Emergency Management** (**Sept 18, 2026**): actively testing
  Non-Weather Emergency Message systems with NWS and broadcasters for National Preparedness
  Month. [LA County OEM](https://lacounty.gov/2026/09/18/office-of-emergency-management-encourages-residents-to-take-action-for-national-preparedness-month)

## Industry-level demand signals

- Insurers are "hungry for more risk data" as climate-driven loss frequency rises -- average
  15 billion-dollar weather disasters/year in the US since 2011, 3x the prior three-decade
  average. [LinkedIn News](https://www.linkedin.com/news/story/insurers-hungry-for-more-risk-data-5541449/)

- Severe convective storms (hail/wind/tornado) are now the most costly insured peril
  globally, $50-60B/year in US losses and rising, treated as a baseline, not an anomaly.
  [Business Insurance](https://www.businessinsurance.com/rising-convective-storm-losses-test-insurers-as-property-rates-fall-in-competitive-market/)

- Industry shift: risk pricing is moving from quarterly review to a continuous, repriced
  signal -- favors frequently-refreshed data (this API refreshes every 15 minutes for
  reported events, 4x daily for forecast) over static annual cat model updates.

- Carriers/MGAs are increasingly pulling parcel-level data into pricing models, not just
  county/ZIP-level. [Zurich](https://www.zurichna.com/knowledge/articles/2026/07/accurate-property-values-are-vital-in-a-changing-climate)

- Weather-related claims are roughly 50% of homeowners insurance losses annually.

- Parametric insurance is moving toward more granular, address/sensor-level triggers, not
  just macro-level hurricane/earthquake. [Insillion](https://insillion.com/blog/parametric-insurance-platform-2026)

## Emergency management -- what's already available, and the real gap

- **FEMA RAPT** -- free GIS tool, real-time forecasts + historic disasters + annualized
  hazard frequency, county/census-tract level.
  [FEMA](https://www.fema.gov/emergency-managers/practitioners/resilience-analysis-and-planning-tool)
- **FEMA National Risk Index** -- baseline expected annual loss + social vulnerability,
  county/tract level, 18 hazards. [FEMA](https://www.fema.gov/about/reports-and-data)
- **PlainHazard** -- combines FEMA disaster declarations (1953-2026) + NOAA Storm Events +
  FEMA NRI into county-level profiles -- the closest public analog to this API, but
  county-level and backward-looking/annualized rather than a rolling forward forecast.

Every one of the above is county or census-tract resolution. None give a rolling,
address-level view of which specific properties are inside a forecast risk window this week
-- that's the real, defensible gap for EM use of this API. See
[audiences/emergency-management.md](audiences/emergency-management.md).
