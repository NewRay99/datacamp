# Measuring Logistic Model Performance - Manual Metric Calculations

## Introduction

Understanding how to manually calculate performance metrics from a confusion matrix is crucial for interpreting logistic regression results. This exercise demonstrates the step-by-step calculation of accuracy, sensitivity, and specificity using the confusion matrix components.

## Confusion Matrix Components

### Understanding the Layout

A confusion matrix for binary classification has the following structure:

```
                    PREDICTED
                 0 (No)  1 (Yes)
ACTUAL    0 (No)   TN      FP
          1 (Yes)  FN      TP
```

Where:

- **TN (True Negative)**: Correctly predicted "No" cases
- **TP (True Positive)**: Correctly predicted "Yes" cases
- **FN (False Negative)**: Incorrectly predicted "No" when actual was "Yes"
- **FP (False Positive)**: Incorrectly predicted "Yes" when actual was "No"

### Extracting Values from Confusion Matrix

In our customer churn context:

- **TN**: Customers who didn't churn and were correctly predicted as staying
- **TP**: Customers who churned and were correctly predicted as churning
- **FN**: Customers who churned but were predicted as staying (missed churners)
- **FP**: Customers who stayed but were predicted as churning (false alarms)

## Manual Metric Calculations

### Task

Calculate accuracy, sensitivity, and specificity from the confusion matrix for the customer churn model.

### Solution

```python
# Extract TN, TP, FN and FP from conf_matrix
TN = conf_matrix[0,0]
TP = conf_matrix[1,1]
FN = conf_matrix[1,0]
FP = conf_matrix[0,1]

# Calculate and print the accuracy
accuracy = (TN + TP) / (TN + FN + FP + TP)
print("accuracy: ", accuracy)

# Calculate and print the sensitivity
sensitivity = TP / (TP + FN)
print("sensitivity: ", sensitivity)

# Calculate and print the specificity
specificity = TN / (TN + FP)
print("specificity: ", specificity)
```

## Understanding Each Metric

### Accuracy

**Formula**: `Accuracy = (TN + TP) / (TN + FN + FP + TP)`

**Interpretation**: The proportion of all predictions that are correct.

**Code Breakdown**:

- **Numerator**: `TN + TP` (all correct predictions)
- **Denominator**: `TN + FN + FP + TP` (total number of predictions)
- **Meaning**: Overall model correctness across all customers

**Business Context**:

- High accuracy means the model is generally reliable
- Useful for overall performance assessment
- Can be misleading with imbalanced datasets

### Sensitivity (Recall/True Positive Rate)

**Formula**: `Sensitivity = TP / (TP + FN)`

**Interpretation**: The proportion of actual churners that were correctly identified.

**Code Breakdown**:

- **Numerator**: `TP` (correctly identified churners)
- **Denominator**: `TP + FN` (all actual churners)
- **Meaning**: How good the model is at catching customers who will actually churn

**Business Context**:

- High sensitivity means fewer churners are missed
- Critical for retention strategy effectiveness
- Low sensitivity means lost revenue from unidentified churners

### Specificity (True Negative Rate)

**Formula**: `Specificity = TN / (TN + FP)`

**Interpretation**: The proportion of actual non-churners that were correctly identified.

**Code Breakdown**:

- **Numerator**: `TN` (correctly identified loyal customers)
- **Denominator**: `TN + FP` (all actual non-churners)
- **Meaning**: How good the model is at identifying customers who will stay

**Business Context**:

- High specificity means fewer false alarms
- Reduces wasted retention efforts on loyal customers
- Low specificity means unnecessary retention costs

## Comprehensive Performance Analysis

### Complete Calculation Function

