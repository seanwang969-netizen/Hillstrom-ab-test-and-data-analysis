# Resume bullets & interview guide — Hillstrom causal inference project

## Best résumé heading

**Randomized Experiment & Uplift Policy Evaluation** | Python, DuckDB SQL, SciPy, statsmodels, scikit-learn, EconML

### Recommended three bullets (for Inference / Experimentation DS)

- **Audited a 64K-customer, three-arm randomized email experiment** using DuckDB SQL; evaluated assignment balance with SRM chi-square (p=0.904), standardized mean differences, and categorical balance tests.
- **Estimated causal revenue lifts with Welch tests, Holm correction, and 95% CIs**; found Mens E-Mail increased revenue by **$0.77 per assigned customer** vs control (95% CI: **$0.49–$1.05**), while benchmarking CUPED, robust ANCOVA, and **1,000 A/A simulations**.
- **Analyzed conversion HTE with robust interaction Wald tests** (historical spend: Holm p=0.033), then trained **5-fold-CV Ridge T-Learner and honest Causal Forest** policies; used **IPW and confidence intervals** to show no statistically supported revenue gain over the best universal email strategy.

### Even shorter (two bullets, tighter résumé)

- Evaluated **64K-customer randomized marketing experiment** with DuckDB SQL, SRM/covariate checks, Welch A/B tests, Holm correction, CUPED and robust ANCOVA; quantified **+$0.77 incremental RPU** (95% CI **$0.49–$1.05**) for Mens E-Mail vs control.
- Modeled HTE via interaction Wald tests and trained **Ridge T-Learner / Causal Forest** uplift policies; validated with **IPW + 95% CIs**, finding no reliable improvement over the simpler all-Mens benchmark.

## 45-second interview pitch

> I analyzed a three-arm randomized email-marketing experiment covering 64,000 customers. I started by auditing data quality, allocation ratios and pre-treatment balance, then estimated revenue treatment effects using Welch tests with multiple-testing correction. The men's email increased revenue by about $0.77 per assigned customer versus no email. I checked whether CUPED or multivariable ANCOVA meaningfully improved precision; neither did. I then investigated heterogeneity using robust interaction tests and built both a Ridge T-Learner and a causal forest. Crucially, I evaluated recommended policies with inverse propensity weighting and confidence intervals rather than relying on outcome prediction MSE. Neither personalized revenue policy showed a statistically credible gain over simply sending the men's email. My takeaway was that a transparent experiment-backed policy can outperform adding model complexity without strong decision evidence.

## Likely interviewer questions

**Why didn't you filter out zero-spend customers?** The estimand is revenue *per randomly assigned customer*; filtering on purchase/visit would condition on post-treatment behavior and introduce bias.

**Why does HTE not imply a better policy?** Statistical variation in a treatment's effect across groups does not imply a different action is best for any group. Also, conversion and revenue are not interchangeable objectives.

**Why did CUPED not help?** Historical spending's observed correlation with post-email revenue was only ~0.022; little predictable variation existed to remove.

**Why IPW?** We observe each customer's outcome under one randomized action, but not the alternative outcomes. IPW reweights matches between observed and recommended actions to estimate each policy's value under known assignment probabilities.

**What is the biggest limitation?** The original analysis explored HTE before preserving a strict untouched holdout, then compared several models; the final policy p-values should be treated as exploratory. Spending is highly skewed, and profit/cost data were unavailable.

**Would you ship the causal forest?** No—not on this evidence. I'd keep Mens E-Mail as the benchmark, quantify costs, and prospectively test a frozen personalized policy against the benchmark in a fresh randomized experiment.

## Claim discipline

**Say:** "Evaluated an existing 3-arm randomized experiment"; "estimated incremental revenue"; "found no statistically supported improvement".

**Do not say:** "I ran/designed the original experiment", "I deployed an uplift system", "causal forest increased revenue", "all customers benefit more from mens email", or "the models proved personalization does not work".

**Résumé placement:** Projects section, 2–3 bullets, one visible GitHub link; this is a stronger fit for **Experimentation DS, Product DS (experiments), Causal Inference DS, Marketing Science, and Decision Science** than for generic BI/reporting roles.
