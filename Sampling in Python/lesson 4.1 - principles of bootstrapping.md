# Revision Notes: Principles of Bootstrapping

## Learning Objectives

1. Understand bootstrapping as the opposite of traditional sampling
2. Learn the key principles that govern bootstrap resampling
3. Distinguish between bootstrap distributions and sampling distributions

## Core Concept

### **What is Bootstrapping?**

Bootstrapping is a statistical technique that:

- **Treats your dataset as a sample** (not the population)
- **Uses it to build up a theoretical population**
- **Estimates the sampling distribution** of a statistic
- **Provides uncertainty estimates** when population is unknown

### **Bootstrap vs. Traditional Sampling**

| Traditional Sampling     | Bootstrapping                   |
| ------------------------ | ------------------------------- |
| Dataset = Population     | Dataset = Sample                |
| Generate subset          | Generate resamples              |
| Study sample variability | Estimate population variability |
| Known population         | Unknown population              |

## Key Principles of Bootstrapping

### **Principle 1: Bootstrap Distribution Consists of Many Replicates**

**True Statement**: "A bootstrap distribution consists of many replicates of the statistic of interest."

**What This Means:**

- Create hundreds or thousands of bootstrap samples
- Calculate the same statistic (mean, median, etc.) for each resample
- Collect all these statistics to form the bootstrap distribution
- More replicates → better approximation of true sampling distribution

**Implementation Concept:**

```python
bootstrap_statistics = []
for i in range(1000):  # Many replicates
    bootstrap_sample = original_sample.sample(n=len(original_sample), replace=True)
    statistic = bootstrap_sample.mean()  # Or any statistic of interest
    bootstrap_statistics.append(statistic)
```

### **Principle 2: Bootstrap Resamples Same Size as Original**

**True Statement**: "Bootstrap resamples should be the same size as the original sample."

**Why This Matters:**

- Maintains the same **sample size effects** as original
- Preserves **statistical properties** of the estimator
- Ensures **comparable precision** to original sample
- Standard practice across all bootstrap applications

**Example:**

- Original sample: n = 50 → Bootstrap samples: n = 50
- Original sample: n = 200 → Bootstrap samples: n = 200

### **Principle 3: Equal Probability Resampling**

**True Statement**: "Each row in the dataset should have an equally likely chance of being drawn in a resample."

**Implementation Details:**

- **With replacement**: Same observation can appear multiple times
- **Equal probability**: Each original observation has 1/n chance each draw
- **Random selection**: No systematic bias in choosing observations
- **Maintains representativeness**: All original data points can contribute

## Common Misconceptions (False Statements)

### **Misconception 1: Resampling Without Replacement**

**False Statement**: "Resampling means sampling without replacement."

**Why This is Wrong:**

- Bootstrap **requires sampling WITH replacement**
- Without replacement → just reshuffling original data
- With replacement → allows variability and uncertainty estimation
- Without replacement → no new information gained

### **Misconception 2: Bootstrap = Sampling Distribution**

**False Statement**: "A bootstrap distribution is the same thing as a sampling distribution."

**Key Differences:**
| Bootstrap Distribution | Sampling Distribution |
|----------------------|---------------------|
| Based on resampling from sample | Based on all possible samples from population |
| **Approximates** sampling distribution | **Is** the true sampling distribution |
| Uses available data | Requires population access |
| Practical method | Theoretical concept |

### **Misconception 3: Always Calculate Mean**

**False Statement**: "The statistic of interest for each bootstrap sample is always the mean."

**Reality:**

- Can bootstrap **any statistic**: mean, median, standard deviation, correlation, etc.
- Choice depends on **research question**
- Common statistics: proportions, differences, regression coefficients
- **Flexibility** is a key advantage of bootstrapping

## Bootstrap Process Overview

### **Step-by-Step Method**

1. **Start with original sample** (your available data)
2. **Resample with replacement** (same size as original)
3. **Calculate statistic** for each bootstrap sample
4. **Repeat many times** (typically 1000+ replicates)
5. **Analyze bootstrap distribution** (confidence intervals, standard errors)

### **What Bootstrap Provides**

- **Standard errors** of statistics
- **Confidence intervals** for parameters
- **Distribution shape** of estimators
- **Bias correction** for estimators
- **Hypothesis testing** capabilities

## When to Use Bootstrapping

### **Ideal Scenarios**

- **Unknown population distribution**
- **Complex statistics** with no known theoretical distribution
- **Small to medium sample sizes**
- **Non-parametric analysis** needed
- **Robust inference** desired

### **Examples**

- Estimating confidence interval for median income
- Bootstrap standard error for correlation coefficient
- Confidence interval for difference in means
- Standard error for regression coefficients

