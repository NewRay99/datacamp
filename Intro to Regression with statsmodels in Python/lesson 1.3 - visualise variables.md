# Visualizing Two Numeric Variables - Taiwan Real Estate Analysis

## Introduction

Before running statistical models, visualizing your data is essential for understanding relationships between variables. This exercise explores the relationship between house prices and nearby convenience stores in the Taiwan real estate dataset.

## The Challenge: Overlapping Data Points

### Integer Data Problem

When one variable contains integer data (like count of convenience stores), multiple data points can occupy the exact same position on the plot, creating overlapping points that hide the true density of observations.

**Common issues:**

- **Hidden observations**: Multiple points stack on top of each other
- **Misleading patterns**: Can't see how many observations are at each location
- **Density obscured**: Difficult to identify areas with many vs. few observations

### Solutions for Integer Data Visualization

1. **Transparency (alpha)**: Make points semi-transparent to show overlaps
2. **Jittering**: Add small random noise to separate overlapping points
3. **Size/color mapping**: Use point size or color to show density
4. **Alternative plot types**: Boxplots, violin plots, or hexbin plots

## Basic Scatter Plot Solution

### Task

Visualize the relationship between house prices and number of convenience stores.

### Solution

```python
# Import seaborn with alias sns
import seaborn as sns

# Import matplotlib.pyplot with alias plt
import matplotlib.pyplot as plt

# Draw the scatter plot
sns.scatterplot(x="n_convenience", y="price_twd_msq", data=taiwan_real_estate)

# Show the plot
plt.show()
```

**Code Explanation:**

- `import seaborn as sns` - imports plotting library with standard alias
- `import matplotlib.pyplot as plt` - imports base plotting functionality
- `sns.scatterplot()` - creates scatter plot of two numeric variables
- `x="n_convenience"` - convenience store count on x-axis
- `y="price_twd_msq"` - house price per area on y-axis
- `data=taiwan_real_estate` - specifies the dataset
- `plt.show()` - displays the plot

## Enhanced Visualization with Transparency

### Addressing Overlapping Points

```python
# Improved scatter plot with transparency
sns.scatterplot(x="n_convenience", y="price_twd_msq",
                data=taiwan_real_estate, alpha=0.6)
plt.xlabel("Number of Convenience Stores")
plt.ylabel("House Price (TWD per sq meter)")
plt.title("House Price vs. Number of Convenience Stores")
plt.show()
```

**Key improvements:**

- `alpha=0.6` - makes points 60% opaque (40% transparent)
- `plt.xlabel()` - adds descriptive x-axis label
- `plt.ylabel()` - adds descriptive y-axis label
- `plt.title()` - adds informative plot title

### Why Transparency Helps

- **Overlaps visible**: Darker areas show where multiple points overlap
- **Density patterns**: Can see concentration of observations
- **True relationships**: Better understanding of data distribution

## Alternative Visualization Approaches

### 1. Jittered Scatter Plot

```python
# Add small random noise to x-values to separate overlapping points
import numpy as np

# Create jittered version of convenience store count
taiwan_real_estate_jittered = taiwan_real_estate.copy()
taiwan_real_estate_jittered['n_convenience_jittered'] = (
    taiwan_real_estate['n_convenience'] +
    np.random.normal(0, 0.1, len(taiwan_real_estate))
)

# Plot with jittered x-values
sns.scatterplot(x="n_convenience_jittered", y="price_twd_msq",
                data=taiwan_real_estate_jittered, alpha=0.6)
plt.xlabel("Number of Convenience Stores (jittered)")
plt.ylabel("House Price (TWD per sq meter)")
plt.title("House Price vs. Convenience Stores (Jittered)")
plt.show()
```

### 2. Box Plot by Convenience Store Count

```python
# Group prices by convenience store count
plt.figure(figsize=(12, 6))
sns.boxplot(x="n_convenience", y="price_twd_msq", data=taiwan_real_estate)
plt.xlabel("Number of Convenience Stores")
plt.ylabel("House Price (TWD per sq meter)")
plt.title("Price Distribution by Number of Convenience Stores")
plt.xticks(rotation=45)
plt.show()
```

### 3. Hexbin Plot for Density

```python
# Hexagonal binning to show density
plt.figure(figsize=(10, 6))
plt.hexbin(taiwan_real_estate['n_convenience'],
           taiwan_real_estate['price_twd_msq'],
           gridsize=20, cmap='Blues')
plt.colorbar(label='Number of Observations')
plt.xlabel("Number of Convenience Stores")
plt.ylabel("House Price (TWD per sq meter)")
plt.title("Density Plot: House Price vs. Convenience Stores")
plt.show()
```

### 4. Regression Plot with Confidence Interval

```python
# Combine scatter plot with regression line
plt.figure(figsize=(10, 6))
sns.regplot(x="n_convenience", y="price_twd_msq",
            data=taiwan_real_estate,
            scatter_kws={'alpha': 0.6})
plt.xlabel("Number of Convenience Stores")
plt.ylabel("House Price (TWD per sq meter)")
plt.title("House Price vs. Convenience Stores with Trend Line")
plt.show()
```

## Comprehensive Visualization Analysis

### Multi-Panel Exploration

