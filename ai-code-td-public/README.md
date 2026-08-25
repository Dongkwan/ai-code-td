# ai-code-td — derived data and figures

Derived data and figures for

> **"Measuring duplicate code in LLM-generated Python classes: specification
> echo and a statement-aware rule"**
> Dong Kwan Kim, *Software Quality Journal* (submitted).

Every number quoted in the paper is a value stored in one of the files here, so
the claims can be checked by reading the data rather than by running anything.
`DATA_DICTIONARY.md` documents every column and every result file, and
`MANIFEST.json` records why each file is here.

A file is included when a number reported in the manuscript **or in the
supplement** is stored in it, or when the Data availability statement promises
it, and a figure is included when a document uses it. Intermediate selection
files and analyses no document reports are not here; they accompany the source
code. `MANIFEST.json` lists the manuscript-backing and supplement-backing files
separately.

## What the paper found, and where each number lives

A duplicate-code detector decides whether two spans of a file are identical, not
what those spans are. A class generated from a written specification carries
that specification in its docstrings, so a model that repeats its instructions
across methods writes identical text that a line-based detector reads as a
clone. The paper calls this *specification echo* and asks how much of a
published AI–human duplication contrast depends on counting it.

| Result in the paper | Value | File |
|---|---|---|
| Density ratio, conventional rule | 3.90× | `results/metrics.csv` |
| Density ratio, statement-aware rule | 1.00× | `results/metrics_logic_bearing.csv` |
| Paired rule change on density | 3.91× [2.82, 5.67] | `results/sqj_revision.json` |
| Size-adjusted rule change (AI × rule) | 1.27× [1.04, 1.54] | `results/sqj_interaction.json` |
| Size exponent differs by counting rule | p < 1e-10 | `results/sqj_interaction.json` |
| Flagged blocks / statement-free | 421 / 302 | `results/sqj_revision.json` |
| Cutoff sensitivity (Table 5) | 1.00×, 0.86×, 0.81× | `results/sqj_revision.json` |
| Leave-one-producer-out | [0.78, 1.06] | `results/sqj_revision.json` |
| Specification-match rates | 281/302, 7.5% | `results/additional/R11_specification_echo.json` |
| Block composition audit | 74.6% docstring lines | `results/additional/R5_duplicate_validation.json` |
| Detector agreement with jscpd | κ = 0.57, ρ = 0.91 | `results/additional/R5_duplicate_validation.json` |
| Published rules re-measured | 5.68× to 0.81× | `results/additional/R12_prior_rule_recompute.json` |
| Repository study | 9,407 classes, 100 repos | `results/oss_large.json` |
| Later model-and-prompt cohort | 17 blocks / 600 classes | `results/metrics_newcohort_logic.csv` |

Every table in the supplement is backed the same way; `MANIFEST.json` maps each
supplement section (S1--S7) to the files behind it.

## The two counting rules, side by side

`results/metrics_logic_bearing.csv` carries both counts for every class, which
is what makes the paper's central comparison recomputable from data alone:

- `smell_Duplicate` — every flagged block (the conventional rule)
- `dup_logic` — only blocks containing at least one executable statement
- `n_smells_logic = n_smells - smell_Duplicate + dup_logic`
- `density_logic = n_smells_logic / sloc * 1000`

`sample_id` repeats across producers, because all twelve solve the same 100
tasks. **The class key is `model` plus `sample_id`**; joining on `sample_id`
alone silently aggregates across producers and will not reproduce the paper.

## What is not here

- **The source code.** The detector, the mining pipeline and the analysis
  scripts are available from the corresponding author on request.
- **The ClassEval generations.** They belong to the benchmark authors and are
  available from them (<https://github.com/FudanSELab/ClassEval>). For the same
  reason, the block audit in `results/additional/R5_duplicate_validation.json`
  keeps each example's file, producer, line numbers and line classification but
  not its source lines.
- **The mined repositories.** `results/oss_large.json` records the pinned commit
  SHA for each of the 100 repositories, so the sample can be reconstructed
  exactly rather than re-sampled.
- **Intermediate files.** Candidate-selection lists, unsorted audit dumps and
  analyses that no document reports are not published; they are produced by the
  scripts and go with them on request.

## Licence

The derived data and figures in this package are released under the MIT Licence
(see `LICENSE`). The ClassEval benchmark and the mined repositories keep their
own terms; nothing here relicenses them.
