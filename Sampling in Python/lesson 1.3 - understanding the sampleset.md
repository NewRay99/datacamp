# Revision Notes: Sample Representativeness and Random Number Generation

## Learning Objectives
1. Understand how to assess whether sample findings are generalizable to the population
2. Master visual comparison techniques using histograms
3. Learn proper random number generation and seed management for reproducible results

## Key Concepts

### 1. **Sample Representativeness and Generalizability**

**Core Principle**: A sample is only useful if it accurately represents the population from which it's drawn. Non-representative samples lead to biased conclusions that don't generalize.

**Common Sampling Bias**: Convenience sampling (using the easiest collection method) often produces samples that systematically differ from the population.

### 2. **Visual Assessment of Sample Quality**

Histograms are the primary tool for comparing population and sample distributions:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# Compare population vs sample distributions
# Population distribution
population_data["variable"].hist(bins=np.arange(start, end, step))
plt.title("Population Distribution")
plt.show()

# Sample distribution (plot separately or overlay)
sample_data["variable"].hist(bins=np.arange(start, end, step))
plt.title("Sample Distribution")
plt.show()
```

**Practical Examples from Exercises**:

```python
# Example 1: Acousticness analysis (0-1 scale)
# Fine-grained bins for detailed distribution view
spotify_population["acousticness"].hist(bins=np.arange(0, 1.01, 0.01))
plt.title("Population: Acousticness Distribution")
plt.show()

# Example 2: Duration analysis (minutes)
# Reasonable bin width for duration data
spotify_population['duration_minutes'].hist(bins=np.arange(0, 15.5, 0.5))
plt.title("Population: Song Duration Distribution")
plt.show()
```

### 3. **Random Number Generation**

**Purpose**: Generate synthetic data that follows specific statistical distributions for simulation, modeling, and testing.

```python
# Basic random number generation
# Uniform distribution between specified bounds
uniforms = np.random.uniform(low=min_value, high=max_value, size=n_samples)

# Normal distribution with specified mean and standard deviation
normals = np.random.normal(loc=mean, scale=std_dev, size=n_samples)

# Other common distributions
exponentials = np.random.exponential(scale=scale_param, size=n_samples)
binomials = np.random.binomial(n=trials, p=probability, size=n_samples)
```

### 4. **Random Seed Management**

**The Problem**: Random number generation produces different results each time, making analysis non-reproducible.

**The Solution**: Set a random seed before generating random numbers.

```python
# Set seed for reproducible results
np.random.seed(123)  # Any integer works as a seed

# Now random number generation is reproducible
x = np.random.normal(size=5)
# Running this again will produce identical results
```

**Important Behavior**: Once a seed is set, each subsequent random number generation call advances the random state:

```python
np.random.seed(123)
x = np.random.normal(size=5)  # First set of random numbers
y = np.random.normal(size=5)  # Different set of random numbers

# x and y will have DIFFERENT values because the seed affects
# the sequence, not individual calls
```

## Technical Details

### **Histogram Bin Selection**
- **Bin width affects interpretation**: Too few bins lose detail, too many create noise
- **Use `np.arange()`** for precise bin control: `np.arange(start, stop, step)`
- **Consider data range**: Bins should cover the full data range appropriately

### **Distribution Parameters**
- **Uniform**: `low` (minimum), `high` (maximum), `size` (number of samples)
- **Normal**: `loc` (mean), `scale` (standard deviation), `size`
- **Always specify `size`** parameter for consistent output

### **Reproducibility Best Practices**
1. **Set seed once** at the beginning of analysis
2. **Document the seed value** used in your code
3. **Don't reset seed multiple times** unless specifically needed
4. **Use different seeds** for different experiments/analyses

## Assessment Questions

**Understanding Random Seeds**: When you set `np.random.seed(123)` and then call `np.random.normal(size=5)` twice:
- The values will be **different** between the two calls
- The seed controls the starting point of the random sequence, not individual calls
- Each call advances the random number generator state

## Common Applications
- **A/B Testing**: Ensuring samples are representative before drawing conclusions
- **Quality Control**: Assessing whether sample-based quality metrics represent true population quality
- **Survey Research**: Validating that survey respondents represent the target population
- **Simulation Studies**: Generating controlled random data for testing algorithms

## Red Flags for Non-Representative Samples
- **Convenience sampling** (easiest to collect)
- **Self-selection bias** (voluntary participation)
- **Time-based bias** (collecting only during specific periods)
- **Geographic bias** (sampling from limited locations)
- **Demographic skew** (over/under-representation of groups)

## Extension Opportunities
- Learn about stratified sampling for ensuring representative subgroups
- Explore statistical tests for comparing distributions (Kolmogorov-Smirnov test)
- Study bootstrap resampling for uncertainty quantification
- Practice with different probability distributions for various data types