# Identifying Market Gaps for High-End Grocery Stores in Los Angeles

**April 2026** · [Read the paper (PDF)](Demand%20Forecasting%20in%20LA.pdf)

A ZIP-code-level econometric study of where premium grocers — Erewhon, Whole Foods, and
Bristol Farms — should open their next Los Angeles location, and what neighborhood
characteristics actually drive where they already are.

## The question

High-end grocers can't absorb a bad location the way mass-market chains can: their model
needs a critical mass of customers willing to pay a premium, so a single misplaced store
means years of underperformance. Los Angeles is a useful test case because income,
density, and lifestyle vary sharply across ZIP codes, which supplies the cross-sectional
variation needed to estimate location effects.

The paper asks two things: which neighborhood characteristics predict high-end grocery
presence, and which affluent ZIP codes have **fewer stores than the model predicts** —
the candidate expansion sites.

## Data

A cross-sectional dataset covering all **270 ZIP codes in Los Angeles County**. Each
observation is one ZIP code.

| | Variable | Source |
|---|---|---|
| **Dependent** | Count of high-end grocery stores | Official store locators (Erewhon, Whole Foods, Bristol Farms), geocoded to ZIP |
| **Independent** | Median household income | American Community Survey / Census Reporter |
| | Home prices | Zillow |
| | Median gross rent | ACS / Census Reporter |
| | Population density | ACS (population / area) |
| | Median age | ACS / Census Reporter |
| | Walkability index | LA GeoHub |
| | Parking availability | City of Los Angeles |
| | Competitor dummy | Presence of Trader Joe's / Sprouts |

Income and rent are log-transformed to reduce skew and improve interpretability.

## Method

The dependent variable is a count with many zeros and variance exceeding its mean, so
Poisson regression is inappropriate. The paper estimates a **Negative Binomial regression
by maximum likelihood**, and confirms the choice with a likelihood-ratio test against
Poisson — which rejects Poisson in favor of Negative Binomial, confirming overdispersion.

Supporting specification work:

- **Multicollinearity diagnostics** — correlation matrix plus Variance Inflation Factors,
  flagging correlations above 0.8 and VIFs above 5–10.
- **Multicollinearity treatment** — log rent was severely collinear with log income and is
  dropped from the preferred specification. A PCA-based composite affluence index (first
  principal component of income and rent) was estimated as an alternative; the log-income
  model was preferred for comparable performance with far easier interpretation.
- **Robust inference** — sandwich standard errors throughout, so inference survives
  heteroskedasticity.

## Findings

1. **Income dominates.** Log median household income is by far the strongest predictor of
   high-end grocery presence — positive and significant in *every* specification, and more
   stable once collinear rent is removed. Density, median age, and walkability had smaller
   and less consistent effects; such stores appear in both urban and suburban settings.

2. **Competitors attract rather than repel.** The competitor coefficient is positive and
   significant: high-end grocers cluster in the same neighborhoods instead of spreading
   out. An existing premium grocer in a ZIP code is a *market signal*, not a reason to stay
   away — consistent with Hotelling's account of spatial competition, where firms serving
   the same demand locate near one another.

3. **A usable ranking.** Fitted values from the model rank ZIP codes by predicted store
   count. A chain considering expansion targets the high-ranking ZIP codes where it
   currently has no presence.

## Theoretical framing

- **Hotelling (1929)** — spatial competition and product differentiation; why competing
  sellers converge rather than spread out.
- **Huff (1963, 1964)** — the probabilistic gravity model of retail trade areas, treating
  trade areas as overlapping probability contours shaped by store size and travel time
  rather than hard boundaries. Notably, distance sensitivity is lower for specialty
  purchases, so high-end grocers draw from wider, more overlapping trade areas.
- **Reed, Yu & Hughes (2023)** — specialty grocers locate in urban areas with young,
  affluent, well-educated populations, more pedestrian traffic, and more fitness
  establishments, and are *not* deterred by competitive saturation.
- **Smith, Huang & Lin** — organic purchasing rises with income and with children under
  six; urban and Western households are measurably more likely to buy organic.

## Limitations

- **Cross-sectional data cannot establish causality.** Whether premium grocers *follow*
  neighborhood affluence or help *produce* it through gentrification is not identified here.
- **ZIP codes are an imperfect unit.** Per Huff, real trade areas spill across ZIP
  boundaries rather than respecting them.
- **No per-store revenue data.** The recommended ZIP codes are therefore demographic
  matches to successful locations, not directly measured demand gaps.

## References

- Hotelling, Harold. "Stability in Competition." *The Economic Journal*, vol. 39, no. 153,
  1929, pp. 41–57. <https://doi.org/10.2307/2224214>
- Huff, David L. "A Probabilistic Analysis of Shopping Center Trade Areas." *Land
  Economics*, vol. 39, no. 1, 1963, pp. 81–90. <https://doi.org/10.2307/3144521>
- Huff, David L. *Journal of Marketing*, vol. 28, no. 3, 1964, pp. 34–38.
  <https://doi.org/10.2307/1249154>
- Reed, Connor, T. Edward Yu, and David Hughes. "Evaluating the Factors Influencing the
  Location Strategies of Specialty Grocers versus Traditional Supermarkets in the United
  States." *Applied Geography*, vol. 158, 2023, article 103034.
  <https://doi.org/10.1016/j.apgeog.2023.103034>
- Smith, Travis A., Chung L. Huang, and Biing-Hwan Lin. "Does Price or Income Affect
  Organic Choice?"
