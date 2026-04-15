# IDS705 Machine Learning Project  
**Spring 2026 – Duke MIDS**

## 👥 Team Members
- Tina Yuan  
- Gaurav Law
- Bruce Chen
- Arushi Singh

---

## Project Overview

This project focuses on applying supervised machine learning techniques to solve a real-world problem. Our goal is to build a predictive model that provides actionable insights for a defined stakeholder.

We follow a full machine learning pipeline:
- Data collection and preprocessing
- Feature engineering
- Model training and evaluation
- Interpretation and analysis

---

## Objective

The primary objective of this project is to:

> Develop a predictive model that accurately estimates the recession as an index which we can use to predict upcoming economic downfalls.

We evaluate model performance using appropriate metrics and compare multiple models to identify the best-performing approach.

---

## Repository Structure

.
├── data/                
├── notebooks/           
├── src/                 
├── results/             
├── requirements.txt     
└── README.md            

---

## Data

We use data from the following sources:
- World Bank (macroeconomic indicators)
- IMF (inflation, unemployment, etc.)
- OECD (leading indicators)

Due to size constraints, datasets are not included in this repository.

### To access the data:
1. Download the raw files from here: https://prodduke-my.sharepoint.com/:f:/g/personal/as1685_duke_edu/IgB8QMfkAElWQ6bEDt0SxeelAZnSm0iAzOBr-Dh-WFR4cHA?e=hsklm7

2. Place files in the `data/` directory  
---

## Setup & Installation

Clone the repository:

git clone https://github.com/tina-ytyuan/SP26_IDS705_ML_Project.git
cd SP26_IDS705_ML_Project

Install dependencies:

pip install -r requirements.txt

---

## Methodology

### 1. Data Preprocessing
- Cleaning missing values
- Feature selection
- Merging multiple datasets
- Normalization / scaling

### 2. Feature Engineering
- Creating derived indicators
- Handling temporal data
- Aggregations and transformations

### 3. Model Development
We experiment with:
- Logistic Regression (baseline)
- Random Forest
- Gradient Boosting (XGBoost / LightGBM)

### 4. Evaluation
We evaluate models using:
- AUC-ROC
- Accuracy / Precision / Recall
- Cross-validation

---

## Results

- Best model: **[insert ]**
- Key metric: **[insert ]**

Key insights:
- [Insight 1]
- [Insight 2]
- [Insight 3]

---

## Reproducibility

1. Run data preprocessing notebook:
notebooks/data_preprocessing.ipynb

2. Train models:
notebooks/regressions.ipynb
**[insert other models here]**

---

## Limitations

- Data availability constraints
- Potential distribution shifts over time
- Feature limitations across datasets

---

## Future Work

- Improve feature engineering
- Incorporate additional datasets
- Try deep learning models
- Deploy as an interactive dashboard or API

---

## References

- IDS705: Principles of Machine Learning (Duke University)
- World Bank Data
- IMF Data Portal
- OECD Data

---

## Acknowledgments

We thank the IDS705 instructional team for guidance and feedback throughout the project.

---

## License

This project is for academic purposes only.
