# S&P 500 Portfolio Returns Analysis - Regression to the Mean

## Introduction

Regression to the mean is a crucial concept in investing. This analysis examines the annual returns from S&P 500 companies in 2018 and 2019 to understand whether exceptional performance in one year predicts similar performance the following year.

## Dataset Overview

The `sp500_yearly_returns` dataset contains:

| Variable      | Meaning                                              |
| ------------- | ---------------------------------------------------- |
| `symbol`      | Stock ticker symbol uniquely identifying the company |
| `return_2018` | Investment performance measure in 2018               |
| `return_2019` | Investment performance measure in 2019               |

**Note:** Positive returns indicate value increase; negative returns indicate value loss.

## Part 1: Plotting Consecutive Portfolio Returns

### Concept

A naive prediction might assume investment performance stays constant year-to-year, following the y = x line. However, regression to the mean suggests extreme performers tend to move toward average performance.

### Task

Create a scatter plot comparing 2019 vs 2018 returns with both a y = x reference line and a regression trend line.

### Solution

```python
# Create a new figure, fig
fig = plt.figure()

# Plot the first layer: y = x
plt.axline(xy1=(0,0), slope=1, linewidth=2, color="green")

# Add scatter plot with linear regression trend line
sns.regplot(data=sp500_yearly_returns, y="return_2019", x="return_2018")

# Set the axes so that the distances along the x and y axes look the same
plt.axis("equal")

# Show the plot
plt.show()
```

**Code Explanation:**

- `fig = plt.figure()` - creates new figure for layering plots
- `plt.axline(xy1=(0,0), slope=1, ...)` - draws y = x reference line in green
- `sns.regplot()` - creates scatter plot with regression line (no confidence interval specified)
- `plt.axis("equal")` - ensures equal scaling on both axes for proper comparison
- Layered visualization shows both naive expectation (green line) and actual relationship (blue regression line)

## Part 2: Modeling Consecutive Returns

### Task

Quantify the relationship between 2019 and 2018 returns using linear regression to test if extreme performers maintain their performance.

### Solution

```python
# Run a linear regression on return_2019 vs. return_2018 using sp500_yearly_returns
mdl_returns = ols("return_2019 ~ return_2018", data=sp500_yearly_returns).fit()

# Print the parameters
print(mdl_returns.params)
```

**Code Explanation:**

- `ols("return_2019 ~ return_2018", data=sp500_yearly_returns)` - defines regression model
  - `return_2019` - response variable (dependent)
  - `return_2018` - explanatory variable (independent)
- `.fit()` - fits the model to the data
- `mdl_returns.params` - displays intercept and slope coefficients

## Understanding Regression to the Mean

### Visual Evidence

- **Green y = x line** represents perfect correlation (same performance both years)
- **Blue regression line** shows actual relationship between consecutive years
- If regression line has slope < 1, it indicates regression to the mean

### Statistical Evidence

The model parameters reveal:

- **Intercept** - baseline return when previous year's return was 0
- **Slope** - how much current year's return changes per unit of previous year's return
- **Slope < 1** indicates regression to the mean (extreme performers moderate)
- **Slope = 1** would indicate perfect persistence of performance

### Investment Implications

- Extreme outperformers in 2018 likely had more moderate returns in 2019
- Extreme underperformers in 2018 likely improved toward average in 2019
- This suggests that chasing last year's winners may not be optimal strategy

## Key Insights

### Regression to the Mean in Finance

- Exceptional performance (positive or negative) tends to be followed by more average performance
- This doesn't mean performance is random, but extreme results are partially due to luck
- Understanding this helps in making more informed investment decisions

### Model Interpretation

- Compare the regression slope to 1.0 to quantify regression to the mean
- Values significantly less than 1.0 suggest strong mean reversion
- This analysis helps separate skill from luck in investment performance
