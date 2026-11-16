# APS360 — QFrCoLA Linguistic Acceptability Classification

Applied Fundamentals of Deep Learning course project. The task is **sentence-level binary linguistic acceptability classification** for Quebec French, with a focus on **anglicisms** and generalization to **previously unseen source pages**.

## Files

- [`APS360_QFrCoLA_Progress.ipynb`](APS360_QFrCoLA_Progress.ipynb) — primary reproducible experiment notebook, including saved outputs for source-disjoint splitting, TF–IDF logistic regression, BiLSTM, Character CNN + BiLSTM, and controlled ablations.
- [`reports/progress_report.pdf`](reports/progress_report.pdf) — progress report, compiled with the course-provided NeurIPS-style LaTeX template.

## Reproducing

1. Open `APS360_QFrCoLA_Progress.ipynb` in Google Colab, preferably with GPU enabled (Runtime → Change runtime type).
2. Run all cells in order. The first code cell installs needed packages (a kernel restart may be necessary after upgrades).
3. The notebook downloads the public [`graalul/qfrcola`](https://huggingface.co/datasets/graalul/qfrcola) corpus; no data archive is committed here.
4. It constructs the source-disjoint 70/15/15 split (chosen seed **372**) and fits vocabularies on training data only. Tables, plots, and checkpoints are created in `qfrcola_results/`.
5. Saved notebook outputs record the progress-report experiments. Small numerical differences may occur across hardware and dependency versions; the full notebook was not rerun when preparing this repository bundle.

## Evaluation protocol

The TF–IDF baseline and initial BiLSTM were evaluated on the held-out source-disjoint test set. Later architecture and training-configuration experiments are reported on **validation** only; they are not new held-out test results. Metrics include accuracy, macro-F1, Matthews correlation coefficient, and per-class F1. No independent newly collected final-test dataset has been evaluated yet.

## Dataset and attribution

D. Beauchemin and R. Khoury, *QFrCoLA: a Quebec-French Corpus of Linguistic Acceptability Judgments*, EMNLP 2025. [Paper](https://aclanthology.org/2025.emnlp-main.6/) · [Dataset](https://huggingface.co/datasets/graalul/qfrcola). Dataset license: **CC BY-NC-SA 4.0**; consult its license before reuse or redistribution.