```python
def calculate_detailed_metrics(conf_matrix):
    """Calculate comprehensive performance metrics from confusion matrix"""

    # Extract confusion matrix components
    TN = conf_matrix[0,0]
    TP = conf_matrix[1,1]
    FN = conf_matrix[1,0]
    FP = conf_matrix[0,1]

    # Basic metrics
    accuracy = (TN + TP) / (TN + FN + FP + TP)
    sensitivity = TP / (TP + FN) if (TP + FN) > 0 else 0
    specificity = TN / (TN + FP) if (TN + FP) > 0 else 0

    # Additional useful metrics
    precision = TP / (TP + FP) if (TP + FP) > 0 else 0
    negative_predictive_value = TN / (TN + FN) if (TN + FN) > 0 else 0
    false_positive_rate = FP / (FP + TN) if (FP + TN) > 0 else 0
    false_negative_rate = FN / (FN + TP) if (FN + TP) > 0 else 0

    # F1 Score
    f1_score = 2 * (precision * sensitivity) / (precision + sensitivity) if (precision + sensitivity) > 0 else 0

    return {
        'confusion_matrix_components': {
            'True Negatives (TN)': TN,
            'True Positives (TP)': TP,
            'False Negatives (FN)': FN,
            'False Positives (FP)': FP
        },
        'primary_metrics': {
            'Accuracy': accuracy,
            'Sensitivity (Recall)': sensitivity,
            'Specificity': specificity,
            'Precision': precision
        },
        'additional_metrics': {
            'Negative Predictive Value': negative_predictive_value,
            'False Positive Rate': false_positive_rate,
            'False Negative Rate': false_negative_rate,
            'F1 Score': f1_score
        }
    }

# Apply to churn model
detailed_metrics = calculate_detailed_metrics(conf_matrix)

# Display results
print("=== CONFUSION MATRIX COMPONENTS ===")
for component, value in detailed_metrics['confusion_matrix_components'].items():
    print(f"{component}: {value}")

print("\n=== PRIMARY METRICS ===")
for metric, value in detailed_metrics['primary_metrics'].items():
    print(f"{metric}: {value:.4f} ({value:.1%})")

print("\n=== ADDITIONAL METRICS ===")
for metric, value in detailed_metrics['additional_metrics'].items():
    print(f"{metric}: {value:.4f} ({value:.1%})")
```

## Interpreting Results in Business Context

### Performance Benchmarks

| Metric          | Excellent | Good      | Fair      | Poor   |
| --------------- | --------- | --------- | --------- | ------ |
| **Accuracy**    | > 0.90    | 0.80-0.90 | 0.70-0.80 | < 0.70 |
| **Sensitivity** | > 0.90    | 0.80-0.90 | 0.70-0.80 | < 0.70 |
| **Specificity** | > 0.90    | 0.80-0.90 | 0.70-0.80 | < 0.70 |

**Note**: Benchmarks depend heavily on business context and dataset characteristics.

### Business Trade-offs

#### High Sensitivity Strategy

```python
# Optimize for catching churners (lower threshold)
threshold = 0.3  # Lower threshold catches more churners

# Business implications:
# + Fewer lost customers (reduced FN)
# - More retention efforts on loyal customers (increased FP)
# - Higher retention costs
```

#### High Specificity Strategy

```python
# Optimize for precision (higher threshold)
threshold = 0.7  # Higher threshold reduces false alarms

# Business implications:
# + Lower retention costs (reduced FP)
# + More focused retention efforts
# - More churners missed (increased FN)
```

#### Balanced Strategy

```python
# Optimize for overall accuracy
threshold = 0.5  # Standard threshold

# Business implications:
# + Balanced approach to both types of errors
# + Good overall performance
# - May not optimize for specific business priorities
```

## Validation and Sensitivity Analysis

### Cross-Validation Performance

