# ai-code-td

Derived data and figures for

> **"Specification Echo in Duplicate-Code Measurement of LLM-Generated Classes:
> A Measurement-Validity Audit on ClassEval"**
> Dong Kwan Kim, submitted to the *International Journal of Software
> Engineering and Knowledge Engineering*.

Everything is in [`ai-code-td-public/`](ai-code-td-public/). Start with its
[`README.md`](ai-code-td-public/README.md), which maps each result reported in
the manuscript to the file and key that store it.

| Path | Contents |
|---|---|
| [`ai-code-td-public/results/task_class/`](ai-code-td-public/results/task_class/) | Primary analysis: per-class metrics under both counting rules and every flagged block with its line labels (1,081 AI and 100 reference classes). |
| [`ai-code-td-public/results/whole_file_config/`](ai-code-td-public/results/whole_file_config/) | The whole-file configuration compared in the manuscript's scope table. |
| [`ai-code-td-public/results/repository/`](ai-code-td-public/results/repository/) | Repository boundary check, with pinned commit SHAs. |
| [`ai-code-td-public/results/later_cohort/`](ai-code-td-public/results/later_cohort/) | Later model-and-prompt cohort. |
| [`ai-code-td-public/docs/diagnostic_results.pdf`](ai-code-td-public/docs/diagnostic_results.pdf) | Diagnostic results cited in the manuscript as Tables S1–S7 and Figure S1. |
| [`ai-code-td-public/DATA_DICTIONARY.md`](ai-code-td-public/DATA_DICTIONARY.md) | Every column and every result file. |

## Not included

- **Source code** (detector, line classifier, analysis scripts): available from
  the author on request.
- **ClassEval generations**: obtain them from the benchmark authors
  (<https://github.com/FudanSELab/ClassEval>).
- **Mined repositories**: not redistributed; the pinned commit SHAs allow the
  sample to be reconstructed.

## License

The derived data and figures are released under the MIT License
([`ai-code-td-public/LICENSE`](ai-code-td-public/LICENSE)). The ClassEval
benchmark and the mined repositories keep their own terms.
