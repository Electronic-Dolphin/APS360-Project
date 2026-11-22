# APS360 — Quebec French Linguistic Acceptability Classification

Course project: sentence-level binary acceptability judgments in Quebec French (0 = unacceptable, 1 = acceptable under the QFrCoLA corpus criteria). We compare a lexical baseline, a from-scratch BiLSTM, and pretrained French CamemBERT under a **source-disjoint** evaluation protocol, with particular attention to anglicisms and terminological guidelines.

## Reports and notebooks

| Artifact | Description |
| --- | --- |
| [`APS360_QFrCoLA_Progress.ipynb`](APS360_QFrCoLA_Progress.ipynb) | Progress experiments: cleaning, TF–IDF, BiLSTM, character CNN and ablations |
| [`APS360_QFrCoLA_Final.ipynb`](APS360_QFrCoLA_Final.ipynb) | Final experiments: CamemBERT fine-tuning, three training seeds, validation threshold tuning, error analysis, source-disjoint test and external challenge evaluation |
| [`reports/progress_report.pdf`](reports/progress_report.pdf) | APS360 Project Progress Report |
| [`reports/final_report.pdf`](reports/final_report.pdf) | APS360 Project Final Report (4-page main body + references) |

## Data and evaluation

- Source corpus: [QFrCoLA](https://huggingface.co/datasets/graalul/qfrcola), **25,153** in-domain labeled sentences, **2,798** source-connected groups. The source-disjoint split (chosen split seed **372**) has **17,580 training**, **3,734 validation**, and **3,839 test** examples. Sources and normalized sentences are disjoint across these partitions.
- External challenge set: [`data/external_labeled.csv`](data/external_labeled.csv), **80 newly composed sentences in 40 matched recommended/disfavored pairs** guided by 20 OQLF terminology/use entries. The CSV is a *constructed, AI-assisted and rule-reviewed challenge set*, **not** a randomly sampled natural corpus or a set independently validated by native speakers. It mainly tests anglicisms/terminological usage. The source IDs identify constructed pairs; `rule_url` links to the supporting OQLF reference.
- Before evaluation, the external file was frozen (SHA-256 `7e3e8d705005fb484bcd6e84a91dcd183c5c491fc55f468e300919ee2901443d`). Exact/relaxed string matching against all 25,153 QFrCoLA sentences found no overlaps, and the checked source/rule URLs did not match. This does not rule out paraphrase-level or conceptual overlap.
- Model and threshold were fixed from validation before the external evaluation: **CamemBERT, seed 360, epoch 3, probability threshold 0.80**. The original QFrCoLA test had already been inspected during Progress-stage experiments; it is not a completely untouched holdout across the entire project.

## Key results

| Model / evaluation | Accuracy | Macro-F1 | MCC |
| --- | ---: | ---: | ---: |
| TF–IDF + logistic regression (QFrCoLA test) | 0.6002 | 0.5120 | 0.0249 |
| From-scratch BiLSTM (QFrCoLA test) | 0.5543 | 0.4672 | -0.0655 |
| **Fine-tuned CamemBERT (QFrCoLA test)** | **0.8307** | **0.7740** | **0.5765** |
| CamemBERT (external, 80 constructed examples) | 0.6375 | 0.5827 | 0.3993 |

Three training seeds (360, 361, 362) yield validation macro-F1 = **0.7379 ± 0.0047** (mean ± sample standard deviation, threshold 0.5; fixed split). The external challenge confusion matrix is `[[11, 29], [0, 40]]`, demonstrating poor recall of disfavored expressions despite 40/40 acceptable examples being correctly classified. See [`results/`](results/) for compact machine-readable metrics and plots.

## Running the notebook

1. Use a Python 3.11 environment with a compatible CUDA-enabled PyTorch installation if training. The notebook's **Part A0** checks Hugging Face package compatibility and can repair dependencies; **restart the Python kernel after a repair**. The validated package set included `datasets==3.6.0`, `transformers==4.57.3`, `huggingface-hub==0.36.2`.
2. The notebook fetches the public QFrCoLA corpus. Run Parts A–D to regenerate the split and tokenized datasets. The pretrained CamemBERT tokenizer/weights are fetched from [`almanach/camembert-base`](https://huggingface.co/almanach/camembert-base).
3. Parts E–G build the selected model/analysis. Checkpoints are written under `qfrcola_final_results/e5_seed_*/best_model/` **locally, not included in this repository**. Training may be required on a new computer. The archived notebook retains the results from the author's completed runs.
4. For Part I external evaluation, copy `data/external_labeled.csv` to `qfrcola_final_results/external_labeled.csv`, regenerate or load the seed-360 checkpoint, and **do not retune** using external labels. The notebook has explicit switches for the test/evaluation sections; use these deliberately rather than blindly rerunning them.
5. `results/` is a small published snapshot of metrics/figures. It is separate from the Notebook's local `qfrcola_final_results/` working directory.

## Attribution and limitations

The public QFrCoLA data is due to Beauchemin and Khoury (EMNLP 2025): [paper](https://aclanthology.org/2025.emnlp-main.6/) · [dataset](https://huggingface.co/datasets/graalul/qfrcola). The external challenge pairs were composed for this project using terminology entries from the [OQLF Vitrine linguistique](https://vitrinelinguistique.oqlf.gouv.qc.ca/). See the final report for complete references, labeling limitations and ethics. Classification outputs are not official regulatory judgments and should not be used without expert review.
