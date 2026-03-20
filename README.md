# 🧠 Smart Labeling Pipeline

## Overview

This project builds a cost-effective and scalable data labeling pipeline by combining:

* Human annotation
* Weak supervision (rule-based labeling)
* Active learning
* LLM-based labeling

The goal is to maximize label quality while minimizing manual effort and cost.

---

## 🚀 Objectives

* Create a **Gold Standard dataset** using human annotators
* Use **programmatic rules** to label data at scale
* Optimize labeling using **Active Learning**
* Leverage **LLMs for bulk labeling**
* Detect noisy or incorrect labels

---

## 🛠️ Tech Stack

* Python
* Pandas, NumPy
* Scikit-learn
* Snorkel
* modAL (Active Learning)
* Cleanlab (conceptual implementation)
* Label Studio

---

## 📂 Dataset

* 300 IMDB movie reviews
* File: `movie_reviews_300.csv`

---

## 🔄 Pipeline Workflow

---

## 🧩 Task 1: Human Annotation (Gold Standard)

### Setup

* Hosted **Label Studio** locally
* Created project: `Movie_Sentiments`
* Imported dataset
* Configured labels:

  * Positive
  * Negative
  * Neutral

### Annotation Process

* 3 annotators labeled first 100 reviews independently

  * Annotator A
  * Annotator B
  * Annotator C

### Outputs

* `annotator_a.csv`
* `annotator_b.csv`
* `annotator_c.csv`

---

### 📊 Inter-Annotator Agreement

* Implemented **Fleiss’ Kappa from scratch**
* Compared with `statsmodels` implementation

---

### ⚔️ Conflict Resolution

#### Conflict Detection

* Identified reviews where annotators disagreed

#### Sample Output

* Displayed 5 conflicting examples with all 3 labels

#### Adjudication Logic

* Majority vote → final label
* All different → assign **Neutral**

### Output

* `gold_standard_100.csv`

---

## 🧩 Task 2: Weak Supervision

### Heuristic Functions

Developed at least 3 rules, such as:

* Keyword-based sentiment (e.g., “excellent”, “terrible”)
* Text length checks
* Punctuation intensity (e.g., “!!!”)

---

### Snorkel Integration

* Wrapped heuristics as `@labeling_function`
* Applied to remaining 200 reviews

### Metrics

* **Coverage**: % of labeled samples
* **Conflict Rate**: % of conflicting labels

---

### Adjudication

* Used **Majority Vote** to assign weak labels

### Output

* `weak_labels_200.csv`

---

## 🧩 Task 3: Active Learning

### Setup

* Seed dataset: `gold_standard_100.csv`
* Pool: 200 weakly labeled reviews

---

### Query Strategies (Implemented from Scratch)

* **Least Confidence Sampling**
* **Entropy Sampling**

---

### Iterative Training Loop

* Train Logistic Regression model
* For 5 iterations:

  * Select top 10 uncertain samples
  * Add labeled data to training set
  * Retrain model
  * Record accuracy

---

### Visualization

* Learning curve:

  * X-axis → Number of labeled samples
  * Y-axis → Model accuracy

* Compared:

  * Active Learning
  * Random Sampling

---

## 🧩 Task 4: LLM Labeling & Noise Detection

### LLM Labeling Pipeline

* Designed **Few-Shot Prompt** (3 examples from gold dataset)
* Used Gemini API (free tier)
* Labeled ~150 reviews

### Output

* `llm_labels_150.csv`

---

### 🧹 Noise Detection (Cleanlab Logic)

* Trained Logistic Regression on LLM-labeled data

#### High Confidence Disagreement:

* Model probability > 0.90 for class A
* LLM label = class B

---

### Output

* Printed top 5 suspicious reviews

---

## 📁 Final Deliverables

* Jupyter Notebook (fully executed)

* Label Studio screenshot

* CSV Files:

  * `annotator_a.csv`
  * `annotator_b.csv`
  * `annotator_c.csv`
  * `gold_standard_100.csv`
  * `weak_labels_200.csv`
  * `llm_labels_150.csv`


## ⚠️ Notes

* Human annotation is treated as ground truth but may still contain bias
* Heuristics are noisy and incomplete by design
* Active learning significantly reduces labeling cost
* LLM labels may contain hallucinations

---

## 📌 Future Improvements

* Use transformer models instead of Logistic Regression
* Improve heuristics using NLP techniques
* Add ensemble labeling strategies
* Automate noise correction with Cleanlab

