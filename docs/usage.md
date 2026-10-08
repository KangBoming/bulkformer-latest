# Using bulkformer-latest

## Environment and GPU selection

From the repository root:

```bash
conda env create -f configs/bulkformer-latest.yaml
conda activate bulkformer-latest
jupyter lab notebooks/bulkformer-latest_feature_extraction.ipynb
```

The environment pins PyTorch 2.5.1, PyTorch Geometric 2.6.1, Performer 1.1.4, NumPy 2.0.2, and pandas 2.2.3. Choose a physical GPU before importing torch in the notebook, or set `CUDA_VISIBLE_DEVICES` when launching Jupyter. Restart the kernel after changing GPU selection if CUDA was already initialized.

The notebook finds the repository root from either the root or `notebooks/` working directory. The Python API examples assume the repository root. Download the six model files using the [model instructions](../model/README.md).

## Input conventions

Provide a pandas DataFrame with samples as rows and Ensembl gene IDs as columns. The model uses a fixed 19,973-gene vocabulary.

| Input | Preparation |
|---|---|
| Raw counts | `normalize_data(counts, gene_vocab)` uses bundled gene lengths and vocabulary-matched genes in the TPM denominator |
| TPM | Apply `numpy.log1p` to non-negative TPM values |
| Natural `ln(TPM+1)` | Use directly |
| `log2(TPM+1)` | Multiply by `numpy.log(2.0)` |

`main_gene_selection` strips Ensembl version suffixes, orders the complete gene axis, and fills absent genes and NaN entries with **-10**. Measured expression zero stays **0** and is included in sample pooling. Duplicate IDs after stripping version suffixes require upstream resolution. Gene symbols are annotations; matching uses Ensembl IDs. Do not feed logarithmic values into count normalization.

## Input coverage

Provide as complete a **protein-coding bulk transcriptome** as possible, matching the released vocabulary. More complete coding-gene coverage supplies more transcriptome context for the model's representations. Our recommended target is **≤10% missing genes per sample**, equivalent to at least 90% coverage of the 19,973-gene vocabulary.

Stage1 randomly masked 5–15% of available values, averaging approximately 10%, in addition to any source-unavailable genes. The recommended input coverage is a practical guide based on this pretraining regime; downstream performance also depends on the dataset and task.

Missingness includes absent genes and unavailable entries; measured zeros remain observed. For the complete, aligned NumPy array used below:

```python
missing_rate = (aligned_expression == -10).mean(axis=1)
```

Keep all available coding-gene measurements through input preparation and encoding. Choose an HVG or DEG subset when selecting the contextual output embeddings for pooling.

## Load and extract

```python
from utils.latest import prepare_latest_bundle, load_latest_model, extract_feature

model_dir = prepare_latest_bundle("model/bulkformer-latest")
model, gene_vocab, manifest = load_latest_model(model_dir, device="cuda")
sample_features = extract_feature(model, aligned_expression, batch_size=1)
gene_features = extract_feature(
    model, aligned_expression, output_feature_type="gene_level",
    interested_gene_idx=[0, 1, 2], batch_size=1,
)
expression_predictions = extract_feature(
    model, aligned_expression, output_feature_type="expr_level", batch_size=1,
)
```

`aligned_expression` is a NumPy array of natural `ln(TPM+1)` values in release vocabulary order. The loader freezes the encoder, verifies its resources, and loads the full encoder and expression head with `weights_only=True`.

| Output | Shape | Details |
|---|---|---|
| `sample_level` | samples × 768 | Mean of observed contextual gene vectors, including measured zeros |
| `gene_level` | samples × selected genes × 768 | Sample-dependent representations; selection reduces returned output only |
| `expr_level` | samples × 19,973 | Predictions on the learned natural `ln(TPM+1)` scale |

No additional random masking is applied during extraction. For imputation, set selected inputs to `-10` and retain their true values separately. The expression head is unconstrained and can return negative values. Static gene identity weights are a different representation from contextual gene outputs.

## Sample embeddings from HVGs or DEGs

Encode the complete available coding-gene transcriptome, obtain its contextual gene representations, select your HVGs or DEGs, then pool those contextual vectors. Gene selection is applied after the encoder. The input retains the complete vocabulary axis and all available expression measurements.

The helper can perform output selection and mean pooling in one call:

```python
gene_list = gene_vocab["canonical_ensg"].tolist()
selected_genes = gene_list[:8]  # Replace with the Ensembl IDs of your HVGs or DEGs.
selected_gene_idx = [gene_list.index(gene) for gene in selected_genes]

selected_sample_features = extract_feature(
    model=model,
    expr_array=aligned_expression,
    output_feature_type="sample_level",
    interested_gene_idx=selected_gene_idx,
    aggregate_type="mean",
    batch_size=1,
)
```

`aligned_expression` contains all 19,973 gene positions. The encoder processes that complete input, then the helper selects the requested contextual vectors and averages the observed selected genes separately for each sample. Measured zeros are included. Each sample needs at least one observed gene in the selected set. The result has shape `samples × 768`.

To inspect or save the selected contextual vectors themselves, use `output_feature_type="gene_level"` with the same full input and `interested_gene_idx`; the result has shape `samples × selected genes × 768`. The [notebook](../notebooks/bulkformer-latest_feature_extraction.ipynb) demonstrates this output selection. The published phenotype comparison uses mean pooling over all observed genes; evaluate an HVG/DEG pooling choice for your own task.

## Memory and precision

Start with **one GPU and batch size 1**. The encoder always processes the full gene axis. Returning a small output subset does not reduce the input token count. Select BF16 on supported GPUs or FP16 otherwise; CPU inference uses FP32. The notebook selects a supported precision automatically. The API accepts `amp_dtype="bf16"`, `"fp16"`, or `"fp32"`.

Use the [notebook](../notebooks/bulkformer-latest_feature_extraction.ipynb) to save sample features, selected gene features with gene IDs, and expression predictions with sample IDs. Results go to `outputs/latest_demo/` by default.

## Model identity and original models

This repository's public API is `utils.latest`; the model directory is `model/bulkformer-latest/`. Internal architecture identifiers continue to record the trained V1 Plus implementation. The immutable checkpoint ID is `v1plus-stage1-epoch17-update126055`.

The original 37M–147M models and their matching workflow are available in [BulkFormer](https://github.com/KangBoming/BulkFormer). See the [predecessor guide](models/original-models.md).
