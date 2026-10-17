# Data dictionary

Generated from the published files. Densities in the manuscript are arithmetic means of
per-class densities, not total instances divided by total statement count.

## `results/later_cohort/metrics_newcohort_logic.csv`

600 rows, 20 columns.

| Column | Meaning |
|---|---|
| `sample_id` | ClassEval task identifier (`ClassEval_<n>`); with `model` it identifies a class. |
| `group` | `AI` or `Human`. |
| `model` | Producer name, or `Human` for the benchmark reference solution. |
| `sloc` | Size: number of AST statement nodes in the class, excluding docstring expression nodes (not physical lines). |
| `nom` | Number of methods. |
| `wmc` | Weighted methods per class (summed cyclomatic complexity). |
| `nof` | Number of fields. |
| `n_smells` | Total smells, documentation-preserving duplication rule. |
| `td_density` | `n_smells / sloc * 1000`. |
| `smell_LongMethod` | Long Method instances. |
| `smell_HighComplexity` | High Complexity instances. |
| `smell_ComplexConditional` | Complex Conditional instances. |
| `smell_GodClass` | God Class instances. |
| `smell_LongParamList` | Long Parameter List instances. |
| `smell_DeepNesting` | Deep Nesting instances. |
| `smell_Duplicate` | Duplicate Code blocks, documentation-preserving rule (every flagged block). |
| `dup_logic` | Duplicate Code blocks, statement-aware rule (blocks with at least one implementation statement). |
| `dup_echo` | Statement-free blocks matching the task skeleton. |
| `n_smells_logic` | Total smells, statement-aware rule. |
| `density_logic` | `n_smells_logic / sloc * 1000`. |

## `results/later_cohort/test_pass_newcohort.json`

later cohort test outcomes (pass@1).

Top-level keys: `Qwen2.5-Coder-1.5B`, `Qwen2.5-Coder-3B`, `Qwen2.5-Coder-7B`, `Granite-8B-Code`, `Qwen2.5-Coder-14B`, `StarCoder2-15B`

## `results/repository/oss_large.json`

repository sample, per-repository results and pinned commit SHAs.

Top-level keys: `n_repos`, `n_ai_classes`, `n_human_classes`, `pooled`, `repo_level`, `project_scale`, `per_agent`, `smell_share_ai`, `smell_share_human`, `per_repo`

## `results/repository/copy_verification.json`

verification of repository copies used for deduplication.

Top-level keys: `repos`, `groups`

## `results/repository/statement_free_share.json`

statement-free share, AI minus human, deduplicated repositories (Section 4.3.9).

Top-level keys: `attribution`, `candidate_groups`, `duplicate_groups`, `copies_verified`, `all`, `deduplicated`, `pooled_classes`, `pooled_classes_deduplicated`, `kept_from_each_group`, `n_deduplicated`

## `results/scope_compare.json`

every estimate under the whole-file configuration and the task class side by side (Table 7).

Top-level keys: list of 83 records

## `results/task_class/external_agreement.json`

agreement with Pylint and Radon (Section 3.2).

Top-level keys: `n_classes`, `per_smell`, `config`, `not_externally_comparable`, `summary`

## `results/task_class/line_exclusion_policies.json`

line-exclusion policies emulated (Table 8).

Top-level keys: `note`, `configs`

## `results/task_class/flagged_blocks.csv`

267 rows, 16 columns.

| Column | Meaning |
|---|---|
| `model` | Producer name, or `Human` for the benchmark reference solution. |
| `class_key` | `<model>__<sample_id>`. |
| `group` | `AI` or `Human`. |
| `task` | ClassEval task number. |
| `line_index` | Position of the flagged copy in the class's stripped-line sequence (six-line rule, Section 3.2). |
| `first_seen_at` | Position of the first occurrence of the same six-line window. |
| `lines_decorator` | Lines labelled decorator. |
| `lines_docstring_or_string` | Lines labelled documentation. |
| `lines_logic` | Lines labelled implementation statement. |
| `lines_signature` | Lines labelled signature. |
| `lines_validation_boilerplate` | Lines labelled validation boilerplate (`raise`, `assert`, `pass`). |
| `ast_stmt_lines` | Lines covered by an AST statement node (independent classifier, Section 3.3). |
| `statement_free` | True if the block has no implementation-statement line. |
| `skeleton_match_share` | Share of the block's six lines equal (normalized) to a line of its own task skeleton. |
| `skeleton_prose_share` | The same share counting skeleton docstring (prose) lines only. |
| `skeleton_match` | `skeleton_match_share >= 0.5`. |

