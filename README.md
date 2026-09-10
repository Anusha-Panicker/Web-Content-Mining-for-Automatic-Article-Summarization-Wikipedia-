# 🧠 Web Content Mining for Automatic Article Summarization

### **Wikipedia-based Extractive Text Summarization using TF-IDF, Cosine Similarity & TextRank**

[![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)](https://www.python.org/)
[![Google Colab](https://img.shields.io/badge/Google%20Colab-Notebook-orange?logo=googlecolab)](https://colab.research.google.com/)
[![NLP](https://img.shields.io/badge/Domain-NLP-green)](https://en.wikipedia.org/wiki/Natural_language_processing)
[![TextRank](https://img.shields.io/badge/Algorithm-TextRank-purple)](https://en.wikipedia.org/wiki/Automatic_summarization)

---

## 👩‍💻 Author

**Anusha Panicker**
**Reg. No.:** UST24R0101006
**Subject:** Data Mining & Warehousing (DMW) — CA 1

---

## 📌 Project Overview

This project implements a **Web Content Mining system for automatic article summarization** using content extracted directly from **Wikipedia web pages**.

The system accepts a Wikipedia article URL, retrieves the article content through the Wikipedia API, processes the text, identifies relationships between sentences, ranks sentences according to their importance, and generates a concise **extractive summary**.

The complete summarization pipeline combines:

> **Web Content Extraction → Text Preprocessing → TF-IDF → Cosine Similarity → PageRank/TextRank → Sentence Selection → ROUGE Evaluation**

The core text-processing, similarity, ranking, and evaluation operations are implemented as a **custom Python pipeline**, providing a clear understanding of how the summarization process works internally.

---

## 🎯 Objective

> **Extract and mine key content patterns from web pages for automatic summarization.**

The project aims to:

* Extract article content from Wikipedia URLs
* Clean and preprocess web-based text
* Represent sentences using **TF-IDF**
* Measure sentence relationships using **Cosine Similarity**
* Rank sentences using a **PageRank-based TextRank approach**
* Generate an extractive summary
* Evaluate the generated summary using **ROUGE-1, ROUGE-2 and ROUGE-L**

---

## ✨ Key Features

* 🌐 **Wikipedia URL-based input**
* 📄 Automatic web content extraction
* 🧹 Text preprocessing and tokenization
* 📊 TF-IDF vectorization
* 🔗 Cosine similarity calculation
* 🕸️ Sentence similarity graph
* ⭐ PageRank-based sentence ranking
* ✂️ Extractive summary generation
* 📈 ROUGE-1, ROUGE-2 and ROUGE-L evaluation
* 🧪 Small test case for algorithm verification
* 📉 Visualization of ROUGE scores and sentence importance

---

# 🔄 System Architecture

```text
                    Wikipedia Article URL
                             │
                             ▼
                    ┌─────────────────┐
                    │  Wiki Fetcher   │
                    │  Wikipedia API   │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │     Text        │
                    │  Preprocessing  │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ Sentence Split  │
                    │   & Tokenize    │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │    TF-IDF       │
                    │  Vectorization  │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ Cosine Similarity│
                    │     Matrix      │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │   TextRank /    │
                    │    PageRank     │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ Sentence Ranking│
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ Extractive      │
                    │    Summary      │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ ROUGE Evaluation│
                    └─────────────────┘
```

---

# 🧩 Methodology

## 1️⃣ Wikipedia Data Fetching

The system accepts a Wikipedia article URL such as:

```text
https://en.wikipedia.org/wiki/Quantum_computing
```

The `WikiFetcher` class:

* Parses the Wikipedia URL
* Identifies the language
* Extracts the article title
* Builds a Wikipedia API request
* Retrieves the article's plain text
* Handles missing article content

The implementation supports both **English and Hindi Wikipedia URLs**.

---

## 2️⃣ Text Preprocessing

Raw web content contains unnecessary formatting and words that may not contribute significantly to sentence similarity.

The `TextPreprocessor` performs:

### Sentence Processing

* Removes Wikipedia-style section headings
* Removes unnecessary spaces and line breaks
* Splits the article into sentences
* Removes very short sentences

### Token Processing

* Converts text to lowercase
* Removes special characters
* Tokenizes sentences
* Removes common stopwords

Example:

```text
Original:
"Quantum computers use quantum states to process information."

After preprocessing:
["quantum", "computers", "use", "quantum", "states", "process", "information"]
```

---

# 📊 3️⃣ TF-IDF Vectorization

Each sentence is represented numerically using **TF-IDF (Term Frequency–Inverse Document Frequency)**.

### Term Frequency

TF measures how frequently a word occurs within a sentence.

$$
TF(t,d)=\frac{\text{Number of occurrences of }t}
{\text{Total number of terms in }d}
$$

### Inverse Document Frequency

IDF measures how rare a word is across the sentences.

$$
IDF(t)=\log\left(\frac{N+1}{DF(t)+1}\right)+1
$$

### TF-IDF

$$
TFIDF(t,d)=TF(t,d)\times IDF(t)
$$

The resulting vectors provide a numerical representation of each sentence.

---

# 🔗 4️⃣ Cosine Similarity

Cosine similarity determines how closely two sentences are related based on their TF-IDF vectors.

$$
\text{Similarity}(A,B)=
\frac{A\cdot B}
{\|A\|\|B\|}
$$

A higher value indicates that two sentences have more similar content.

These similarity values are then used to construct a **sentence relationship graph**.

---

# ⭐ 5️⃣ TextRank / PageRank

The project uses a PageRank-style iterative algorithm to calculate the importance of each sentence.

Each sentence is treated as a node in a graph, while cosine similarity represents the relationship between sentences.

The ranking is updated iteratively using:

$$
S(V_i)=
\frac{1-d}{N}
+d\sum_{V_j\rightarrow V_i}
w_{ji}S(V_j)
$$

where:

* `d` = damping factor
* `N` = number of sentences
* `w` = normalized sentence similarity
* `S(V)` = sentence importance score

The implementation uses:

```text
Damping factor = 0.85
Maximum iterations = 100
Tolerance = 1e-4
```

Sentences receiving higher importance scores are considered more relevant to the article.

---

# ✂️ 6️⃣ Extractive Summarization

After calculating sentence importance scores:

1. Sentences are ranked by their TextRank score.
2. The top `N` sentences are selected.
3. Selected sentences are reordered according to their original position.
4. The final sentences form the extractive summary.

Unlike abstractive summarization, the system **does not generate new sentences**.

It selects important sentences directly from the original article.

---

# 📈 7️⃣ ROUGE Evaluation

The generated summary is evaluated using:

### ROUGE-1

Measures overlap of individual words (unigrams).

### ROUGE-2

Measures overlap of two-word sequences (bigrams).

### ROUGE-L

Uses the **Longest Common Subsequence (LCS)** between the generated and reference summaries.

For each metric, the system calculates:

* Precision
* Recall
* F1 Score

The ROUGE evaluator is implemented directly in the project.

> **Evaluation note:** For the current demonstration, the first line of the fetched article is used as a proxy reference summary. A human-written reference summary would provide a more meaningful evaluation.

---

# 🧪 Example Experiment

### Input Article

```text
Quantum Computing
```

Wikipedia URL:

```text
https://en.wikipedia.org/wiki/Quantum_computing
```

### Input Statistics

```text
Total sentences: 337
Summary sentences: 10
```

### Generated Summary

The system identifies important information related to:

* Quantum computers
* Qubits
* Quantum computation
* Quantum gates
* Quantum circuits
* Quantum teleportation
* Quantum algorithms
* Cryptography
* Machine learning applications

The selected sentences are extracted from the original article and arranged in their original order.

---

# 📊 ROUGE Results

| Metric  | Precision | Recall | F1 Score   |
| ------- | --------- | ------ | ---------- |
| ROUGE-1 | 0.0864    | 0.5676 | **0.1500** |
| ROUGE-2 | 0.0371    | 0.2466 | **0.0645** |
| ROUGE-L | 0.0679    | 0.4459 | **0.1179** |

### Interpretation

The evaluation shows that the generated summary captures a portion of the information present in the reference text, while the relatively low precision indicates that the proxy reference is much shorter than the generated extractive summary.

Therefore, the ROUGE values should be interpreted as a demonstration of the implemented evaluation process rather than as a benchmark of summarization quality.

---

# 📉 Visualizations

The project generates visualizations for:

### ROUGE F1 Scores

A bar chart compares the F1 scores of:

```text
ROUGE-1
ROUGE-2
ROUGE-L
```

### Sentence Importance

A line plot displays the PageRank/TextRank importance score assigned to each sentence.

This provides a visual representation of how the algorithm identifies important sentences within the article.

---

# 🧪 Algorithm Verification

A small test case is included to verify that the TextRank algorithm works correctly.

### Input

```text
Cats are small animals.
Cats like to sleep a lot.
Dogs are loyal animals.
```

### Vocabulary

```text
['animals', 'cats', 'small', 'like',
 'lot', 'sleep', 'dogs', 'loyal']
```

### Sentence Importance Scores

```text
[0.48650659, 0.23984437, 0.27364904]
```

### Generated Summary

```text
Cats are small animals.
Dogs are loyal animals.
```

This test demonstrates that the algorithm can identify and select important sentences based on sentence relationships.

---

# 🛠️ Technologies & Libraries

| Technology              | Purpose                               |
| ----------------------- | ------------------------------------- |
| **Python**              | Core programming language             |
| **NumPy**               | Numerical computations and matrices   |
| **Pandas**              | Data handling and ROUGE result tables |
| **Matplotlib**          | Visualization                         |
| **Seaborn**             | ROUGE visualization                   |
| **Regular Expressions** | Text cleaning                         |
| **urllib**              | Wikipedia API requests                |
| **JSON**                | API response processing               |
| **Google Colab**        | Development environment               |

---

# 📁 Project Structure

```text
dmw-project/
│
├── summarizer_model.py
│
├── DMW_CA1_Anusha.ipynb
│
└── README.md
```

### `summarizer_model.py`

Contains the complete custom summarization pipeline:

```text
WikiFetcher
      ↓
TextPreprocessor
      ↓
TFIDFVectorizer
      ↓
TextRankSummarizer
      ↓
ROUGEEvaluator
      ↓
summarize_url()
```

---

# 🚀 How to Run

### Step 1 — Open the Notebook

Open the project notebook in Google Colab.

### Step 2 — Mount Google Drive

The project creates a dedicated directory:

```text
/content/drive/MyDrive/dmw_project
```

### Step 3 — Generate the Model File

Run the cell that creates:

```text
summarizer_model.py
```

### Step 4 — Import the Custom Model

```python
from summarizer_model import summarize_url
```

### Step 5 — Provide a Wikipedia URL

```python
url = "https://en.wikipedia.org/wiki/Quantum_computing"
```

### Step 6 — Select Summary Length

```python
TOP_N_SENTENCES = 10
```

### Step 7 — Generate the Summary

```python
result = summarize_url(url, top_n=TOP_N_SENTENCES)
```

The system then performs the complete pipeline automatically.

---

# 💡 What Makes This Project Interesting?

### 🌐 Real Web Content

The system does not rely on a fixed article stored inside the notebook. It can retrieve content directly from a Wikipedia URL.

### 🧠 Explainable NLP

Every major step of the summarization process can be inspected:

```text
Tokens
   ↓
TF-IDF
   ↓
Similarity
   ↓
Sentence Graph
   ↓
PageRank Scores
   ↓
Summary
```

### 🔍 Graph-Based Summarization

Instead of simply selecting sentences containing frequent words, the project considers **relationships between sentences**.

### 📐 Mathematical Implementation

The project demonstrates the underlying mathematics of:

* TF
* IDF
* TF-IDF
* Cosine Similarity
* PageRank
* N-gram overlap
* Longest Common Subsequence
* Precision
* Recall
* F1 Score

### 🧩 Modular Design

The project follows an object-oriented structure with separate classes for:

```text
Data Fetching
Text Preprocessing
Vectorization
Summarization
Evaluation
```

This makes the system easier to understand, test, and extend.

---

# 🎓 Learning Outcomes

Through this project, I gained practical experience in:

* Web Content Mining
* Natural Language Processing
* Text preprocessing
* Feature extraction
* TF-IDF vectorization
* Cosine similarity
* Graph-based ranking
* TextRank summarization
* ROUGE evaluation
* Object-Oriented Programming
* Building an end-to-end NLP pipeline

---

# 🔮 Future Improvements

The current system can be extended by:

* Adding more web content sources
* Improving sentence segmentation
* Using a larger and more comprehensive stopword list
* Adding stemming or lemmatization
* Improving reference-summary generation
* Using human-written reference summaries for evaluation
* Adding ROUGE-L visualization and detailed comparisons
* Supporting additional languages
* Creating a simple web interface
* Allowing users to paste any supported article URL directly into an application

---

# 📌 Conclusion

This project demonstrates how **Web Content Mining and NLP techniques can be combined to automatically identify important information from online articles**.

By integrating **TF-IDF, Cosine Similarity, and TextRank**, the system converts a raw Wikipedia article into a concise extractive summary while also providing quantitative evaluation through **ROUGE metrics**.

The project provides an interpretable, modular, and practical implementation of an automatic text summarization pipeline.

---

## 👩‍💻 Developed by

### **Anusha Panicker**

**B.Tech Artificial Intelligence & Machine Learning**

**Data Mining & Warehousing — CA 1**

---

### 🚀 End-to-End Web Content Mining & NLP Project

