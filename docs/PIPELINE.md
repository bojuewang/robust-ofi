# Pipeline

This project uses benchmark-gated modules. Each step produces a markdown report under `reports/benchmarks/` and a PASS marker under `.pipeline_state/`.

## Steps

| Step | Script | Output |
|---|---|---|
| 01 | `scripts/pipeline/step01_lobster_raw_audit_clean.sh` | raw data audit, row counts, time-grid checks |
| 02 | `scripts/pipeline/step02_feature_build_500ms.sh` | `X_raw`, `X_std`, targets, event masks |
| 03 | `scripts/pipeline/step03_graph_build_and_baseline.sh` | fixed graphs, Ridge/Lasso, graph-linear ERM |
| 04 | `scripts/pipeline/step04_feature_robust_pgda.sh` | initial feature-robust PGDA check |
| 04b | `scripts/pipeline/step04b_feature_robust_calibrated_eval.sh` | calibrated feature-adversarial evaluation |
| 05 | `scripts/pipeline/step05_graph_robust_pgda.sh` | graph-robust PGDA and graph degradation curves |
| 06 | `scripts/pipeline/step06_feature_graph_robust_summary.sh` | final ablation table, figures, summary |
| 07 | `scripts/pipeline/step07_github_packaging.sh` | GitHub safety checks and repository packaging |

## Run all reproducible steps

```bash
bash scripts/run_pipeline_to_step06.sh
```

Step 07 is not included in the scientific pipeline because it only prepares the repository for GitHub.

## Gate logic

A later step should only be run after the previous step has a PASS marker. For example:

```text
.pipeline_state/step02_feature_build.PASS
.pipeline_state/step03_graph_baseline.PASS
.pipeline_state/step04b_calibrated_eval.PASS
.pipeline_state/step05_graph_robust_pgda.PASS
.pipeline_state/step06_summary.PASS
```

This makes failures local: if a benchmark fails, inspect the matching markdown report before continuing.
