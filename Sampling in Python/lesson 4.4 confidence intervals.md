# Revision Notes: Confidence Intervals

## Learning Objectives

1. Understand what confidence intervals represent and communicate
2. Master two methods for calculating confidence intervals: quantile and standard error
3. Learn when to use each method and how to interpret results

## What Do Confidence Intervals Provide?

### **Answer to the Question**

**Correct Answer: "A range of plausible values for an unknown quantity."**

### **Why This is Correct**

- Confidence intervals estimate **unknown population parameters**
- They provide a **range of plausible values**, not just a single point estimate
- The "unknown quantity" could be population mean, median, proportion, difference, etc.
- They quantify **uncertainty** around our estimate

### **Why Other Options Are Wrong**

- **"Range of all possible values for a variable"**: ❌ This describes the data range, not a confidence interval
- **"All numbers between 0 and 1"**: ❌ This describes probability scale, not confidence intervals
- **"Range for a variable in our population"**: ❌ Too specific; CIs can be for any parameter, not just variables

### **What Confidence Intervals Actually Tell Us**

- **"We are 95% confident that the true population [parameter] lies between [lower] and [upper]"**
- **Uncertainty quantification**: How precise is our estimate?
- **Range of reasonable values**: What values are consistent with our data?
- **Statistical inference**: Bridge from sample to population

## Two Methods for Calculating Confidence Intervals

### **Method 1: Quantile Method (Bootstrap Percentile)**

#### **Implementation**

```python
# Generate a 95% confidence interval using the quantile method
lower_quant = np.quantile(bootstrap_distribution, 0.025)
upper_quant = np.quantile(bootstrap_distribution, 0.975)

# Print quantile method confidence interval
print((lower_quant, upper_quant))
```

#### **How It Works**

- **Use bootstrap distribution directly**: Take percentiles from actual bootstrap values
- **2.5th percentile**: Lower bound of 95% CI
- **97.5th percentile**: Upper bound of 95% CI
- **Middle 95%**: Contains 95% of bootstrap estimates

#### **Advantages**

- **No distributional assumptions**: Works regardless of bootstrap distribution shape
- **Direct interpretation**: Uses actual bootstrap values
- **Robust**: Works with skewed or unusual distributions
- **Simple**: Just find percentiles

#### **When to Use**

- **Non-normal distributions**: When bootstrap distribution is skewed
- **Complex statistics**: Medians, correlations, custom statistics
- **Small samples**: When normality assumptions questionable
- **Default choice**: Generally reliable approach

### **Method 2: Standard Error Method (Normal Approximation)**

#### **Implementation**

```python
# Find the mean and std dev of the bootstrap distribution
point_estimate = np.mean(bootstrap_distribution)
standard_error = np.std(bootstrap_distribution, ddof=1)

# Find the lower limit using normal distribution
lower_se = norm.ppf(0.025, loc=point_estimate, scale=standard_error)

# Find the upper limit using normal distribution
upper_se = norm.ppf(0.975, loc=point_estimate, scale=standard_error)

# Print standard error method confidence interval
print((lower_se, upper_se))
```

#### **How It Works**

- **Assume normality**: Bootstrap distribution is approximately normal
- **Use normal distribution**: Apply normal theory to calculate bounds
- **Mathematical formula**: point_estimate ± (critical_value × standard_error)
- **Standard approach**: Based on Central Limit Theorem

#### **Mathematical Background**

```python
# The formula being implemented:
# CI = mean ± z_critical × standard_error
# Where z_critical = 1.96 for 95% confidence

# norm.ppf(0.025) gives z = -1.96
# norm.ppf(0.975) gives z = +1.96
```

#### **Advantages**

- **Theoretical foundation**: Based on well-established statistical theory
- **Consistent with textbooks**: Standard approach in statistics
- **Easy to explain**: Clear mathematical interpretation
- **Generalizable**: Works for any confidence level

#### **When to Use**

- **Large samples**: When Central Limit Theorem applies
- **Normal-ish distributions**: When bootstrap distribution looks roughly normal
- **Standard statistics**: Means, proportions with adequate sample sizes
- **Consistency needed**: When following standard statistical practices

## Comparing the Two Methods

### **Expected Results**

