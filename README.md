# Foreign Currency Exchange Rate Forecast

## Machine Learning Mini Project

**Course:** UE24CS352A – Machine Learning  
**Project:** Foreign Currency Exchange Rate Forecast

---

## 1. Project Overview

Foreign exchange rates are highly volatile and difficult to forecast because they are influenced by financial markets, economic conditions, and historical price movements.

The objective of this project is to investigate whether machine learning models can predict short-term foreign exchange rate movements and future exchange-rate returns.

The project considers three major currency pairs:

- GBP/USD
- USD/EUR
- USD/JPY

Two machine learning prediction tasks are performed:

1. **Classification:** Predict whether the next-day return will be **UP** or **DOWN**.
2. **Regression:** Predict the numerical value of the **next-day log return**.

A simple machine-learning-based trading strategy is also evaluated using the classification predictions.

---

## 2. Reference Work

This project is based on the Stanford CS229 project:

> **Foreign Currency Exchange Rate Forecast**  
> Stanford CS229 Project, 2019

The reference work investigates machine learning approaches for forecasting foreign exchange movements.

The original study uses Bloomberg financial data. Since Bloomberg data is not freely available, this project uses publicly available **FRED** data as a public-data adaptation.

---

## 3. Problem Statement

Foreign exchange markets exhibit substantial volatility, and short-term movements are difficult to predict.

The problem addressed in this project is:

> Can historical foreign exchange information and engineered financial features be used to predict the direction and numerical value of the next-day foreign exchange return?

The project evaluates multiple machine learning algorithms and compares their performance using standard classification and regression metrics.

---

## 4. Dataset

### Data Source

The project uses historical foreign exchange data from the:

**Federal Reserve Bank of St. Louis (FRED)**

The following FRED exchange-rate series were used:

| Currency Pair | FRED Series |
|---|---|
| GBP/USD | `DEXUSUK` |
| USD/EUR | `DEXUSEU` |
| USD/JPY | `DEXJPUS` |

### Study Period

**January 2001 – December 2018**

The raw datasets are stored in:

```text
data/raw/
```

Files:

```text
GBP_USD.csv
USD_EUR.csv
USD_JPY.csv
```

### Dataset Characteristics

After preprocessing and feature engineering, the final modelling dataset contains:

- **4673 observations**
- **36 predictive features**
- **3 currency pairs**
- Chronological observations from **2001 to 2018**

---

## 5. Data Preprocessing

The following preprocessing steps were performed:

1. Load historical exchange-rate data.
2. Convert dates to datetime format.
3. Convert exchange-rate values to numeric values.
4. Restrict the data to the selected study period.
5. Sort observations chronologically.
6. Check for duplicate dates.
7. Handle missing exchange-rate observations using forward filling.
8. Calculate daily log returns.
9. Generate lagged and rolling financial features.
10. Create classification and regression targets.
11. Remove rows containing missing engineered features or targets.
12. Perform chronological train/validation/test splitting.
13. Scale features using `StandardScaler` where required.

**No random shuffling** was used when creating the train, validation, and test sets because future observations must not be used to predict the past.

---

## 6. Exploratory Data Analysis

Exploratory data analysis was performed to understand the behaviour of currency exchange rates and their returns.

The analysis includes:

- Historical exchange-rate plots
- Normalized exchange-rate trends
- Daily log-return plots
- Return summary statistics
- Correlation analysis

### Return Correlation

The observed correlation matrix between the daily returns was:

| | GBP/USD | USD/EUR | USD/JPY |
|---|---:|---:|---:|
| **GBP/USD** | 1.000000 | 0.646054 | -0.134159 |
| **USD/EUR** | 0.646054 | 1.000000 | -0.305487 |
| **USD/JPY** | -0.134159 | -0.305487 | 1.000000 |

The results show a relatively strong positive correlation between GBP/USD and USD/EUR returns, while USD/JPY has negative correlation with the other two currency pairs during the selected period.

---

## 7. Feature Engineering

Financial features were engineered from historical exchange-rate returns.

### 7.1 Lag Features

Previous return values were used as predictive features.

The following lags were created:

- 1-day lag
- 2-day lag
- 3-day lag
- 5-day lag

These features represent recent historical return information.

