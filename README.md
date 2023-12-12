# EDA

The EDA of this dataset can be found [here](https://anananan116.github.io/US_Power_Outage_Analysis/)

# Fitting a Model

## Prediction Problem

### Objective

- **Predict the Duration of Power Outages**

### Type

- **Regression**
  - The target variable, the duration of the power outage, is continuous.

### Response Variable (Target)

- `OUTAGE.DURATION`
  - This is chosen as it directly indicates the length of the power interruption.

### Features Considered

- `U.S._STATE`
  - Geographic location can be a significant factor due to different infrastructure and weather patterns.
- `ANOMALY.LEVEL`, `CLIMATE.CATEGORY`
  - These indicate climate conditions, crucial for predicting weather-related outages.
- `CAUSE.CATEGORY`, `CAUSE.CATEGORY.DETAIL`
  - Understanding the cause of the outage helps in estimating duration.
- `CUSTOMERS.AFFECTED`
  - The number of customers affected might correlate with the severity and duration of the outage.
- `RES.PRICE`
  - Residential price as an indirect indicator of infrastructure quality.
- `SEASON`
  - Different seasons have unique patterns impacting outage durations.

### Metric for Model Evaluation

- **Primary Metric**: Mean Absolute Error (MAE)
  - Chosen for its clear representation of average prediction error magnitude.
  - RMSE or MSE could be considered, but they emphasize larger errors more.

### Justification of Feature Selection

- All features are known at the time of the power outage, making them relevant for real-time predictions.
- Utility companies typically have access to this information (cause, affected customers, climate conditions) when an outage occurs.

## Baseline Model

### Baseline Linear Regression Model Description

We developed a baseline linear regression model focusing on predicting the duration of power outages. This model incorporated two quantitative features: `SEASON`(norminal) and `CUSTOMERS.AFFECTED`(quantitative). These features were selected based on their availability and presumed relevance to the prediction task. No encoding was needed as both are quantitative. To normalize the range of these variables, we employed StandardScaler, a common practice in regression analysis.

### Model Performance

The model's performance was evaluated using the Mean Absolute Error (MAE). It yielded an MAE of 2901 on the training set and 2427 on the test set. These high MAE values suggest a significant deviation between the model's predictions and the actual outage durations. Given the context of power outages, this level of error indicates that the model's predictive accuracy is relatively low.

### Evaluation and Conclusion

The current linear regression model appears inadequate for accurately predicting power outage durations. The use of only two quantitative features and the simplistic nature of a linear regression model limit its capability to capture the complex dynamics of power outage durations. These durations are likely influenced by various factors, including but not limited to environmental conditions and infrastructure, which are not adequately represented in the current model. To enhance predictive accuracy, it would be prudent to consider a broader range of features and explore more sophisticated modeling approaches that can capture non-linear relationships and interactions among variables.

## Final Model Development and Evaluation

In developing my final model, I placed significant emphasis on the selection and engineering of features. I incorporated a mix of categorical variables - `U.S._STATE`, `CLIMATE.CATEGORY`, `CAUSE.CATEGORY`, `CAUSE.CATEGORY.DETAIL`, and `SEASON` - and numerical features like `ANOMALY.LEVEL`, `CUSTOMERS.AFFECTED`, and `RES.PRICE`. My rationale was rooted in the intrinsic relevance of these features to the phenomenon of power outages. For instance, the specific state reflects varying infrastructure and weather conditions, while the cause and climate category directly correlate with outage durations. These features were chosen to capture the broad spectrum of factors impacting power outages, aiming for a more accurate and nuanced predictive model. More detailed rationale of choosing these features are provided when introducing them in the first section.

The RandomForestRegressor was the chosen algorithm for its proficiency in handling complex, non-linear data interactions and its ensemble learning approach, which is effective against overfitting. The best performing hyperparameters in my model were `n_estimators=100` and `max_depth=20`, identified through a systematic GridSearchCV process. This process tested combinations of `n_estimators` (10, 50, 100) and `max_depth` (None, 10, 20, 30), crucial for defining the forest structure. The choice of a higher number of estimators and unrestricted tree depth was aimed at capturing detailed patterns in the data while maintaining computational efficiency.

The performance of my final model, as indicated by a lower Mean Absolute Error on both training and test sets (1068.75,
 1188.43), marked a notable improvement over the baseline model. This enhancement is attributed to the comprehensive feature set, which offered a deeper insight into the dynamics of power outages, and the optimized RandomForestRegressor, capable of effectively learning from this diverse dataset. The methodical hyperparameter tuning via GridSearchCV contributed significantly to ensuring not just the accuracy but also the generalizability of the model, making it a robust advancement over the simpler baseline approach.

## Fairness Analysis of the Final Model

### Groups Definition

- **Group X**: Power outages caused by severe weather.
- **Group Y**: Power outages due to other reasons.

### Hypotheses

- **Null Hypothesis**: The model is fair. The MAE for Group X (severe weather) and Group Y (other causes) is roughly the same, with any differences being due to random chance.
- **Alternative Hypothesis**: The model is unfair. The MAE for Group X differs significantly from that of Group Y.

### Test Statistic and Significance Level

- **Test Statistic**: Absolute difference in MAE between Group X and Group Y.
- **Significance Level**: 0.05

### Results

- **Original MAE Difference**: 246.34
- **Mean of Permutation MAE Differences**: 300.50
- **P-Value**: 0.505

### Conclusion

The p-value of 0.505 suggests that there is insufficient evidence to reject the null hypothesis. This indicates that the model's performance, as measured by MAE, does not significantly differ between Group X (severe weather-related outages) and Group Y (outages due to other reasons).
