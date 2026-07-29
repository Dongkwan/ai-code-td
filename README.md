# ai-code-td

Derived data for the paper **"Technical Debt Is a Model Property, Not an AI
Property: Evidence from Twelve Code LLMs and 100 Repositories"** (submitted to
*Information and Software Technology*).

Every statistic reported in the paper can be recomputed from the files here. The
headline result is that code-level technical debt separates by **model**, not by
AI-versus-human **authorship**: density spans a factor of 36 across twelve LLMs,
while the AI–human contrast changes sign depending on which models are placed in
the AI group.

---

## What is in this repository

```
data/
  metrics.csv                    per-class metrics, 1,183 classes (primary dataset)
  metrics_extended.csv           2,167 rows: adds the t=0.2 decoding condition
  stats_results.json             RQ1–RQ3 primary tests
  extra_stats.json               capability correlation, Friedman, bootstrap CIs
  validation.json                agreement with the `lizard` analyser
  syntactic_validity.json        parse-success rate per producer
  test_pass.json                 ClassEval functional-correctness outcomes
  correctness_filtered.json      debt measured on functionally correct classes only
  correctness_maint.json         correctness x maintainability cross-tabulation
  oss_large.json                 100-repository mining results (9,370 classes)
  oss_candidates.json            the repository inventory and selection record
  additional/
    E1_equivalence.json          TOST + Cliff's delta CI for the RQ1 null
    E2_posthoc_dunn.json         78 pairwise Dunn comparisons, BH-corrected
    E3_negative_binomial.json    count models M1/M2/M3 with a log(SLOC) offset
    E4_threshold_sensitivity.json  the pipeline re-run at 0.75x and 1.25x thresholds
    E5_effect_size_cis.json      per-model Cliff's delta against the human baseline
    E6_power.json                minimum detectable effect on the ordinal scale
    E7_duplicate.json            per-model duplicated-code rates and odds ratios
    E8_normalisation.json        four alternative debt normalisations
    E9_friedman_with_human.json  Friedman test blocked on task
    E10_repayment.json           the Appendix A prioritisation simulation
    E11_profile_uncertainty.json Wilson intervals on the per-model profiles
    E12_external_agreement.json  agreement with Pylint and Radon
    E13_elif_sensitivity.json    chained-`elif` nesting sensitivity
    E14_extended.json            twelve-model expansion, decoding condition
    all_additional.json          E1–E9 concatenated
figures/
  fig1 .. fig7                   the figures as they appear in the paper, 300 dpi
```

Total size is under 2 MB.

---

## `metrics.csv` — data dictionary

One row per generated or human-written class. 1,183 rows: 100 human reference
classes plus 1,083 classes from twelve LLMs.

| Column | Meaning |
|---|---|
| `sample_id` | ClassEval task identifier (`ClassEval_0` … `ClassEval_99`) |
| `group` | `Human` or `AI` |
| `model` | producer name; `Human` for the reference implementations |
| `sloc` | source lines of code, counted as logical statements |
| `nom` | number of methods |
| `wmc` | weighted methods per class (summed cyclomatic complexity) |
| `nof` | number of fields |
| `n_smells` | total smell instances detected in the class |
| `td_density` | `n_smells / sloc * 1000` — smells per kLoC |
| `smell_LongMethod` … `smell_Duplicate` | instance count for each of the seven smells |

**Important — how density is aggregated.** Every density figure in the paper is
the **mean over classes** of `td_density`, so each class counts equally regardless
of its length. This is *not* the same as pooling (total smells divided by total
SLOC), which weights long classes more heavily and gives a narrower spread:
**36× between models by class, 21.5× pooled**. Use `df.groupby('model').td_density.mean()`
to reproduce the numbers in Table 1 and Figure 1.

`metrics_extended.csv` has the same columns plus `decoding`, taking the values
`reference` (human), `greedy`, and `t0.2`. Filtering it to
`decoding in ('greedy', 'reference')` reproduces `metrics.csv` exactly (1,183
rows); filtering to `'greedy'` alone gives the 1,083 AI rows without the human
baseline.

### Reproducing the sample-dependence result

The paper's central claim is that the AI–human contrast changes sign with the
model set. The earlier seven-model version of this study used:

```python
SEVEN = ['GPT-4-Turbo', 'GPT-3.5-Turbo', 'Gemini-Pro', 'DeepSeek-Coder',
         'CodeLlama-13B', 'WizardCoder-15B', 'Magicoder-6.7B']

import pandas as pd
df = pd.read_csv('data/metrics.csv')
hu = df[df.group == 'Human'].td_density.mean()            # 11.62
ai7 = df[df.model.isin(SEVEN)].td_density.mean()          #  9.93  -> ratio 0.85x
ai12 = df[df.group == 'AI'].td_density.mean()             # 36.27  -> ratio 3.12x
```

Note that these seven are **not** the seven strongest models — StarCoder-15B and
InstructCodeGen-16B both carry less debt than Gemini-Pro and WizardCoder-15B but
were not in the earlier corpus. The set is historical, which is precisely the
point: the "AI effect" follows the model list, not the measurement.

---

## Where each claim in the paper comes from