```python
# Create comprehensive visualization
fig, axes = plt.subplots(2, 2, figsize=(15, 12))

# 1. Basic scatter plot with transparency
sns.scatterplot(x="n_convenience", y="price_twd_msq",
                data=taiwan_real_estate, alpha=0.6, ax=axes[0,0])
axes[0,0].set_title("Scatter Plot with Transparency")

# 2. Box plot
sns.boxplot(x="n_convenience", y="price_twd_msq",
            data=taiwan_real_estate, ax=axes[0,1])
axes[0,1].set_title("Box Plot by Convenience Store Count")
axes[0,1].tick_params(axis='x', rotation=45)

# 3. Regression plot
sns.regplot(x="n_convenience", y="price_twd_msq",
            data=taiwan_real_estate,
            scatter_kws={'alpha': 0.6}, ax=axes[1,0])
axes[1,0].set_title("Regression Plot with Trend Line")

# 4. Violin plot
sns.violinplot(x="n_convenience", y="price_twd_msq",
               data=taiwan_real_estate, ax=axes[1,1])
axes[1,1].set_title("Violin Plot - Distribution Shapes")
axes[1,1].tick_params(axis='x', rotation=45)

plt.tight_layout()
plt.show()
```

## Interpreting the Visualization

### What to Look For

**Relationship Patterns:**

- **Positive correlation**: As convenience stores increase, do prices tend to increase?
- **Linear relationship**: Is the relationship approximately straight-line?
- **Outliers**: Are there unusual price-convenience combinations?
- **Variance patterns**: Does price variability change with convenience store count?

**Data Distribution:**

- **Range of values**: What's the span of convenience store counts and prices?
- **Concentration**: Where do most observations cluster?
- **Gaps**: Are there convenience store counts with no observations?
- **Density**: Which combinations are most/least common?

### Expected Insights

**Business Logic Predictions:**

- **Positive relationship**: More convenience stores should increase neighborhood desirability
- **Diminishing returns**: First few stores might matter more than additional ones
- **Price premium**: Convenient locations should command higher prices
- **Variability**: Some expensive properties might have few stores (other amenities compensate)

## Statistical Preparation

### Pre-Modeling Insights

```python
# Calculate correlation
correlation = taiwan_real_estate['n_convenience'].corr(taiwan_real_estate['price_twd_msq'])
print(f"Correlation between convenience stores and price: {correlation:.3f}")

# Summary statistics
print("\nConvenience Store Count Summary:")
print(taiwan_real_estate['n_convenience'].describe())

print("\nPrice Summary:")
print(taiwan_real_estate['price_twd_msq'].describe())

# Value counts for convenience stores
print("\nFrequency of Convenience Store Counts:")
print(taiwan_real_estate['n_convenience'].value_counts().sort_index())
```

### Identifying Visualization Challenges

```python
# Check for overlapping issues
overlap_analysis = taiwan_real_estate.groupby('n_convenience').size()
print("Number of observations per convenience store count:")
print(overlap_analysis)

# Identify heavily overlapped points
heavily_overlapped = overlap_analysis[overlap_analysis > 20]
if len(heavily_overlapped) > 0:
    print(f"\nHeavily overlapped convenience store counts (>20 observations):")
    print(heavily_overlapped)
else:
    print("\nNo heavily overlapped points found.")
```

## Best Practices for Numeric Variable Visualization

### General Guidelines

1. **Start simple**: Basic scatter plot to see overall pattern
2. **Address overlaps**: Use transparency, jittering, or alternative plots
3. **Add context**: Include trend lines, confidence intervals
4. **Label clearly**: Descriptive titles, axis labels, units
5. **Consider alternatives**: Box plots, violin plots for categorical-like numeric data

### When to Use Each Approach

| Visualization Type  | Best When                    | Advantages                    | Disadvantages           |
| ------------------- | ---------------------------- | ----------------------------- | ----------------------- |
| **Scatter Plot**    | Continuous-continuous        | Shows individual points       | Overlapping issues      |
| **Scatter + Alpha** | Many overlapping points      | Shows density patterns        | Can still be cluttered  |
| **Box Plot**        | Integer explanatory variable | Clear distribution comparison | Loses individual points |
| **Hexbin Plot**     | Very large datasets          | Excellent for density         | Less intuitive          |
| **Regression Plot** | Interested in trend          | Shows relationship clearly    | May oversimplify        |

## Key Takeaways

### Essential Points

1. **Visualization first**: Always explore data before modeling
2. **Address overlaps**: Integer data requires special consideration
3. **Multiple perspectives**: Use different plot types for complete understanding
4. **Transparency helps**: Alpha parameter reveals hidden patterns
5. **Context matters**: Clear labels and titles improve interpretation

### Preparation for Modeling

- **Relationship assessment**: Is the relationship linear enough for linear regression?
- **Outlier detection**: Are there unusual observations to investigate?
- **Data quality**: Are there any obvious data issues?
- **Transformation needs**: Would any transformations improve the relationship?

This visualization foundation ensures you understand your data before proceeding to statistical modeling, leading to better model specification and more reliable results.

# Predicting House Prices with Linear Regression

## Introduction

```python
# Import the ols function
from statsmodels.formula.api import ols

# Create the model object
mdl_price_vs_conv = ols("price_twd_msq ~ n_convenience", data=taiwan_real_estate)

# Fit the model
mdl_price_vs_conv = mdl_price_vs_conv.fit()

# Print the parameters of the fitted model
print(mdl_price_vs_conv.params)
```

This code:

Imports the ols function from statsmodels, which is used for ordinary least squares regression
Creates the model object using the formula syntax:

"price_twd_msq ~ n_convenience" means "price_twd_msq is explained by n_convenience"
The ~ symbol separates the response variable (left) from the explanatory variable(s) (right)
data=taiwan_real_estate specifies which dataset to use

Fits the model using the .fit() method, which calculates the regression coefficients
Prints the parameters which will show:

The intercept (around 8.22 based on your earlier visual estimation)
The slope coefficient for n_convenience (around 0.8 based on your earlier estimation)

The output will look something like:
Intercept 8.224
n_convenience 0.798
This confirms the visual estimates you made from the scatter plot and gives you precise numerical values for the linear relationship between convenience stores and house prices.