### 7.2 Moving Average Features

Moving-average features were created using:

- 5-day moving average
- 10-day moving average
- 20-day moving average

The moving averages are calculated using previous observations to avoid using future information.

### 7.3 Volatility Features

A **20-day rolling standard deviation** was calculated as a measure of recent return volatility.

### 7.4 Momentum Features

Momentum features were created using cumulative returns over:

- 5-day period
- 10-day period

These features capture recent directional movement in the currency market.

---

## 8. Prediction Targets

Two prediction targets were created.

### 8.1 Classification Target

The classification task predicts whether the next-day return is positive.

| Next-Day Return | Target |
|---|---:|
| `> 0` | `1` — UP |
| `<= 0` | `0` — DOWN |

The final target distribution was:

| Class | Count |
|---|---:|
| DOWN (`0`) | 2412 |
| UP (`1`) | 2281 |

The two classes are reasonably balanced.

### 8.2 Regression Target

The regression task predicts the numerical value of the **next-day log return**.

Target:

```text
Next-day log return
```

Approximate target statistics after preprocessing:

| Statistic | Value |
|---|---:|
| Count | 4693 |
| Mean | -0.000035 |
| Standard Deviation | 0.005905 |
| Minimum | -0.081694 |
| 25% | -0.003039 |
| Median | 0.000000 |
| 75% | 0.003178 |
| Maximum | 0.044349 |

---

## 9. Train / Validation / Test Split

A chronological split was used to prevent information from the future from leaking into the training process.

The final modelling dataset contains **4673 observations**.

Approximate split:

- **80% Training**
- **10% Validation**
- **10% Testing**

### Actual Split

| Dataset | Observations | Period |
|---|---:|---|
| Training | 3738 | 2001-01-31 to 2015-05-29 |
| Validation | 467 | 2015-06-01 to 2017-03-14 |
| Testing | 468 | 2017-03-15 to 2018-12-28 |

The test set was kept separate from model selection and was used only for final evaluation.

---

## 10. Machine Learning Models

Several machine learning algorithms were evaluated.

### Classification Models

The following models were used to predict the direction of the next-day return:

- Logistic Regression
- Random Forest
- Support Vector Machine (SVM)
- Multi-Layer Perceptron (MLP)

### Regression Models

The following models were used to predict the numerical next-day return:

- Random Forest
- Support Vector Machine (SVM)
- Multi-Layer Perceptron (MLP)
- Lasso Regression
- Ridge Regression

### Project Extension

As an additional model beyond the reference-model set, **XGBoost** was introduced for regression.

XGBoost is a gradient-boosted decision-tree method evaluated using the same validation procedure.

---

## 11. Classification Evaluation Metrics

The classification models were evaluated using:

- Accuracy
- Precision
- Recall
- F1 Score
- Confusion Matrix

The **F1 score** was also used for model comparison because it considers both precision and recall.

---

## 12. Regression Evaluation Metrics

The regression models were evaluated using:

- Mean Absolute Error (MAE)
- Mean Squared Error (MSE)
- Root Mean Squared Error (RMSE)
- R² Score

For regression:

- Lower MAE, MSE, and RMSE indicate smaller prediction errors.
- Higher R² indicates better explanatory performance.

---

## 13. Classification Validation Results

| Model | Accuracy | Precision | Recall | F1 Score |
|---|---:|---:|---:|---:|
| Random Forest | 0.5161 | 0.4710 | 0.6190 | **0.5350** |
| SVM | 0.4968 | 0.4552 | 0.6048 | 0.5194 |
| Logistic Regression | 0.4775 | 0.4346 | 0.5381 | 0.4809 |
| MLP | 0.5075 | 0.4565 | 0.5000 | 0.4773 |

### Best Classification Model

Based on validation F1 score:

**Random Forest**

- Validation F1: **0.5350**
- Validation Accuracy: **0.5161**

Therefore, Random Forest was selected as the final classification model.

---

## 14. Regression Validation Results