| Paper claim | File | Key |
|---|---|---|
| Density spans a factor of 36 across models | `metrics.csv` | mean `td_density` by `model` |
| Kruskal–Wallis H = 96.33, p = 9.5e-16 | `stats_results.json` | `model_effect_kruskal` |
| 28 of 78 pairs significant; 26 between models | `additional/E2_posthoc_dunn.json` | `pairwise`, `n_significant_pairs` |
| RQ1 null: p = 0.706, delta = +0.017, CI [-0.07, 0.10] | `additional/E1_equivalence.json` | `cliffs_delta`, `delta_ci95`, `tost_p` |
| Power 80% for \|delta\| >= 0.17 | `additional/E6_power.json` | `min_detectable_delta_80pct_power` |
| Seven-model density 9.93, ratio 0.85x, p = 0.602 | `additional/E14_extended.json`, `metrics.csv` | `RQ1_published_seven_recheck`; see the snippet above |
| Authorship IRR moves 0.85 → 2.00 | `additional/E3_negative_binomial.json` | `M1_authorship` (twelve-model term) |
| AIC 2026.6 (model identity) vs 2471.2 (authorship) | `additional/E3_negative_binomial.json` | `M3_model_identity.aic`, `M1_authorship.aic` |
| Capability correlation rho = -0.68, p = 0.02 | `extra_stats.json` | `capability_corr` |
| Duplicated code 63.9% of AI debt vs 5.7% human | `metrics.csv`, `additional/E7_duplicate.json` | `smell_Duplicate`; `share_difference` |
| Significant in five models (OR 8.9–77.8) | `additional/E7_duplicate.json` | `per_model[*].p_adj`, `odds_ratio_vs_human` |
| Null holds at every threshold | `additional/E4_threshold_sensitivity.json` | `configs`, `rq1_null_robust` |
| Null holds under four normalisations | `additional/E8_normalisation.json` | — |
| `lizard` agreement r = 0.99 over 3,589 methods | `validation.json` | `n_matched_methods`, `pearson_cc` |
| Pylint / Radon oracle agreement | `additional/E12_external_agreement.json` | per-smell kappa |
| Chained-`elif` sensitivity | `additional/E13_elif_sensitivity.json` | `elif_aware` |
| Decoding does not change the ranking (rho = 0.973) | `additional/E14_extended.json` | `H3_decoding` |
| 100 repositories, 9,370 classes, 0.61x pooled | `oss_large.json` | `pooled`, `repo_level` |
| Prioritisation simulation (Appendix A) | `additional/E10_repayment.json` | `B_capability_selection_by_benefit_per_cost` |

---

## Corpus provenance

The generated classes analysed here come from **ClassEval** (Du et al., 2024), a
manually constructed class-level Python benchmark, and are not redistributed in
this repository. The benchmark and its model generations are available from the
original authors: <https://github.com/FudanSELab/ClassEval>. `metrics.csv` is the
derived measurement over those classes.

The 100 open-source repositories analysed for RQ4 are likewise not redistributed.
`oss_candidates.json` records each repository with the commit SHA pinned at
mining time, so the corpus can be reconstructed exactly.

AI authorship in those repositories was attributed from commit trailers
(`Co-authored-by: Copilot`, `Claude`, `Cursor`, `Devin`, `Codex` and equivalents),
with the human baseline restricted to files added after each project began using
AI assistance.

---

## Scope and known limitations

Stated here so the data are not read for more than they support:

- **Python only.** ClassEval is a Python benchmark. Nesting and complexity counts
  in particular are affected by Python-specific constructs (comprehensions,
  `with` blocks) that have no one-to-one equivalent in other languages.
- **The detector's nesting rule counts chained `elif` as additional nesting**,
  which Pylint does not. This is documented rather than silently corrected;
  `additional/E13_elif_sensitivity.json` reports every conclusion under both rules,
  and none of them change.
- **Some per-model profiles rest on few smell instances.** GPT-4-Turbo and
  DeepSeek-Coder contribute 14 and 18 instances respectively. Confidence intervals
  are in `additional/E11_profile_uncertainty.json`; the composition percentages
  for those two models indicate shape, not magnitude.
- **Equivalence was not established on the mean-difference scale.** The RQ1 null is
  supported on the ordinal scale (Cliff's delta CI inside the negligible band), but
  TOST does not reject on means (`E1_equivalence.json`, `tost_p = 0.064`). The paper
  says so; do not read the null as proven absence of an effect.
- **The repository result is weaker once clustering is respected.** Pooled
  p = 1.4e-4; the project-level Wilcoxon gives p = 0.029 on means and p = 0.103 on
  medians. The clustered test is the confirmatory one.

---

## What is not included

- **Analysis scripts.** This repository publishes the derived data only. The
  detector, mining pipeline, and analysis scripts are available from the author on
  request.
- **Cloned repositories** (~31 GB). Reconstructible from the SHAs in
  `oss_candidates.json` and `oss_large.json`.

All bootstrap and Monte-Carlo steps in the analysis were seeded with `20260728`.

---

## Citation

```bibtex
@article{kim2026td,
  title   = {Technical Debt Is a Model Property, Not an AI Property:
             Evidence from Twelve Code {LLM}s and 100 Repositories},
  author  = {Kim, Dong Kwan},
  journal = {Information and Software Technology},
  year    = {2026},
  note    = {Under review}
}
```

## Licence

Data and figures are released under **CC BY 4.0**. ClassEval and the mined
open-source repositories remain under their own licences.

## Contact

Dong Kwan Kim — Department of Computer Engineering, Mokpo National Maritime
University — <dongkwan@mmu.ac.kr>
