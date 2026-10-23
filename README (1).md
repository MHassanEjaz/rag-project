# RAG Project: Document Intelligence Assistant (arXiv)

**Dataset:** arXiv abstracts, Hugging Face mirror [`gfissore/arxiv-abstracts-2021`](https://huggingface.co/datasets/gfissore/arxiv-abstracts-2021) of the [Kaggle arXiv dataset](https://www.kaggle.com/datasets/Cornell-University/arxiv) (Cornell University). The notebook uses the first 20,000 papers.

## Summary

An 8-week internship project that grows from data exploration into a production-style question-answering system over scientific papers.
The plan is hybrid retrieval (BM25 + embeddings) with a reranker and cited LLM answers (RAG), plus a text-to-SQL path for numbers, charts and KPIs, all measured with an evaluation set.
Everything is developed in one notebook (`notebooks/rag-project.ipynb`), and each week adds a new section and a new commit.

## Progress

| Week | Topic | Status |
|---|---|---|
| 1 | Dataset exploration (EDA), supervised vs unsupervised decision | Done |
| 2 | Data preparation: cleaning, features, stratified split, scaling and encoding (`X_train, X_test, y_train, y_test`) | Done |
| 3 | Retrieval baseline: embeddings, BM25, vector search | Planned |
| 4+ | Reranker, cited answers, evaluation, text-to-SQL, dashboard, deployment | Planned |

### Week 2 in short

The notebook builds a field classifier dataset from arXiv metadata: it predicts the main subject area of a paper (astro-ph, hep, cond-mat, math, ...).
It removes duplicates, handles missing values per column, creates new features (pages, figures, math symbols, journal family, DOI prefix),
splits 80/20 with `stratify=y`, and preprocesses with median + StandardScaler for numbers and most-frequent + OneHotEncoder for categories, fitted on the training data only.
The report is in `reports/`.

## How to run

1. Install the requirements: `pip install -r requirements.txt`
2. Open `notebooks/rag-project.ipynb` in Jupyter or Google Colab and choose **Run all**. The notebook downloads the 20,000-paper sample from Hugging Face by itself, no manual download needed.
3. To build the Week 2 report after the notebook has run:
   `python make_report.py --name "Your Name" --repo "https://github.com/<user>/rag-project"`



### Week 3: Supervised models

Trained KNN, Decision Tree and Logistic Regression on the arXiv field-classification data from Week 2 (10 classes, stratified 80/20 split). Best K and best max_depth were chosen with 5-fold cross-validation on the training data.
Models were compared with accuracy, confusion matrix, precision, recall and F1. Because the classes are unbalanced, macro F1 was also checked.

**Best model:** <model name>, accuracy <xx.x%>, macro F1 <0.xxx>

## Repository structure

```
rag-project/
├── README.md
├── requirements.txt
├── make_report.py            # builds the Word report from figures/ and outputs/results.json
├── notebooks/
│   └── rag-project.ipynb     # one notebook, new section every week
├── figures/                  # charts saved by the notebook
├── outputs/
│   └── results.json          # numbers saved by the notebook
└── reports/
    └── Week2_Report.docx
```
