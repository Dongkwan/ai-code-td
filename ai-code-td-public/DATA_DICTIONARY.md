# Data dictionary

Generated from the published files. Every column and every top-level
key below exists in the file it is listed under.

## Tabular files

### `results/metrics.csv`

1183 rows, 16 columns.

| Column | Meaning |
|---|---|
| `sample_id` | ClassEval task identifier. Repeats across producers: all twelve solve the same 100 tasks, so the class key is `model` plus `sample_id`. |
| `group` | `AI` or `Human`. |
| `model` | Producer name, or `Human` for the benchmark reference solution. |
| `sloc` | Source lines of code, counted as logical statements. |
| `nom` | Number of methods. |
| `wmc` | Weighted methods per class (summed cyclomatic complexity). |
| `nof` | Number of fields. |
| `n_smells` | Total detected smells under the conventional duplication rule. |
| `td_density` | `n_smells / sloc * 1000`, i.e. smells per kLoC, under the conventional rule. |
| `smell_LongMethod` | Count of Long Method instances. |
| `smell_HighComplexity` | Count of High Complexity instances. |
| `smell_ComplexConditional` | Count of Complex Conditional instances. |
| `smell_GodClass` | Count of God Class instances. |
| `smell_LongParamList` | Count of Long Parameter List instances. |
| `smell_DeepNesting` | Count of Deep Nesting instances. |
| `smell_Duplicate` | Duplicate Code instances under the conventional rule: every flagged block. |

### `results/metrics_logic_bearing.csv`

1183 rows, 19 columns.

| Column | Meaning |
|---|---|
| `sample_id` | ClassEval task identifier. Repeats across producers: all twelve solve the same 100 tasks, so the class key is `model` plus `sample_id`. |
| `group` | `AI` or `Human`. |
| `model` | Producer name, or `Human` for the benchmark reference solution. |
| `sloc` | Source lines of code, counted as logical statements. |
| `nom` | Number of methods. |
| `wmc` | Weighted methods per class (summed cyclomatic complexity). |
| `nof` | Number of fields. |
| `n_smells` | Total detected smells under the conventional duplication rule. |
| `td_density` | `n_smells / sloc * 1000`, i.e. smells per kLoC, under the conventional rule. |
| `smell_LongMethod` | Count of Long Method instances. |
| `smell_HighComplexity` | Count of High Complexity instances. |
| `smell_ComplexConditional` | Count of Complex Conditional instances. |
| `smell_GodClass` | Count of God Class instances. |
| `smell_LongParamList` | Count of Long Parameter List instances. |
| `smell_DeepNesting` | Count of Deep Nesting instances. |
| `smell_Duplicate` | Duplicate Code instances under the conventional rule: every flagged block. |
| `dup_logic` | Duplicate Code instances under the statement-aware rule: only blocks containing at least one executable statement. |
| `n_smells_logic` | `n_smells - smell_Duplicate + dup_logic`. |
| `density_logic` | `n_smells_logic / sloc * 1000`. |

### `results/metrics_newcohort.csv`

100 rows, 16 columns.

| Column | Meaning |
|---|---|
| `sample_id` | ClassEval task identifier. Repeats across producers: all twelve solve the same 100 tasks, so the class key is `model` plus `sample_id`. |
| `group` | `AI` or `Human`. |
| `model` | Producer name, or `Human` for the benchmark reference solution. |
| `sloc` | Source lines of code, counted as logical statements. |
| `nom` | Number of methods. |
| `wmc` | Weighted methods per class (summed cyclomatic complexity). |
| `nof` | Number of fields. |
| `n_smells` | Total detected smells under the conventional duplication rule. |
| `td_density` | `n_smells / sloc * 1000`, i.e. smells per kLoC, under the conventional rule. |
| `smell_LongMethod` | Count of Long Method instances. |
| `smell_HighComplexity` | Count of High Complexity instances. |
| `smell_ComplexConditional` | Count of Complex Conditional instances. |
| `smell_GodClass` | Count of God Class instances. |
| `smell_LongParamList` | Count of Long Parameter List instances. |
| `smell_DeepNesting` | Count of Deep Nesting instances. |
| `smell_Duplicate` | Duplicate Code instances under the conventional rule: every flagged block. |