## Advantages of Bootstrapping

### **Practical Benefits**

1. **No distributional assumptions** required
2. **Works with any statistic**
3. **Uses only available data**
4. **Computationally straightforward**
5. **Provides uncertainty quantification**

### **Statistical Benefits**

1. **Consistent estimation** of standard errors
2. **Bias correction** capabilities
3. **Robust to outliers** (depending on statistic)
4. **Flexible confidence intervals**

## Limitations and Assumptions

### **Key Assumptions**

- **Sample is representative** of population
- **Original sample size adequate** for bootstrap to work
- **i.i.d. data** (independent, identically distributed)
- **Statistic has reasonable properties**

### **Limitations**

- **Small samples** may not work well
- **Extreme values** may be over/under-represented
- **Complex dependence structures** may be missed
- **Computational intensity** for very large datasets

## Best Practices

### **Implementation Guidelines**

- Use **1000+ bootstrap replicates** for standard errors
- Use **10000+ replicates** for confidence intervals
- **Set random seeds** for reproducibility
- **Check convergence** with different numbers of replicates

### **Quality Checks**

- Bootstrap distribution should be **reasonably smooth**
- **Center** should be close to original sample statistic
- **Shape** should make sense for the statistic
- **Outliers** in bootstrap distribution warrant investigation

## With or Without Replacement? Decision Framework

### **When to Use Sampling WITH Replacement**

Use when you have a **sample** and want to estimate population properties through bootstrapping.

#### **Scenario 1: Stock Market Analysis**

**Situation**: "A small hedge fund lacks the resources to track every stock, but feels confident that the 100 stocks they have tracked represent the market."

**Why WITH replacement:**

- You have a **sample** of 100 stocks (not the full population)
- Want to estimate **market-wide properties** from this sample
- Bootstrap resampling allows uncertainty quantification
- Each stock should have equal chance to appear multiple times

#### **Scenario 2: Wildlife Research**

**Situation**: "While studying variability in tigers' weights, you collected data on 10 tigers. You suspect that there are around 50 in your region of Siberia."

**Why WITH replacement:**

- You have a **small sample** (10 tigers) from larger population (50 tigers)
- Want to estimate **population parameters** (mean weight, variability)
- Bootstrap helps estimate uncertainty in your sample statistics
- Tigers can appear multiple times in resamples to create variability

### **When to Use Sampling WITHOUT Replacement**

Use when you have the **full population** or **complete dataset** and want to create samples.

#### **Scenario 3: Social Media Analysis**

**Situation**: "A social media company released all their members' posts from the last year. You need to report on this data to your manager by the end of the week."

**Why WITHOUT replacement:**

- You have the **complete dataset** (all posts from last year)
- Want to **analyze a subset** for efficiency (time constraint)
- Each post should only appear once in your sample
- Traditional sampling from population to sample

#### **Scenario 4: Census Data Analysis**

**Situation**: "A regional census was conducted to understand the mean income of people living there. Analyzing the full dataset will take too much time."

**Why WITHOUT replacement:**

- You have the **full population data** (complete census)
- Want to create a **manageable sample** for analysis
- Each person should only appear once in your sample
- Standard sampling approach for large populations

### **Decision Rules Summary**

| Your Data      | Goal                           | Method    | Replacement             |
| -------------- | ------------------------------ | --------- | ----------------------- |
| **Sample**     | Estimate population properties | Bootstrap | **WITH** replacement    |
| **Sample**     | Understand sample uncertainty  | Bootstrap | **WITH** replacement    |
| **Population** | Create manageable subset       | Sample    | **WITHOUT** replacement |
| **Population** | Quick analysis of trends       | Sample    | **WITHOUT** replacement |

### **Key Questions to Ask**

1. **Do I have the full population or just a sample?**

   - Sample → Consider bootstrapping WITH replacement
   - Population → Traditional sampling WITHOUT replacement

2. **What am I trying to estimate?**

   - Population parameters from sample → Bootstrap WITH replacement
   - Subset analysis from population → Sample WITHOUT replacement

3. **What are my resource constraints?**
   - Time/computational limits with full data → Sample WITHOUT replacement
   - Need uncertainty estimates → Bootstrap WITH replacement

## Key Takeaways

1. **Bootstrapping approximates** what we can't calculate exactly
2. **Resampling with replacement** is essential for creating variability
3. **Bootstrap sample size equals original** sample size
4. **Many replicates** create the bootstrap distribution
5. **Any statistic** can be bootstrapped, not just means
6. **Bootstrap ≠ sampling distribution** (approximation vs. truth)
7. **Equal probability resampling** ensures representativeness
8. **WITH replacement** for bootstrapping from samples
9. **WITHOUT replacement** for sampling from populations
