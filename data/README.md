# Synthetic example data

## bulkformer-latest

[latest_demo_counts.csv](latest_demo_counts.csv) contains **four synthetic samples** with samples as rows and Ensembl IDs as columns. NumPy seed 42 was used to generate the counts; measured zeros are retained and eight genes are omitted to demonstrate missing-gene alignment. This example contains no biological samples.

The model's ordered 19,973-gene vocabulary, gene lengths, weighted graph, and configuration are all supplied in its [Google Drive model folder](https://drive.google.com/drive/folders/1AfekiOBNs6GzPBbA9BDlpuoolZIWbGQ2). See the [download instructions](../model/README.md) and [latest notebook](../notebooks/bulkformer-latest_feature_extraction.ipynb).