### `results/metrics_newcohort_logic.csv`

600 rows, 20 columns.

| Column | Meaning |
|---|---|
| `sample_id` | ClassEval task identifier. Repeats across producers: all twelve solve the same 100 tasks, so the class key is `model` plus `sample_id`. |
| `group` | `AI` or `Human`. |
| `model` | Producer name, or `Human` for the benchmark reference solution. |
| `sloc` | Source lines of code, counted as logical statements. |
| `nom` | Number of methods. |
| `wmc` | Weighted methods per class (summed cyclomatic complexity). |
| `nof` | Number of fields. |
| `n_smells` | Total detected smells under the conventional duplication rule. |
| `td_density` | `n_smells / sloc * 1000`, i.e. smells per kLoC, under the conventional rule. |
| `smell_LongMethod` | Count of Long Method instances. |
| `smell_HighComplexity` | Count of High Complexity instances. |
| `smell_ComplexConditional` | Count of Complex Conditional instances. |
| `smell_GodClass` | Count of God Class instances. |
| `smell_LongParamList` | Count of Long Parameter List instances. |
| `smell_DeepNesting` | Count of Deep Nesting instances. |
| `smell_Duplicate` | Duplicate Code instances under the conventional rule: every flagged block. |
| `dup_logic` | Duplicate Code instances under the statement-aware rule: only blocks containing at least one executable statement. |
| `dup_echo` | Duplicate Code instances that are statement-free and match the supplied specification. |
| `n_smells_logic` | `n_smells - smell_Duplicate + dup_logic`. |
| `density_logic` | `n_smells_logic / sloc * 1000`. |

## Result files

Each entry lists the top-level keys of that file.

### `results/`

- `oss_large.json` — `n_repos`, `n_ai_classes`, `n_human_classes`, `pooled`, `repo_level`, `project_scale`, `per_agent`, `smell_share_ai`, `smell_share_human`, `per_repo`
- `oss_large_no_vendored.json` — `n_repos`, `n_ai_classes`, `n_human_classes`, `pooled`, `repo_level`, `project_scale`, `per_agent`, `smell_share_ai`, `smell_share_human`, `per_repo`
- `refocus_stats.json` — `seed`, `bootstrap_replicates`, `rule_change`, `producer_inflation`, `boundary`
- `review_addenda.json` — `seed`, `bootstrap_replicates`, `heterogeneity`, `skillings_mack`, `hurdle`, `normalisation`, `design_effect`, `zero_cell_odds_ratios`
- `review_dependence.json` — `seed`, `shared_task_correlation`, `heterogeneity`, `heterogeneity_unrestricted`, `heterogeneity_open_cohort`, `blocked_posthoc`, `capability`
- `review_recomputations.json` — `seed`, `bootstrap_replicates`, `headline_ratios`, `rq3_composition`, `between_model_spread`, `posthoc`, `dup_validation_positive_agreement`
- `sqj_interaction.json` — `n_classes`, `n_rows`, `primary_specification`, `slope_test`, `rule_specific_size_slope`, `common_size_slope`, `note`
- `sqj_revision.json` — `seed`, `replicates`, `audit`, `cutoff_sensitivity`, `rule_change`, `count_models`, `leave_one_producer_out`
- `stats_results.json` — `overall`, `paired`, `per_model`, `model_effect_kruskal`, `smell_presence`, `smell_distribution`, `model_smell_profile`, `continuous`, `measure`, `unrestricted`
- `syntactic_validity.json` — `Human`, `GPT-4-Turbo`, `DeepSeek-Coder`, `StarCoder-15B`, `InstructCodeGen-16B`, `Magicoder-6.7B`, `CodeLlama-13B`, `GPT-3.5-Turbo`, `Gemini-Pro`, `WizardCoder-15B`, `CodeGeeX2-6B`, `PolyCoder-2.7B`, ... (13 keys)
- `test_pass_newcohort.json` — `Qwen2.5-Coder-1.5B`, `Qwen2.5-Coder-3B`, `Qwen2.5-Coder-7B`, `Granite-8B-Code`, `Qwen2.5-Coder-14B`, `StarCoder2-15B`
- `validation.json` — `n_matched_methods`, `spearman_cc`, `pearson_cc`, `spearman_loc`, `highcomplexity_kappa`, `highcomplexity_agreement`, `longmethod_kappa`, `longmethod_agreement`, `bh_adjusted`

