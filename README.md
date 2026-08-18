# ai-code-td — derived data and figures

Derived data and figures for the paper

> **"Specification echo inflates duplicate-code measurements in LLM-generated
> classes: an audit and a logic-bearing counting rule"**
> Dong Kwan Kim, submitted to *Empirical Software Engineering*.

**Release 2026-08-10**, README updated 2026-08-18 for the change of venue and
title; no data file changed. This release carries **data and figures only**. The
detector, the mining pipeline and the analysis scripts are available from the
corresponding author on request. Every number quoted below is a value stored in
one of the files here, so the claims in the paper can be checked by reading the
data without running anything.

The paper's deliverable is a counting rule: a repeated block counts as duplicate
code only if it contains at least one executable statement. Both rules are stored
side by side in these files (see *The two duplication rules* below), so the effect
of the rule can be recomputed rather than taken on trust.

---

## What the paper found, and where each number lives

A duplicate-code detector cannot tell repeated program logic from a
natural-language specification reproduced inside generated code. In this corpus
that distinction accounts for essentially the whole apparent AI–human gap.

| Result | File | Key |
|---|---|---|
| Conventional rule: AI 34.22 vs human 8.78 smells/kLoC, ratio 3.90x | `data/refocus_stats.json` | `rule_change.ratio_unrestricted` |
| Statement-per-block rule: 8.75 vs 8.78, ratio 1.00x | `data/refocus_stats.json` | `rule_change.ratio_logic` |
| The rule change divides the contrast by 3.91x, 95% CI [2.80, 5.69] | `data/refocus_stats.json` | `rule_change.fold_change`, `fold_change_ci` |
| 421 flagged blocks; 302 contain no executable statement | `data/additional/R11_specification_echo.json` | `n_flagged_blocks`, `n_statement_free` |
| Of those 302, 281 (93%) reproduce at least half their lines from the specification | `data/additional/R11_specification_echo.json` | `n_echo_at_least_half` |
| 182 reproduce every line; mean specification-line share 0.843 | `data/additional/R11_specification_echo.json` | `n_echo_every_line`, `mean_specification_line_share` |
| Echo is task-specific: against a foreign specification the share falls to 0.170 and the ≥half rate to 7.5% | `data/additional/R16_echo_permutation.json` | `foreign_mean_share`, `foreign_rate_at_least_half` |
| Line composition of the flagged blocks: 74.6% docstring/string, 20.0% logic | `data/additional/R5_duplicate_validation.json` | `composition_audit.line_kind_shares` |
| Logic-bearing duplication: 116 AI blocks vs 3 human, densities 8.75 vs 8.78 | `data/additional/R5_duplicate_validation.json` | `logic_bearing_only` |
| Pooled rank effect, logic-bearing: Cliff's delta = -0.042, CI [-0.121, 0.031] | `data/additional/E5_effect_size_cis.json` | `AI (pooled)` |
| Task-paired Wilcoxon, logic-bearing: p = 0.234; unrestricted: p < 0.001 | `data/additional/R8_rq1_task_paired.json` | `logic_bearing`, `unrestricted` |
| Ratio intervals: 1.00x [0.59, 1.90] logic-bearing, 3.90x [2.25, 7.94] conventional | `data/review_recomputations.json` | `headline_ratios` |
| Two of twelve producers emit 269 of the 302 blocks (89%) | `data/refocus_stats.json` | `boundary.echo_producers` |
| Newer six-model cohort: 17 statement-free blocks, ratio 0.77x conventional / 0.66x logic-bearing | `data/refocus_stats.json` | `boundary.open_cohort` |
| Repository study: 100 repositories, 9,407 classes, pooled ratio 0.62x | `data/oss_large.json` | `n_repos`, `pooled` |
| Repository block audit: 97 repositories, 0.83x conventional / 0.72x logic-bearing, 17.8% of blocks statement-free | `data/additional/R13_oss_audit.json` | `ratio_conventional`, `ratio_logic_bearing`, `statement_free_share_of_blocks` |
| Published duplication rules span 5.68x to 0.81x on this one corpus | `data/additional/R12_prior_rule_recompute.json` | `configs` |
| Specification-echo ablation on PolyCoder-2.7B | `data/additional/R15_spec_ablation_PolyCoder-2.7B.json` | `conditions` |

