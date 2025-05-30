# Revision Notes: Properties of Sampling Distributions

## Learning Objectives

1. Understand the relationship between sampling distribution mean and population mean
2. Learn how sample size affects the standard deviation of sampling distributions
3. Master the Central Limit Theorem's key properties

## Key Concepts

### **What Are We Studying?**

We're examining sampling distributions created by:

1. Taking simple random samples from employee attrition data
2. Calculating the mean attrition for each sample
3. Repeating this process 1000 times
4. Comparing results across different sample sizes (5, 50, 500)

## Property 1: Mean of Sampling Distribution

### **Implementation**

```python
# Calculate the mean of the mean attritions for each sampling distribution
mean_of_means_5 = np.mean(sampling_distribution_5)
mean_of_means_50 = np.mean(sampling_distribution_50)
mean_of_means_500 = np.mean(sampling_distribution_500)

# Print the results
print(mean_of_means_5)
print(mean_of_means_50)
print(mean_of_means_500)
```

### **Expected Results**

All three values should be very close to the population mean attrition rate.

### **Answer to Question 1**

**Correct Answer: "Regardless of sample size, the mean of the sampling distribution is a close approximation to the population mean."**

### **Why This is True**

- **Unbiased estimator**: Sample mean is an unbiased estimator of population mean
- **Law of Large Numbers**: With many samples (1000), the average approaches the true value
- **Sample size independence**: This property holds regardless of individual sample size
- **Mathematical proof**: E[X̄] = μ (expected value of sample mean equals population mean)

### **Why Other Options Are Wrong**

- **"Mean decreases/increases until reaching population"**: ❌ Sample size doesn't systematically bias the mean
- **"Mean is biased"**: ❌ Sample mean is mathematically unbiased

## Property 2: Standard Deviation of Sampling Distribution

### **Implementation**

```python
# Calculate the std. dev. of the mean attritions for each sampling distribution
sd_of_means_5 = np.std(sampling_distribution_5, ddof=1)
sd_of_means_50 = np.std(sampling_distribution_50, ddof=1)
sd_of_means_500 = np.std(sampling_distribution_500, ddof=1)

# Print the results
print(sd_of_means_5)
print(sd_of_means_50)
print(sd_of_means_500)
```

### **Expected Pattern**

- **Largest**: `sd_of_means_5` (smallest sample size)
- **Medium**: `sd_of_means_50` (medium sample size)
- **Smallest**: `sd_of_means_500` (largest sample size)

### **Answer to Question 2**

**Correct Answer: "The standard deviation of the sampling distribution is approximately equal to the population standard deviation divided by the square root of the sample size."**

### **The Standard Error Formula**

```
Standard Error = σ / √n

Where:
- σ = population standard deviation
- n = sample size
- Standard Error = standard deviation of sampling distribution
```

### **Why This Formula Makes Sense**

1. **Larger samples** → **smaller standard error** (more precise estimates)
2. **Square root relationship** → **diminishing returns** (need 4x sample size to halve error)
3. **Population variability matters** → **more variable population** → **more variable sample means**

### **Why Other Options Are Wrong**

- **"Equal to population σ"**: ❌ Ignores sample size effect
- **"σ × n"**: ❌ Would make larger samples less precise (opposite of reality)
- **"σ × √n"**: ❌ Would make larger samples much less precise
- **"σ / n"**: ❌ Too strong an effect (would approach zero too quickly)

## The Central Limit Theorem

### **Key Properties**

1. **Mean Property**: E[X̄] = μ (sampling distribution mean equals population mean)
2. **Variance Property**: Var[X̄] = σ²/n (sampling distribution variance decreases with sample size)
3. **Shape Property**: Distribution approaches normal as n increases

### **Mathematical Relationships**

```python
# For any sample size n:
mean_of_sampling_distribution = population_mean
std_of_sampling_distribution = population_std / sqrt(n)
```

## Practical Implications

### **Sample Size Effects**

| Sample Size | Standard Error Effect | Practical Meaning                    |
| ----------- | --------------------- | ------------------------------------ |
| **n = 5**   | Large standard error  | Individual estimates highly variable |
| **n = 50**  | Medium standard error | Reasonable precision                 |
| **n = 500** | Small standard error  | High precision                       |

### **Diminishing Returns**

```python
# To halve the standard error, you need 4x the sample size
n_original = 100
standard_error_original = sigma / sqrt(100)

n_half_error = 400  # 4x original
standard_error_half = sigma / sqrt(400) = sigma / 20 = (sigma / 10) / 2
```

## Implementation Notes

### **Important Parameters**

- **`ddof=1`**: Uses sample standard deviation formula (divides by n-1)
- **Without `ddof=1`**: Uses population standard deviation formula (divides by n)
- **For sampling distributions**: `ddof=1` is typically appropriate

### **Verification Example**

```python
# Check if standard error formula holds
population_std = np.std(attrition_pop['Attrition'], ddof=1)

# Predicted standard errors
predicted_se_5 = population_std / np.sqrt(5)
predicted_se_50 = population_std / np.sqrt(50)
predicted_se_500 = population_std / np.sqrt(500)

# Compare with actual
print(f"Sample 5 - Actual: {sd_of_means_5:.4f}, Predicted: {predicted_se_5:.4f}")
print(f"Sample 50 - Actual: {sd_of_means_50:.4f}, Predicted: {predicted_se_50:.4f}")
print(f"Sample 500 - Actual: {sd_of_means_500:.4f}, Predicted: {predicted_se_500:.4f}")
```

## Real-World Applications

### **Quality Control**

```python
# Monitor production with different sample sizes
# Larger samples give more precise quality estimates
```

### **Survey Research**

```python
# Plan survey size based on required precision
# Budget vs. accuracy trade-off decisions
```

### **Medical Research**

```python
# Determine patient sample sizes for clinical trials
# Balance statistical power with resource constraints
```

## Best Practices

### **Choosing Sample Size**

1. **Consider required precision**: How accurate do estimates need to be?
2. **Use standard error formula**: Calculate precision for different sample sizes
3. **Account for costs**: Larger samples cost more but provide better precision
4. **Power analysis**: Formal statistical methods for optimal sample size

### **Interpreting Results**

- **Check mean convergence**: Sample distribution mean should ≈ population mean
- **Verify standard error pattern**: Should decrease with √n
- **Look for normality**: Distribution shape should approach normal with larger samples

### **Common Mistakes**

- **Confusing standard deviation with standard error**: Different concepts
- **Expecting linear relationship**: Effect diminishes with √n, not n
- **Ignoring population variability**: More variable populations need larger samples

## Key Takeaways

1. **Sample mean is unbiased** regardless of sample size
2. **Standard error decreases** with square root of sample size
3. **Diminishing returns** make very large samples costly for small gains
4. **Central Limit Theorem** provides theoretical foundation
5. **Standard error formula** enables precision planning
6. **Population variability affects** how much sample size matters
