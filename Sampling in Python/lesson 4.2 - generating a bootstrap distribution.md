# Revision Notes: Generating a Bootstrap Distribution

## Learning Objectives

1. Understand the step-by-step process of creating a bootstrap distribution
2. Learn how bootstrap distribution generation differs from sampling distribution generation
3. Master the implementation of bootstrap resampling with replacement

## Key Concepts

### **Bootstrap vs. Sampling Distribution Process**

| Step                       | Sampling Distribution      | Bootstrap Distribution  |
| -------------------------- | -------------------------- | ----------------------- |
| **1. Starting point**      | Population                 | Sample                  |
| **2. Resampling method**   | Sample WITHOUT replacement | Sample WITH replacement |
| **3. Calculate statistic** | Same                       | Same                    |
| **4. Repeat many times**   | Same                       | Same                    |
| **5. Visualize**           | Same                       | Same                    |

### **Why the Difference Matters**

- **Sampling distribution**: Studies how sample statistics vary when drawing from known population
- **Bootstrap distribution**: Estimates how sample statistics would vary if we could resample from unknown population

## Step-by-Step Implementation

### **Step 1: Generate One Bootstrap Resample**

```python
# Generate 1 bootstrap resample
spotify_1_resample = spotify_sample.sample(frac=1, replace=True)
print(spotify_1_resample)
```

**Key Parameters:**

- `frac=1`: Sample 100% of original sample size
- `replace=True`: Allow same observations to appear multiple times
- **Result**: Bootstrap sample same size as original, but with replacement

**What to Observe:**

- Some original rows appear multiple times
- Some original rows don't appear at all
- Total sample size remains the same

### **Step 2: Calculate Statistic for One Resample**

```python
# Calculate mean of the danceability column of spotify_1_resample
mean_danceability_1 = np.mean(spotify_1_resample['danceability'])
print(mean_danceability_1)
```

**Why This Works:**

- Each bootstrap resample gives slightly different mean
- Variation comes from different combinations of original observations
- Some observations weighted more heavily (appear multiple times)

### **Step 3: Repeat Many Times**

```python
# Replicate this 1000 times
mean_danceability_1000 = []
for i in range(1000):
    bootstrap_mean = np.mean(spotify_sample.sample(frac=1, replace=True)['danceability'])
    mean_danceability_1000.append(bootstrap_mean)

print(mean_danceability_1000)
```

**Process Breakdown:**

- **Loop 1000 times**: Create many bootstrap resamples
- **Each iteration**: New resample + calculate mean
- **Collect results**: Store all bootstrap means in list
- **Result**: 1000 different estimates of the mean

### **Step 4: Visualize the Bootstrap Distribution**

```python
# Draw a histogram of the resample means
plt.hist(mean_danceability_1000)
plt.show()
```

**What the Histogram Shows:**

- **Distribution shape**: How bootstrap means are distributed
- **Center**: Should be close to original sample mean
- **Spread**: Indicates uncertainty in the original estimate
- **Pattern**: Approximates true sampling distribution

## Technical Details

### **Key Implementation Points**

#### **Using `frac=1` vs `n=sample_size`**

```python
# Both approaches give same result
method1 = spotify_sample.sample(frac=1, replace=True)  # Fraction approach
method2 = spotify_sample.sample(n=len(spotify_sample), replace=True)  # Count approach
```

#### **Why `replace=True` is Critical**

```python
# WITH replacement (correct for bootstrap)
bootstrap_sample = spotify_sample.sample(frac=1, replace=True)
# Some rows appear multiple times, some don't appear

# WITHOUT replacement (just reshuffles original data)
shuffled_sample = spotify_sample.sample(frac=1, replace=False)
# Every row appears exactly once - no new information
```

### **Understanding the Resampling Process**

#### **What Happens in Each Bootstrap Sample**

- **Original sample**: 100 unique songs
- **Bootstrap sample**: 100 songs, but some repeated, some missing
- **Variation source**: Different combinations create different means
- **Information gain**: Estimates uncertainty from available data

