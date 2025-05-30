# Revision Notes: Sampling and Statistical Calculations with NumPy and Pandas

## Learning Objective

Master the fundamentals of data sampling and statistical parameter calculation using NumPy and Pandas - essential skills for exploratory data analysis and statistical inference.

## Key Concepts

### 1. **Data Extraction from DataFrames**

When working with datasets, you often need to isolate specific columns for analysis. Pandas provides direct column access using bracket notation.

```python
# Extract a single column as a pandas Series
column_series = dataframe["column_name"]
# This creates a Series object, not a DataFrame
```

### 2. **Random Sampling Techniques**

Sampling is crucial for statistical analysis, especially when working with large datasets where analyzing the entire population is impractical.

```python
# Simple random sampling from a pandas Series
sample_data = series.sample(n=sample_size)
# n parameter specifies the number of observations to sample
# By default, sampling is without replacement
# Use replace=True for sampling with replacement
```

### 3. **Complete Working Example**

```python
import numpy as np
import pandas as pd

# Step 1: Extract column of interest as a pandas Series
# This isolates the specific variable we want to analyze
loudness_pop = spotify_population["loudness"]

# Step 2: Generate random sample for analysis
# Sample size of 100 is often sufficient for basic statistical inference
loudness_samp = loudness_pop.sample(n=100)

# Step 3: Display the sample for inspection
print(loudness_samp)
```

## Technical Details

### **Series vs DataFrame Distinction**

- `spotify_population["loudness"]` returns a **pandas Series** (1-dimensional)
- This is different from `spotify_population[["loudness"]]` which returns a **DataFrame** (2-dimensional)
- Series objects have different methods and properties than DataFrames

### **Sampling Parameters**

- `n`: Number of samples to draw
- `replace`: Boolean for sampling with/without replacement (default: False)
- `random_state`: Set for reproducible results
- `frac`: Alternative to `n`, specifies fraction of data to sample

### **Why This Approach Matters**

1. **Memory Efficiency**: Working with Series instead of full DataFrames when analyzing single variables
2. **Statistical Validity**: Random sampling helps ensure representative samples from populations
3. **Scalability**: Sampling techniques become essential when datasets are too large to process entirely

## Common Applications

- **Exploratory Data Analysis**: Quick insights from large datasets
- **Statistical Inference**: Estimating population parameters from samples
- **A/B Testing**: Creating control and treatment groups
- **Data Quality Assessment**: Spot-checking data integrity on subsets

## Extension Opportunities

After mastering basic sampling, you can explore:

- Stratified sampling for ensuring representation across groups
- Bootstrap sampling for confidence interval estimation
- Statistical calculations on samples (mean, median, standard deviation)
- Hypothesis testing comparing samples to populations
