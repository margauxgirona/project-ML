# Machine Learning Project: Predicting Electricity Price Variations

## 1. Context 

A wide range of factors influence electricity prices on a daily basis. Local weather conditions can directly impact both electricity production and demand. Longer-term phenomena such as climate change also play an important role. Geopolitical events—such as the war in Ukraine—can affect the price of key commodities used in electricity production, knowing that each country has its own specific energy mix (nuclear, solar, hydro, gas, coal, etc.).  
Moreover, countries can import or export electricity through dynamic interconnected markets such as those in Europe.  
These different elements make electricity price modeling particularly challenging at the country level.

### Objective

<ins> How can we explain and predict the daily variation in electricity futures prices using weather, energy, and commercial data from France and Germany? </ins>

The goal is to predict the **daily variation** (`TARGET`) of electricity prices based on multiple factors:

- **Local factors:** production, consumption, weather conditions  
- **Cross-border factors:** electricity exchanges between European countries  
- **Macroeconomic factors:** commodity price returns (gas, coal, carbon)

As explained in the project context, we also aim to relate our results to potential geopolitical events that occurred during the period covered by our data, in order to better understand the model’s predictio


## 2. Electricity Futures Price Variation Prediction 

### Project Overview
This project aims to predict the daily variation of electricity futures prices using multi-source data from *France* and *Germany*.  
Electricity prices depend on many interacting factors such as energy production levels, weather conditions, cross-border exchanges, and commodity price fluctuations.

The objective is to build predictive models capable of capturing non-linear relationships, country-specific behaviors, and threshold effects, while optimizing Spearman correlation, which measures the quality of ranking rather than exact values.

## 3. Dataset Summary 

- 1494 total samples  
  - France: 851  
  - Germany: 643  
- 35 numerical features (production, weather, exchanges, commodity returns)  
- 1 target: `TARGET` (daily return of electricity futures)
- All features are standardized (mean ≈ 0, std ≈ 1)

Preprocessing steps:
- Removal of redundant features with perfect correlation  
- Missing values imputed using median  
- `COUNTRY` encoded  
- `DAY_ID` removed (non-predictive)

## 4. Problem Formalization

**Task:** Regression  
**Goal:** Predict the relative variation of electricity futures prices  
**Metric:** Spearman correlation  
**Challenges:**
- Very low linear correlations (Pearson between 0.04 and 0.10)
- Strong non-linear effects
- Different behaviors between France and Germany
- Volatile and noisy target variable

## 5. Approach & Methodology

### **Baseline Models**
- Linear Regression (global dataset)
- Linear Regression (France only)
- Linear Regression (Germany only)

### **Feature Engineering**
- Threshold-based transformations (ReLU) for non-linear behaviors  
  Examples:
  - FR_CONSUMPTION > 1.5
  - DE_WINDPOW > 0.3
  - COAL_RET < -0.8
- Quantile transformation for skewed features
- Removal of redundant variables
- Feature selection based on Spearman correlation

### **Model Training**
- Train/test split: 80/20
- Scikit-learn pipeline including preprocessing and regression
- Evaluation using:
  - Spearman correlation
  - R²
  - Mean Squared Error (MSE)

## **Results Summary**

| Model | Dataset | Spearman (Train) | Spearman (Test) |
|-------|---------|------------------|------------------|
| Linear Regression | Global | ~28.9% | ~19.5% |
| Linear Regression | France only | ~24.5% | ~3.6% |
| Linear Regression | Germany only | ~44.9% | ~37.3% |
| Feature-engineered Linear Model | Global | ~30.4% | ~19.8% |

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
