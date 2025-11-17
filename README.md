# MSCS-634 Project Deliverable 2: Regression Modeling and Performance Evaluation

Author: Nitish Dhinaharan
Course: MSCS-634 Advanced Big Data and Data Mining

## Dataset Summary

This deliverable uses the Telco Customer Churn dataset (7,043 records, 21 features) to build regression models predicting MonthlyCharges based on customer demographics, contract types, and service usage. The dataset includes a mix of categorical and numerical features, requiring data preprocessing, encoding, and scaling before modeling. Because the dataset contains rich service-level detail (e.g., InternetService, StreamingTV, DeviceProtection), it is well-suited for regression tasks.

## Regression Modeling and Performance Evaluation

1. Feature Engineering

  To improve prediction performance:
    - A new variable, num_services, was created to reflect the number of subscribed services per customer.
    - Categorical features were encoded using OneHotEncoder.
    - Numerical features (tenure, TotalCharges, num_services) were standardized with StandardScaler.

2. Data Pipeline and Models

  A full preprocessing + modeling pipeline was built using ColumnTransformer and Pipeline. Three regression models were trained: Linear Regression, Ridge Regression and Lasso Regression.

3. Evaluation Metrics

  Models were evaluated using:
    - R² (Coefficient of Determination)
    - MSE (Mean Squared Error)
    - RMSE (Root Mean Squared Error)
    - 5-fold Cross-Validation R²

4. Performance Visualization

<img width="476" height="154" alt="Screenshot 2025-11-16 at 9 23 31 PM" src="https://github.com/user-attachments/assets/4ff6aeb3-0097-4c1e-ba8e-1d59668cb7a4" />

<img width="1013" height="493" alt="Screenshot 2025-11-16 at 9 23 52 PM" src="https://github.com/user-attachments/assets/b5fe0778-b8f7-48f5-92c7-de0ab2fa6a65" />

The plots show:
  - All models reach near-perfect R² (~0.999).
  - RMSE values are extremely low (~1.05) and nearly identical.
  - No meaningful visual difference across the three models.

## Key Insights and Observations

1. All models performed almost identically

- The extremely high R² (0.999) and tight RMSE range (~1.048–1.051) indicate:
- The target variable (MonthlyCharges) is highly predictable from the available features.
- The underlying relationship is nearly linear after one-hot encoding.
- Regularization (Ridge, Lasso) had minimal impact because:
  - Multicollinearity appears low after encoding.
  - The dataset contains strong, clear predictive signals (tenure, InternetService, service add-ons).
   
2. Regularized models did not significantly outperform Linear Regression

Because the performance gap is negligible:
  - Ridge and Lasso did not meaningfully reduce error.
  - The data’s structure supports linear modeling extremely well.

3. Cross-validation confirms models are not overfitting

Cross-validated R² = 0.999 for all models, indicating:
  - Excellent generalization
  - Stable performance across folds
  - No variance inflation or model instability

4. Strong predictors

From preprocessing and model weights (optional inspection), the following heavily influence MonthlyCharges:
  - InternetService type
  - StreamingTV / StreamingMovies
  - OnlineSecurity / DeviceProtection
  - num_services
  - tenure

These make intuitive sense, as customers pay more when they subscribe to more premium services.

## Challenges

1. Extremely high R² scores suggested possible data leakage

One notable challenge was the extremely high R² scores across all models, which initially raised concerns about potential data leakage or overly optimistic model behavior. Upon review, however, this outcome was traced to the dataset’s structure—many service-related categorical features directly correspond to how monthly charges are calculated, making the target variable inherently predictable.

I addressed this by reviewing the preprocessing to ensure MonthlyCharges was not accidentally included in features, and confirmed that the model’s high performance is legitimate due to tightly structured billing rules.

2. Large number of categorical variables

Another challenge involved the large number of categorical variables, particularly those related to service subscriptions. These features required proper encoding to avoid dimensionality issues or inconsistent handling during model training. 

I addressed this by implementing a robust preprocessing pipeline using OneHotEncoder within a ColumnTransformer, ensuring that all categories were encoded consistently and efficiently. 

3. Differences across models were extremely small

One final challenge was -- because all three models—Linear Regression, Ridge, and Lasso—produced nearly identical performance metrics, comparing them meaningfully became difficult. This was resolved by shifting the evaluation focus from absolute performance values to considerations of model stability, cross-validation results, and interpretability. 

Collectively, addressing these challenges improved the reliability of the regression pipeline and confirmed that modeling decisions were grounded in a thorough understanding of dataset characteristics.
