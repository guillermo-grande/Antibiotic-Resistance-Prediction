# Antibiotic Resistance Prediction in *Staphylococcus aureus* using MALDI-TOF Spectra

This project explores the use of classical machine learning and deep learning methods to classify antimicrobial resistance in *Staphylococcus aureus* based on MALDI-TOF mass spectrometry data. It is a result of our work for the *IA en Salud* (AI in Health) course, focusing on the challenges of high-dimensional data, class imbalance, and the need for explainable models in a clinical microbiology setting. Check the [final report](https://github.com/guillermo-grande/Antibiotic-Resistance-Prediction/blob/main/Practica1_IASalud_Microbiologia.pdf) for more detailed information.

## Authors

- Guillermo Grande Santi  
- Jesús Martín Trilla  
- Lucía Ferrer Duaso

---

## 🧪 Problem Statement

The aim is to classify whether *S. aureus* isolates are resistant to:
- **Erythromycin**
- **Ciprofloxacin**

Each isolate is represented by a MALDI-TOF spectrum transformed into binned features. This high-dimensional and imbalanced dataset presents a challenging classification problem requiring dimensionality reduction, advanced sampling strategies, and model interpretation.

---

## 📊 Dataset

- Preprocessed and binned MALDI-TOF data.
- Continuous numerical features only (stepwise bins).
- Multi-label classification:
  - `00`: Sensitive to both
  - `10`: Resistant to Erythromycin only
  - `01`: Resistant to Ciprofloxacin only
  - `11`: Resistant to both
- Class distribution is highly imbalanced:
  - 64.1% `00`, 25.4% `10`, 5.3% `01`, 5.0% `11`

---

## 🔄 Preprocessing Steps

- **Scaling**: StandardScaler, MinMaxScaler, Log Transform
- **Dimensionality Reduction**:
  - PCA (95% variance or elbow at 324 components)
  - LASSO feature selection
  - Autoencoder latent space
  - mRMR (on reduced sets due to computation)
- **Data Resampling**:
  - Undersampling (Random)
  - Oversampling (SMOTE, SMOTETomek)
  - Balanced Batch Generators for neural nets
- **t-SNE** used for non-linear visualization

---

## 🧠 Models and Performance

### 🟩 Balanced Random Forest
- Best results for both antibiotics
- Max balanced accuracy:
  - **0.63** for Erythromycin
  - **0.70** for Ciprofloxacin
- Preprocessing: StandardScaler + LASSO

### 🔵 Dense Neural Networks
- Two dense layers with dropout and regularization
- Comparable to Random Forest:
  - **0.61** for Erythromycin
  - **0.66** for Ciprofloxacin

### 🟠 CNN with One-Class Calibration
- Conv1D with batch norm + ReLU
- Focal BCE as loss function
- Ensemble with one-class detection to calibrate predictions
- Accuracy ~0.60 but **no improvement in recall**

---

## 🧠 Explainability

We used **SHAP (SHapley Additive exPlanations)** to identify key protein bins contributing to predictions:

- **Erythromycin**:
  - 4508–4513 Da and 5438–5446 Da positively correlated with resistance
- **Ciprofloxacin**:
  - 6554–6556 Da, 4997–5010 Da among most influential

---

## 🧾 Conclusions

- **Balanced Random Forest** and **Dense Neural Networks** offer the best performance.
- **Linear dimensionality reduction** methods (e.g., PCA, LASSO) were limited due to the data’s **non-linear structure**.
- Data augmentation methods like SMOTE were **not effective** due to high dimensionality.
- **Explainable AI (SHAP)** helped highlight the importance of certain protein mass ranges in resistance prediction.

---

## ⚙️ Requirements

Key packages:
- `scikit-learn`
- `imbalanced-learn`
- `tensorflow` / `keras`
- `xgboost`
- `shap`
- `matplotlib`
- `seaborn`
- `pandas`
- `numpy`

---

## 📚 License

This repository is released under the MIT License.
