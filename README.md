## Project Overview

This project builds a 60-minute-ahead household electricity forecasting system using classical machine learning and deep learning on structured time-series data.

### Problem

The objective is to predict future `Global_active_power` one hour ahead using current and historical household electrical measurements.

Rather than predicting current power from simultaneous measurements, the project is framed as a true forecasting problem. This avoids an artificially easy setup and makes the task more representative of real-world predictive modeling.

### Dataset

The project uses the **UCI Individual Household Electric Power Consumption** dataset, which contains more than two million minute-level observations collected over several years.

The data includes measurements such as:

- Global active power
- Global reactive power
- Voltage
- Global intensity
- Sub-metering values

The original timeline was validated for timestamp continuity and duplicate records. Missing electrical measurements were preserved during feature creation so that long multi-day gaps were not artificially interpolated.

### Methodology

The project follows a chronological, leakage-aware modeling workflow.

Feature engineering included:

- lagged power measurements
- rolling means and standard deviations
- calendar features
- recent power-change features
- rolling maximum and minimum values
- rolling power range
- short-term volatility measures

The data was split chronologically into training, validation, and test periods. The test set remained untouched until final model selection was complete.

Models evaluated included:

- Persistence baseline
- Linear Regression
- Random Forest
- Histogram Gradient Boosting
- PyTorch multilayer perceptron
- Huber-loss neural network
- Reduced-dropout neural network

A second feature-engineering phase expanded the input set from 22 to 31 features and was evaluated using the same modeling procedure.

### Final Comparison

The strongest validation models were:

| Model | Validation RMSE |
|---|---:|
| Phase 2 Histogram Gradient Boosting | **0.7867** |
| Phase 1 Histogram Gradient Boosting | 0.7885 |
| Phase 2 Neural Network | 0.7973 |
| Phase 1 Neural Network | 0.7983 |
| Random Forest | 0.8074 |
| Linear Regression | 0.8724 |
| Persistence | 1.0713 |

The Phase 2 temporal feature expansion produced a modest improvement for both Histogram Gradient Boosting and the neural network, but it did not change the overall model ranking.

### Selected Model

The final selected model is **Phase 2 Histogram Gradient Boosting**.

It was selected because it achieved the best validation performance while remaining simpler and easier to train than the neural-network alternatives.

On the previously untouched test set, the selected model achieved:

- Test RMSE: **0.6912**
- Validation RMSE: **0.7867**
- Test RMSE change vs validation: **-12.14%**

The lower test error suggests that the final test period was easier to forecast than the validation period. Most importantly, the selected model maintained strong performance on unseen future data and showed no evidence of severe overfitting.

Residual analysis showed that most errors were concentrated near zero, while rare high-consumption periods remained more difficult to predict.

### Key Takeaway

The main conclusion of the project is that **deep learning was competitive but did not outperform Histogram Gradient Boosting on this structured tabular forecasting problem**.

The final model choice was therefore based on validation evidence rather than model complexity. The project demonstrates the value of chronological evaluation, careful feature engineering, controlled experiments, and evidence-based model selection.