# ai-code-td — derived data and figures

Derived data and figures for

> **"Specification Echo in Duplicate-Code Measurement of LLM-Generated Classes:
> A Measurement-Validity Audit on ClassEval"**
> Dong Kwan Kim, submitted to the *International Journal of Software
> Engineering and Knowledge Engineering*.

Every number quoted in the manuscript is a value stored in one of the files
here, so the claims can be checked by reading the data rather than by running
anything. `DATA_DICTIONARY.md` documents every column and every result file,
and `MANIFEST.json` states what each file backs. The diagnostic results that
the manuscript cites as Tables S1–S7 and Figure S1 are in
`docs/diagnostic_results.pdf`.

## Layout

| Path | Contents |
|---|---|
| `results/task_class/` | **Primary analysis.** Detection, size and all smells restricted to the class each ClassEval task requests (1,081 AI and 100 reference classes). |
| `results/whole_file_config/` | The whole-file configuration of an earlier version of the analysis (detection over the whole file, size and other smells from the largest class; 1,083 AI classes). Reported only in Table 7. |
| `results/scope_compare.json` | Every estimate under both configurations side by side. |
| `results/repository/` | The repository boundary check (100 repositories, 89 after deduplication). |
| `results/later_cohort/` | The later model-and-prompt cohort (six models, 600 classes). |
| `figures/` | Figures as they appear in the manuscript and the diagnostic document. |
| `docs/diagnostic_results.pdf` | Tables S1–S7 and Figure S1. |

Each of `task_class/` and `whole_file_config/` contains
`per_class_both_rules.csv` (per-class counts and densities under both counting
rules) and `flagged_blocks.csv` (every flagged block with its line labels,
statement-free flag and skeleton match share).

## Where the main results live

All values refer to the task class unless stated.

| Result in the manuscript | Value | File and key |
|---|---|---|
| Flagged blocks / statement-free / matching the skeleton | 267 / 198 / 181 | `task_class/flagged_blocks.csv`; `task_class/main_results.json` → `blocks.n`, `blocks.statement_free`, `echo.own_match_half_any` |
| AI Duplicate Code density, both rules | 15.36 → 1.49 blocks per 1,000 statements | `main_results.json` → `duplication.ai_dup_conv`, `duplication.ai_dup_stmt` |
| AI–reference duplication difference, both rules | 14.9 → 1.1 | `main_results.json` → `duplication.diff_dup_conv`, `duplication.diff_dup_stmt` |
| Total smell ratio, both rules | 2.29× → 0.71× | `main_results.json` → `duplication.ratio_tot_conv`, `duplication.ratio_tot_stmt` |
| Descriptive rule change | 3.23× | `main_results.json` → `duplication.tot_rule_change` |
| Size-adjusted policy contrast, common size range | 1.14×–1.53× | `size_models.json` → `models_common.*.point.policy` |
| Size exponent of log(statement count) | 2.70 (statement-aware), 0.81 | `size_exponent.json` |
| Rule change without the two dominant producers / complete outputs | 1.21× / 1.12× | `main_results.json` → `subsets.without_dominant_two`, `subsets.complete_only` |
| Detection-only scope: duplication differences | 32.8 and 5.6 | `detection_only.json` → `diff_dup_conv`, `diff_dup_stmt` |
| Whole-file configuration column of Table 7 | — | `scope_compare.json` |
| Line-exclusion policies (Table 8) | 0.65×–3.94× | `line_exclusion_policies.json` → `configs` |
| Maximal repeated spans | 345 spans | `segmentation.json` |
| jscpd clones (matched setting) | 72 | `jscpd.json` → `matched` |
| Producer heterogeneity (Skillings–Mack) | 28.44 | `producers.json` |
| Repository check: paired difference in statement-free share | +1.4 points | `repository/statement_free_share.json` → `deduplicated.share.mean_paired_diff` |
| Later cohort: statement-free blocks | 17 in 600 classes | `later_cohort/metrics_newcohort_logic.csv` |
| Size-measure sensitivity (physical lines with and without docstrings) | 13.4 → 0.9; 0.54× | `size_measure_sensitivity.json`; per-class measures in `size_measures.json` |
| Equal-task-weight estimates | 2.26× → 0.69×; 3.27× | `task_weighted.json` |
| Actual Pylint 2.14.5 runs (no within-file duplicates by design) | 0 | `pylint_actual.json` → `counts.r0801` |
| Documented references: duplication difference; total smell ratio | 13.3; 1.92× (0.80× with docstring lines in size) | `documentation_asymmetry.json` → `C_estimates` |
| AI blocks repeated in the skeleton / echo added / generated | 30 / 155 / 79 | `documentation_asymmetry.json` → `B_blocks` |
| Task-indicator Poisson M0: refits converging (IRLS / Newton) | 224 / 300; [1.03, 1.73] | `task_effects_convergence.json` |
| Repository sample flow; share difference over all 89 | 100/97/89/79; +2.7 points | `repository/repository_flow.json` |

## The two counting rules

`per_class_both_rules.csv` carries both counts for every class:

- `dup_conv` — every flagged block (documentation-preserving rule)
- `dup_stmt` — only blocks containing at least one implementation statement
  (statement-aware rule)
- `tot_conv = other6 + dup_conv`, `tot_stmt = other6 + dup_stmt`
- `d_*` columns are the same counts per 1,000 statements (`/ sloc * 1000`)

The column `sloc` holds the statement count (Stmt in the manuscript): the number of AST statement nodes in the class, excluding docstring
expression nodes; it is not a count of physical lines. The manuscript's
densities are arithmetic means of these per-class densities.

## Collection and preprocessing

- **ClassEval release.** The class-level generations shipped with the
  benchmark under `output/model_output_v1.0.0`, files
  `<model>_class_H_greedy.json` (holistic strategy, greedy decoding), for the
  twelve models listed in the manuscript; task data from
  `data/ClassEval_data.json`.
- **Extraction.** The ClassEval protocol: the text after the response marker,
  then the fenced code block if present, else the raw completion.
- **Salvage.** If the extracted text does not parse, the longest parseable
  prefix that still contains a class or function is kept; outputs with no such
  prefix are excluded. `per_class_both_rules.csv` flags salvaged classes.
- **Task class.** The top-level class whose name the task requests; the last
  definition when a file defines it more than once. Identification and dropped
  files: `task_class/task_scope.json`.
- **Test environment.** Python 3.10.11, NumPy 2.2.6, 25-second limit per task.
- **Software versions.** Analysis: Python 3.10.11, numpy 2.2.6, pandas 2.3.3,
  scipy 1.15.3, statsmodels 0.14.6, matplotlib 3.10.8. Comparison detector:
  jscpd 4.0.5. Pylint's default similarity policy is emulated as in
  version 2.14.5.
- **Repositories.** GitHub search results taken in order until 100 repositories
  had at least eight analyzable files per group; pinned commit SHAs in
  `repository/oss_large.json`.

## What is not here, and why

- **Detector and analysis code.** Available from the author on request, as the
  manuscript's Data Availability statement says.
- **ClassEval generations.** Third-party artifacts under their own terms; obtain
  them from the benchmark authors (<https://github.com/FudanSELab/ClassEval>).
  Block records give positions and line labels, not the text of the blocks.
- **Mined repositories.** Not redistributed; `repository/oss_large.json`
  records the pinned commit SHA of each repository so the sample can be
  reconstructed exactly.

## License

The derived data and figures are released under the MIT License (`LICENSE`).
The ClassEval benchmark and the mined repositories keep their own terms.
