# Changelog

## 2026-10-09 — Standalone bulkformer-latest release

- Publish the V1 Plus Stage1 epoch 17 / update 126,055 checkpoint as **bulkformer-latest** in its own repository: 101.3M parameters, 19,973 human genes, and 768-dimensional representations.
- Provide one complete Google Drive weight file with configuration, vocabulary, graph, environment, and checksums in the model folder.
- Include a GPU-validated Jupyter workflow for sample embeddings, contextual gene embeddings, and expression predictions using synthetic example counts.
- Match the original BulkFormer notebook's short-cell style and retain executed GPU outputs for data previews, missing-gene counts, and all three feature types.
- Emphasize complete coding-gene input, the recommended ≤10% missingness target, and full-transcriptome encoding before HVG/DEG selection and pooling.
- Publish retained frozen-feature phenotype comparisons with bulkformer-147M on identical samples, fold assignments, and classifier settings.
- Organize inference, notebooks, environments, documentation, figures, download metadata, and benchmark records into dedicated directories.

Checkpoint ID: `v1plus-stage1-epoch17-update126055`. The public name and repository organization preserve the model tensors and trained architecture.

Earlier model-family updates remain documented in the [original BulkFormer project](https://github.com/KangBoming/BulkFormer).