### `results/additional/`

- `E12_external_agreement.json` — `n_classes`, `per_smell`, `config`, `not_externally_comparable`, `summary`
- `E2_posthoc_dunn.json` — `kruskal_including_human`, `n_per_group`, `mean_ranks`, `pairwise`, `n_significant_pairs`, `significant_pairs`, `measure`, `unrestricted`
- `E3_negative_binomial.json` — `M1_authorship`, `M2_authorship_plus_structure`, `M3_model_identity`, `aic_comparison`, `measure`, `unrestricted`
- `E4_threshold_sensitivity.json` — `configs`, `rq1_null_robust`, `rq2_effect_robust`, `rq3_dup_skew_robust`, `measure`
- `E5_effect_size_cis.json` — `GPT-4-Turbo`, `DeepSeek-Coder`, `StarCoder-15B`, `InstructCodeGen-16B`, `Magicoder-6.7B`, `CodeLlama-13B`, `GPT-3.5-Turbo`, `Gemini-Pro`, `WizardCoder-15B`, `CodeGeeX2-6B`, `PolyCoder-2.7B`, `SantaCoder-1.1B`, ... (15 keys)
- `E8_normalisation.json` — `smells_per_kloc`, `smells_per_class`, `smells_per_method`, `smells_per_wmc`, `conclusion_stable`, `measure`, `unrestricted`
- `R11_specification_echo.json` — `criterion`, `n_flagged_blocks`, `n_statement_free`, `n_pure_string_literal`, `n_echo_every_line`, `n_echo_at_least_half`, `n_echo_at_least_one_line`, `mean_specification_line_share`, `per_model`
- `R12_prior_rule_recompute.json` — `note`, `configs`
- `R13_oss_audit.json` — `cutoff`, `n_repos`, `reconstruction`, `ai`, `human`, `ratio_conventional`, `ratio_logic_bearing`, `mannwhitney_p_conventional`, `mannwhitney_p_logic_bearing`, `statement_free_share_of_blocks`, `per_repo`, `failed`
- `R16_echo_permutation.json` — `n_statement_free_blocks`, `permutations_per_block`, `own_mean_share`, `foreign_mean_share`, `own_share_at_least_half`, `own_rate_at_least_half`, `foreign_share_at_least_half`, `foreign_rate_at_least_half`, `seed`
- `R1_task_clustered_nb.json` — `nb_cluster_robust_authorship`, `nb_cluster_robust_model_identity`, `gee`, `poisson_task_random_intercept`, `task_effect_lrt`, `dispersion`, `measure`, `unrestricted`
- `R5_duplicate_validation.json` — `external_agreement`, `composition_audit`, `logic_bearing_only`
- `R7_attrition_robustness.json` — `corpus`, `A_attrition_vs_task_difficulty`, `B_failure_overlap`, `C_drop_low_validity`, `limitation`

## Not in this package

The block audit in `results/additional/R5_duplicate_validation.json` keeps
each example's file, producer, line numbers and line classification, but
not the source lines themselves. Publishing those would redistribute the
ClassEval generations, which the Data availability statement says is not
done. The source code that produced every file here is available from the
corresponding author on request.
