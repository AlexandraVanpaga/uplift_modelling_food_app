# Uplift Modeling for Yandex Food

Uplift modeling project for identifying users most likely to respond to marketing interventions.

---

## Description

The project builds an uplift model to detect users with the highest incremental response to marketing treatment (e.g., promotional offers or campaigns). The dataset contains features related to user activity, communication channels, behavioral patterns, and treatment assignment information.

### Key Features:
- X-Learner architecture with RandomForest as the base model
- Feature engineering based on correlation and importance analysis
- Hyperparameter tuning via Optuna
- 12x better than random prediction in identifying responsive users
- Applicable for customer prioritization in marketing campaigns

---

## Project Structure

```
uplift_modeling/
├── data/
│   ├── raw/                      # Raw data
│   └── processed/                # Processed data
├── src/
│   ├── eda.py                    # Exploratory data analysis
│   ├── feature_engineering.py    # Feature generation
│   ├── model.py                  # X-Learner model training
│   ├── hyperparameter_tuning.py  # Optuna-based hyperparameter search
│   ├── evaluation.py             # Model evaluation and metrics
│   └── interpret.py              # Results interpretation and visualization
├── notebooks/
│   └── solution.ipynb            # Main notebook with analysis
├── requirements.txt
└── README.md
```

---

## Installation

### Requirements
- Python 3.8+
- scikit-learn
- causalml
- Optuna

### Installing Dependencies

```bash
# Create virtual environment
python -m venv .venv

# Windows
.venv\Scripts\activate.bat

# Linux/Mac
source .venv/bin/activate

# Install packages
pip install -r requirements.txt
```

---

## Methodology

### Why X-Learner?

X-Learner was chosen for the following reasons:

- **Data heterogeneity** — significant differences in response across user subgroups
- **Class imbalance** — imbalance between control and treatment groups, which X-Learner handles more robustly than simpler approaches (e.g., T-Learner or S-Learner)

RandomForestClassifier was used as the base estimator after benchmarking against other algorithms.

### How It Works

```
User Features → X-Learner (Stage 1: Outcome Models)
                         → (Stage 2: CATE Models)
                         → Predicted Uplift Score per User
                         → Prioritization (Top-K users)
```

1. Two outcome models are trained separately on treatment and control groups
2. CATE (Conditional Average Treatment Effect) models are trained on the residuals
3. A propensity-weighted combination produces the final uplift scores
4. Users are ranked by predicted uplift for campaign targeting

---

## Feature Engineering

Features were generated based on correlation analysis and feature importance scores. Interaction features between the most informative variables were added to capture non-linear relationships.

### Final Feature Set

| Feature | Description |
|---------|-------------|
| `zip_code` | User location |
| `newbie` | New user flag |
| `channel` | Communication channel |
| `recency` | Recency of last activity |
| `history_segment` | User activity segment |
| `womens` | Women's category activity |
| `mens` | Men's category activity |
| `womens_x_recency_high` | Interaction: women's activity × high recency |
| `premium_inactive` | Premium user with low activity |
| `newbie_womens_high_recency` | Interaction: new user × women's × high recency |
| `recency history_segment` | Interaction: recency × activity segment |
| `recency womens` | Interaction: recency × women's activity |
| `recency mens` | Interaction: recency × men's activity |
| `history_segment womens` | Interaction: activity segment × women's |
| `history_segment mens` | Interaction: activity segment × men's |
| `womens mens` | Interaction: women's × men's activity |

---

## Hyperparameter Tuning

Hyperparameter optimization was performed using **Optuna**.

- **Target metric:** Uplift@30% (maximization)
- **Search space:** RandomForest parameters (n_estimators, max_depth, min_samples_split, etc.)
- **Strategy:** TPE sampler with early stopping

---

## 📈 Results

### Model Metrics (Best X-Learner + RandomForest)

| Metric | Value |
|--------|-------|
| Uplift AUC | 0.0223 |
| Qini AUC | 0.0513 |
| Uplift@30% | 0.0461 |
| Lift vs Random | 12x |

### Uplift Curve

The uplift curve demonstrates consistent positive lift across the top user segments, confirming the model's ability to separate responsive users from the general population.

---

## Interpretation

The metrics are moderate, which reflects the relatively small uplift present in the original data. However, the statistically significant difference between treatment and control groups confirms the presence of a real effect.

**Uplift@30% = 0.0461** means that among the top 30% of users ranked by predicted uplift, the actual response rate is **4.6% higher** than in the control group.

The model performs **12x better than random** selection, demonstrating a clear ability to identify users with elevated response to treatment.

For certain segments (e.g., users with low spending), uplift may be negative. This is expected behavior for uplift models, as they focus on users who genuinely respond to the intervention — applying treatment to unresponsive or negatively-responsive users is correctly avoided.

---

## Conclusions

- X-Learner with RandomForest delivered stable results on a task with heterogeneous and imbalanced data
- Synthetic interaction features improved predictive performance by capturing non-linear relationships between user attributes
- The model is ready for production use in customer prioritization for marketing campaigns, increasing the efficiency of promotional communications

---

## Technologies

- **scikit-learn** — base ML models (RandomForest)
- **causalml** — X-Learner implementation
- **Optuna** — hyperparameter optimization
- **pandas / numpy** — data processing
- **matplotlib / seaborn** — visualization and uplift curves
- **scipy** — statistical significance testing
EOF
Salida