## `results/task_class/detection_only.json`

detection-only scope column (Table 7).

Top-level keys: `diff_dup_conv`, `diff_dup_stmt`, `ratio_tot_conv`, `ratio_tot_stmt`, `tot_rule_change`, `ai_dup_conv`, `ai_dup_stmt`, `blocks`, `blocks_stmt`, `n_ai`

## `results/task_class/discordance.json`

docstring exclusion vs statement-aware blocks (Section 5.2).

Top-level keys: `scope`, `sa_blocks`, `sa_overlapped`, `sa_not_overlapped`, `not_overlapped_line_kinds`, `not_overlapped_lines`, `overlap_share_of_statement_lines`, `same_statement_lines_as_an_overlapping_dx_block`

## `results/task_class/location_estimators.json`

robust location estimators (Section 4.1).

Top-level keys: `scope`, `conv`, `stmt`

## `results/task_class/producers.json`

Skillings-Mack and post-hoc producer pairs (Section 4.3.6).

Top-level keys: `scope`, `skillings_mack`, `posthoc`, `significant_pairs`

## `results/task_class/table_values.json`

values printed in Tables 2 and 4.

Top-level keys: `scope`, `dataset`, `audit`, `first_occ`, `examples`, `presence`, `dup_share_of_instances`, `per_producer_dup_density_conv`, `all_classes_no_smell_stmt`

## `results/task_class/window_lengths.json`

window lengths 4, 6 and 8 (Section 6).

Top-level keys: `scope`, `replicates`, `W4`, `W6`, `W8`, `copies_overlapping_own_first_occurrence`

## `results/task_class/size_measure_sensitivity.json`

key estimates with physical-line size measures (Section 4.3.3).

Top-level keys: `measures`, `describe`, `correlation_with_sloc`, `descriptive`, `reproduction_descriptive`, `common_range_models`, `reproduction_common_models`

## `results/task_class/size_measures.json`

per-class statement count and physical-line size measures (Section 4.3.3).

list of 1181 records; fields: `key`, `model`, `group`, `task`, `sloc`, `phys_ncnb`, `phys_ncnb_nodoc`, `phys_all`, `doc_lines`, `dup_conv`, `dup_stmt`, `other6`

## `results/task_class/documentation_asymmetry.json`

skeleton baseline, decomposition of the 264 AI blocks (repeated in skeleton / echo added / generated), and AI vs references given the skeleton docstrings (Section 4.3.4). `B_per_block` lists every AI block with its category.

Top-level keys: `A_skeleton`, `B_blocks`, `C_documented_reference`, `C_estimates`, `B_per_block`

## `results/task_class/task_weighted.json`

equal-task-weight and within-task estimates (Section 4.3.1).

Top-level keys: `definition`, `by_measure`, `existing`

## `results/task_class/task_effects_convergence.json`

task-indicator Poisson M0 refits: IRLS convergence, failure diagnostics and Newton-Raphson refits on the same 300 resamples per range (Section 4.3.2). `per_replicate` holds every refit.

Top-level keys: `full`, `common`

## `results/task_class/task_effects_separation.json`

task-indicator Poisson M0: all-zero tasks dropped, linear-programming separation check and state of the fits IRLS does not finish (Section 4.3.2, Table S10). `per_replicate` holds every refit; `full.tail` locates the near-zero fitted means on the size axis.

Top-level keys: `full`, `common`

## `results/task_class/pylint_actual.json`

actual Pylint 2.14.5 duplicate-code runs and controls (Section 5.1). `self_compare_*` (its similarity engine applied to a file paired with itself, diagonal and mirrored matches dropped) is exploratory and not used in the manuscript.

Top-level keys: `pylint_version_output`, `controls`, `self_compare_engine`, `counts`, `pylint_runs`, `self_compare_densities`, `emulated_row_table8`, `per_class`

## `results/repository/repository_flow.json`

repository sample flow 100/97/89/79 and share estimates over all 89 (Sections 3.5, 4.3.9).

Top-level keys: `flow`, `paired_79_reproduced`, `paired_79_published`, `paired_79_identical`, `published_all_97_share`, `published_all_97_reproduced_identical`, `note_on_minus_0_7`, `shares`

## `results/task_class/policy_stability.json`

size-adjusted policy contrast with 2,000 refits from a new seed, and after dropping the same influential classes under both rules (Sections 4.3.2, 6; Table S9).

Top-level keys: `A_common_2000`, `B_cooks_full`, `B_cooks_common`

## `results/later_cohort/later_cohort_status.json`

per-model parse, salvage, completeness, test and statement-free counts of the later cohort (Table S8).