#### **Example Pattern**

```
Original sample: [Song1, Song2, Song3, Song4, Song5]
Bootstrap 1:     [Song1, Song1, Song3, Song5, Song2]  # Song1 appears twice, Song4 missing
Bootstrap 2:     [Song2, Song4, Song4, Song1, Song3]  # Song4 appears twice, Song5 missing
Bootstrap 3:     [Song3, Song1, Song2, Song5, Song5]  # Song5 appears twice, Song4 missing
```

## Expected Results

### **Properties of Bootstrap Distribution**

1. **Center**: Bootstrap distribution mean ≈ original sample mean
2. **Shape**: Often approximately normal (Central Limit Theorem)
3. **Spread**: Indicates standard error of the original estimate
4. **Range**: Shows plausible values for population parameter

### **Quality Indicators**

- **Smooth distribution**: Enough replicates (1000+) for stable pattern
- **Reasonable center**: Close to original sample statistic
- **Sensible spread**: Not too narrow (no variability) or too wide (unstable)

## Common Applications

### **Bootstrap Standard Error**

```python
# Estimate standard error of the mean
bootstrap_means = [bootstrap process as above]
bootstrap_se = np.std(bootstrap_means, ddof=1)
print(f"Bootstrap standard error: {bootstrap_se}")
```

### **Bootstrap Confidence Interval**

```python
# 95% confidence interval using percentile method
confidence_interval = np.percentile(bootstrap_means, [2.5, 97.5])
print(f"95% CI: {confidence_interval}")
```

### **Other Statistics**

```python
# Bootstrap any statistic, not just mean
bootstrap_medians = []
bootstrap_stds = []
for i in range(1000):
    resample = spotify_sample.sample(frac=1, replace=True)
    bootstrap_medians.append(np.median(resample['danceability']))
    bootstrap_stds.append(np.std(resample['danceability'], ddof=1))
```

## Best Practices

### **Number of Bootstrap Replicates**

- **Quick exploration**: 100-500 replicates
- **Standard analysis**: 1000-2000 replicates
- **Confidence intervals**: 5000-10000 replicates
- **Publication quality**: 10000+ replicates

### **Reproducibility**

```python
# Set random seed for reproducible results
np.random.seed(123)
bootstrap_sample = spotify_sample.sample(frac=1, replace=True)
```

### **Validation Checks**

```python
# Check bootstrap distribution properties
original_mean = np.mean(spotify_sample['danceability'])
bootstrap_mean = np.mean(mean_danceability_1000)
print(f"Original mean: {original_mean}")
print(f"Bootstrap mean: {bootstrap_mean}")
print(f"Difference: {abs(original_mean - bootstrap_mean)}")
# Should be very small
```

## When Bootstrap Works Well

### **Ideal Conditions**

- **Reasonable sample size**: n ≥ 30 generally recommended
- **Representative sample**: Original sample reflects population
- **Well-behaved statistic**: Mean, median work better than extreme percentiles
- **Independent observations**: No strong dependence structure

### **Caution Needed**

- **Very small samples**: n < 10 may not bootstrap well
- **Extreme statistics**: Min, max may not bootstrap reliably
- **Heavy dependence**: Time series may need specialized methods
- **Heavily skewed data**: May need bias correction

## Advantages of Bootstrap Approach

1. **No distributional assumptions**: Works regardless of data distribution
2. **Flexible**: Can bootstrap any statistic
3. **Intuitive**: Easy to understand conceptually
4. **Robust**: Generally works well across different scenarios
5. **Practical**: Uses only available data

## Key Takeaways

1. **Bootstrap starts with sample**, not population
2. **Sample WITH replacement** to create variability
3. **Same size resamples** as original sample
4. **Many replicates** create bootstrap distribution
5. **Histogram visualization** shows uncertainty pattern
6. **Works for any statistic**, not just means
7. **Approximates true sampling distribution** when population unknown
8. **Bootstrap mean should equal** original sample mean
9. **More replicates give** smoother, more stable results
