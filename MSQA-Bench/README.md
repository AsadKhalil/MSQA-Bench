# MSQA-Bench

Code and reproducibility materials for **MSQA-Bench**, a large-scale question-answering benchmark for computational mass spectrometry.

The dataset release is hosted separately on Hugging Face:

<https://huggingface.co/datasets/asad00027/MSQA-Bench>

This GitHub repository is intended for the NeurIPS Evaluations & Datasets code artifact. It contains the extraction, dataset-construction, filtering, training, evaluation, and release-preparation code. Large generated artifacts such as PDFs, JSONL splits, model checkpoints, logs, and Hugging Face release files are intentionally excluded.

## Repository Layout

```text
src/                 Core Python pipeline modules
scripts/             Command-line workflows and utility scripts
config/              JSON configuration files
paper/               Dataset/evaluation/annotation helpers used for the paper
docs/                Setup and usage notes
examples/            Small usage examples
figures/             Paper/result figures
requirements.txt     Python dependencies
```

## Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Some workflows require external services or hardware: GROBID for structured PDF parsing, vLLM/Ollama/OpenAI-compatible generation endpoints for QA generation, and CUDA GPUs for model fine-tuning.

## Quickstart: load the dataset and run the BM25 baseline

This reproduces the BM25 row of Table 3 in the paper (R@10 = 0.742) on the
redistributable test split.

```python
from datasets import load_dataset

# Two configs: "redistributable" (text-bearing, CC-BY-4.0) and
# "restricted" (metadata-only; reconstruct text locally with the script below).
ds = load_dataset("asad00027/MSQA-Bench", "redistributable", split="test")
print(ds[0]["question"], "->", ds[0]["answer"][:80])
```

```bash
# Five-minute reproduction of the BM25 retrieval baseline on a 5K-query sample.
python3 - <<'PY'
from datasets import load_dataset

ds = load_dataset("asad00027/MSQA-Bench", "redistributable", split="test")
ds.to_json("/tmp/msqa_redistributable_test.jsonl")
PY

python3 scripts/evaluate_bm25_baseline.py \
  --data /tmp/msqa_redistributable_test.jsonl \
  --output paper_results/evaluation \
  --sample-size 5000
```

Expected output: `recall@10 ~= 0.742, mrr@10 ~= 0.668, ndcg@10 ~= 0.686` (matches
Table 3 of the paper).

## Common Commands

Run tests:

```bash
python3 -m pytest
```

Validate embedding training setup:

```bash
python3 scripts/validate_embedding_setup.py
```

Run the paper pipeline:

```bash
python3 scripts/run_paper_pipeline.py --config config/paper_pipeline.json
```

Prepare the two-tier NeurIPS/Hugging Face dataset release from generated splits:

```bash
python3 scripts/prepare_neurips_release.py \
  --input-dir paper_results/dataset/splits \
  --output-dir paper_results/neurips_release
```

Reconstruct a restricted metadata-only record from a local PDF collection:

```bash
python3 scripts/reconstruct_restricted_record.py \
  --restricted-jsonl paper_results/neurips_release/restricted/test.jsonl \
  --pdf-dir /path/to/local/pdfs \
  --output-jsonl reconstructed_test.jsonl
```

## Data And Artifact Policy

Do not commit raw PDFs, generated JSONL splits, Hugging Face release folders, trained model checkpoints, API keys, or private paths. Dataset files belong on Hugging Face; archival mirrors and DOIs belong on Zenodo or another long-term artifact host.

## License

Code is released under the MIT License. Dataset records hosted on Hugging Face follow the two-tier release described in the dataset card.