| Model | MAE | MSE | RMSE | R² |
|---|---:|---:|---:|---:|
| Lasso | 0.00482885 | 0.00005488 | **0.00740834** | -0.0059 |
| Ridge | 0.00494931 | 0.00005547 | 0.00744764 | -0.0166 |
| XGBoost | 0.00536010 | 0.00006188 | 0.00786628 | -0.1341 |
| Random Forest | 0.00545112 | 0.00006269 | 0.00791755 | -0.1490 |
| SVM | 0.00690770 | 0.00008907 | 0.00943771 | -0.6325 |
| MLP | 0.01359265 | 0.00045994 | 0.02144613 | -7.4299 |

### Best Regression Model

Based on validation RMSE:

**Lasso Regression**

- Validation RMSE: **0.00740834**

Therefore, Lasso Regression was selected as the final regression model.

---

## 15. Final Test Evaluation

After model selection, the selected classification and regression models were evaluated on the unseen test set.

### Random Forest Classification

| Metric | Test Result |
|---|---:|
| Accuracy | 0.5171 |
| Precision | 0.5043 |
| Recall | 0.2599 |
| F1 Score | 0.3430 |

### Test Confusion Matrix

```text
[[183  58]
 [168  59]]
```

The rows represent actual classes and the columns represent predicted classes.

---

## 16. Final Lasso Regression Results

The selected Lasso regression model produced the following test results:

| Metric | Test Result |
|---|---:|
| MAE | 0.00367070 |
| MSE | 0.00002380 |
| RMSE | 0.00487818 |
| R² | -0.0074 |

The results show that the numerical next-day return is difficult to predict accurately using the engineered historical features.

---

## 17. XGBoost Extension

XGBoost was introduced as an additional regression model.

Its validation performance was:

| Metric | XGBoost |
|---|---:|
| MAE | 0.00536010 |
| MSE | 0.00006188 |
| RMSE | 0.00786628 |
| R² | -0.1341 |

XGBoost did not outperform Lasso Regression on the validation dataset.

This provides an additional comparison between a traditional regularized linear regression model and a gradient-boosted tree ensemble.

---

## 18. Prediction Visualization

The project generates a comparison between:

- Actual next-day returns
- Predicted next-day returns

The figure is stored at:

```text
results/figures/actual_vs_predicted_returns.png
```

This visualization helps illustrate the difference between the actual market movement and the model's predicted return.

---

## 19. Trading Strategy Backtest

A simple trading strategy was developed using the Random Forest classification predictions.

### Strategy

```text
Predicted UP   → Invest in the currency
Predicted DOWN → Stay out of the market
```

The initial investment was:

**$10,000**

The strategy was evaluated over the test period.

### Machine Learning Strategy

| Metric | Result |
|---|---:|
| Final Value | $9,387.11 |
| Total Return | -6.13% |
| Maximum Drawdown | -8.86% |
| Sharpe Ratio | -0.7594 |
| Days Invested | 117 |
| Total Test Days | 468 |

### Buy-and-Hold

| Metric | Result |
|---|---:|
| Final Value | $10,385.05 |
| Total Return | +3.85% |

Therefore, the simple machine-learning trading strategy underperformed the buy-and-hold strategy during the test period.

---

## 20. Backtest Visualization

The trading strategy comparison is stored at:

```text
results/figures/trading_strategy_comparison.png
```

The backtest results are stored at:

```text
results/backtest_results.csv
```

---

## 21. Project Outputs

### Result Files

```text
results/
├── final_classification_results.csv
├── final_regression_results.csv
├── test_predictions.csv
└── backtest_results.csv
```

### Figures

```text
results/figures/
├── confusion_matrix_test.png
├── actual_vs_predicted_returns.png
└── trading_strategy_comparison.png
```

### Trained Models

```text
models/
├── random_forest_classifier.pkl
├── lasso_regressor.pkl
└── final_scaler.pkl
```

### Notebook

```text
notebooks/currency_exchange_forecasting.ipynb
```

The notebook contains the complete implementation, including:

- Data preprocessing
- Exploratory analysis
- Feature engineering
- Model training
- Evaluation
- Backtesting

---

## 22. Project Structure