Top-level keys: `Qwen2.5-Coder-1.5B`, `Qwen2.5-Coder-3B`, `Qwen2.5-Coder-7B`, `Qwen2.5-Coder-14B`, `Granite-8B-Code`, `StarCoder2-15B`

## `results/repository/repository_heads.json`

HEAD commit of each of the 97 repositories still public at the re-audit, and the history cutoff (Section S7).

Top-level keys: `cutoff`, `note`, `repositories`

## `results/task_class/metrics.csv`

1181 rows, 16 columns.

| Column | Meaning |
|---|---|
| `sample_id` | ClassEval task identifier (`ClassEval_<n>`); with `model` it identifies a class. |
| `group` | `AI` or `Human`. |
| `model` | Producer name, or `Human` for the benchmark reference solution. |
| `sloc` | Size: number of AST statement nodes in the class, excluding docstring expression nodes (not physical lines). |
| `nom` | Number of methods. |
| `wmc` | Weighted methods per class (summed cyclomatic complexity). |
| `nof` | Number of fields. |
| `n_smells` | Total smells, documentation-preserving duplication rule. |
| `td_density` | `n_smells / sloc * 1000`. |
| `smell_LongMethod` | Long Method instances. |
| `smell_HighComplexity` | High Complexity instances. |
| `smell_ComplexConditional` | Complex Conditional instances. |
| `smell_GodClass` | God Class instances. |
| `smell_LongParamList` | Long Parameter List instances. |
| `smell_DeepNesting` | Deep Nesting instances. |
| `smell_Duplicate` | Duplicate Code blocks, documentation-preserving rule (every flagged block). |

## `results/task_class/per_class_both_rules.csv`

1181 rows, 18 columns.

| Column | Meaning |
|---|---|
| `model` | Producer name, or `Human` for the benchmark reference solution. |
| `sample_id` | ClassEval task identifier (`ClassEval_<n>`); with `model` it identifies a class. |
| `group` | `AI` or `Human`. |
| `sloc` | Size: number of AST statement nodes in the class, excluding docstring expression nodes (not physical lines). |
| `salvaged` | True if the class comes from a salvaged (longest parseable) prefix. |
| `methods_wanted` | Methods the ClassEval skeleton requests. |
| `methods_implemented` | Requested methods present with a non-empty body. |
| `complete` | Structurally complete: every requested method has a non-empty body. |
| `passes` | Passes all ClassEval tests when re-executed. |
| `other6` | The six smells other than Duplicate Code, summed. |
| `dup_conv` | Duplicate Code blocks, documentation-preserving rule. |
| `dup_stmt` | Duplicate Code blocks, statement-aware rule. |
| `tot_conv` | Total smells, documentation-preserving rule. |
| `tot_stmt` | Total smells, statement-aware rule. |
| `d_dup_conv` | Per 1,000 statements (`/ sloc * 1000`) of `dup_conv`. |
| `d_dup_stmt` | Per 1,000 statements of `dup_stmt`. |
| `d_tot_conv` | Per 1,000 statements of `tot_conv`. |
| `d_tot_stmt` | Per 1,000 statements of `tot_stmt`. |

## `results/task_class/statement_criterion.json`

statement-criterion variants (Section 4.3.2; Table S7).

Top-level keys: `stmt_or_def`, `stmt>=1`, `share>=0.25`, `share>=0.5`

## `results/task_class/jscpd.json`

jscpd 4.0.5 clone counts and contrasts (Section 4.3.5).

Top-level keys: `matched`, `default`

## `results/task_class/main_results.json`

corpus flags, block audit, duplication contrasts, subsets, count models, echo, later cohort (Tables 2, 4, 6; Sections 4.1-4.3).

Top-level keys: `corpus`, `blocks`, `duplication`, `transitions`, `subsets`, `models`, `echo`, `newcohort`, `annotation_sample_sizes`

## `results/task_class/paired_contrasts.json`

task-paired contrasts and presence odds ratios (Section 4.3.1).

Top-level keys: `item5`, `item2`, `item1`

## `results/task_class/size_models.json`

size-adjusted models, full and common size range (Table 5; Table S2).

Top-level keys: `size`, `item3`, `item4`, `item5`, `item5_all_tasks_wo_env`, `models_full`, `common_support`, `models_common`

## `results/task_class/size_sensitivity.json`

dense core, extrapolation and task fixed effects (Section 4.3.2; Table S1).

Top-level keys: `item5_3_stubs`, `itemE_boundary`, `itemD_scope`, `item6_discordance`, `item5_2_task_effects`, `item4_3_support`, `item4_1_extrapolation`

