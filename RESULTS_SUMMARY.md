# Results Summary

## The question

Under a hypothetical scenario combining the SAVE Act (documentary proof of citizenship,
shown in person, required to register) and a 2026 executive order (DHS-verified eligibility
required to receive a mail ballot), which access barrier — lacking required documents, or
lacking the ability to physically show up in person — better explains why an eligible citizen
would fail to vote?

Neither policy is currently active law. This project models a hypothetical scenario using
real, current survey and Census data as inputs.

## The model

A discrete Bayesian Network with four variables: `has_documents` and `can_access_in_person`
(independent priors) both feed into `registered`, which feeds into `voted`. Conditional
probability tables were specified from real data — a national DPOC survey (Brennan
Center/CDCE, fielded fall 2023, n = 2,386) for the documents gate, and Census CPS Table 10
(reasons for not voting, Nov 2024) as a proxy for the physical-access gate. The registration
CPT's best-case value was additionally grounded in *Fish v. Kobach* (Kansas, 2013–2016), a
real DPOC-enforcement case in which a federal court found approximately 14% of all
registration attempts were blocked — including cases where documentation had already been
submitted, evidencing real administrative friction beyond simple document possession.

## The measure: comparison via Bayesian inference

Using exact inference (`pgmpy`'s `VariableElimination`), we computed the posterior
probability of each barrier given the evidence that a person did not vote:

| Measure | Result |
|---|---|
| P(lacks documents \| didn't vote) | **15.1%** |
| P(lacks physical access \| didn't vote) | **71.95%** |

A person who didn't vote in our model was **about 5 times more likely** to have been
stuck by a physical-access barrier than a documents barrier.

## Downstream impact

This gap cascades into the model's overall predictions:

| Measure | Today (real) | Predicted (hypothetical scenario) |
|---|---|---|
| Registration rate | 73.6% | 46.8% |
| Turnout rate | 65.3% | 41.5% |

The rate of voting among people who *do* register (88.7%) is unaffected by either policy in
our model — the entire predicted decline occurs at the registration stage, driven by the two
new access gates.

## Case analysis: why Kansas matters

Kansas ran a real documentary-proof-of-citizenship law from 2013–2016 — the closest existing
real-world precedent to the SAVE Act. Two independently reported figures from that case (a
35,000+ raw block count, and a court-stated ~14% block rate) were cross-checked against each
other and found consistent (implied total registration attempts ≈ 250,000, in a plausible
range). This gave us a real, non-hypothetical anchor for at least one cell of our model,
rather than relying entirely on reasoned estimates.

## Interpretation

Physical access — not documentation — is this model's predicted dominant barrier to voting
under the combined policy scenario. This is a meaningful, non-obvious finding: public
discussion of proof-of-citizenship requirements tends to center on the paperwork itself, but
our model suggests the in-person requirement embedded in the SAVE Act may be the larger
practical obstacle.

## Limitations

- Three of the four `registered` CPT values are reasoned estimates, not measured data; only
  the best-case value is grounded in the Kansas case
- The Stage 2 (physical access) figure is a proxy — CPS Table 10 measures registered
  non-voters, not people blocked from registering
- The Brennan Center and CDCE sources trace to a single 2023 survey fielding, not two
  independent estimates
- DPOC access was not measured for name-change mismatch, though the SAVE Act re-triggers
  after one — likely an undercount of the barrier for women
- The DHS mail-ballot eligibility-matching risk is discussed as a qualitative caveat only; no
  dataset exists to quantify its failure rate, so it is not part of the probability model

Full source-by-source documentation is in `SOURCES.md`.
