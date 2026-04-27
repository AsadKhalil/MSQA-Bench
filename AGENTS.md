# Repository Guidelines

## Project Structure & Module Organization

MS-GPT is a Python pipeline for turning mass spectrometry PDFs into extracted text, Q&A datasets, and fine-tuned models. Core code lives in `src/`: `pdf_processors/` handles PDF conversion, `vision_extractors/` handles OCR/vision extraction, `qa_generators/` builds Q&A data, and `embedding_trainers/` plus `llm_trainers/` run training. Entry points are in `scripts/`, JSON settings in `config/`, examples in `examples/`, and docs in `docs/`. Data and generated artifacts are under `data/`, `paper_results/`, `models/`, `training_charts/`, `figures/`, `paper/`, `paper_v2/`, and `thesis/`; avoid committing large regenerated outputs unless required.

## Build, Test, and Development Commands

```bash
source .venv/bin/activate
pip install -r requirements.txt
python -m pytest
python -m pytest src/vision_extractors/test_vision_extractor.py
python scripts/validate_embedding_setup.py
python src/qa_generators/qa_generator.py --config config/qa_generator.json
python scripts/train_all_models.py --config config/embedding_finetuner.json -y
```

Use the first two commands to enter the local environment and install dependencies. Run `pytest` before submitting changes; use the narrower command while iterating on vision extraction. Q&A generation and training may require local services, CUDA, and substantial disk/GPU resources.

## Coding Style & Naming Conventions

Use Python 3.10+ style with 4-space indentation, `snake_case` for functions/modules, `PascalCase` for classes, and descriptive JSON config keys. Keep modules focused on existing pipeline stages. Development tools in `requirements.txt` include `black`, `flake8`, and `mypy`; run them on changed Python files when practical, for example `black src scripts` and `flake8 src scripts`.

## Testing Guidelines

Tests use `pytest`. Existing tests are colocated with modules, such as `src/vision_extractors/test_vision_extractor.py`; name new tests `test_*.py` and keep fixtures small. Prefer tests for parsing, file handling, config loading, and failure paths without external LLM services. For GPU or service-backed workflows, add a smoke test or document the manual command used.

## Commit & Pull Request Guidelines

Recent history uses short, lower-case messages such as `fix` and `benchmarking dataset`; improve on that with concise imperative subjects, for example `fix qa consolidation ordering` or `add embedding setup validation`. PRs should describe the pipeline stage affected, list commands run, note required services or GPUs, and call out any generated data/model artifacts intentionally included.

## Security & Configuration Tips

Do not hard-code API keys, private paths, or credentials in source files or configs. Prefer environment variables or local-only config overrides. Keep `data/input/`, logs, and model outputs free of sensitive or licensed material unless they are explicitly approved for repository use.
