# Revision Notes: Bootstrap vs. Sampling Distribution Statistics

## Learning Objectives

1. Understand when bootstrap distribution statistics accurately estimate population parameters
2. Compare bootstrap and sampling distribution performance for means and standard deviations
3. Learn the fundamental relationships between different types of distributions

## Key Question: Can Bootstrap Always Estimate Population Parameters?

### **Answer to Initial Question**

**Correct Answer: "No, the mean of the bootstrap distribution will always be near the sample mean, which may not necessarily be very close to the population mean."**

### **Why This is Correct**

- **Bootstrap distribution** is based on resampling from your original sample
- **Bootstrap mean** ≈ **Original sample mean** (always)
- **Original sample mean** may or may not be close to **population mean** (depends on sample quality)
- **Bootstrap cannot fix** a biased or unrepresentative original sample

### **Why Other Options Are Wrong**

- **"Standard deviation issue"**: This describes standard error relationship, not the main limitation
- **"Both estimates match closely"**: ❌ Bootstrap can't improve on original sample bias
- **"Variability will be similar"**: ❌ Variability depends heavily on sample size

## Generating Sampling vs. Bootstrap Distributions

### **Sampling Distribution Generation**

```python
mean_popularity_2000_samp = []

# Generate a sampling distribution of 2000 replicates
for i in range(2000):
    # Sample 500 rows from POPULATION without replacement
    sample_mean = np.mean(spotify_population.sample(n=500, replace=False)["popularity"])
    mean_popularity_2000_samp.append(sample_mean)

print(mean_popularity_2000_samp)
```

**Key Points:**

- Starts with **population** (spotify_population)
- Samples **without replacement** (replace=False)
- Shows how sample means vary when we know the true population

### **Bootstrap Distribution Generation**

```python
# (From previous exercises)
bootstrap_means = []
for i in range(2000):
    # Resample from SAMPLE with replacement
    bootstrap_mean = np.mean(spotify_sample.sample(frac=1, replace=True)["popularity"])
    bootstrap_means.append(bootstrap_mean)
```

**Key Points:**

- Starts with **sample** (spotify_sample)
- Samples **with replacement** (replace=True)
- Estimates how sample means would vary if we could resample from population

## Comparing Means: Four Key Values

### **Implementation**

```python
# Calculate the population mean popularity
pop_mean = spotify_population["popularity"].mean()

# Calculate the original sample mean popularity
samp_mean = spotify_sample["popularity"].mean()

# Calculate the sampling distribution estimate of mean popularity
samp_distn_mean = np.mean(sampling_distribution)

# Calculate the bootstrap distribution estimate of mean popularity
boot_distn_mean = np.mean(bootstrap_distribution)

# Print all four means
print([pop_mean, samp_mean, samp_distn_mean, boot_distn_mean])
```

### **Expected Relationships**

1. **pop_mean**: The true population value we want to estimate
2. **samp_mean**: Our original sample estimate
3. **samp_distn_mean** ≈ **pop_mean**: Sampling distribution centers on population mean
4. **boot_distn_mean** ≈ **samp_mean**: Bootstrap distribution centers on sample mean

### **Answer: Mean Comparison**

**Correct Answer: "The sampling distribution mean is the best estimate of the true population mean; the bootstrap distribution mean is closest to the original sample mean."**

**Why This Makes Sense:**

- **Sampling distribution** samples from true population → estimates population mean accurately
- **Bootstrap distribution** resamples from original sample → reproduces sample mean

## Comparing Standard Deviations: Population Variability

### **Implementation**

```python
# Calculate the population std dev popularity
pop_sd = spotify_population["popularity"].std(ddof=0)

# Calculate the original sample std dev popularity
samp_sd = spotify_sample["popularity"].std(ddof=1)

# Calculate the sampling distribution estimate of std dev popularity
samp_distn_sd = (np.std(sampling_distribution, ddof=1)) * np.sqrt(5000)

# Calculate the bootstrap distribution estimate of std dev popularity
boot_distn_sd = (np.std(bootstrap_distribution, ddof=1)) * np.sqrt(5000)

# Print all four standard deviations
print([pop_sd, samp_sd, samp_distn_sd, boot_distn_sd])
```

