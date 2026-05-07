# CEI Benchmark: Replication Package (NeurIPS 2026)

**Paper:** CEI: A Benchmark for Evaluating Pragmatic Reasoning in Language Models
**Venue:** NeurIPS 2026 (Datasets & Benchmarks Track)

This repository contains all code, data, and reference outputs needed to replicate the analyses in the NeurIPS 2026 paper. It is self-contained and independent of the larger project repository.

> Note: pipeline script names (`run_pipeline_dmlr2026.py`), config (`config-dmlr.yml`), and the `papers/dmlr2026/` and `reports/dmlr2026/` subdirectories retain a `dmlr2026` suffix from a prior submission target. They will be renamed to a venue-neutral `cei2026` slug in a follow-up commit; current filenames are kept intentionally so existing references and reports continue to resolve.

## Repository Contents

```
data/human-gold/                   # The CEI dataset (300 scenarios, 5 CSVs)
scripts/run_pipeline_dmlr2026.py   # Main pipeline (all stages)
scripts/generate_model_confusion_matrix.py  # Model confusion matrix figure
config/config-dmlr.yml             # Model definitions and pricing
reports/dmlr2026/                   # Reference baseline outputs
papers/dmlr2026/                    # Paper source, figures, bibliography
requirements.txt                    # Python dependencies
LICENSE                             # MIT
```

## Quick Start

### 1. Install dependencies

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
```

Requires Python 3.10+. Only `pyyaml` is needed for the pipeline; `matplotlib` and `numpy` are optional (for figure generation).

### 2. Run all local analysis (no API keys, ~30 seconds, $0 cost)

```bash
python scripts/run_pipeline_dmlr2026.py --stage all_local
```

This recomputes from the raw CSV data:
- Fleiss' kappa per subtype with 95% bootstrap CIs
- Power distribution counts (peer / high-to-low / low-to-high)
- Human agreement patterns (unanimous / majority / split)
- VAD ICC(2,1) per dimension per subtype
- Scale justification (power analysis)
- Stratified train/val/test splits (70/15/15, seed=42)
- Candidate worked examples

### 3. Generate paper tables and figures

```bash
python scripts/run_pipeline_dmlr2026.py --stage generate_outputs
```

Outputs LaTeX tables and figures to `reports/dmlr2026/`.

### 4. Generate model confusion matrix (requires matplotlib)

```bash
python scripts/generate_model_confusion_matrix.py
```

Reads `reports/dmlr2026/baseline_results.json` and produces `papers/dmlr2026/figures/fig8_model_confusion_matrix.pdf`.

### 5. Run LLM baselines (requires API keys, ~$2.50 per prompt mode)

```bash
# Dry run first (estimate costs, no API calls)
python scripts/run_pipeline_dmlr2026.py --stage run_baselines --dry-run

# Set API keys for the providers you have access to
export OPENAI_API_KEY="sk-..."
export ANTHROPIC_API_KEY="sk-ant-..."
export XAI_API_KEY="xai-..."
export GOOGLE_API_KEY="..."
export TOGETHER_API_KEY="..."
export FIREWORKS_API_KEY="..."

# Run all three prompt modes
python scripts/run_pipeline_dmlr2026.py --stage run_baselines --prompt-mode zero-shot
python scripts/run_pipeline_dmlr2026.py --stage run_baselines --prompt-mode cot
python scripts/run_pipeline_dmlr2026.py --stage run_baselines --prompt-mode few-shot
```

The pipeline runs whichever models have keys configured and skips the rest. Use `--resume` to continue from checkpoint after interruption.

### 6. Compile the paper (requires LaTeX)

```bash
cd papers/dmlr2026
latexmk -pdf dmlr2026_cei-tom_dataset.tex
```

## Baseline Models

| Model | Provider | Est. Cost/300 scenarios |
|-------|----------|------------------------|
| GPT-5-mini | OpenAI | ~$0.17 |
| Claude Sonnet 4.5 | Anthropic | ~$1.35 |
| Grok-4.1-Fast | xAI | ~$0.05 |
| Gemini 2.5 Flash | Google | ~$0.21 |
| Llama-3.1-70B | Together | ~$0.26 |
| DeepSeek-V3 | Fireworks | ~$0.07 |
| Qwen2.5-7B | Together | ~$0.09 |

Total estimated cost across all 7 models and 3 prompt modes: ~$7.59.

## Data Schema

Each CSV in `data/human-gold/` contains 60 scenarios with columns:

| Column | Description |
|--------|-------------|
| `id` | Scenario ID (1-60 within subtype) |
| `sd_situation` | Situational context |
| `sd_utterance` | Speaker's utterance |
| `sd_speaker_role` | Speaker's role/relationship |
| `sd_listener_role` | Listener's role/relationship |
| `sl_plutchik_primary_<Name>` | Annotator's emotion label (Plutchik 8) |
| `gold_standard` | Adjudicated ground truth emotion |
| `sl_v_<Name>`, `sl_a_<Name>`, `sl_d_<Name>` | Per-annotator VAD ratings |
| `sl_confidence_<Name>` | Per-annotator confidence |

## Reproducibility

- All stochastic operations use seed=42 by default
- Temperature=0 (greedy decoding) for all model inference
- VAD: 7-point text labels mapped to [-1.0, +1.0] at equal intervals
- Baseline prompt targets the speaker's emotion (not the listener's response)
- Reference outputs in `reports/dmlr2026/` can be compared against fresh runs

## License

- Data: CC-BY-4.0
- Code: MIT