- **Similar results**: When bootstrap distribution is approximately normal
- **Different results**: When bootstrap distribution is skewed or has unusual shape
- **Quantile method more robust**: Better handles non-normal distributions

### **Which Method to Choose?**

| Scenario                     | Recommended Method    | Reason                           |
| ---------------------------- | --------------------- | -------------------------------- |
| **Normal-looking bootstrap** | Either method         | Both should give similar results |
| **Skewed bootstrap**         | Quantile method       | Handles asymmetry better         |
| **Small sample size**        | Quantile method       | Fewer assumptions                |
| **Standard reporting**       | Standard error method | More commonly used               |
| **Complex statistic**        | Quantile method       | More flexible                    |

## Technical Implementation Details

### **Confidence Level Translation**

```python
# For 95% confidence interval:
alpha = 0.05  # Significance level
lower_percentile = alpha/2 = 0.025  # 2.5th percentile
upper_percentile = 1 - alpha/2 = 0.975  # 97.5th percentile

# For 90% confidence interval:
alpha = 0.10
lower_percentile = 0.05  # 5th percentile
upper_percentile = 0.95  # 95th percentile
```

### **Critical Values**

```python
# Standard normal critical values
90% CI: ±1.645
95% CI: ±1.96
99% CI: ±2.576

# Using scipy
from scipy.stats import norm
z_95 = norm.ppf(0.975)  # Returns 1.96
```

### **Standard Error Calculation**

```python
# Using ddof=1 for sample standard deviation
standard_error = np.std(bootstrap_distribution, ddof=1)

# Alternative: using numpy with Bessel's correction
standard_error = np.std(bootstrap_distribution) * np.sqrt(len(bootstrap_distribution)/(len(bootstrap_distribution)-1))
```

## Interpretation Guidelines

### **Correct Interpretation**

- **"We are 95% confident that the true population mean lies between X and Y"**
- **"The interval [X, Y] contains plausible values for the population parameter"**
- **"If we repeated this process many times, 95% of intervals would contain the true parameter"**

### **Common Misinterpretations to Avoid**

- ❌ **"There's a 95% chance the true value is in this interval"** (frequentist vs. Bayesian confusion)
- ❌ **"95% of the data falls in this interval"** (parameter interval vs. data interval)
- ❌ **"The true value is definitely in this interval"** (confidence, not certainty)

## Practical Applications

### **Business Example**

```python
# Bootstrap confidence interval for average customer satisfaction
customer_scores = [bootstrap process for satisfaction scores]
ci_lower, ci_upper = np.quantile(customer_scores, [0.025, 0.975])
print(f"We are 95% confident the true average satisfaction is between {ci_lower:.2f} and {ci_upper:.2f}")
```

### **Medical Research Example**

```python
# Confidence interval for treatment effect
treatment_effects = [bootstrap process for effect sizes]
ci_lower, ci_upper = np.quantile(treatment_effects, [0.025, 0.975])
if ci_lower > 0 and ci_upper > 0:
    print("Treatment appears to have positive effect")
```

## Best Practices

### **Choosing Confidence Level**

- **95%**: Most common, good balance of precision and confidence
- **90%**: When narrower intervals acceptable
- **99%**: When higher confidence required (but wider intervals)

### **Reporting Standards**

```python
# Good reporting format
point_est = np.mean(bootstrap_distribution)
ci_lower, ci_upper = np.quantile(bootstrap_distribution, [0.025, 0.975])
print(f"Mean popularity: {point_est:.2f} (95% CI: {ci_lower:.2f}, {ci_upper:.2f})")
```

### **Quality Checks**

- **Interval width**: Reasonable given sample size and variability
- **Symmetry**: Check if interval is roughly symmetric around point estimate
- **Plausibility**: Do the bounds make practical sense?
- **Consistency**: Do different methods give similar results?

## Key Takeaways

1. **Confidence intervals quantify uncertainty** around parameter estimates
2. **Range of plausible values** for unknown population parameters
3. **Quantile method**: Uses bootstrap distribution percentiles directly
4. **Standard error method**: Assumes normality and uses z-scores
5. **Choose method based on** distribution shape and context
6. **Interpret correctly**: Confidence about process, not probability about parameter
7. **Both methods should agree** when bootstrap distribution is approximately normal
8. **Quantile method more robust** for non-normal distributions
