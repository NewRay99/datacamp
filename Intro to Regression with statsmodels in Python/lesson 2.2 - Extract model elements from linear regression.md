# Extracting Model Elements from Linear Regression

## Introduction

The model object created by `ols()` contains many elements that are crucial for further analysis. Understanding how to extract and interpret these components is essential for comprehensive model evaluation.

## Key Model Elements

The most important pieces of a linear model object include:

- **Model coefficients** - The slope and intercept parameters
- **Fitted values** - The predicted values for each observation
- **Residuals** - The differences between actual and predicted values
- **Model summary** - Comprehensive statistical information

## Instructions & Solutions

### Part 1: Model Parameters

Print the coefficients (parameters) of the model.

```python
# Print the model parameters of mdl_price_vs_conv
print(mdl_price_vs_conv.params)
```

### Part 2: Fitted Values

Print the predicted values for each observation in the dataset.

```python
# Print the fitted values of mdl_price_vs_conv
print(mdl_price_vs_conv.fittedvalues)
```

### Part 3: Residuals

Print the residuals (actual - predicted values).

```python
# Print the residuals of mdl_price_vs_conv
print(mdl_price_vs_conv.resid)
```

### Part 4: Model Summary

Print comprehensive model statistics and diagnostics.

```python
# Print a summary of mdl_price_vs_conv
print(mdl_price_vs_conv.summary())
```

## Understanding Each Element

### Model Parameters (`params`)

- **Intercept** - The predicted response when all explanatory variables are zero
- **Slope coefficients** - The change in response for a one-unit change in each explanatory variable
- Used for making predictions and understanding variable relationships

### Fitted Values (`fittedvalues`)

- The model's predictions for each observation in the training data
- Calculated using: intercept + (slope × explanatory_variable_value)
- Essential for evaluating model fit and creating residual plots

### Residuals (`resid`)

- The difference between actual and predicted values: actual - fitted
- Positive residuals indicate the model under-predicted
- Negative residuals indicate the model over-predicted
- Used for model diagnostics and assumption checking

### Model Summary (`summary()`)

Provides comprehensive information including:

- **Coefficients table** - Parameters, standard errors, t-statistics, p-values
- **Model fit statistics** - R-squared, adjusted R-squared, F-statistic
- **Diagnostic information** - Degrees of freedom, residual statistics
- **Assumption tests** - Various statistical tests for model validity

## Why These Elements Matter

### For Model Evaluation

- **Parameters** tell you the strength and direction of relationships
- **Fitted values** help assess how well the model fits the data
- **Residuals** reveal patterns that might indicate model problems

### For Further Analysis

- Residuals are used in diagnostic plots to check assumptions
- Fitted values can be compared to actual values to assess accuracy
- Parameters are used to make predictions on new data

### For Model Comparison

- Summary statistics help compare different models
- R-squared values indicate how much variance is explained
- Residual patterns can reveal which model fits better

# Manually Predicting House Prices

## Introduction

While using `.predict()` is the standard approach for making predictions in real life, manually calculating predictions helps demystify the process. Linear regression predictions are simply arithmetic - not magic!

## The Linear Regression Formula

For simple linear regression, the predicted value is:

```
response = intercept + slope × explanatory_variable
```

Where:

- **Intercept** - The predicted response when explanatory variable = 0
- **Slope** - The change in response for each unit change in explanatory variable
- **Explanatory variable** - The input value we're making a prediction for

## Task

Manually calculate house price predictions and compare them to the automatic `.predict()` method results.

## Solution

```python
# Get the coefficients of mdl_price_vs_conv
coeffs = mdl_price_vs_conv.params

# Get the intercept
intercept = coeffs["Intercept"]

# Get the slope
slope = coeffs["n_convenience"]

# Manually calculate the predictions
price_twd_msq = intercept + slope*explanatory_data
print(price_twd_msq)

# Compare to the results from .predict()
print(price_twd_msq.assign(predictions_auto=mdl_price_vs_conv.predict(explanatory_data)))
```

## Code Explanation

### Step 1: Extract Coefficients

```python
coeffs = mdl_price_vs_conv.params
```

- Gets all model parameters as a pandas Series
- Contains both intercept and slope values with their names as indices

### Step 2: Get Intercept

```python
intercept = coeffs["Intercept"]
```

- Extracts the intercept value by name
- This is the predicted price when there are 0 convenience stores

### Step 3: Get Slope

```python
slope = coeffs["n_convenience"]
```

- Extracts the slope coefficient for the convenience store variable
- This tells us how much price changes per additional convenience store

### Step 4: Manual Calculation

```python
price_twd_msq = intercept + slope*explanatory_data
```

- Applies the linear regression formula
- `explanatory_data` contains the convenience store counts (0 to 10)
- Pandas automatically broadcasts the calculation across all rows

### Step 5: Comparison

```python
print(price_twd_msq.assign(predictions_auto=mdl_price_vs_conv.predict(explanatory_data)))
```

- Uses `.assign()` to add automatic predictions alongside manual ones
- Demonstrates that both methods produce identical results

## Why This Matters

### Understanding the Process

- Removes the "black box" mystery from predictions
- Shows that linear regression is straightforward arithmetic
- Builds confidence in interpreting and using models

### Practical Benefits

- Helps debug prediction issues
- Makes it easier to explain models to others
- Provides insight into how coefficients directly impact predictions

### Mathematical Foundation

- Reinforces the linear relationship concept
- Shows how model parameters translate to real predictions
- Demonstrates the predictable nature of linear models

## Expected Results

The manual calculations should produce identical results to `.predict()`, proving that:

- Linear regression predictions follow a simple mathematical formula
- The model coefficients directly determine the prediction equation
- There's no hidden complexity in the prediction process

This exercise builds fundamental understanding of how linear regression works under the hood!


