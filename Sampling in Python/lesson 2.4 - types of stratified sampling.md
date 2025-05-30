# Revision Notes: Proportional Stratified Sampling Implementation

## Learning Objective

Master the implementation of proportional stratified sampling to maintain population subgroup proportions in your sample, and learn to verify that proportions are preserved.

## Key Concepts

### **What is Proportional Stratified Sampling?**

A sampling method that:

- Maintains the same proportion of each subgroup in the sample as exists in the population
- Is equivalent to performing simple random sampling on each subgroup separately
- Ensures your sample is a "mini version" of the population

### **Why Use Proportional Stratified Sampling?**

- **Representative samples**: Sample reflects real-world distribution
- **Unbiased estimates**: Population parameters can be estimated accurately
- **Natural weighting**: Larger groups have more influence, smaller groups have appropriate influence

## Implementation Steps

### **Step 1: Examine Population Proportions**

```python
# Check the distribution in the population first
education_counts_pop = attrition_pop['Education'].value_counts(normalize=True)
print(education_counts_pop)

# This shows you what proportions you should expect in your sample
```

**Why this matters**: You need to know the target proportions to verify your sampling worked correctly.

### **Step 2: Perform Proportional Stratified Sampling**

```python
# Sample 40% from each Education group
attrition_strat = attrition_pop.groupby("Education").sample(frac=0.4, random_state=2022)
print(attrition_strat)
```

**Key Parameters**:

- `frac=0.4`: Takes 40% from each subgroup (maintains proportions)
- `random_state=2022`: Ensures reproducible results
- `groupby("Education")`: Applies sampling within each education level

### **Step 3: Verify Proportions Are Maintained**

```python
# Calculate proportions in your sample
education_counts_strat = attrition_strat["Education"].value_counts(normalize=True)
print(education_counts_strat)

# Compare with population proportions - they should be identical!
```

## Technical Details

### **Using `frac` vs `n` Parameter**

```python
# Option 1: Sample fraction (maintains proportions automatically)
sample = population.groupby('category').sample(frac=0.3, random_state=123)

# Option 2: Sample fixed number (requires calculation for proportions)
sample = population.groupby('category').sample(n=50, random_state=123)
```

**Best Practice**: Use `frac` for proportional stratified sampling because it automatically maintains proportions.

### **The `normalize=True` Parameter**

```python
# Get counts
counts = data['column'].value_counts()

# Get proportions (percentages as decimals)
proportions = data['column'].value_counts(normalize=True)
```

**Understanding the output**:

- `normalize=False` (default): Raw counts
- `normalize=True`: Proportions that sum to 1.0

## Verification Process

### **What to Check**

1. **Population proportions**: `population.groupby('category').size() / len(population)`
2. **Sample proportions**: `sample.groupby('category').size() / len(sample)`
3. **Comparison**: These should be identical (within rounding error)

### **Example Verification**

```python
# Population proportions
pop_props = attrition_pop['Education'].value_counts(normalize=True).sort_index()

# Sample proportions
sample_props = attrition_strat['Education'].value_counts(normalize=True).sort_index()

# Compare (should be identical)
comparison = pd.DataFrame({
    'Population': pop_props,
    'Sample': sample_props
})
print(comparison)
```

## When Proportions Might Differ

### **Rounding Issues**

- Very small groups might have slight differences due to rounding
- Use larger sample sizes to minimize this effect

### **Implementation Errors**

- Using `n` instead of `frac` can distort proportions
- Forgetting `random_state` makes results non-reproducible
- Wrong grouping variable

## Practical Applications

### **Market Research**

```python
# Maintain customer segment proportions
customer_sample = customers.groupby('segment').sample(frac=0.1, random_state=123)
```

### **Survey Research**

```python
# Preserve demographic distributions
survey_sample = population.groupby('age_group').sample(frac=0.05, random_state=456)
```

### **Quality Control**

```python
# Sample products maintaining production batch proportions
product_sample = production.groupby('batch').sample(frac=0.02, random_state=789)
```

## Advantages of Proportional Stratified Sampling

1. **Natural representation**: Sample mirrors population structure
2. **Unbiased estimates**: Population means, proportions estimated accurately
3. **Easy implementation**: Simple `groupby().sample(frac=X)` pattern
4. **Automatic weighting**: No need for post-sampling adjustments

## Common Mistakes to Avoid

- **Using equal sample sizes**: This creates equal stratified sampling, not proportional
- **Forgetting to verify**: Always check that proportions match
- **Wrong fraction calculation**: Make sure `frac` represents the desired sampling rate
- **Inconsistent random states**: Use same seed for reproducible results

## Best Practices

1. **Always examine population first**: Know what proportions to expect
2. **Use meaningful random states**: Choose memorable seeds for reproducibility
3. **Verify your results**: Compare sample and population proportions
4. **Document your sampling rate**: Record what fraction you used for future reference
