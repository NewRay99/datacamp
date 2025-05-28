# Choosing the Response Variable in Regression Analysis

## Introduction

In regression analysis, the choice of response variable (dependent variable) is crucial and should be driven by the research question you're trying to answer. The response variable is what you want to predict or explain using other variables in your dataset.

## Taiwan Real Estate Dataset

### Variable Overview

| Variable                | Meaning                                                            | Type        | Role         |
| ----------------------- | ------------------------------------------------------------------ | ----------- | ------------ |
| `dist_to_mrt_station_m` | Distance to nearest MRT metro station, in meters                   | Numeric     | Explanatory  |
| `n_convenience`         | Number of convenience stores in walking distance                   | Numeric     | Explanatory  |
| `house_age_years`       | Age of the house, in years, in three groups                        | Categorical | Explanatory  |
| `price_twd_msq`         | House price per unit area, in New Taiwan dollars per meter squared | Numeric     | **Response** |

## The Correct Answer: `price_twd_msq`

### Why House Price is the Best Response Variable

**Business Relevance:**

- **Primary interest**: House prices are what buyers, sellers, investors, and real estate professionals care most about
- **Predictive value**: Understanding what drives prices helps in valuation, investment decisions, and market analysis
- **Economic significance**: Price is the ultimate outcome that reflects all other property characteristics

**Statistical Appropriateness:**

- **Continuous variable**: Allows for precise predictions across a wide range of values
- **Dependent nature**: Price logically depends on location, convenience, and property characteristics
- **Outcome variable**: Price is determined by the other factors, not the other way around

**Practical Applications:**

- **Property valuation**: Estimate fair market value based on characteristics
- **Investment analysis**: Predict returns based on location and property features
- **Market research**: Understand price drivers in different areas
- **Policy analysis**: Assess impact of transportation and commercial development on housing costs

## Why Other Variables Are Not Ideal Response Variables

### `dist_to_mrt_station_m` (Distance to MRT Station)

**Why not suitable as response:**

- **Fixed geographic characteristic**: Distance is determined by infrastructure, not by house characteristics
- **Independent factor**: MRT station locations are planned independently of individual properties
- **Limited business interest**: Predicting distance doesn't provide actionable insights
- **Reverse causality issue**: Houses don't determine where MRT stations are built

**Better as explanatory variable:** Distance to transit affects desirability and price

### `n_convenience` (Number of Convenience Stores)

**Why not suitable as response:**

- **Commercial planning factor**: Store locations are business decisions, not determined by individual houses
- **Neighborhood characteristic**: Reflects commercial development patterns, not housing features
- **Limited predictive interest**: Knowing house features doesn't meaningfully predict store count
- **External factor**: Convenience store density is determined by commercial factors

**Better as explanatory variable:** Store proximity affects neighborhood desirability and property values

### `house_age_years` (House Age)

**Why not suitable as response:**

- **Time-determined**: Age is simply the passage of time since construction
- **Not influenced by other variables**: Current house characteristics don't change when it was built
- **Historical fact**: Age is a given attribute, not an outcome to predict
- **Limited practical value**: Predicting age from current features doesn't provide useful insights

**Better as explanatory variable:** Age affects condition, style, and value of properties

## Regression Framework

### Logical Causal Structure

```
Explanatory Variables → Response Variable
(Location, convenience,   (House price)
 house characteristics)
```

**Causal reasoning:**

- **Location factors** (MRT distance) → affect desirability → influence **price**
- **Convenience factors** (nearby stores) → affect quality of life → influence **price**
- **Property characteristics** (age) → affect condition and appeal → influence **price**

### Research Questions That Support This Choice

**Primary questions answered:**

1. "How much should this house cost given its location and characteristics?"
2. "What premium do buyers pay for proximity to MRT stations?"
3. "How does neighborhood convenience affect property values?"
4. "What's the depreciation rate for housing based on age?"

**Business applications:**

- **Real estate valuation**: Automated property appraisal systems
- **Investment analysis**: ROI calculations for property purchases
- **Market analysis**: Understanding price drivers in different submarkets
- **Development planning**: Assessing value impact of new infrastructure

