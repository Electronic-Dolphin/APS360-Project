# APS360-Project
# Quebec French Linguistic Acceptability Classification

APS360 – Introduction to Artificial Intelligence, Fall 2026

## Project Overview

This project aims to develop a binary classifier for Quebec French linguistic acceptability, with a particular focus on anglicisms.

The objective is to compare a bidirectional LSTM (BiLSTM) neural network against a traditional machine learning baseline.

## Dataset

QFrCoLA – Quebec French Corpus of Linguistic Acceptability Judgments

Dataset: https://huggingface.co/datasets/graalul/qfrcola

## Planned Models

- **Baseline:** TF-IDF (word unigrams and bigrams) with Logistic Regression
- **Neural Network:** Embedding + BiLSTM + Fully Connected Layer
- **Optional Extension:** Fine-tuned CamemBERT

## Evaluation

Models will be evaluated using accuracy, macro-F1, Matthews correlation coefficient (MCC), and category-specific performance.

## Project Status

Project proposal stage. Implementation and experiments have not yet begun.