# Original BulkFormer project

The [original BulkFormer repository](https://github.com/KangBoming/BulkFormer) provides the 37M, 50M, 93M, 127M, and 147M models, their matching resources, and the original feature extraction workflow.

| Resource | Original repository link |
|---|---|
| Model weights and introduction | [BulkFormer README](https://github.com/KangBoming/BulkFormer/blob/main/README.md) |
| Feature extraction notebook | [bulkformer_extract_feature.ipynb](https://github.com/KangBoming/BulkFormer/blob/main/bulkformer_extract_feature.ipynb) |
| Inference environment | [bulkformer.yaml](https://github.com/KangBoming/BulkFormer/blob/main/bulkformer.yaml) |
| Model downloads | [model/README.md](https://github.com/KangBoming/BulkFormer/blob/main/model/README.md) |
| Data resources | [data/README.md](https://github.com/KangBoming/BulkFormer/blob/main/data/README.md) |

The original 147M model uses 20,010 genes, 640 hidden channels, and 12 Performer layers with 8 heads. Its sample representation has 640 contextual channels plus three summary channels. Its vocabulary, graph, and loader are supplied by the original project.

## Comparisons

- [Matched latest vs 147M phenotype evaluation](../../benchmarks/phenotype/README.md): identical downstream samples, folds, and classifier settings, with native model representations.
- [Original publication benchmark table](../benchmarks/paper-results.md): the original paper's separate evaluation protocol.

For the current model's architecture and downloads, see the [bulkformer-latest model card](bulkformer-latest.md).
