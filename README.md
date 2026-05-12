# 🧬 PCA for Dimensionality Reduction — NHANES Diabetes Risk

Comparing a baseline KNN classifier against a PCA-reduced KNN classifier on the NHANES diabetes risk dataset to evaluate the trade-off between predictive performance and prediction speed.

---

## 📌 Overview

This project explores whether applying **Principal Component Analysis (PCA)** before training a K-Nearest Neighbors (KNN) classifier improves or hurts performance on a high-dimensional health survey dataset. It also measures the impact on prediction speed.

**Key question:** Is it worth compressing 166 features into 99 principal components if it retains 95% of the variance?

---

## 📂 Dataset

- **Source:** Modified [NHANES dataset](https://drive.google.com/file/d/1CE2giLLTp0Cjn0bn9hmtCWIxGZf8_uO1/view?usp=sharing)
- **Target variable:** `Diabetes_Risk` (binary classification)
- **Features:** 166 numerical survey responses related to diet and health
- **Split:** 75% train / 25% test (`random_state=321`)

---

## ⚙️ Workflow

```
1. Load & explore data
2. Preprocess
   ├── Set SEQN as index & drop duplicates
   ├── Train/test split
   ├── Impute missing values (median strategy)
   └── Standard scale all features
3. Model A — KNN on all 166 features
4. Apply PCA (retain 95% variance → 99 components)
5. Model B — KNN on 99 principal components
6. Compare performance & prediction speed
```

---

## 🤖 Models

| | Model A | Model B |
|---|---|---|
| Algorithm | KNN (default) | KNN (default) |
| Input | 166 original features | 99 principal components |
| PCA | ✗ | ✓ (`n_components=0.95`) |

---

## 📊 Results

### Performance (Test Set)

| Metric | Model A (No PCA) | Model B (PCA 95%) |
| ----------------- | ---------------- | ----------------- |
| Accuracy | 0.81 | **0.82** |
| Precision (macro) | 0.39 | **0.47** |
| Recall (macro) | 0.34 | **0.35** |
| F1 (macro) | 0.32 | **0.33** |

### Speed

**Model B (PCA)** was faster at prediction time — reducing 166 features to 99 principal components directly lowers the cost of KNN distance calculations across the test set.

---

## 💡 Key Findings

- **PCA improved performance**, not just speed. With 166 features, many dimensions are redundant or noisy, which degrades KNN distance calculations (curse of dimensionality). PCA's compression acts as a noise-reduction step that makes distances more meaningful.
- **`n_components=0.95`** is the correct, data-driven way to set PCA components — it automatically selects the minimum number of components needed to explain ≥ 95% of variance, rather than hardcoding an arbitrary number.
- **99 out of 166 features** were sufficient to capture 95% of the dataset's variance.

---

## 🛠️ Requirements

```bash
pip install numpy pandas matplotlib scikit-learn
```

---

## 🚀 Usage

1. Clone the repo and open the notebook:

```bash
git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name
jupyter notebook PCA_Core_Final.ipynb
```

2. The dataset is loaded automatically from Google Drive inside the notebook — no manual download needed.

---

## 📁 Project Structure

```
├── PCA_Core_Final.ipynb   # Main notebook
└── README.md
```

---

## 📖 References

- [NHANES — National Health and Nutrition Examination Survey](https://www.cdc.gov/nchs/nhanes/index.htm)
- [scikit-learn PCA documentation](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html)
- [scikit-learn KNeighborsClassifier documentation](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html)
