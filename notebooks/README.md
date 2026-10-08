# Jupyter notebook

| Notebook | Environment |
|---|---|
| [bulkformer-latest feature extraction](bulkformer-latest_feature_extraction.ipynb) | [configs/bulkformer-latest.yaml](../configs/bulkformer-latest.yaml) |

The notebook prepares raw counts or normalized expression, aligns the complete vocabulary, loads verified weights, extracts sample/gene/expression features, and saves outputs. Its included data are synthetic. It automatically locates the repository root when opened from this directory.

The cells follow the original BulkFormer notebook's `count_df`, `log_tpm_df`, `input_df`, and `res1` / `res2` / `res3` workflow. Saved GPU outputs show the checkpoint metadata, raw and normalized data, missing-gene counts, selected genes, and feature arrays with shapes `(4, 768)`, `(4, 8, 768)`, and `(4, 19973)`.

See the [usage guide](../docs/usage.md) for input and GPU conventions. The original model-family notebook is available in [BulkFormer](https://github.com/KangBoming/BulkFormer/blob/main/bulkformer_extract_feature.ipynb).
