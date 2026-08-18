# EMSE submission package

Empirical Software Engineering (Springer) version of the manuscript previously
prepared for Information and Software Technology. The IST tree at
`D:\dongkwan\papers\ist\paper\latex` is untouched; this is a separate copy.

## Build

```
pdflatex main && bibtex main && pdflatex main && pdflatex main
pdflatex supplementary && bibtex supplementary && pdflatex supplementary && pdflatex supplementary
pdflatex cover_letter
```

Current output: `main.pdf` 30 pages, `supplementary.pdf` 17 pages,
`cover_letter.pdf` 2 pages. No LaTeX errors, no undefined references,
no overfull boxes.

## What changed from the IST version

### Format (svjour3, the class EMSE points authors to)

| | IST | EMSE |
|---|---|---|
| Class | `elsarticle` `[review,preprint,12pt]` | `svjour3` `[smallextended,natbib]` |
| Front matter | `frontmatter` / `ead` / `affiliation` | `title` / `author` / `institute` / `maketitle` |
| Abstract | labelled Context--Objectives--Methods--Results--Conclusion | one paragraph (EMSE house style) |
| Highlights | five bullets | removed; Springer has no highlights |
| Citations | numeric `\cite` | author-year `\citep` / `\citet`, `spbasic.bst` |
| End matter | CRediT + competing-interest + AI-use sections | `Declarations` block (funding, conflict, ethics, data availability, generative-AI use) |
| Spacing | double-spaced review layout, 49 pp | single-spaced, 30 pp, line numbers on |

Seventeen citations where the author's name is already in the sentence were
converted to `\citet` so the name is not printed twice; the rest are `\citep`.
Two tables were re-fitted to the narrower svjour3 measure.

Switches left in the preamble: add `referee` to the class options for a
double-spaced copy; comment out `\linenumbers` for a camera-ready version.

### Content (the reframing agreed after the IST desk rejection)

1. **Title** now names the contribution rather than the correction:
   *Specification echo inflates duplicate-code measurements in LLM-generated
   classes: an audit and a logic-bearing counting rule.*
2. **Abstract** ends on what the reader gets (the rule, and the instruction to
   report duplication by producer and setting) instead of ending on four
   sentences of self-limitation.
3. **Introduction** states on page 1 that the paper *supplies* a counting rule
   and an audit procedure, and introduces the two corpora together, so the
   9,407-class repository study is visible before the benchmark framing sets.
4. **Contribution 3** is retitled "The boundaries of the effect, in three
   settings" and says explicitly that the artifact does not appear outside the
   benchmark at this sample size.
5. **Implications** section 1 now says how to obtain the rule in practice: a
   filter on returned blocks, not a detector option, because a Python docstring
   is a string literal rather than a comment.

No result, number, or claim was changed. Every figure in the abstract was
checked against the body text.

## Not done (decide before submitting)

- **Length.** 30 pages is within EMSE norms, so the robustness material was left
  in the manuscript. Moving Section 4.4 (robustness checks) to the supplement
  would save roughly two pages if a reviewer asks.
- **Code release.** The repository is a *data and figures* release; the
  detector and the analysis scripts are not in it. The manuscript's Data
  availability statement and the cover letter now say exactly that (an earlier
  draft of both claimed the code was public, which it is not). EMSE reviewers
  tend to ask for the code, and the paper's contribution is a counting rule, so
  releasing `src/` would strengthen the submission. The repository README at
  `D:\dongkwan\papers\ist\papereleasei-code-td-2026-08-10\README.md`
  was updated for the new title and venue and describes the current, data-only
  scope.
- **ORCID.** Springer's Editorial Manager will ask for it at submission.
