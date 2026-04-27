# GitHub Upload Steps

Recommended repository name: `MSQA-Bench`

Recommended public URL:

```text
https://github.com/asad00027/MSQA-Bench
```

## 1. Review The Clean Folder

Upload only this folder:

```bash
cd /home/asad/MS-GPT/github_release/MSQA-Bench
```

Included: `src/`, `scripts/`, `config/`, `paper/`, `docs/`, `examples/`, `figures/`, `requirements.txt`, `README.md`, `LICENSE`, `.gitignore`, `AGENTS.md`.

Excluded: `data/`, `models/`, `paper_results/`, `paper_results/neurips_release/`, `.venv/`, logs, raw PDFs, JSONL splits, checkpoints, and thesis files.

## 2. Create The GitHub Repository

Create a new public GitHub repo named:

```text
MSQA-Bench
```

Do not add GitHub's default README, license, or `.gitignore`; these files already exist locally.

## 3. Push With GitHub CLI

If `gh` is installed and logged in:

```bash
git init
git branch -M main
git add .
git commit -m "Initial MSQA-Bench code release"
gh repo create asad00027/MSQA-Bench --public --source=. --remote=origin --push
```

## 4. Push Without GitHub CLI

If you created the repo in the browser:

```bash
git init
git branch -M main
git add .
git commit -m "Initial MSQA-Bench code release"
git remote add origin https://github.com/asad00027/MSQA-Bench.git
git push -u origin main
```

## 5. Add URLs To Submission

Use these artifact links:

```text
Code: https://github.com/asad00027/MSQA-Bench
Data: https://huggingface.co/datasets/asad00027/MSQA-Bench
```

If GitHub says the repository name already exists, create `MSQA-Bench-code` and use:

```text
https://github.com/asad00027/MSQA-Bench-code
```