# Uplift Modeling for Yandex Food

Uplift modeling project for identifying users most likely to respond to marketing interventions.

---

## Description

The project builds an uplift model to detect users with the highest incremental response to marketing treatment (e.g., promotional offers or campaigns). The dataset contains features related to user activity, communication channels, behavioral patterns, and treatment assignment information.

### Key Features:
- X-Learner architecture with RandomForest as the base model
- Feature engineering based on correlation and importance analysis
- Hyperparameter tuning via Optuna
- 12x better than random prediction in identifying responsive users
- Applicable for customer prioritization in marketing campaigns

---

## Project Structure

```
uplift_modeling/
├── data/
│   ├── raw/                      # Raw data
│   └── processed/                # Processed data
├── src/
│   ├── eda.py                    # Exploratory data analysis
│   ├── feature_engineering.py    # Feature generation
│   ├── model.py                  # X-Learner model training
│   ├── hyperparameter_tuning.py  # Optuna-based hyperparameter search
│   ├── evaluation.py             # Model evaluation and metrics
│   └── interpret.py              # Results interpretation and visualization
├── notebooks/
│   └── solution.ipynb            # Main notebook with analysis
├── requirements.txt
└── README.md
```

---

## Installation

### Requirements
- Python 3.8+
- scikit-learn
- causalml
- Optuna

### Installing Dependencies

```bash
# Create virtual environment
python -m venv .venv

# Windows
.venv\Scripts\activate.bat

# Linux/Mac
source .venv/bin/activate

# Install packages
pip install -r requirements.txt
```

---

## Methodology

### Why X-Learner?

X-Learner was chosen for the following reasons:

- **Data heterogeneity** — significant differences in response across user subgroups
- **Class imbalance** — imbalance between control and treatment groups, which X-Learner handles more robustly than simpler approaches (e.g., T-Learner or S-Learner)

RandomForestClassifier was used as the base estimator after benchmarking against other algorithms.

### How It Works

```
User Features → X-Learner (Stage 1: Outcome Models)
                         → (Stage 2: CATE Models)
                         → Predicted Uplift Score per User
                         → Prioritization (Top-K users)
```

1. Two outcome models are trained separately on treatment and control groups
2. CATE (Conditional Average Treatment Effect) models are trained on the residuals
3. A propensity-weighted combination produces the final uplift scores
4. Users are ranked by predicted uplift for campaign targeting

---

## Feature Engineering

Features were generated based on correlation analysis and feature importance scores. Interaction features between the most informative variables were added to capture non-linear relationships.

### Final Feature Set

| Feature | Description |
|---------|-------------|
| `zip_code` | User location |
| `newbie` | New user flag |
| `channel` | Communication channel |
| `recency` | Recency of last activity |
| `history_segment` | User activity segment |
| `womens` | Women's category activity |
| `mens` | Men's category activity |
| `womens_x_recency_high` | Interaction: women's activity × high recency |
| `premium_inactive` | Premium user with low activity |
| `newbie_womens_high_recency` | Interaction: new user × women's × high recency |
| `recency history_segment` | Interaction: recency × activity segment |
| `recency womens` | Interaction: recency × women's activity |
| `recency mens` | Interaction: recency × men's activity |
| `history_segment womens` | Interaction: activity segment × women's |
| `history_segment mens` | Interaction: activity segment × men's |
| `womens mens` | Interaction: women's × men's activity |

---

## Hyperparameter Tuning

Hyperparameter optimization was performed using **Optuna**.

- **Target metric:** Uplift@30% (maximization)
- **Search space:** RandomForest parameters (n_estimators, max_depth, min_samples_split, etc.)
- **Strategy:** TPE sampler with early stopping

---

## 📈 Results

### Model Metrics (Best X-Learner + RandomForest)

| Metric | Value |
|--------|-------|
| Uplift AUC | 0.0223 |
| Qini AUC | 0.0513 |
| Uplift@30% | 0.0461 |
| Lift vs Random | 12x |

### Uplift Curve

The uplift curve demonstrates consistent positive lift across the top user segments, confirming the model's ability to separate responsive users from the general population.

---

## Interpretation

The metrics are moderate, which reflects the relatively small uplift present in the original data. However, the statistically significant difference between treatment and control groups confirms the presence of a real effect.

**Uplift@30% = 0.0461** means that among the top 30% of users ranked by predicted uplift, the actual response rate is **4.6% higher** than in the control group.

The model performs **12x better than random** selection, demonstrating a clear ability to identify users with elevated response to treatment.

For certain segments (e.g., users with low spending), uplift may be negative. This is expected behavior for uplift models, as they focus on users who genuinely respond to the intervention — applying treatment to unresponsive or negatively-responsive users is correctly avoided.

---

## Conclusions

- X-Learner with RandomForest delivered stable results on a task with heterogeneous and imbalanced data
- Synthetic interaction features improved predictive performance by capturing non-linear relationships between user attributes
- The model is ready for production use in customer prioritization for marketing campaigns, increasing the efficiency of promotional communications

---

## Technologies

- **scikit-learn** — base ML models (RandomForest)
- **causalml** — X-Learner implementation
- **Optuna** — hyperparameter optimization
- **pandas / numpy** — data processing
- **matplotlib / seaborn** — visualization and uplift curves
- **scipy** — statistical significance testing
