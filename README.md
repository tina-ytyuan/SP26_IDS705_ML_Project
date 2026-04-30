# Predicting Recessions Using Global Macroeconomic Indicators

**IDS 705: Principles of Machine Learning | Spring 2026 | Duke MIDS**  
**Team 5:** Bruce Chen, Gaurav Law, Arushi Singh, Tina Yuan

---

## Project Overview

Recessions impose major economic and social costs, but official recession labels are often assigned only after a downturn has already begun. This project builds a machine learning recession early-warning system that estimates the probability that a country will experience a GDP contraction in the following year.

Using publicly available macroeconomic indicators from the World Bank, IMF, and OECD, we construct a country-year panel covering more than 200 economies. The final output is a country-level **recession risk index** between 0 and 1, where higher values indicate greater estimated risk of recession in the next year.

This project is designed for stakeholders such as:

- Government policy institutions and central banks
- Multilateral development organizations
- Financial institutions and risk teams
- Economic research and monitoring teams

---

## Research Question

Can publicly available global macroeconomic indicators be used to predict next-year recession risk across a large set of countries?

More specifically, we ask:

1. How well can macroeconomic indicators rank countries by next-year recession risk?
2. Do broader feature sets improve performance relative to GDP-only features?
3. How reliable are the model's probability estimates after calibration?
4. Does performance vary across country income groups?
5. How does the model behave during an unprecedented shock such as COVID-19?

---

## Data Sources

This project uses three public macroeconomic data sources:

| Source | Description |
|---|---|
| World Bank World Development Indicators (WDI) | GDP growth, inflation, trade, government consumption, current account, and other country-level macro indicators |
| IMF World Economic Outlook (WEO) | Core macroeconomic indicators including inflation, unemployment, and GDP-related variables |
| OECD Composite Leading Indicators (CLI) | Forward-looking indicators for OECD economies |

Due to file size and data-access constraints, the raw datasets are not stored directly in this repository.

### Accessing the Data

1. Download the project data from the shared data folder:

   `https://prodduke-my.sharepoint.com/:f:/g/personal/as1685_duke_edu/IgB8QMfkAElWQ6bEDt0SxeelAZnSm0iAzOBr-Dh-WFR4cHA?e=hsklm7`

2. Place the downloaded files in the repository's `data/` directory.

3. Keep the folder structure consistent with the paths used in the notebook.

---

## Target Definition

The prediction target is:

```text
recession_next_year = 1 if GDP_growth(t + 1) < 0
recession_next_year = 0 otherwise
```

This means each country-year observation is used to predict whether that country will experience negative real GDP growth in the following year.

---

## Repository Structure

```text
SP26_IDS705_ML_Project/
├── code/                  # Supporting scripts or code used during the project
├── data/                  # Raw and processed data files; not fully tracked in GitHub
├── regressions.ipynb      # Existing modeling notebook in the repository
├── rf_recession_index.ipynb      # Existing modeling notebook in the repository
├── requirements.txt       # Python package requirements
├── README.md              # Project documentation
└── .gitignore             # Files and folders excluded from version control
```

---

## Environment Setup

Clone the repository:

```bash
git clone https://github.com/tina-ytyuan/SP26_IDS705_ML_Project.git
cd SP26_IDS705_ML_Project
```

Create and activate a virtual environment:

```bash
python -m venv venv
source venv/bin/activate
```

For Windows PowerShell:

```bash
python -m venv venv
venv\Scripts\Activate.ps1
```

Install dependencies:

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

If Jupyter is not available after installation, run:

```bash
pip install notebook ipykernel
python -m ipykernel install --user --name recession-ml --display-name "Python (recession-ml)"
```

---

## How to Run the Pipeline

The project pipeline is currently run through the notebook workflow.

### Step 1: Add the Data

Download the raw data files and place them in the `data/` directory.

Expected workflow:

```text
data/
├── raw files from World Bank / IMF / OECD
├── intermediate cleaned files, if generated
└── final merged panel, if generated
```

### Step 2: Launch Jupyter

```bash
jupyter notebook
```

or:

```bash
jupyter lab
```

### Step 3: Open the Modeling Notebook


```text
rf_recession_index.ipynb
```

### Step 4: Run the Notebook from Top to Bottom

The notebook should be run in order. The expected pipeline is:

1. Import packages
2. Load World Bank, IMF, and OECD data
3. Clean and harmonize country names
4. Remove aggregate regions and non-country entities
5. Merge all sources into a country-year panel
6. Engineer lagged and change-based macroeconomic features
7. Define `recession_next_year`
8. Split data using time-based windows
9. Train Random Forest models across feature configurations
10. Evaluate models using AUC-ROC, Brier score, and AUPRC
11. Apply probability calibration using Platt scaling
12. Run income-group analysis
13. Run threshold and false-alarm analysis
14. Run COVID-era stress test
15. Generate final tables and figures

### Step 5: Review Outputs

The main outputs are:

- Model performance tables
- AUC-ROC and Brier score comparisons
- Calibrated recession risk index
- Income-group performance breakdown
- Threshold tradeoff table
- COVID stress-test results
- Feature-importance analysis

If output files are generated, we recommend saving them to:

```text
outputs/
├── figures/
└── tables/
```

---

## Modeling Approach

We test multiple feature configurations to understand whether broader macroeconomic data improves recession prediction relative to simpler GDP-based indicators.

| Feature Set | Description |
|---|---|
| A: GDP only | GDP growth, lags, and year-over-year changes |
| B: Core macro | GDP, inflation, and government consumption indicators |
| C: All macro | Broader macroeconomic indicators including trade and current account variables |
| D: OECD only | OECD leading indicators |
| E: Full model | Combined macroeconomic and OECD indicators |