## `results/task_class/statement_aware_contrasts.json`

statement-aware contrasts, compact form (Section 4.3.1).

Top-level keys: `diff_conv`, `diff_stmt`, `any_ai_stmt`, `any_hu_stmt`, `ratio_conv`, `ratio_stmt`, `rule_change`, `blocks_conv`, `blocks_stmt`, `ai_classes_any_stmt`, `passing_and_complete`, `passing_ai`, `multi_class_by_model`

## `results/task_class/multiclass_files.json`

multi-class file diagnostics (Section 3.2).

Top-level keys: `multi_class_files`, `largest_not_task_class`, `without_multi_class`, `without_not_task_class`

## `results/task_class/segmentation.json`

maximal repeated spans, duplicated lines per 1,000 statements (Section 4.3.5).

Top-level keys: `ai_spans_all`, `hu_spans_all`, `diff_spans_all`, `ai_spans_stmt`, `hu_spans_stmt`, `diff_spans_stmt`, `ai_lines_all`, `hu_lines_all`, `diff_lines_all`, `ai_lines_stmt`, `hu_lines_stmt`, `diff_lines_stmt`, `n_spans`, `n_statement_free_spans`, `duplicated_lines`, `duplicated_lines_in_statement_free_spans`, `statement_free_spans_by_model`, `statement_free_spans_from_dominant_two`, `human_classes_with_span`, `ai_classes_with_span`

## `results/task_class/size_exponent.json`

free log(Stmt) exponent under both rules (Section 3.4; Figure S1).

Top-level keys: `statement_aware`, `documentation_preserving`

## `results/task_class/syntactic_validity.json`

parse and salvage counts per producer (Table 2, Salv.).

Top-level keys: `Human`, `GPT-4-Turbo`, `DeepSeek-Coder`, `StarCoder-15B`, `InstructCodeGen-16B`, `Magicoder-6.7B`, `CodeLlama-13B`, `GPT-3.5-Turbo`, `Gemini-Pro`, `WizardCoder-15B`, `CodeGeeX2-6B`, `PolyCoder-2.7B`, `SantaCoder-1.1B`

## `results/task_class/task_scope.json`

task-class identification, dropped files, multi-class files (Section 3.2).

Top-level keys: `identification`, `classes_in`, `classes_out`, `dropped`, `task_class_not_largest`, `files_with_two_or_more_top_level_classes`, `sloc_changed`, `other_smells_changed`, `duplicate_blocks_file_scope`, `duplicate_blocks_task_scope`, `per_file`

## `results/task_class/task_scope_reconcile.json`

reconciliation of the 421 whole-file and 267 task-class blocks (Table S3).

Top-level keys: `AI/boundary`, `AI/first_out`, `AI/kept`, `AI/outside`, `Human/kept`, `totals`, `check`

## `results/task_class/test_pass.json`

ClassEval test outcome per class (Table 2, Pass).

Top-level keys: `Human`, `GPT-4-Turbo`, `DeepSeek-Coder`, `StarCoder-15B`, `InstructCodeGen-16B`, `Magicoder-6.7B`, `CodeLlama-13B`, `GPT-3.5-Turbo`, `Gemini-Pro`, `WizardCoder-15B`, `CodeGeeX2-6B`, `PolyCoder-2.7B`, `SantaCoder-1.1B`

## `results/task_class/validation.json`

agreement with lizard over 6,540 matched methods (Section 3.2).

Top-level keys: `n_matched_methods`, `spearman_cc`, `pearson_cc`, `spearman_loc`, `highcomplexity_kappa`, `highcomplexity_agreement`, `longmethod_kappa`, `longmethod_agreement`, `bh_adjusted`

## `results/whole_file_config/flagged_blocks.csv`

421 rows, 16 columns.

| Column | Meaning |
|---|---|
| `model` | Producer name, or `Human` for the benchmark reference solution. |
| `class_key` | `<model>__<sample_id>`. |
| `group` | `AI` or `Human`. |
| `task` | ClassEval task number. |
| `line_index` | Position of the flagged copy in the class's stripped-line sequence (six-line rule, Section 3.2). |
| `first_seen_at` | Position of the first occurrence of the same six-line window. |
| `lines_decorator` | Lines labelled decorator. |
| `lines_docstring_or_string` | Lines labelled documentation. |
| `lines_logic` | Lines labelled implementation statement. |
| `lines_signature` | Lines labelled signature. |
| `lines_validation_boilerplate` | Lines labelled validation boilerplate (`raise`, `assert`, `pass`). |
| `ast_stmt_lines` | Lines covered by an AST statement node (independent classifier, Section 3.3). |
| `statement_free` | True if the block has no implementation-statement line. |
| `skeleton_match_share` | Share of the block's six lines equal (normalized) to a line of its own task skeleton. |
| `skeleton_prose_share` | The same share counting skeleton docstring (prose) lines only. |
| `skeleton_match` | `skeleton_match_share >= 0.5`. |