### **Understanding the Formula: Why Multiply by √n?**

```
Standard Error = Population SD / √n
Therefore: Population SD = Standard Error × √n

Where:
- Standard Error = std(sampling_distribution) or std(bootstrap_distribution)
- n = sample size (5000 in this case)
- Population SD = what we're trying to estimate
```

### **Expected Relationships**

1. **pop_sd**: True population standard deviation
2. **samp_sd**: Sample estimate of population standard deviation
3. **samp_distn_sd**: Standard error × √n = estimate of population SD
4. **boot_distn_sd**: Standard error × √n = estimate of population SD

### **Answer: Standard Deviation Comparison**

**Correct Answer: "The calculations from both the sampling distribution and from the bootstrap distribution are equally close to the population standard deviation."**

**Why Both Work Equally Well:**

- Both correctly estimate the **standard error** of the sample mean
- Both use the same **mathematical relationship**: SD = SE × √n
- Bootstrap can accurately estimate **variability** even when sample mean is biased
- **Standard error estimation** doesn't depend on bias in the original sample

## Key Insights

### **What Bootstrap Does Well**

1. **Standard error estimation**: Accurately estimates variability of sample statistics
2. **Confidence intervals**: Provides uncertainty bounds around estimates
3. **Distribution shape**: Shows how sample statistics are distributed
4. **No distributional assumptions**: Works regardless of population distribution

### **What Bootstrap Cannot Do**

1. **Fix sample bias**: Cannot improve accuracy if original sample is biased
2. **Estimate population mean**: Always centers on sample mean, not population mean
3. **Create new information**: Limited by original sample representativeness
4. **Overcome sample limitations**: Small or biased samples remain problematic

### **When Sampling Distribution vs. Bootstrap**

| Aspect                     | Sampling Distribution             | Bootstrap Distribution            |
| -------------------------- | --------------------------------- | --------------------------------- |
| **Starting point**         | Known population                  | Available sample                  |
| **Mean estimation**        | Excellent (unbiased)              | Only as good as original sample   |
| **Variability estimation** | Excellent                         | Excellent                         |
| **Practical use**          | Rare (population usually unknown) | Common (sample usually available) |

## Practical Implications

### **For Mean Estimation**

- **Bootstrap mean** tells you what your **sample mean** is (redundant)
- **Bootstrap distribution** tells you **how variable** that estimate is (useful)
- **Don't use bootstrap** to "improve" your point estimate
- **Do use bootstrap** to understand uncertainty around your estimate

### **For Standard Deviation Estimation**

- **Both methods work equally well** for estimating population variability
- **Bootstrap is practical** because you don't need population access
- **Standard error formula** works regardless of potential bias in sample mean

### **Best Practices**

1. **Use bootstrap for uncertainty quantification**, not point estimation
2. **Focus on confidence intervals** rather than bootstrap means
3. **Ensure representative samples** before bootstrapping
4. **Validate bootstrap assumptions** (adequate sample size, independence)

## Real-World Applications

### **When Bootstrap is Useful**

```python
# Confidence interval for median (no theoretical formula)
bootstrap_medians = [np.median(sample.sample(frac=1, replace=True)) for _ in range(1000)]
ci_lower, ci_upper = np.percentile(bootstrap_medians, [2.5, 97.5])
```

### **When Bootstrap Has Limitations**

```python
# If original sample is biased, bootstrap won't fix it
biased_sample = population[population['income'] > 50000]  # Excludes low-income
# Bootstrap from biased_sample will still be biased toward high-income
```

## Key Takeaways

1. **Bootstrap distribution mean ≈ original sample mean** (not population mean)
2. **Sampling distribution mean ≈ population mean** (when population known)
3. **Both methods estimate standard error equally well**
4. **Bootstrap cannot fix biased samples**
5. **Use bootstrap for uncertainty, not point estimation**
6. **Standard error relationship: SD = SE × √n**
7. **Bootstrap variability estimation is as good as sampling distribution**
8. **Quality of bootstrap depends on quality of original sample**
