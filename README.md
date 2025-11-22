# Machine Learning Project: Predicting Electricity Price Variations

## 1. Context

A wide range of factors influence electricity prices on a daily basis. Local weather conditions can directly impact both electricity production and demand. Longer-term phenomena such as climate change also play an important role. Geopolitical events (such as the war in Ukraine) can affect the price of key commodities used in electricity production, knowing that each country has its own specific energy mix (nuclear, solar, hydro, gas, coal, etc.).  
Moreover, countries can import or export electricity through dynamic interconnected markets such as those in Europe.  
These different elements make electricity price modeling particularly challenging at the country level.

### Business Case

Electricity markets are highly volatile and influenced by multiple interconnected factors such as weather conditions, commodity prices, energy production levels, and cross-border flows. Market participants — including electricity producers, traders, and risk managers — must understand price dynamics to manage risk, hedge portfolios, and plan their operations.

This project focuses on modeling (not forecasting in time-series terms) the daily variation of short-term electricity futures prices in France and Germany. This aligns directly with Financial Market specialization.

By building a model capable of capturing the complex relationships between weather variables, production levels, commodity returns, and electricity usage metrics, we aim to understand the factors that explain the day-to-day variation in electricity baseload futures prices.

### Objective

<ins> How can we explain and predict the daily variation in electricity futures prices using weather, energy, and commercial data from France and Germany? </ins>

The goal is to predict the **daily variation** (`TARGET`) of electricity prices based on multiple factors:

- **Local factors:** production, consumption, weather conditions
- **Cross-border factors:** electricity exchanges between European countries
- **Macroeconomic factors:** commodity price returns (gas, coal, carbon)

As explained in the project context, we also aim to relate our results to potential geopolitical events that occurred during the period covered by our data, in order to better understand the model’s predictio

## 2. Electricity Futures Price Variation Prediction

### What is an electricity futures contract?

An _electricity future_ is a financial contract that allows market participants to buy, sell, speculate, or hedge against future movements in electricity prices.  
It represents an agreement to buy or sell electricity at a predefined price for delivery at a specific future date.  
Because electricity cannot be stored easily, futures play a key role in stabilizing revenues, reducing risk, and enabling speculation on price volatility.

### Project Overview

This project aims to predict the daily variation of electricity futures prices using multi-source data from _France_ and _Germany_.  
Electricity prices depend on many interacting factors such as energy production levels, weather conditions, cross-border exchanges, and commodity price fluctuations.

The objective is to build predictive models capable of capturing non-linear relationships, country-specific behaviors, and threshold effects, while optimizing Spearman correlation, which measures the quality of ranking rather than exact values.

## 3. Dataset Description

### 3.1 Source

Dataset available on the ENS Challenge Data platform:

🔗 **https://challengedata.ens.fr/participants/challenges/97/**

Provided by:

- **RTE France** (Réseau de Transport d’Électricité)
- **ENS Paris**

### 3.2 Files Provided

- `X_train.csv` — 1494 samples, 35 features
- `Y_train.csv` — TARGET (daily futures price variation)
- `X_test.csv` — 654 samples, same features but different time period

### 3.3 Feature Overview (35 variables)

**Identifiers:**

- `ID`
- `DAY_ID` (anonymized date)
- `COUNTRY` (FR or DE)

**Commodity returns:**

- `GAS_RET`
- `COAL_RET`
- `CARBON_RET`

**Weather metrics:**

- `x_TEMP`
- `x_RAIN`
- `x_WIND`

**Energy production:**

- `x_GAS`, `x_COAL`, `x_HYDRO`
- `x_NUCLEAR`, `x_SOLAR`, `x_WINDPOW`, `x_LIGNITE`

**Electricity usage:**

- `x_CONSUMPTION`
- `x_RESIDUAL_LOAD`
- `x_NET_IMPORT`
- `x_NET_EXPORT`

**Cross-border exchanges:**

- `DE_FR_EXCHANGE`
- `FR_DE_EXCHANGE`

### 3.4 Target Variable

- `TARGET`: the daily variation of 24h baseload electricity futures (France or Germany).

## 4. Problem Formalization

- **Type:** Regression
- **Goal:** Explain daily futures price variation (`TARGET`)
- **Metric:** Spearman correlation
- **Nature:** _Non-linear_, _noisy_, _heterogeneous_ modeling problem

### Main Challenges:

- Weak linear relationships
- High noise level in TARGET
- Country-specific dynamics
- Many interacting variables (35 features)
- Differences in predictability between FR and DE

## 5. Approach & Methodology

### Baseline Models

- Linear Regression (global dataset)
- Linear Regression (France only)
- Linear Regression (Germany only)
- Polynomial Regression
- Decision Tree Regression

### Advanced Models

- Random Forest Regressor
- Gradient Boosting Regressor
- XGBoost Regressor

### Feature Engineering

- Threshold-based transformations (ReLU) for non-linear behaviors  
  Examples:
  - FR_CONSUMPTION > 1.5
  - DE_WINDPOW > 0.3
  - COAL_RET < -0.8
- Quantile transformation for skewed features
- Removal of redundant variables
- Feature selection based on Spearman correlation

### Model Training

- Train/test split: 80/20
- Scikit-learn pipeline including preprocessing and regression
- Evaluation using:
  - Spearman correlation
  - R²
  - Mean Squared Error (MSE)

### WHY Spearman Correlation

Spearman correlation evaluates **rank-order monotonicity** rather than linearity.  
It is the official challenge metric because:

- TARGET is very noisy and highly volatile
- Relationships are non-linear
- Correctly ranking variations is more important than predicting exact values
- Pearson correlation is too weak to be useful

Spearman is computed by applying Pearson correlation on ranked variables, making it robust and appropriate for this task.

## 6. Handling Obstacles

### 1. Low Signal-to-Noise Ratio

→ Introduced non-linear transformations and threshold features  
→ Used Spearman correlation to evaluate monotonic relationships

### 2. Underfitting of Linear Models

→ Added polynomial features  
→ Switched to tree-based models

### 3. Overfitting in Tree-Based Models

→ Controlled depth  
→ Used early stopping for boosting methods  
→ Reduced feature space

### 4. Country Heterogeneity

→ Built separate models for France and Germany  
→ Compared performance per country

## **Results Summary**

_(To be completed later as the project progresses)_

| Model                    | Dataset      | Spearman (Train) | Spearman (Test) |
| ------------------------ | ------------ | ---------------- | --------------- |
| Linear Regression        | Global       | —                | —               |
| Linear Regression        | France only  | —                | —               |
| Linear Regression        | Germany only | —                | —               |
| Polynomial Regression    | —            | —                | —               |
| Decision Tree Regression | —            | —                | —               |

## **Key Observations**

- Germany's price variations are significantly more predictable than France’s.
- France shows extremely low signal-to-noise ratio.
- Threshold-based features improve monotonic relationships.
- Linear models establish a baseline but clearly show limitations.

## Team

- Mathis Fourreau
- Natacha Gaussin
- Margaux Girona  
  ESILV — IF3

### Bibliography

https://challengedata.ens.fr/participants/challenges/97/

https://www.sciencedirect.com/science/article/abs/pii/S1364032115013866?casa_token=7cGV5UA93xwAAAAA:BUhcOVfLbD5dupC4zZ53jStxBoCLME_91glTYGRvhO2NUIli0geEjzznQf_kU0hz8UXoAb44