## Alternative Analytical Frameworks

### If Using Different Response Variables

**Scenario 1: Predicting MRT Distance**

```python
# Theoretical model (not recommended)
# dist_to_mrt_station_m ~ price_twd_msq + n_convenience + house_age_years
```

- **Limited insight**: Knowing price doesn't help find MRT stations
- **Reverse logic**: Infrastructure planning doesn't depend on individual property values

**Scenario 2: Predicting Convenience Store Count**

```python
# Theoretical model (not recommended)
# n_convenience ~ price_twd_msq + dist_to_mrt_station_m + house_age_years
```

- **Commercial planning**: Store locations depend on foot traffic, zoning, competition
- **Neighborhood-level**: Individual house characteristics don't drive store placement

**Scenario 3: Predicting House Age**

```python
# Theoretical model (not recommended)
# house_age_years ~ price_twd_msq + dist_to_mrt_station_m + n_convenience
```

- **Time-fixed**: Age is historical fact, not influenced by current characteristics
- **No practical value**: Current features don't change construction date

## Model Specification

### Recommended Model Structure

```python
# Primary model: Predicting house prices
price_twd_msq ~ dist_to_mrt_station_m + n_convenience + house_age_years

# This answers: "What drives house prices in Taiwan?"
```

**Interpretation framework:**

- **Coefficient for distance**: Price decrease per meter further from MRT
- **Coefficient for convenience**: Price premium for each additional nearby store
- **Coefficient for age**: Price depreciation pattern by house age group

### Expected Relationships

| Explanatory Variable    | Expected Relationship with Price | Business Logic                                  |
| ----------------------- | -------------------------------- | ----------------------------------------------- |
| `dist_to_mrt_station_m` | **Negative**                     | Closer to transit = higher value                |
| `n_convenience`         | **Positive**                     | More stores = better convenience = higher value |
| `house_age_years`       | **Negative**                     | Older houses = more depreciation = lower value  |

## Practical Implementation

### Complete Analysis Framework

```python
# 1. Data exploration
print(taiwan_real_estate.describe())
print(taiwan_real_estate.info())

# 2. Correlation analysis
correlation_matrix = taiwan_real_estate.corr()
print(correlation_matrix['price_twd_msq'].sort_values(ascending=False))

# 3. Visualization
import seaborn as sns
import matplotlib.pyplot as plt

# Price vs each explanatory variable
fig, axes = plt.subplots(1, 3, figsize=(15, 5))

sns.scatterplot(data=taiwan_real_estate, x='dist_to_mrt_station_m', y='price_twd_msq', ax=axes[0])
sns.scatterplot(data=taiwan_real_estate, x='n_convenience', y='price_twd_msq', ax=axes[1])
sns.boxplot(data=taiwan_real_estate, x='house_age_years', y='price_twd_msq', ax=axes[2])

plt.tight_layout()
plt.show()

# 4. Regression analysis
from statsmodels.formula.api import ols

model = ols('price_twd_msq ~ dist_to_mrt_station_m + n_convenience + house_age_years',
            data=taiwan_real_estate).fit()
print(model.summary())
```

## Key Takeaways

### Decision Criteria for Response Variables

1. **Business relevance**: What do stakeholders want to predict or understand?
2. **Causal logic**: What is the outcome that other factors influence?
3. **Practical value**: What predictions would be useful for decision-making?
4. **Statistical appropriateness**: What variable type allows for meaningful modeling?

### Best Practices

- **Start with business question**: What problem are you trying to solve?
- **Consider causality**: What influences what in the real world?
- **Think about applications**: How will the model be used?
- **Validate choice**: Does the response variable make logical and practical sense?

### Final Answer

**`price_twd_msq`** is clearly the best choice for the response variable because:

- It's what people most want to predict (business relevance)
- It's logically influenced by location and property characteristics (causal structure)
- It enables valuable applications like property valuation and market analysis (practical utility)
- It's a continuous variable suitable for regression modeling (statistical appropriateness)

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
