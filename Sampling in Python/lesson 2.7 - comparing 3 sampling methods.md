# Revision Notes: Comparing Three Sampling Methods

## Learning Objectives

1. Understand how to implement three major sampling methods on the same dataset
2. Learn to compare point estimates across different sampling approaches
3. Evaluate how well each sampling method estimates population parameters

## Three Sampling Methods Setup

### **1. Simple Random Sampling**

```python
# Get 25% of the population using simple random sampling
attrition_srs = attrition_pop.sample(frac=0.25, random_state=2022)
```

**Characteristics:**

- Every employee has equal probability of selection
- No consideration of subgroups (RelationshipSatisfaction levels)
- Sample composition depends on chance

### **2. Stratified Sampling**

```python
# (Implementation would sample from each RelationshipSatisfaction level)
# attrition_strat = attrition_pop.groupby("RelationshipSatisfaction").sample(frac=0.25, random_state=2022)
```

**Characteristics:**

- Ensures representation from each satisfaction level
- Maintains population proportions of each subgroup
- Guarantees no satisfaction level is missed

### **3. Cluster Sampling**

```python
# (Implementation would randomly select some satisfaction levels, then sample within those)
# Example: Select 2 satisfaction levels, then sample extensively within those
```

**Characteristics:**

- Randomly selects which satisfaction levels to include
- May miss some satisfaction levels entirely
- Focuses intensively on selected subgroups

## Comparing Point Estimates

### **Population Parameter (True Value)**

```python
# Calculate the true population parameter
mean_attrition_pop = attrition_pop.groupby("RelationshipSatisfaction")["Attrition"].mean()
print(mean_attrition_pop)
```

**What This Shows:**

- Proportion of employees who left (Attrition = 1) for each satisfaction level
- This is the "true" value we want our samples to estimate
- All sampling methods should be evaluated against this benchmark

### **Sample Estimates Comparison**

```python
# Compare estimates from each sampling method
mean_attrition_srs = attrition_srs.groupby("RelationshipSatisfaction")["Attrition"].mean()
mean_attrition_strat = attrition_strat.groupby("RelationshipSatisfaction")["Attrition"].mean()
mean_attrition_clust = attrition_clust.groupby("RelationshipSatisfaction")["Attrition"].mean()

# Print comparisons
print("Population:", mean_attrition_pop)
print("Simple Random:", mean_attrition_srs)
print("Stratified:", mean_attrition_strat)
print("Cluster:", mean_attrition_clust)
```

## Expected Performance of Each Method

### **Simple Random Sampling**

**Expected Results:**

- Should provide **unbiased estimates** on average
- May have **higher variability** for subgroups
- **Small subgroups** might be underrepresented by chance
- Overall good performance with sufficient sample size

**Potential Issues:**

- Rare satisfaction levels might have very few observations
- Estimates for small groups could be unreliable
- More variable results across repeated samples

### **Stratified Sampling**

**Expected Results:**

- Should provide the **most accurate estimates** for subgroups
- **Guaranteed representation** of all satisfaction levels
- **Lower variability** in subgroup estimates
- Each subgroup gets adequate sample size

**Advantages:**

- Most reliable for comparing satisfaction levels
- Reduces sampling error for subgroup analysis
- Ensures no satisfaction level is missed

### **Cluster Sampling**

**Expected Results:**

- May **miss some satisfaction levels** entirely
- **Very accurate** for included satisfaction levels
- **Cannot estimate** parameters for excluded groups
- Most efficient if satisfaction levels are geographically clustered

**Limitations:**

- Incomplete coverage of all subgroups
- May provide biased overall population estimates
- Good for cost reduction, poor for comprehensive analysis

## Key Metrics to Compare

### **1. Bias**

How close are sample estimates to population values?

```python
# Calculate bias for each method
bias_srs = mean_attrition_srs - mean_attrition_pop
bias_strat = mean_attrition_strat - mean_attrition_pop
bias_clust = mean_attrition_clust - mean_attrition_pop
```

### **2. Coverage**

Does the sample include all subgroups?

```python
# Check which satisfaction levels are represented
print("Population groups:", mean_attrition_pop.index.tolist())
print("SRS groups:", mean_attrition_srs.index.tolist())
print("Stratified groups:", mean_attrition_strat.index.tolist())
print("Cluster groups:", mean_attrition_clust.index.tolist())
```

### **3. Precision**

How consistent are the estimates? (Would require multiple samples to assess)

## Practical Interpretation

### **RelationshipSatisfaction Analysis Context**

- **Business Question**: Does employee satisfaction affect attrition rates?
- **Key Insight**: Compare attrition rates across satisfaction levels
- **Expected Pattern**: Higher satisfaction → Lower attrition

### **Method Suitability Assessment**

| Method            | Best For                     | Limitations                  |
| ----------------- | ---------------------------- | ---------------------------- |
| **Simple Random** | General population estimates | May miss rare subgroups      |
| **Stratified**    | Subgroup comparisons         | More complex implementation  |
| **Cluster**       | Cost-effective sampling      | Incomplete subgroup coverage |

## Best Practices for Method Comparison

### **1. Always Start with Population Parameter**

- Calculate the true value you're trying to estimate
- This becomes your benchmark for evaluating sampling performance

### **2. Use Consistent Random Seeds**

- Same `random_state` values ensure fair comparison
- Different seeds would introduce additional variability

### **3. Consider Your Analysis Goals**

- **Population estimates**: Simple random sampling often sufficient
- **Subgroup analysis**: Stratified sampling usually superior
- **Cost constraints**: Cluster sampling when logistics matter

### **4. Check Sample Sizes**

```python
# Verify adequate sample sizes for reliable estimates
print("Sample sizes by satisfaction level:")
print("SRS:", attrition_srs.groupby("RelationshipSatisfaction").size())
print("Stratified:", attrition_strat.groupby("RelationshipSatisfaction").size())
print("Cluster:", attrition_clust.groupby("RelationshipSatisfaction").size())
```

## Key Takeaways

1. **Different sampling methods can yield different estimates** for the same population
2. **Stratified sampling typically provides the most reliable subgroup estimates**
3. **Simple random sampling is unbiased but may miss small subgroups**
4. **Cluster sampling trades completeness for efficiency**
5. **Always compare sample estimates to known population parameters** when possible
6. **The "best" method depends on your specific analysis goals and constraints**
