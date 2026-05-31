# Drive for Show or Putt for Dough? — Bayesian Analysis of PGA Tour Scoring Factors

## Overview
This project applies Bayesian linear regression to a question as old as professional golf: does long game (driving and approach play) or short game (chipping and putting) have a greater impact on a player's scoring average? Using 2018 PGA Tour season data and MCMC sampling via JAGS, the model estimates and compares the posterior distributions of each component's effect on average score.

## Research Question
Does long game or short game performance have a greater effect on professional golfers' average scores, and has the "drive for show, putt for dough" saying held up in modern golf?

## Dataset
2018 PGA Tour season statistics for 261 players (193 after removing missing values), including Strokes Gained metrics:
- **SG.OTT** – Strokes Gained: Off the Tee
- **SG.APR** – Strokes Gained: Approach
- **SG.ARG** – Strokes Gained: Around the Green
- **Average.SG.Putts** – Strokes Gained: Putting
- **Rounds** – number of rounds played (used as a confounding variable)

Two composite metrics were constructed:
- `long_game = SG.OTT + SG.APR`
- `short_game = SG.ARG + Average.SG.Putts`

## Model
**Bayesian linear regression** with a Gaussian likelihood:

$$Y_i \sim \text{Normal}(\beta_0 + \beta_1 \cdot \text{Short Game}_i + \beta_2 \cdot \text{Long Game}_i + \beta_3 \cdot \text{Rounds}_i, \sigma^2)$$

**Priors:**
- β₀ ~ Normal(71, 2²) — informed by the 2017–18 tour average score
- β₁, β₂, β₃ ~ Normal(0, 5²) — weakly informative
- σ² ~ Inverse-Gamma(0.1, 0.1)

**MCMC:** 5 chains × 10,000 iterations = 50,000 posterior samples in JAGS

## Key Findings
- **Long game posterior mean: −0.641** strokes per SD improvement
- **Short game posterior mean: −0.425** strokes per SD improvement
- The posterior probability that long game has a larger effect than short game is **100%** — credible intervals do not overlap
- Conclusions are **robust across prior specifications** (skeptical, original, and flat priors all yield consistent posteriors)
- Results hold when rounds played is excluded as a covariate

## Convergence Diagnostics
- Trace plots show well-mixed, stationary chains
- Gelman-Rubin statistic = 1.0 across all parameters
- Effective sample sizes ~41,000–47,000 (well above iteration count)
- Posterior predictive check confirms good model fit

## Conclusion
In the 2018 PGA Tour season, long game performance had a measurably larger impact on average score than short game performance. Both components matter substantially, but driving and approach play carry a slight edge in the modern game.

## Tools & Libraries
R · JAGS · rjags · coda · ggplot2 · dplyr

## How to Run
1. Clone the repository
2. Ensure JAGS is installed on your system: [mcmc-jags.sourceforge.net](https://mcmc-jags.sourceforge.net)
3. Open `markdown_script.md` file in RStudio
4. Install required R packages: `rjags`, `coda`, `ggplot2`, `dplyr`
5. Run all chunks
