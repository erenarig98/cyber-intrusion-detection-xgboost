[readme_md.md](https://github.com/user-attachments/files/33971588/readme_md.md)
# 🛡️ Cybersecurity Intrusion Detection System (IDS) using XGBoost

An end-to-end Machine Learning pipeline designed to detect network intrusions, unauthorized access, and malicious activities using telemetry data from network metrics and user behavior analytics.

---

## 📌 Table of Contents
- [Project Overview](#-project-overview)
- [Dataset Summary](#-dataset-summary)
- [Key Findings & Methodology](#-key-findings--methodology)
- [Model Performance](#-model-performance)
- [Project Structure](#-project-structure)
- [Installation & Usage](#-installation--usage)
- [Future Enhancements](#-future-enhancements)

---

## 🚀 Project Overview

Modern Security Operations Centers (SOC) deal with thousands of network alerts daily. Traditional rule-based security systems often fail against zero-day attacks or novel threat patterns. 

This project aims to automate **Network Intrusion Detection** by leveraging **XGBoost (Extreme Gradient Boosting)**. Originally evaluated with an unsupervised *Isolation Forest*, the pipeline was transitioned to a supervised approach due to the non-sparse distribution of malicious events (~45% attack traffic).

---

## 📊 Dataset Summary

The model uses the **Cybersecurity Intrusion Detection Dataset** (by Dinesh Naveen Kumar Samudrala) from Kaggle.

* **Network Metrics:** `network_packet_size`, protocol details, encryption status.
* **User Behavior Analytics:** `login_attempts`, `session_duration`, `failed_logins`, `unusual_time_access`, `ip_reputation_score`, `browser_type`.
* **Target Label:** `attack_detected` ($0$ = Normal Traffic, $1$ = Intrusion / Attack).

---

## 🔬 Key Findings & Methodology

1. **Why Unsupervised Isolation Forest Failed:** 
   Exploratory Data Analysis (EDA) revealed that attacks constitute roughly 45% of the overall traffic. Since Isolation Forest assumes anomalies are rare (< 5-10%), it produced low performance (Accuracy: 0.55, ROC-AUC: 0.56).
2. **Why XGBoost Excelled:**
   Intrusions in this dataset are driven by complex non-linear feature interactions (e.g., normal packet size combined with unusual access hours and poor IP reputation). XGBoost captures these relationships effectively using sequential gradient boosted decision trees.

---

## 📈 Model Performance

The XGBoost model achieved an overall **Accuracy of 88%** and an **AUPRC score of 0.9071**.

| Class | Precision | Recall | F1-Score | Support |
| :--- | :---: | :---: | :---: | :---: |
| **Normal Traffic (0)** | 0.83 | 0.99 | 0.90 | 1582 |
| **Attack / Intrusion (1)** | **0.98** | **0.75** | **0.85** | 1280 |

* **ROC-AUC Score:** `0.8767`
* **AUPRC Score:** `0.9071`
* **Key Takeaway:** High precision on attacks (0.98) minimizes False Positives and prevents alert fatigue in real-world SOC environments.

---

## 📂 Project Structure

```text
├── data/
│   └── cybersecurity_intrusion_dataset.csv
├── notebooks/
│   └── intrusion_detection_pipeline.ipynb
├── src/
│   ├── preprocess.py
│   └── train_xgboost.py
├── assets/
│   ├── confusion_matrix.png
│   └── auprc_curve.png
├── README.md
└── requirements.txt
```

---

## 🛠️ Installation & Usage

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/your-username/cyber-intrusion-detection-xgboost.git
   cd cyber-intrusion-detection-xgboost
   ```

2. **Install Dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the Pipeline:**
   ```bash
   python src/train_xgboost.py
   ```

---

## 🔮 Future Enhancements

* **Threshold Tuning:** Adjust classification threshold (e.g., from `0.50` to `0.35`) to boost Attack Recall from 0.75 to > 0.85.
* **Deep Learning Comparison:** Train a TabNet or Autoencoder architecture for comparative analysis.
* **API Deployment:** Wrap the trained model in a **FastAPI** interface for real-time network payload scoring.
