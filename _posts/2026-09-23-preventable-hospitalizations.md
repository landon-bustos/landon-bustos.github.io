---
layout: post
title: "Why Some Counties Have Far More Preventable Hospitalizations Than Others"
date: 2026-09-23 09:00:00 -0700
categories: economy health
---

This one started as a term project for ECN 140 back in undergrad and has been sitting on my hard drive since. It fits exactly what this blog is for, so I'm adapting it into a proper post rather than letting it rot in a folder.

**The question:** a "preventable hospitalization" is an admission for a condition that a primary care provider could typically have managed before it got bad enough to need a hospital, things like uncontrolled diabetes or an asthma flare-up that got out of hand. Rates vary a lot by county, and I wanted to know how much of that variation traces back to plain economic hardship rather than anything about the hospitals themselves.

The stakes are real in dollar terms too. In 2022 the average hospital stay in the US ran 5.2 days and cost about $12,694, against a median household income of $76,580. For someone uninsured, one stay eats roughly 17 percent of a typical year's income, and a lot more than that for anyone below the median.

**The data:** the 2025 County Health Rankings dataset (County Health Rankings & Roadmaps, a University of Wisconsin / Robert Wood Johnson Foundation project), covering 3,159 US counties and 225 variables, cross-sectional for that year.

| Variable | Mean | Std. dev. |
|---|---|---|
| Preventable hospitalizations (per 100k) | 2,823 | 1,073 |
| Uninsured rate | 10.45% | |
| 20th percentile income | $28,450 | |
| 80th percentile income | $125,207 | |
| Severe housing problems | 12.86% | |
| Unemployment rate | 3.59% | |

**The approach:** I built this up in stages rather than throwing every variable into one regression and calling it a day.

First, a single-regressor model: hospitalization rate against the uninsured rate alone. Then a model using only economic variables (income at the 20th and 80th percentiles, unemployment), log-transformed since those distributions are all right-skewed. Then a fuller model adding primary care physician access and severe housing problems, with a squared term on 80th percentile income to let its effect flatten out at higher incomes. Last, a version with two interaction terms, unemployment with the uninsured rate, and 20th percentile income with unemployment, to see whether hardship compounds rather than just adding up.

**What came out of it:**

The single-regressor model says each 1 percentage point rise in the uninsured rate is associated with about 27 more preventable hospitalizations per 100,000 people, and the effect is statistically significant. But its R-squared is 0.013. The uninsured rate on its own explains barely more than one percent of the variation across counties, so whatever's driving most of the difference, it isn't just insurance coverage in isolation.

The economics-only model fills in more of the picture: a 1 percent increase in 20th percentile income is associated with about 11 fewer hospitalizations per 100,000, and a 1 percent increase in the unemployment rate with about 2.85 more. Both significant. In the fuller model, adding one more primary care physician per 100,000 residents is associated with roughly 2 fewer hospitalizations, which lines up with the basic story that access to a doctor before things get bad matters.

One result did surprise me: the share of households with severe housing problems came in negative and significant in every specification, more housing problems associated with fewer hospitalizations, which runs opposite to what I expected going in and opposite to what the housing and health literature generally finds. I don't have a clean explanation for it. My best guess is that it's picking up something about which counties end up with high measured housing-problem rates (denser, higher cost-of-living areas that also have better healthcare infrastructure) rather than a real protective effect of bad housing, but that's a guess, not a finding, and I'd want to dig into it more before leaning on it.

The interaction terms suggest the relationships aren't simply additive. Unemployment combined with being uninsured has a negative interaction coefficient (about -65), meaning the combined penalty is smaller than you'd get by just adding the two effects separately, and low income combined with unemployment behaves the same way (about -573). In both cases, the marginal damage from one hardship seems to shrink once a county is already dealing with the other. That's consistent with a diminishing-returns story: once a county is already struggling on one dimension, an additional one moves the needle less.

I ran an F-test on the uninsured rate's slope by itself (F = 40.1, p = 2.78e-10) and a joint F-test on the three economic variables together (F = 131.8, p < .001). Both reject the null of no effect cleanly.

I also ran a logit model, coding a county as "high hospitalization" if its rate topped 4,000 per 100,000, to see whether the same variables predict crossing that threshold rather than just shifting the average. Logged 20th percentile income was the strongest predictor: a one-unit increase in log income is associated with a 23.6 percentage point drop in the probability of being a high-hospitalization county. Logged unemployment pushes the odds up by about 5.1 points, and severe housing problems, again with that same counterintuitive negative sign, pulls them down by about 8.1 points.

**Caveat worth stating plainly:** this is a cross-sectional OLS setup, and I think there's a real omitted-variable problem around healthcare infrastructure itself. Counties with better-funded, higher-quality hospitals likely also employ more primary care physicians for reasons that have nothing to do with the socioeconomic variables in this model, which would bias the physician-access coefficient. Population density looks like a reasonable instrument for physician access here: it's plausibly correlated with physician supply through demand for care, but less obviously tied to hospital funding or administrative quality. I didn't have time to run the instrumented version for this draft, but it's the natural next step if I come back to this dataset.

**Where I land:** none of this is a surprise at the level of "poverty and lack of insurance are bad for health." What's useful here is the magnitude and the shape, income at the bottom of the distribution and unemployment move the needle more than the uninsured rate does on its own, physician access matters on the same order as income, and the hardships don't simply stack. If you're trying to lower preventable hospitalizations in a given county, this says the lever isn't just "get people insurance", it's income support and physician access working together, and the payoff to fixing one problem seems to depend on whether the others are already fixed.

*Sources: County Health Rankings & Roadmaps (2025); Agency for Healthcare Research and Quality (2022); U.S. Census Bureau (2022); Pickett & Wilkinson (2015); Williams & Jackson (2005); Berkman et al. (2011); Krieger & Higgins (2002); Oronce et al. (2020); Moy et al. (2013).*
