# Enhancing Biomedical QA through Task-Adaptive Pretraining: A Transfer Learning Approach

## Project Overview
This project explores **transfer learning and Task-Adaptive Pre-Training (TAPT)** to enhance the performance of general-purpose language models on biomedical question answering tasks. Specifically, we adapt models originally trained on the **BoolQ** dataset (general yes/no questions) to the **PubMedQA** dataset (biomedical yes/no questions) to improve domain-specific reasoning and accuracy.

## Motivation
General-purpose language models often underperform in specialized fields like healthcare, where domain-specific language and contextual understanding are critical. Annotated biomedical datasets are scarce and expensive to produce. This project investigates how **TAPT** can bridge the domain gap using unlabeled biomedical text, minimizing reliance on large labeled datasets while improving real-world applicability in clinical and research settings.

## Datasets
- **Source Domain**: [BoolQ](https://github.com/google-research-datasets/boolean-questions)
  - Yes/No questions from general English sources
  - 9,427 training examples
- **Target Domain**: [PubMedQA](https://pubmedqa.github.io/)
  - Yes/No/Maybe questions derived from biomedical research papers
  - Labeled subset used for supervised fine-tuning
  - Unlabeled passages used for TAPT

## Methodology

### 1. Preprocessing
- Combined `ori_pqal.json` (labeled) and 5,000 samples from `ori_pqaa.json` (unlabeled).
- Mapped answers to binary classification (Yes/No).
- Created train/dev splits for evaluation.

### 2. Source Task Fine-tuning
- Models: **BERT**, **DistilBERT**, and **RoBERTa**.
- Fine-tuned on BoolQ to learn general yes/no question patterns.

### 3. Task-Adaptive Pre-Training (TAPT)
- Further pre-training on **unlabeled PubMedQA passages**.
- Sample sizes: 6K, 10K, and 15K.
- Objective: Expose models to biomedical language distribution without using labels.

### 4. Target Task Fine-tuning
- Fine-tuned TAPT models on labeled PubMedQA samples.
- Task: Binary classification (Yes/No, with \"Maybe\" mapped to \"No\").

### 5. Evaluation Metrics
- **Accuracy**: Measures overall correctness.
- **F1 Score**: Balances precision and recall, especially important for slightly imbalanced datasets.

## Workflow
```mermaid
graph TD
A[Preprocess BoolQ and PubMedQA] --> B[Fine-tune Models on BoolQ]
B --> C[TAPT on Unlabeled PubMedQA]
C --> D[Fine-tune on Labeled PubMedQA]
D --> E[Evaluate on PubMedQA Dev Set]
