# Predicting House Prices with Linear Regression

## Introduction

Statistical models like linear regression are powerful tools for making predictions. You can specify values for explanatory variables, feed them to the model, and get predictions for the response variable.

## General Code Flow for Predictions

```python
explanatory_data = pd.DataFrame({"explanatory_var": list_of_values})
predictions = model.predict(explanatory_data)
prediction_data = explanatory_data.assign(response_var=predictions)
```

## Part 1: Creating Explanatory Data

### Task

Create explanatory data for predicting house prices based on the number of convenience stores (0 to 10).

### Solution

```python
# Import numpy with alias np
import numpy as np

# Create the explanatory_data
explanatory_data = pd.DataFrame({'n_convenience': np.arange(0,11)})

# Print it
print(explanatory_data)
```

**Code Explanation:**

- `import numpy as np` - imports numpy for array operations
- `np.arange(0,11)` - creates integers from 0 to 10 (11 is excluded)
- `pd.DataFrame({'n_convenience': ...})` - creates DataFrame with convenience store counts

## Part 2: Visualizing Predictions

### Task

Plot the original data with regression line and overlay prediction points in red.

### Solution

```python
# Create a new figure, fig
fig = plt.figure()

sns.regplot(x="n_convenience",
            y="price_twd_msq",
            data=taiwan_real_estate,
            ci=None)

# Add a scatter plot layer to the regplot
sns.scatterplot(x="n_convenience",
                y="price_twd_msq",
                data=prediction_data,
                color="red")

# Show the layered plot
plt.show()
```

**Code Explanation:**

- `fig = plt.figure()` - creates new figure for layered plotting
- `sns.regplot()` - plots original data with regression line
- `ci=None` - removes confidence interval bands
- `sns.scatterplot()` - adds prediction points as red scatter plot
- `color="red"` - makes prediction points stand out

## Part 3: Testing Model Limits

### Task

Test the model's behavior with impossible values: -1 and 2.5 convenience stores.

### Solution

```python
# Define a DataFrame impossible
impossible = pd.DataFrame({"n_convenience": [-1, 2.5]})
print(impossible)
```

**Code Explanation:**

- Creates DataFrame with impossible values
- `-1` convenience stores (negative count is impossible)
- `2.5` convenience stores (fractional stores don't exist)

## Key Concepts

### Prediction Process

1. **Create explanatory data** - DataFrame with input values
2. **Generate predictions** - Use `model.predict()`
3. **Combine results** - Use `assign()` to add predictions
4. **Visualize** - Layer predictions on original data plots

### Model Limitations

- Models will make predictions even for impossible inputs
- Linear regression extends infinitely in both directions
- Real-world constraints aren't built into the mathematical model
- Always consider whether predictions make practical sense

### Visualization Benefits

- Red prediction points show model's predicted values
- Overlaying on original data reveals model fit quality
- Helps identify where model predictions are most reliable
