# What is public, and what is not

The manuscript's Data availability statement says:

> The derived data and figures are publicly available at
> https://github.com/Dongkwan/ai-code-td, with a data dictionary and a mapping
> from the reported results to their data files. The source code is available
> from the corresponding author on request. The ClassEval generations and the
> mined repositories are not redistributed; the repository inventory records the
> pinned commit SHAs.

`ai-code-td/` in this directory is the **full working package** — detector,
analysis scripts, derived data, class sources. It is what the author sends when
someone requests the code. **It is not the public release.**

## Publishing it

Do not push `ai-code-td/` as it stands. Its `README.md` describes a package that
ships the code, which the manuscript no longer claims. Publish only:

| Publish | Withhold |
|---|---|
| `results/` (derived data, both counting rules side by side) | `src/` |
| `figures/` (the figures as they appear in the paper) | the driver scripts at the package root |
| a data dictionary and result-to-file mapping | `srcdump/` (the 1,183 class sources) |
| `LICENSE` | |

`srcdump/` is withheld because it is a redistribution of the ClassEval
generations under a different name, which the Data availability statement says
is not done.

## If the policy changes

If the code is released after all, three places state the policy and all three
must move together — the manuscript's Data availability statement, the detector
subsection in Section 3.3 ("available on request"), and the Artifacts paragraph
of the cover letter. `ai-code-td/README.md` would also need its opening
rewritten. The two review documents recorded the earlier state, in which some of
these said one thing and some another, as a P0 defect; keeping them in step is
the point.
