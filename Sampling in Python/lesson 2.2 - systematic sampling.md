# Revision Notes: Complete Guide to Basic Sampling Methods

## Learning Objectives

1. Master simple random sampling as the foundation of unbiased sampling
2. Understand systematic sampling implementation and when to use it
3. Learn to detect when systematic sampling may be problematic
4. Know how to choose between sampling methods based on data characteristics

## Simple Random Sampling (SRS)

### **What is Simple Random Sampling?**

The most fundamental sampling method where:

- Each row has **equal probability** of being selected
- Selections are **independent** of each other
- No systematic bias in the selection process
- Forms the **gold standard** for statistical inference

### **Implementation**

```python
# Basic simple random sampling
attrition_samp = attrition_pop.sample(n=70, random_state=18900217)
print(attrition_samp)
```

**Key Parameters:**

- `n=70`: Number of observations to sample
- `random_state=18900217`: Seed for reproducible results

**Why Use SRS:**

- **Unbiased**: No systematic selection bias
- **Statistically sound**: Enables valid statistical inference
- **Simple implementation**: Easy to use with pandas
- **Widely accepted**: Standard method across industries

## Systematic Sampling

### **What is Systematic Sampling?**

A **non-random** sampling method where:

- Rows are selected at **regular intervals**
- Uses mathematical spacing rather than randomness
- Provides **even coverage** across the dataset

### **Implementation**

```python
# Step 1: Calculate sampling parameters
sample_size = 70
pop_size = len(attrition_pop)
interval = pop_size // sample_size

# Step 2: Select every nth row
attrition_sys_samp = attrition_pop.iloc[::interval]
print(attrition_sys_samp)
```

**How It Works:**

1. **Calculate interval**: Population size ÷ Sample size
2. **Select systematically**: Every nth row where n = interval
3. **Result**: Evenly spaced observations

**Example:** For 1,000 rows wanting 5 samples:

- Interval = 1,000 ÷ 5 = 200
- Select rows: 0, 200, 400, 600, 800

## When Systematic Sampling Can Go Wrong

### **The Problem**

Systematic sampling fails when data has:

- **Sorted arrangements**: Data ordered by important variables
- **Hidden patterns**: Periodic cycles or groupings
- **Meaningful row order**: Non-random organization

### **Detection Method**

```python
# Step 1: Add index to track original positions
attrition_pop_id = attrition_pop.reset_index()

# Step 2: Plot variable vs. original index
attrition_pop_id.plot(x="index", y="YearsAtCompany", kind="scatter")
plt.show()

# Step 3: Compare with shuffled data
attrition_shuffled = attrition_pop.sample(frac=1)
attrition_shuffled = attrition_shuffled.reset_index(drop=True).reset_index()
attrition_shuffled.plot(kind="scatter", x="index", y="YearsAtCompany")
plt.show()
```

### **Interpreting Results**

**Problem Indicators (Original Data):**

- Clear upward/downward trends
- Step patterns or groupings
- Periodic cycles
- Any systematic arrangement

**Safe Indicators (Shuffled Data):**

- Random scatter with no patterns
- Even distribution across all indices
- No systematic trends

## Comparison: SRS vs. Systematic Sampling

| Aspect               | Simple Random Sampling | Systematic Sampling          |
| -------------------- | ---------------------- | ---------------------------- |
| **Selection method** | Random probability     | Fixed intervals              |
| **Randomness**       | Full randomness        | Deterministic pattern        |
| **Bias risk**        | Minimal (unbiased)     | Depends on data order        |
| **Coverage**         | May cluster randomly   | Guaranteed even spacing      |
| **Implementation**   | `.sample()` method     | Index slicing `[::interval]` |
| **Best for**         | General analysis       | Ordered populations          |

## When to Use Each Method

### **Choose Simple Random Sampling When:**

- Data order is random or unknown
- Statistical inference is primary goal
- Need to eliminate all selection bias
- Standard approach is acceptable

### **Choose Systematic Sampling When:**

- Data has no problematic patterns
- Even coverage is specifically desired
- Working with naturally ordered data (like time series)
- Computational simplicity is important

### **Avoid Systematic Sampling When:**

- Data is sorted by key variables
- Hidden periodic patterns exist
- Row order has meaning
- You detect trends in the scatter plot

## Solutions When Systematic Sampling is Problematic

### **Option 1: Shuffle First**

```python
# Shuffle data, then apply systematic sampling
shuffled_data = attrition_pop.sample(frac=1, random_state=123)
# Now systematic sampling is equivalent to SRS
```

### **Option 2: Switch to SRS**

```python
# Use simple random sampling instead
safe_sample = attrition_pop.sample(n=70, random_state=123)
```

### **Option 3: Stratified Sampling**

```python
# If patterns relate to important subgroups
stratified_sample = attrition_pop.groupby('category').sample(frac=0.1, random_state=123)
```

## Answer to the Question

**Correct Answer: "No. This is not true if the data is sorted in some way."**

**Explanation:**

- Systematic sampling ≠ Simple random sampling when data has order
- If data is randomly arranged → both methods give similar results
- If data is sorted/patterned → systematic sampling can be severely biased
- The presence of patterns, not random seeds, determines the difference

## Best Practices

### **Before Choosing a Sampling Method:**

1. **Examine data structure**: Check for sorting or patterns
2. **Visualize key variables**: Plot against row index
3. **Consider your goals**: Need for even coverage vs. unbiased estimates
4. **Test assumptions**: Compare original vs. shuffled data plots

### **Quality Assurance:**

- Always set `random_state` for reproducibility
- Document your sampling method choice and reasoning
- Validate sample characteristics against population
- Check for unexpected bias in resulting samples

### **General Recommendation:**

When in doubt, **simple random sampling** is usually the safer choice because it guarantees unbiased selection regardless of data organization.

## Practical Applications

### **Simple Random Sampling Examples:**

- General employee satisfaction surveys
- Medical research patient selection
- Market research customer sampling
- Quality control product testing

### **Systematic Sampling Examples:**

- Production line quality checks (every nth item)
- Audit sampling (every nth transaction)
- Time series analysis (regular intervals)
- Geographic surveys (evenly spaced locations)

## Key Takeaways

1. **SRS is the gold standard** for unbiased sampling
2. **Systematic sampling can be efficient** but requires careful validation
3. **Data order matters critically** for systematic sampling success
4. **Always check for patterns** before using systematic methods
5. **When uncertain, choose SRS** for guaranteed unbiased results