The final reported model family is a **Random Forest classifier** with class-weighted balancing. Hyperparameters are selected using time-aware validation rather than random train-test splitting.

---

## Train / Validation / Test Design

To avoid leaking future information into the model, the data is split by year:

| Split | Years | Purpose |
|---|---:|---|
| Training | Through 2012 | Model fitting |
| Validation | 2013-2016 | Hyperparameter tuning and calibration |
| Test | 2017-2018 | Main held-out evaluation |
| COVID stress test | 2019-2022 | Regime-shift robustness check |

This design reflects the real forecasting problem: models must be trained on past data and evaluated on future years.

---

## Evaluation Metrics

Because the goal is to produce a recession risk index rather than only a binary yes/no prediction, we evaluate both ranking performance and probability quality.

| Metric | Interpretation |
|---|---|
| AUC-ROC | How well the model ranks recession cases above non-recession cases |
| Brier Score | How accurate the predicted probabilities are; lower is better |
| AUPRC | Precision-recall performance under class imbalance |
| Bootstrap Confidence Intervals | Uncertainty around model performance estimates |
| Threshold Analysis | Tradeoff between catching recessions and generating false alarms |

---

## Key Results

The strongest model configurations identify recession risk substantially better than random guessing on the held-out 2017-2018 test period.

| Configuration | Test AUC | Test Brier Score |
|---|---:|---:|
| GDP only | 0.835 | 0.154 |
| Core macro | 0.800 | 0.161 |
| Full macro + OECD | 0.762 | 0.134 |
| All macro | 0.759 | 0.112 |
| OECD only | 0.697 | 0.202 |

Key takeaways:

- The GDP-only model achieves the highest test AUC point estimate.
- Confidence intervals overlap across configurations, so no feature set is statistically superior based on the available test data.
- Probability calibration improves the usefulness of the risk scores without changing ranking performance.
- The model performs most reliably for lower-middle-income countries.
- Performance drops sharply during the COVID-era stress test, showing that the model cannot anticipate shocks driven by mechanisms outside historical macroeconomic patterns.

---

## Income-Group Analysis

Model performance varies meaningfully across World Bank income groups.

| Income Group | Interpretation |
|---|---|
| Low income | Too few recession cases to draw strong conclusions |
| Lower middle income | Strongest and most reliable performance |
| Upper middle income | Weakest performance; use with caution |
| High income | Moderate performance; likely needs financial-market indicators |

This is an important limitation because a global recession monitoring tool should not be assumed to work equally well across all types of economies.

---

## Threshold Analysis

The calibrated recession risk score can be converted into warnings using different thresholds. Lower thresholds catch more recessions but also generate more false alarms.

Examples:

| Threshold | Best Use Case |
|---:|---|
| 0.05 | Highly risk-averse monitoring; catches many recessions but creates many false alarms |
| 0.10 | Balanced surveillance setting |
| 0.20 | More conservative monitoring with fewer false alarms |
| 0.40 | High-confidence alerts only |

The appropriate threshold depends on the stakeholder's cost of missing a recession versus the cost of acting on a false alarm.

---

## Limitations

This project has several important limitations:

1. **Annual data limits forecasting precision.** The model cannot provide monthly or quarterly warnings.
2. **Financial-market indicators are missing for many countries.** This likely limits performance for high-income economies.
3. **COVID-19 caused a regime shift.** The model performs close to chance during the COVID stress-test period.
4. **Some countries have sparse or inconsistent reporting.** Missingness may affect model reliability, especially for low-income or fragile economies.
5. **The recession target is based on negative annual GDP growth.** This is useful for global comparability but simpler than official country-specific recession dating.

---

## Recommended Use

This recession index should be used as a complementary monitoring tool, not as a standalone decision system.

Recommended use cases:

- Prioritizing countries for further analyst review
- Supporting early-warning dashboards
- Comparing relative recession risk across countries
- Identifying where macroeconomic conditions are deteriorating

Not recommended as-is for:

- Automatic policy triggers
- High-income economy monitoring without financial indicators
- Real-time crisis detection during unprecedented global shocks

---

## Future Work

Future improvements could include:

- Adding financial-market indicators such as yield curves, credit spreads, and equity volatility
- Testing quarterly or monthly versions of the pipeline
- Expanding model families beyond Random Forests
- Improving country-level missing-data handling
- Developing a dashboard for recession risk monitoring
- Adding model cards or documentation for stakeholder use
- Conducting additional subgroup analysis by region and income group

---

## Team Contributions

- **Bruce Chen:** Modeling lead; implemented model training, hyperparameter tuning, and test-set evaluation.
- **Gaurav Law:** Feature engineering lead; constructed lagged features, change features, and the next-year recession target.
- **Arushi Singh:** Data engineering lead; acquired and harmonized World Bank, IMF, and OECD datasets; built country-name mapping and aggregate-entity filtering.
- **Tina Yuan:** Evaluation, visualization, and writing lead; produced figures, income-group analysis, threshold analysis, and final report materials.

---

## References

- World Bank World Development Indicators
- IMF World Economic Outlook
- OECD Composite Leading Indicators
- Stock and Watson (1989), *New Indexes of Coincident and Leading Economic Indicators*
- Estrella and Mishkin (1998), *Predicting U.S. Recessions: Financial Variables as Leading Indicators*
- Vrontos, Galakis, and Vrontos (2021), *Modeling and Predicting U.S. Recessions Using Machine Learning Techniques*
- Chalaux and Turner (2024), *Doombot Versus Other Machine-Learning Methods for Evaluating Recession Risks in OECD Countries*

---

## License

This repository was created for academic use as part of Duke University's IDS 705: Principles of Machine Learning course.