```text
FOREIGN_CURRENCY_FORECAST/
│
├── data/
│   └── raw/
│       ├── GBP_USD.csv
│       ├── USD_EUR.csv
│       └── USD_JPY.csv
│
├── models/
│   ├── final_scaler.pkl
│   ├── lasso_regressor.pkl
│   └── random_forest_classifier.pkl
│
├── notebooks/
│   └── currency_exchange_forecasting.ipynb
│
├── report/
│   └── final_report.pdf
│
├── results/
│   ├── final_classification_results.csv
│   ├── final_regression_results.csv
│   ├── test_predictions.csv
│   ├── backtest_results.csv
│   │
│   └── figures/
│       ├── confusion_matrix_test.png
│       ├── actual_vs_predicted_returns.png
│       └── trading_strategy_comparison.png
│
├── .gitignore
├── README.md
└── requirements.txt
```

---

## 23. Technologies Used

| Technology | Purpose |
|---|---|
| Python | Project implementation |
| NumPy | Numerical computation |
| Pandas | Data processing |
| Matplotlib | Data visualization |
| Scikit-learn | Machine learning models and preprocessing |
| XGBoost | Gradient-boosted regression |
| Joblib | Model and scaler persistence |
| Jupyter Notebook | Development and experimentation |
| FRED | Historical financial data |

---

## 24. Installation

### 1. Clone or Download the Repository

```bash
git clone <repository-url>
cd FOREIGN_CURRENCY_FORECAST
```

### 2. Create a Virtual Environment

#### Linux / macOS

```bash
python3 -m venv venv
source venv/bin/activate
```

#### Windows

```powershell
python -m venv venv
venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## 25. Running the Project

Start Jupyter Notebook:

```bash
jupyter notebook
```

Open:

```text
notebooks/currency_exchange_forecasting.ipynb
```

Run the notebook cells sequentially from top to bottom.

### Complete Workflow

```text
Data Loading
     ↓
Data Cleaning
     ↓
Exploratory Data Analysis
     ↓
Return Calculation
     ↓
Feature Engineering
     ↓
Target Creation
     ↓
Chronological Data Split
     ↓
Feature Scaling
     ↓
Model Training
     ↓
Validation
     ↓
Model Selection
     ↓
Final Test Evaluation
     ↓
Trading Backtest
     ↓
Results and Figures
```

---

## 26. Reproducibility

The project follows a chronological data-processing and evaluation procedure.

Key reproducibility practices include:

- The test dataset is kept separate from model selection.
- Feature scaling is fitted only on the training data.
- The fitted scaler is then applied to the validation and test sets.
- The trained models and scaler are saved in the `models/` directory.
- The main implementation is contained in:

```text
notebooks/currency_exchange_forecasting.ipynb
```

---

## 27. Limitations

The project has several limitations:

1. The original Bloomberg dataset used in the reference work was not publicly available.
2. Public FRED data was therefore used as an alternative dataset.
3. The project uses historical data from 2001 to 2018.
4. Foreign exchange returns are highly noisy and difficult to predict.
5. The models use historical market-derived features and do not include all possible macroeconomic variables.
6. The simple trading strategy does not model transaction costs, commissions, bid-ask spreads, or slippage.
7. The trading backtest is intended for academic evaluation and should not be interpreted as a production trading system.

---

## 28. Conclusion

This project investigated machine learning approaches for forecasting foreign exchange movements using historical currency data.

Four classification models and six regression models, including the additional XGBoost extension, were evaluated.

**Random Forest** achieved the best validation classification performance:

- F1 Score: **0.5350**
- Accuracy: **0.5161**

**Lasso Regression** achieved the best validation regression performance:

- RMSE: **0.00740834**

On the unseen test set:

- Random Forest achieved approximately **51.7% accuracy**.
- Lasso Regression achieved an RMSE of approximately **0.00488**.

The results demonstrate that short-term foreign exchange forecasting is challenging. The relatively low classification performance and negative R² values indicate that historical features alone provide limited predictive power for next-day currency returns.

The trading backtest further showed that predictive performance does not necessarily translate into profitable trading:

- Machine-learning strategy: **-6.13%**
- Buy-and-hold strategy: **+3.85%**

Overall, the project demonstrates a complete machine-learning workflow for financial forecasting, from data collection and feature engineering through model comparison, final evaluation, and backtesting.

---

## Academic Note

This project is intended for **academic and educational evaluation**. The trading strategy and model predictions should not be interpreted as financial advice or as a production-ready trading system.
