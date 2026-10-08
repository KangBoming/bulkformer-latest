# Model implementation

| Path | Responsibility |
|---|---|
| `latest/model.py` | Exact trained architecture used by bulkformer-latest |
| `latest/graph.py` | Loading and validation of the released weighted gene graph |
| `latest/inference.py` | Download verification, model loading, input normalization, alignment, feature extraction |
| `latest/__init__.py` | Public `BulkFormerLatest` aliases and inference API |

```python
from utils.latest import load_latest_model, normalize_data, main_gene_selection, extract_feature
```

`BulkFormerLatest` and `BulkFormerLatestConfig` alias the trained classes. The internal `BulkFormerV1Plus-v1` architecture identifier and state tensor names preserve checkpoint compatibility. The architecture and graph computation are unchanged by the public name and standalone repository.

See the [usage guide](../docs/usage.md) for examples and [model resources](../model/README.md) for downloads.
