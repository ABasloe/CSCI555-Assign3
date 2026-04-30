# CSCI 455/555 Assignment 3

This folder contains the reproducible notebook for the assignment pipelines:

- Pipeline A: SentencePiece tokenizer + T5-small pre-training + fine-tuning.
- Pipeline B: same tokenizer/config, no pre-training, fine-tuning only.
- RAG comparison: Qwen2.5-Coder zero-shot vs 3-shot RAG.

The main notebook is:

```text
assignment_pipelines_a_b_combined.ipynb
```

## Setup

Use Python 3.10 with a virtual environment. I did this one the lab machines in the isc as the GPUs were much faster. Originally I used the google colab CPUs but the virtual enviornment worked a lot better.

```bash
python3.10 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

`requirements.txt` installs:
--extra-index-url https://download.pytorch.org/whl/cu128
torch==2.11.0+cu128
transformers==4.46.0
tokenizers==0.20.3
sentencepiece==0.1.99
datasets
tqdm
ipykernel
codebleu==0.7.0
tree-sitter-java==0.21.0

## How to Reproduce

Open and run:

```text
assignment_pipelines_a_b_combined.ipynb
```

The notebook is ordered as follows:

1. Shared setup and SentencePiece tokenizer.
2. Pipeline A pre-training on CodeSearchNet Java.
3. Pipeline A fine-tuning on CodeXGLUE code refinement.
4. Pipeline A evaluation.
5. Pipeline B fine-tuning from fresh T5-small weights.
6. Pipeline B evaluation.
7. RAG retrieval with CodeBERT embeddings.
8. Qwen zero-shot and Qwen 3-shot RAG evaluation.
9. Final comparison table.

The notebook reuses existing outputs by default. To force a stage to rerun, change the `FORCE_*` flags near the top of the notebook. The Pipeline evaluations are split seperately so you can know how well each one did but there is also a combined evaluation to show comparisons. 

## Output Locations

All generated artifacts are written under:

```text
outputs/
```

Important paths:

```text
outputs/tokenizer/
```

Shared SentencePiece tokenizer used by all T5 pipelines.

```text
outputs/pipeline_a_pretrained_model/
```

Final T5-small model after Pipeline A span-corruption pre-training.

```text
outputs/pipeline_a_finetuned_model/
outputs/pipeline_a_finetuned_best_model/
```

Pipeline A fine-tuned models.

```text
outputs/pipeline_b_finetuned_model/
outputs/pipeline_b_finetuned_best_model/
```

Pipeline B fine-tuned models.

```text
outputs/rag/
```

CodeBERT retrieval caches, Qwen predictions, and RAG metrics.

Key result files:

```text
outputs/pipeline_a_test_metrics.json
outputs/pipeline_b_test_metrics.json
outputs/rag/qwen_zero_shot_metrics.json
outputs/rag/qwen_rag_3shot_metrics.json
outputs/rag/all_pipeline_metrics_summary.json
```

Training logs:

```text
outputs/pipeline_a_training_log.csv
outputs/pipeline_a_finetune_log.csv
outputs/pipeline_b_finetune_log.csv
```