The research questions map onto the same files: **RQ1** is the conventional
measurement (`stats_results.json`, `E1`, `E5`, `E7`, `R8`), **RQ2** is the block
audit and the paired rule change (`R11`, `R16`, `R5`, `refocus_stats.json`), and
**RQ3** is where that change recurs (`refocus_stats.json` `boundary`,
`metrics_newcohort_logic.csv`, `oss_large.json`, `R13`, `R14`).

---

## The two duplication rules

Almost every file here stores results under **both** rules, because several of
them reverse between the two. Check which one you are reading before quoting a
number.

- **Unrestricted (conventional).** A repeated block of six or more lines counts,
  whatever those lines contain. This is what the detector does as specified, and
  what the comparable literature uses.
- **Logic-bearing (this paper's primary measure).** The same rule, plus the
  requirement that a block contain at least one executable statement. Blocks made
  entirely of docstring, string, signature, import or decorator lines are not
  counted.

In `stats_results.json`, `E1`–`E9`, `R1` and `R2`, the logic-bearing result sits
at the top level and the unrestricted result is nested under `"unrestricted"`.
Each file also carries a `"measure"` field naming the top-level rule.
`E4_threshold_sensitivity.json` is the exception: it re-detects from source at
rescaled thresholds, so it exists only under the unrestricted rule;
`E4b_threshold_greedy.json` is the corresponding sweep on the 1,083-class greedy
corpus the manuscript reports.

> **Nesting measure.** All files use the corrected, `elif`-aware nesting rule, in
> which a chained `elif` is a sibling branch rather than a deeper level — how
> Pylint's `R1702` reads it. The earlier rule that counted each `elif` link as a
> level deeper survives only as a legacy replication, in
> `additional/E13_elif_sensitivity.json` and `oss_large_legacy_elif.json`.

---

## Contents

```
README.md
data/
  metrics.csv                     per-class metrics, 1,183 classes, unrestricted rule
  metrics_logic_bearing.csv       the same 1,183 classes with the logic-bearing columns
  metrics_extended.csv            2,167 rows: adds the t=0.2 decoding condition
  metrics_newcohort_logic.csv     600 rows: the six-model 2025-26 replication cohort
  stats_results.json              RQ1 primary tests, both rules
  extra_stats.json                capability correlation, Friedman, bootstrap CIs
  refocus_stats.json              the paired rule change, per-producer inflation, boundaries
  review_recomputations.json      task-cluster intervals on the headline ratios
  review_addenda.json             heterogeneity, Skillings-Mack, occurrence/magnitude hurdle
  review_dependence.json          re-analyses that block on task instead of assuming independence
  validation.json                 agreement with the lizard analyser
  syntactic_validity.json         parse-success rate per producer
  test_pass.json                  ClassEval functional-correctness outcomes, 12 producers
  test_pass_newcohort.json        the same for the six newer models
  correctness_joint.csv           per-producer parse/test/smell outcomes as one table
  correctness_filtered.json       smell density on functionally correct classes only
  correctness_maint.json          correctness x maintainability cross-tabulation
  subset_cohorts_logic.json       the leave-out model subsets
  oss_large.json                  100-repository results (9,407 classes), primary
  oss_large_legacy_elif.json      the same repositories under the legacy nesting rule
  oss_large_no_vendored.json      the same repositories excluding vendored trees
  oss_candidates.json             the repository inventory, with commit SHAs pinned
  additional/
    E1_equivalence.json           TOST and Cliff's delta CI for the RQ1 null
    E2_posthoc_dunn.json          78 pairwise Dunn comparisons, BH-corrected
    E3_negative_binomial.json     count models M1/M2/M3 with a log(SLOC) offset
    E4_threshold_sensitivity.json thresholds rescaled 0.75x and 1.25x (extended corpus)
    E4b_threshold_greedy.json     the same sweep on the 1,083-class greedy corpus
    E5_effect_size_cis.json       per-producer and pooled Cliff's delta with CIs
    E6_power.json                 minimum detectable effect on the ordinal scale
    E7_duplicate.json             per-producer duplicated-code rates and odds ratios
    E8_normalisation.json         four alternative normalisations
    E9_friedman_with_human.json   Friedman test blocked on task
    E10_repayment.json            the prioritisation simulation
    E11_profile_uncertainty.json  Wilson intervals on the per-producer profiles
    E12_external_agreement.json   agreement with Pylint and Radon
    E13_elif_sensitivity.json     primary vs legacy elif counting (replication)
    E14_extended.json             twelve-producer corpus, decoding condition
    R1_task_clustered_nb.json     task-clustered NB, GEE, task random intercept
    R2_model_ladder.json          stepwise ladder with out-of-sample validation
    R3_distribution.json          mean-vs-rank diagnostics, leave-out subsets
    R4_manuscript_numbers.json    producer subsets, continuous indicators, per-100 kLoC
    R5_duplicate_validation.json  jscpd agreement, block composition audit, logic-only variant
    R6_m4_structure_adjusted.json producer identity with structure covariates
    R7_attrition_robustness.json  parse-failure attrition checks
    R8_rq1_task_paired.json       task-paired Wilcoxon counterpart of the RQ1 test
    R9_oss_delta_ci.json          cluster bootstrap for the repository Cliff's delta
    R10_oss_threshold_sensitivity.json  repository result across detector thresholds
    R11_specification_echo.json   the specification-echo audit of all 421 blocks
    R12_prior_rule_recompute.json four published duplication rules on this corpus
    R13_oss_audit.json            block-level audit of the repository corpus
    R13_oss_audit_unsorted.json   the same audit in log order, for the ordering check
    R14_oss_audit_stats.json      deterministic vs log-order comparison of R13
    R15_spec_ablation_PolyCoder-2.7B.json  specification-echo ablation
    R16_echo_permutation.json     foreign-specification permutation control
    all_additional.json           E1-E9 concatenated
figures/
  fig_mechanism.png               Figure 1 — the measurement split into its components
  fig_exponent_forest.png         the exponent on log(SLOC), against the smells/kLoC assumption
  fig_presence_or.png             per-class occurrence of each smell, AI vs human, as odds ratios
  fig_inflation.png               what the conventional rule adds to each producer
  fig_dup_by_producer.png         Duplicate Code by producer under the rule as specified
  fig_permodel_forest.png         producer density and Cliff's delta on the logic-bearing measure
  fig3_model_profile.png          Supplementary — smell composition by producer
```

Sixty-two files, 2.5 MB unpacked (1.4 MB as a zip). All figures are the 300 dpi
files the compiled manuscript loads.

---

## Data dictionary

### `metrics.csv` — 1,183 rows

One row per class: 100 human reference classes plus 1,083 classes from twelve
LLMs (GPT-4-Turbo, GPT-3.5-Turbo, Gemini-Pro, DeepSeek-Coder, StarCoder-15B,
InstructCodeGen-16B, Magicoder-6.7B, CodeLlama-13B, WizardCoder-15B,
CodeGeeX2-6B, PolyCoder-2.7B, SantaCoder-1.1B).

| Column | Meaning |
|---|---|
| `sample_id` | ClassEval task identifier (`ClassEval_0` … `ClassEval_99`) |
| `group` | `Human` or `AI` |
| `model` | producer name; `Human` for the reference implementations |
| `sloc` | source lines of code, counted as logical statements |
| `nom` | number of methods |
| `wmc` | weighted methods per class (summed cyclomatic complexity) |
| `nof` | number of fields |
| `n_smells` | total smell instances in the class, unrestricted rule |
| `td_density` | `n_smells / sloc * 1000` — smells per kLoC, unrestricted rule |
| `smell_LongMethod` … `smell_Duplicate` | instance count for each of the seven smells |

### `metrics_logic_bearing.csv` — the same rows, three columns added

| Column | Meaning |
|---|---|
| `dup_logic` | duplicated blocks containing at least one executable statement |
| `n_smells_logic` | total smell instances under the logic-bearing rule |
| `density_logic` | `n_smells_logic / sloc * 1000` — **the paper's primary measure** |

`metrics_extended.csv` adds a `decoding` column taking `reference`, `greedy` and
`t0.2`; filtering to `decoding in ('greedy','reference')` reproduces
`metrics.csv` exactly. `metrics_newcohort_logic.csv` has the logic-bearing
columns plus `dup_echo`, the count of statement-free blocks, for the six
2025–26 models (Qwen2.5-Coder 1.5B/3B/7B/14B, Granite-8B-Code, StarCoder2-15B).

**What this measures.** Seven statically detectable code smells, which
operationalise one structural dimension of technical debt. They are not a direct
measurement of technical debt; the column name `td_density` is retained only for
continuity with the earlier release.

**How density is aggregated.** Every density figure in the paper is the **mean
over classes**, so each class counts equally regardless of length. This is not
the same as pooling total smells over total SLOC, which weights long classes more
heavily. Use `df.groupby('model').density_logic.mean()` for the primary measure.

### Reproducing the headline contrast

```python
import pandas as pd
df = pd.read_csv('data/metrics_logic_bearing.csv')
ai, hu = df[df.group == 'AI'], df[df.group == 'Human']

ai.td_density.mean()    / hu.td_density.mean()      # 3.90  conventional rule
ai.density_logic.mean() / hu.density_logic.mean()   # 1.00  logic-bearing rule
```

The ratio of those two ratios is the paper's central quantity; its task-cluster
interval is in `refocus_stats.json` under `rule_change.fold_change_ci`.

---

## Provenance

The generated classes come from **ClassEval** (Du et al., ICSE 2024), a manually
constructed class-level Python benchmark. They are **not redistributed here** and
are available from the original authors:
<https://github.com/FudanSELab/ClassEval>. The files here are the derived
measurement over those classes.

The 100 open-source repositories are likewise not redistributed.
`oss_candidates.json` records each one with the commit SHA pinned at mining time,
so the corpus can be reconstructed exactly. AI authorship in those repositories
was attributed from commit trailers (`Co-authored-by: Copilot`, `Claude`,
`Cursor`, `Devin`, `Codex` and equivalents), with the human baseline restricted to
files added after each project began using AI assistance.

Seeds: `20260728` for the main analysis, `20260808` for the review-pass scripts
(`refocus_stats`, `review_addenda`, `review_recomputations`,
`review_dependence`), `20260809` for the permutation control in `R16`. Reruns are
byte-identical except for the two repository scripts, which depend on the
upstream repositories being reachable at the pinned SHAs.

---

## Scope and known limitations

Stated here so the data are not read for more than they support.

- **This is a measurement result, not a maintainability result.** Showing that
  the apparent duplication excess is specification echo does not establish that
  AI-generated and human-written code are equally maintainable.
- **Python only, and one benchmark.** ClassEval is a Python class-level
  benchmark whose prompts embed the specification in the code skeleton. That
  design is what makes specification echo detectable here, and is also why the
  magnitude should not be extrapolated to other corpora.
- **The human comparison group is a single reference solution per task**, written
  by the benchmark's authors. Between-human variance cannot be estimated from it.
- **The effect is concentrated.** Two of twelve producers (PolyCoder-2.7B and
  SantaCoder-1.1B) account for 269 of the 302 statement-free blocks. The newer
  cohort emits 17. Read the pooled figure as a property of this producer mix.
- **Equal ranks are not equivalence.** The logic-bearing Cliff's delta interval
  lies inside the conventional negligible band, but the mean-ratio interval is
  wide ([0.59, 1.90] in `review_recomputations.json`). Do not read it as proven
  absence of an effect.
- **Some per-producer profiles rest on few instances.** GPT-4-Turbo and
  DeepSeek-Coder contribute 9 and 10 smell instances. Confidence intervals are in
  `additional/E11_profile_uncertainty.json`; their composition percentages
  indicate shape, not magnitude.
- **The repository comparison is a boundary check, not a replication.** AI
  authorship there is machine-attributed from commit trailers, the unit is a
  whole file rather than a task-matched class, and no prompt text is available,
  so specification echo cannot be measured the same way.

---

## What is not in this archive

| Excluded | Reason |
|---|---|
| `src/`, `reproduce.py`, `requirements.txt` | This is a data-and-figures release. The code is available from the corresponding author on request. |
| ClassEval generations and reference solutions | They belong to the benchmark; fetched from it, not copied here. |
| Cloned repositories (~31 GB) | Not ours to redistribute, and reconstructible from the pinned SHAs. |
| `metrics_newcohort.csv` | A partial intermediate covering only the first of the six newer models. `metrics_newcohort_logic.csv` is the complete cohort. |
| The 40-repository pilot | Superseded by the 100-repository run the paper reports; shipping both would invite a comparison between two inconsistent numbers. |

---

## Citation

```bibtex
@article{kim2026echo,
  title   = {Specification echo inflates duplicate-code measurements in
             {LLM}-generated classes: an audit and a logic-bearing counting rule},
  author  = {Kim, Dong Kwan},
  journal = {Empirical Software Engineering},
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
