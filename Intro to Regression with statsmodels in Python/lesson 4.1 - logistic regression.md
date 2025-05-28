# Logistic Regression - Customer Churn Analysis

## Introduction

When working with binary response variables (like customer churn: yes/no), standard linear regression isn't appropriate. Logistic regression is designed specifically for binary outcomes and provides more meaningful predictions. This analysis explores customer churn in a financial services dataset.

## Part 1: Exploring Binary Response Variables

### The Challenge with Binary Data

When the response variable is binary (0/1, True/False), all data points lie on horizontal lines at y=0 and y=1. This makes it difficult to see:

- How the explanatory variable is distributed within each group
- Patterns that might predict the binary outcome
- The relationship between predictor and response variables

### Solution: Grouped Histograms

Use histograms of the explanatory variable, split by the binary response, to reveal underlying patterns.

### Task

Create histograms showing the distribution of `time_since_last_purchase` for churned vs. non-churned customers.

### Solution

```python
# Create the histograms of time_since_last_purchase split by has_churned
sns.displot(x="time_since_last_purchase", data=churn, col="has_churned")
plt.show()
```

**Code Explanation:**

- `x="time_since_last_purchase"` - explanatory variable for histogram
- `data=churn` - dataset containing customer information
- `col="has_churned"` - creates separate histogram panels for each churn status
- Results in side-by-side histograms: one for churned customers, one for retained customers

### What to Look For

- **Different distributions** between churned and non-churned customers
- **Separation patterns** that suggest predictive relationships
- **Overlap regions** where prediction might be difficult
- **Skewness differences** between the two groups

## Part 2: Visualizing Linear vs. Logistic Models

### The Problem with Linear Regression for Binary Outcomes

Linear regression can produce:

- **Predictions outside [0,1] range** (e.g., -0.3 or 1.7 probability)
- **Straight-line trends** that don't reflect sigmoid reality of binary processes
- **Inappropriate assumptions** about error distributions

### Logistic Regression Advantages

- **Predictions bounded between 0 and 1** (proper probabilities)
- **S-shaped (sigmoid) curve** that reflects real binary relationships
- **Appropriate statistical properties** for binary outcomes

### Task

Compare linear and logistic regression trend lines for customer churn data.

### Solution

```python
# Draw a linear regression trend line and a scatter plot of time_since_first_purchase vs. has_churned
sns.regplot(x="time_since_first_purchase", y="has_churned", data=churn,
            line_kws={"color": "red"})
plt.show()
```

**Code Explanation:**

- `sns.regplot()` - creates scatter plot with regression line
- `x="time_since_first_purchase"` - length of customer relationship
- `y="has_churned"` - binary churn outcome (0 or 1)
- `data=churn` - customer dataset
- `line_kws={"color": "red"}` - makes trend line red for visibility
- By default, `regplot()` fits a linear trend, but can be configured for logistic

### Expected Visual Differences

- **Linear trend**: Straight line that may extend beyond [0,1] bounds
- **Logistic trend**: S-shaped curve bounded between 0 and 1
- **Data points**: All clustered on y=0 and y=1 lines
- **Prediction quality**: Logistic curve better matches binary nature

## Part 3: Fitting Logistic Regression Models

### The logit() Function

Logistic regression uses the `logit()` function instead of `ols()`:

- **Same syntax** as linear regression for formula and data
- **Different underlying mathematics** using maximum likelihood estimation
- **Sigmoid transformation** of linear combination of predictors

### Task

Model customer churn probability based on relationship length.

### Solution

```python
# Import logit
from statsmodels.formula.api import logit

# Fit a logistic regression of churn vs. length of relationship using the churn dataset
mdl_churn_vs_relationship = logit("has_churned~time_since_first_purchase", data=churn).fit()

# Print the parameters of the fitted model
print(mdl_churn_vs_relationship.params)
```

**Code Explanation:**

- `from statsmodels.formula.api import logit` - imports logistic regression function
- `logit("has_churned~time_since_first_purchase", data=churn)` - defines model
  - `has_churned` - binary response variable (0 = retained, 1 = churned)
  - `time_since_first_purchase` - explanatory variable (relationship length)
  - `data=churn` - specifies dataset
- `.fit()` - estimates model parameters using maximum likelihood
- `mdl_churn_vs_relationship.params` - displays intercept and slope coefficients

## Understanding Logistic Regression Output

### Parameter Interpretation

Unlike linear regression, logistic regression coefficients represent:

- **Log-odds ratios** rather than direct changes in response variable
- **Multiplicative effects** on odds rather than additive effects on means
- **Need transformation** to interpret as probabilities

### Common Interpretation Steps

**From Coefficients to Probabilities:**

```python
import numpy as np

# Example: Predict churn probability for customer with 24 months relationship
time_value = 24
intercept = mdl_churn_vs_relationship.params['Intercept']
slope = mdl_churn_vs_relationship.params['time_since_first_purchase']

# Calculate log-odds
log_odds = intercept + slope * time_value

# Convert to probability using sigmoid function
probability = 1 / (1 + np.exp(-log_odds))
print(f"Churn probability for {time_value} months: {probability:.3f}")
```

**Coefficient Interpretation:**

- **Positive coefficient**: Increases churn probability as variable increases
- **Negative coefficient**: Decreases churn probability as variable increases
- **Magnitude**: Larger absolute values indicate stronger effects

## Model Comparison Framework

### Linear vs. Logistic Regression for Binary Outcomes

| Aspect              | Linear Regression  | Logistic Regression             |
| ------------------- | ------------------ | ------------------------------- |
| **Predictions**     | Can be < 0 or > 1  | Always between 0 and 1          |
| **Trend Line**      | Straight line      | S-shaped curve                  |
| **Interpretation**  | Direct probability | Log-odds (needs transformation) |
| **Assumptions**     | Normal errors      | Binomial distribution           |
| **Appropriateness** | ❌ Poor for binary | ✅ Designed for binary          |

### When to Use Each Method

- **Linear Regression**: Continuous response variables
- **Logistic Regression**: Binary response variables (yes/no, success/failure, churn/retain)

## Practical Business Applications

### Customer Churn Analysis

**Business Questions:**

- Which customers are most likely to churn?
- What relationship length indicates higher churn risk?
- How can we prioritize retention efforts?

**Model Applications:**

- **Risk Scoring**: Assign churn probability to each customer
- **Segmentation**: Group customers by churn risk levels
- **Intervention Timing**: Identify when customers become high-risk
- **Resource Allocation**: Focus retention efforts on high-probability churners

### Key Insights from Churn Model

Based on the relationship between time since first purchase and churn:

- **New customers**: May have different churn patterns
- **Long-term customers**: Relationship length might reduce or increase churn risk
- **Sweet spot identification**: Optimal relationship lengths for retention
- **Intervention strategies**: Targeted approaches for different customer tenure groups

## Diagnostic Considerations

### Model Evaluation for Logistic Regression

- **Confusion Matrix**: Classification accuracy assessment
- **ROC Curves**: Trade-off between sensitivity and specificity
- **AUC**: Overall discrimination ability
- **Hosmer-Lemeshow Test**: Goodness of fit for logistic models

### Next Steps in Analysis

1. **Evaluate model performance** using classification metrics
2. **Add more predictors** for comprehensive churn modeling
3. **Validate on holdout data** to assess generalization
4. **Implement business rules** based on model predictions
5. **Monitor model performance** over time as business conditions change

This logistic regression approach provides a statistically sound and business-relevant method for analyzing and predicting customer churn.
