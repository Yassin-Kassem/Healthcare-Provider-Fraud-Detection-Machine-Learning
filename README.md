# DataOrbit - HealthCare Provider Fraud Detection

## Project Overview
This project aims to detect fraudulent Medicare providers using a dataset of inpatient and outpatient claims, along with beneficiary details. The goal is to build a machine learning model that can classify providers as "Fraud" or "Non-Fraud" based on aggregated claim patterns.

## Repository Structure
```
fraud_detection_project/
│
├── data/                 # Contains raw CSV files and processed data
│   ├── Train_Beneficiarydata.csv
│   ├── Train_Inpatientdata.csv
│   ├── Train_Outpatientdata.csv
│   ├── Train_Labels.csv
│   └── provider_features.csv (Generated)
│
├── notebooks/            # Jupyter Notebooks for analysis
│   ├── 01_data_exploration_and_feature_engineering.ipynb
│   ├── 02_modeling.ipynb
│   ├── 03_evaluation.ipynb
│
├── reports/              # Generated models and reports
│   ├── presentation.pptx
│   └── (Saved Models: lr_model.pkl, rf_model.pkl, xgb_model.pkl)
│
├── requirements.txt      # Python dependencies
└── README.md             # Project documentation
```

## Installation
1. Clone the repository.
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

## Usage
Run the notebooks in order:
1. **01_data_exploration_and_feature_engineering.ipynb**: 
   - Loads raw data and performs comprehensive EDA
   - Analyzes missing values, distributions, and fraud patterns
   - Engineers provider-level features through aggregation
   - Saves `provider_features.csv`

2. **02_modeling.ipynb**: 
   - Compares class imbalance strategies (Class Weights vs SMOTE)
   - Trains Logistic Regression, Random Forest, and XGBoost models
   - Uses Stratified K-Fold Cross-Validation
   - Generates model comparison table

3. **03_evaluation.ipynb**: 
   - Evaluates models using Precision, Recall, F1, ROC-AUC, PR-AUC
   - Generates ROC and PR curves, confusion matrices
   - Performs SHAP analysis for explainability
   - Conducts detailed error analysis on False Positives and False Negatives
   - Provides business justification and recommendations

## Methodology

### Data Relationships
- **BeneID**: Links beneficiaries to claims
- **Provider**: Links claims to fraud labels
- **Unit of Analysis**: Provider-level (all features aggregated to provider)

### Feature Engineering
Provider-level features include:
- **Financial**: Total/Average reimbursement amounts, deductibles
- **Volume**: Total claims, inpatient/outpatient ratios
- **Demographics**: Average patient age, unique beneficiary count
- **Medical Complexity**: Average diagnoses/procedures, chronic condition prevalence
- **Temporal**: Claim durations, visit patterns

### Class Imbalance Handling
- **Primary Strategy**: Class weights (balanced) for tree models, `scale_pos_weight` for XGBoost
- **Alternative Tested**: SMOTE (Synthetic Minority Over-sampling)
- **Rationale**: Class weights avoid introducing synthetic noise while effectively handling the ~10% fraud rate

### Models
1. **Logistic Regression**: Baseline linear model for interpretability
2. **Random Forest**: Robust ensemble method
3. **XGBoost**: High-performance gradient boosting (selected as final model)

### Validation
- **Strategy**: Stratified 5-Fold Cross-Validation
- **Test Set**: 20% hold-out for final evaluation
- **Metrics**: Precision, Recall, F1, ROC-AUC, PR-AUC

## Results
- **Best Model**: XGBoost
- **ROC-AUC**: 0.80 (strong discriminative ability)
- **F1 Score**: 0.17 (reflects class imbalance challenge)
- **Key Fraud Indicators**:
  1. High total inpatient reimbursement
  2. Extended claim durations
  3. High claim volume
  4. Specific chronic condition patterns

## Key Findings
- Providers with unusually high reimbursement amounts and claim counts are more likely to be fraudulent
- Inpatient claim patterns are stronger fraud predictors than outpatient patterns
- The model successfully identifies high-risk providers for investigation prioritization
- SHAP analysis reveals that financial features (reimbursement totals) are the strongest fraud signals

## Business Impact
- **Cost Reduction**: Enables targeted investigations, reducing wasted resources on legitimate providers
- **Fraud Detection**: Identifies sophisticated fraud patterns beyond simple rule-based systems
- **Explainability**: SHAP values provide investigators with clear reasons for flagging each provider
- **Scalability**: Can process thousands of providers monthly to prioritize audit resources

## Limitations & Future Work
- **False Positives**: High-volume legitimate providers may be flagged; requires domain expert review
- **Model Drift**: Fraud patterns evolve; quarterly retraining recommended
- **Feature Expansion**: Could incorporate network analysis (physician referral patterns) and temporal trends
- **Threshold Tuning**: Adjust probability threshold based on investigation capacity

## Contributors
- Yassin Kassem 13004730
- Youssef Othman 13002689
- Malek Khaled 13007364
- Ibrahim Madany 13006877