```python
from sklearn.model_selection import cross_val_score
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import make_scorer, accuracy_score, recall_score, precision_score

# Convert to sklearn format for cross-validation
X = churn[['time_since_first_purchase']]
y = churn['has_churned']

# Create logistic regression model
sklearn_model = LogisticRegression()

# Define custom scoring functions
def sensitivity_score(y_true, y_pred):
    return recall_score(y_true, y_pred)

def specificity_score(y_true, y_pred):
    tn, fp, fn, tp = confusion_matrix(y_true, y_pred).ravel()
    return tn / (tn + fp)

# Perform cross-validation
cv_accuracy = cross_val_score(sklearn_model, X, y, cv=5, scoring='accuracy')
cv_sensitivity = cross_val_score(sklearn_model, X, y, cv=5, scoring=make_scorer(sensitivity_score))
cv_specificity = cross_val_score(sklearn_model, X, y, cv=5, scoring=make_scorer(specificity_score))

print("=== CROSS-VALIDATION RESULTS ===")
print(f"Accuracy: {cv_accuracy.mean():.3f} ± {cv_accuracy.std():.3f}")
print(f"Sensitivity: {cv_sensitivity.mean():.3f} ± {cv_sensitivity.std():.3f}")
print(f"Specificity: {cv_specificity.mean():.3f} ± {cv_specificity.std():.3f}")
```

### Threshold Sensitivity Analysis

```python
import matplotlib.pyplot as plt

# Test different thresholds
thresholds = np.linspace(0.1, 0.9, 17)
metrics_by_threshold = []

for threshold in thresholds:
    # Make predictions with custom threshold
    predictions = (mdl_churn_vs_relationship.predict() >= threshold).astype(int)

    # Create confusion matrix
    actual = churn["has_churned"]
    tn = ((actual == 0) & (predictions == 0)).sum()
    tp = ((actual == 1) & (predictions == 1)).sum()
    fn = ((actual == 1) & (predictions == 0)).sum()
    fp = ((actual == 0) & (predictions == 1)).sum()

    # Calculate metrics
    accuracy = (tn + tp) / (tn + tp + fn + fp)
    sensitivity = tp / (tp + fn) if (tp + fn) > 0 else 0
    specificity = tn / (tn + fp) if (tn + fp) > 0 else 0

    metrics_by_threshold.append({
        'threshold': threshold,
        'accuracy': accuracy,
        'sensitivity': sensitivity,
        'specificity': specificity
    })

# Convert to DataFrame
threshold_df = pd.DataFrame(metrics_by_threshold)

# Plot threshold sensitivity
plt.figure(figsize=(10, 6))
plt.plot(threshold_df['threshold'], threshold_df['accuracy'], 'o-', label='Accuracy')
plt.plot(threshold_df['threshold'], threshold_df['sensitivity'], 's-', label='Sensitivity')
plt.plot(threshold_df['threshold'], threshold_df['specificity'], '^-', label='Specificity')
plt.xlabel('Prediction Threshold')
plt.ylabel('Metric Value')
plt.title('Performance Metrics vs. Prediction Threshold')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()

# Find optimal threshold for different objectives
best_accuracy_threshold = threshold_df.loc[threshold_df['accuracy'].idxmax(), 'threshold']
best_sensitivity_threshold = threshold_df.loc[threshold_df['sensitivity'].idxmax(), 'threshold']
best_specificity_threshold = threshold_df.loc[threshold_df['specificity'].idxmax(), 'threshold']

print(f"Best accuracy threshold: {best_accuracy_threshold:.2f}")
print(f"Best sensitivity threshold: {best_sensitivity_threshold:.2f}")
print(f"Best specificity threshold: {best_specificity_threshold:.2f}")
```

## Key Takeaways

### Essential Points

1. **Manual calculation** reinforces understanding of what each metric represents
2. **Confusion matrix extraction** follows standard indexing: `[row, column]`
3. **Three core metrics** provide different perspectives on model performance
4. **Business context** determines which metrics are most important
5. **Threshold choice** significantly impacts all performance measures

### Best Practices

- **Always examine confusion matrix** before calculating summary metrics
- **Consider business costs** of false positives vs. false negatives
- **Test multiple thresholds** to find optimal operating point
- **Use cross-validation** to assess model stability
- **Monitor performance over time** as data patterns evolve

### Formula Summary

- **Accuracy**: `(TN + TP) / Total` - Overall correctness
- **Sensitivity**: `TP / (TP + FN)` - Ability to detect positives
- **Specificity**: `TN / (TN + FP)` - Ability to detect negatives

Understanding these calculations enables you to make informed decisions about model performance and business strategy implementation.