## `results/whole_file_config/metrics.csv`

1183 rows, 16 columns.

| Column | Meaning |
|---|---|
| `sample_id` | ClassEval task identifier (`ClassEval_<n>`); with `model` it identifies a class. |
| `group` | `AI` or `Human`. |
| `model` | Producer name, or `Human` for the benchmark reference solution. |
| `sloc` | Size: number of AST statement nodes in the class, excluding docstring expression nodes (not physical lines). |
| `nom` | Number of methods. |
| `wmc` | Weighted methods per class (summed cyclomatic complexity). |
| `nof` | Number of fields. |
| `n_smells` | Total smells, documentation-preserving duplication rule. |
| `td_density` | `n_smells / sloc * 1000`. |
| `smell_LongMethod` | Long Method instances. |
| `smell_HighComplexity` | High Complexity instances. |
| `smell_ComplexConditional` | Complex Conditional instances. |
| `smell_GodClass` | God Class instances. |
| `smell_LongParamList` | Long Parameter List instances. |
| `smell_DeepNesting` | Deep Nesting instances. |
| `smell_Duplicate` | Duplicate Code blocks, documentation-preserving rule (every flagged block). |

## `results/whole_file_config/metrics_logic_bearing.csv`

1183 rows, 19 columns.

| Column | Meaning |
|---|---|
| `sample_id` | ClassEval task identifier (`ClassEval_<n>`); with `model` it identifies a class. |
| `group` | `AI` or `Human`. |
| `model` | Producer name, or `Human` for the benchmark reference solution. |
| `sloc` | Size: number of AST statement nodes in the class, excluding docstring expression nodes (not physical lines). |
| `nom` | Number of methods. |
| `wmc` | Weighted methods per class (summed cyclomatic complexity). |
| `nof` | Number of fields. |
| `n_smells` | Total smells, documentation-preserving duplication rule. |
| `td_density` | `n_smells / sloc * 1000`. |
| `smell_LongMethod` | Long Method instances. |
| `smell_HighComplexity` | High Complexity instances. |
| `smell_ComplexConditional` | Complex Conditional instances. |
| `smell_GodClass` | God Class instances. |
| `smell_LongParamList` | Long Parameter List instances. |
| `smell_DeepNesting` | Deep Nesting instances. |
| `smell_Duplicate` | Duplicate Code blocks, documentation-preserving rule (every flagged block). |
| `dup_logic` | Duplicate Code blocks, statement-aware rule (blocks with at least one implementation statement). |
| `n_smells_logic` | Total smells, statement-aware rule. |
| `density_logic` | `n_smells_logic / sloc * 1000`. |

## `results/whole_file_config/per_class_both_rules.csv`

1183 rows, 18 columns.

| Column | Meaning |
|---|---|
| `model` | Producer name, or `Human` for the benchmark reference solution. |
| `sample_id` | ClassEval task identifier (`ClassEval_<n>`); with `model` it identifies a class. |
| `group` | `AI` or `Human`. |
| `sloc` | Size: number of AST statement nodes in the class, excluding docstring expression nodes (not physical lines). |
| `salvaged` | True if the class comes from a salvaged (longest parseable) prefix. |
| `methods_wanted` | Methods the ClassEval skeleton requests. |
| `methods_implemented` | Requested methods present with a non-empty body. |
| `complete` | Structurally complete: every requested method has a non-empty body. |
| `passes` | Passes all ClassEval tests when re-executed. |
| `other6` | The six smells other than Duplicate Code, summed. |
| `dup_conv` | Duplicate Code blocks, documentation-preserving rule. |
| `dup_stmt` | Duplicate Code blocks, statement-aware rule. |
| `tot_conv` | Total smells, documentation-preserving rule. |
| `tot_stmt` | Total smells, statement-aware rule. |
| `d_dup_conv` | Per 1,000 statements (`/ sloc * 1000`) of `dup_conv`. |
| `d_dup_stmt` | Per 1,000 statements of `dup_stmt`. |
| `d_tot_conv` | Per 1,000 statements of `tot_conv`. |
| `d_tot_stmt` | Per 1,000 statements of `tot_stmt`. |
