# Architecture figure

[Full-size PNG](bulkformer-latest-overview.png) · [Generation and correction prompts](bulkformer-latest-architecture.prompt.txt)

The README figure describes the released **bulkformer-latest** checkpoint, `v1plus-stage1-epoch17-update126055`. It is a **1,672 × 941 px RGB PNG**, created with the built-in image generation tool and reviewed against the released implementation.

## Panels

- **a — Gene token construction.** A trainable gene identity embedding uses RMS normalization. The fixed sinusoidal expression encoding has unit RMS and is scaled by a learned scalar and a per-sample coverage correction. The branches are summed before input LayerNorm.
- **b — Graph and attention encoder.** One weighted residual GCN block uses a 192-dimensional bottleneck and a learned residual scale per channel. LayerNorm, 12 Performer layers and output LayerNorm follow. The GCN also contains an internal pre-normalization step; the figure groups its operations into one block.
- **c — Contextual representations.** The full 19,973 × 768 representation supports observed-gene mean pooling, contextual output gene selection and the expression head. Measured zeros are observed values.
- **d — HVG/DEG sample representation.** Encode the full gene axis, select the desired contextual vectors, then mean-pool the observed selected genes.

The matrices and weighted graph are schematic illustrations. They do not display training data, measured embeddings, attention weights or benchmark results. See the [model card](../models/bulkformer-latest.md), [model implementation](../../utils/latest/model.py) and [pooling implementation](../../utils/latest/inference.py) for the full computation.

The visual treatment follows the restrained palette, clear panel labels and legible typography described in the [Nature research figure guide](https://research-figure-guide.nature.com/figures/preparing-figures-our-specifications/). The diagram is original. The delivered artwork is a raster illustration; the generation interface does not expose a specific model identifier.
