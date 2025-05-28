# Analyzing Categorical Data in Taiwan Real Estate

## Part 1: Visualizing Numeric vs. Categorical Data

### Problem Description

When the explanatory variable is categorical, scatter plots don't make sense. Instead, histograms for each category provide better visualization.

The Taiwan real estate dataset has a categorical variable representing house age, split into 3 groups:

- 0 to 15 years
- 15 to 30 years
- 30 to 45 years

### Task

Plot a histogram of `price_twd_msq` with 10 bins, split by `house_age_years` to create 3 panels.

### Solution

```python
# Histograms of price_twd_msq with 10 bins, split by the age of each house
sns.displot(data=taiwan_real_estate,
           x="price_twd_msq",
           bins=10,
           col="house_age_years")

# Show the plot
plt.show()
```

### Code Explanation

**Key components:**

- `data=taiwan_real_estate` - specifies the dataset
- `x="price_twd_msq"` - sets the variable for the histogram (house prices)
- `bins=10` - creates exactly 10 bins for each histogram
- `col="house_age_years"` - creates separate panels (columns) for each house age category
- `plt.show()` - displays the plot

### Why This Works

This visualization is particularly effective because it allows you to compare the price distributions across the three age groups side by side. You can easily observe:

- **Distribution shape** - How prices are distributed within each age category
- **Price ranges** - The span of prices for newer vs. older houses
- **Central tendencies** - Whether certain age groups tend toward higher or lower prices
- **Variability** - The spread of prices within each age group

The `col` parameter in `sns.displot()` is ideal for categorical comparisons, creating separate subplots while maintaining consistent scales for easy comparison across categories.

## Part 2: Calculating Means by Category

### Problem Description

A good way to explore categorical variables further is to calculate summary statistics for each category. You can calculate the mean and median of your response variable, grouped by a categorical variable to compare each category in more detail.

This approach helps you understand the output of a linear regression with a categorical variable by examining grouped means for house prices.

### Task

Group `taiwan_real_estate` by `house_age_years` and calculate the mean price (`price_twd_msq`) for each age group.

### Solution

```python
# Calculate the mean of price_twd_msq, grouped by house age
mean_price_by_age = taiwan_real_estate.groupby("house_age_years")["price_twd_msq"].mean()

# Print the result
print(mean_price_by_age)
```

### Code Explanation

**Key components:**

- `taiwan_real_estate.groupby("house_age_years")` - groups the data by house age categories
- `["price_twd_msq"]` - selects the price column for aggregation
- `.mean()` - calculates the mean for each group
- Result is stored in `mean_price_by_age` for further analysis

### Why This is Useful

Calculating grouped means provides several insights:

- **Baseline comparison** - Shows the average price for each age category
- **Linear regression preparation** - These means become the predicted values in categorical regression
- **Pattern identification** - Reveals whether newer or older houses command higher prices
- **Statistical foundation** - Forms the basis for understanding how categorical variables work in regression models

The grouped means will show you exactly what a linear regression model with categorical variables would predict for each category.

# Linear Regression with Categorical Explanatory Variables

## Background

The grouped means calculated in the previous exercise become the coefficients of a linear regression model with categorical variables. This exercise demonstrates how categorical variables work in linear regression using the Taiwan real estate dataset.

## Key Concept

When using categorical explanatory variables in linear regression:

- The model coefficients directly relate to the category means
- The same `ols()` function works for both numeric and categorical variables
- The interpretation of coefficients differs from numeric variables

## Instructions - Part 1

Run and fit a linear regression with:

- `price_twd_msq` as the response variable
- `house_age_years` as the explanatory variable
- `taiwan_real_estate` as the dataset

### Solution

```python
# Create the model, fit it
mdl_price_vs_age = ols("price_twd_msq~house_age_years", data=taiwan_real_estate).fit()

# Print the parameters of the fitted model
print(mdl_price_vs_age.params)
```

## Code Explanation

**Key components:**

- `ols("price_twd_msq~house_age_years", data=taiwan_real_estate)` - defines the linear regression model
  - `price_twd_msq` - response (dependent) variable
  - `house_age_years` - explanatory (independent) categorical variable
  - `data=taiwan_real_estate` - specifies the dataset
- `.fit()` - fits the model to the data
- `mdl_price_vs_age.params` - displays the model coefficients

## Understanding Categorical Regression Coefficients

Unlike numeric variables, categorical regression coefficients represent:

- **Intercept** - The mean of the reference category (usually the first alphabetically)
- **Other coefficients** - The difference between each category's mean and the reference category mean

This approach allows the model to predict the group mean for each category, proving the connection between grouped means and regression coefficients with categorical variables.

## What to Expect

The output will show coefficients that, when combined properly, give you the mean price for each house age category - exactly matching the grouped means calculated previously.
