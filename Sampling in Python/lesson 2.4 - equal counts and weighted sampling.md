# Revision Notes: Equal Counts and Weighted Sampling

## Learning Objectives

1. Master equal counts stratified sampling to ensure equal statistical power across subgroups
2. Understand weighted sampling for custom probability selection at the row level
3. Learn to interpret the effects of weighted sampling on sample characteristics

## Equal Counts Stratified Sampling

### **What is Equal Counts Stratified Sampling?**

A sampling method that:

- Gives each subgroup the same sample size, regardless of population size
- Equalizes statistical power across subgroups for analysis
- Does NOT maintain population proportions

### **When to Use Equal Counts**

- **Subgroup comparisons**: When you want equal ability to analyze each group
- **Rare group analysis**: Ensure sufficient sample from small subgroups
- **Statistical power**: Equal sample sizes provide equal precision for group estimates

### **Implementation**

```python
# Get equal sample sizes from each Education group
attrition_eq = attrition_pop.groupby("Education").sample(n=30, random_state=2022)
print(attrition_eq)

# Check the resulting proportions (will NOT match population)
education_counts_eq = attrition_eq["Education"].value_counts(normalize=True)
print(education_counts_eq)
```

**Key Points**:

- Use `n=30` instead of `frac=0.4` to get fixed sample sizes
- Resulting proportions will be equal (not population-representative)
- Each group contributes exactly the same number of observations

### **Example Applications**

- **Blood type analysis**: Equal samples of O, A, B, AB regardless of natural frequency
- **Medical studies**: Equal samples from each condition for fair comparison
- **A/B testing**: Equal sample sizes for each experimental condition

## Weighted Sampling

### **What is Weighted Sampling?**

A generalization of stratified sampling where:

- Each individual row has a specified probability of selection
- Probability is proportional to an assigned weight value
- Higher weights = higher chance of selection

### **Implementation Process**

#### **Step 1: Examine Distribution of Weight Variable**

```python
# Visualize the distribution you'll use for weighting
attrition_pop['YearsAtCompany'].hist(bins=np.arange(0, 41, 1))
plt.show()

# This helps you understand what the weighting will do
```

#### **Step 2: Perform Weighted Sampling**

```python
# Sample with weights proportional to YearsAtCompany
attrition_weight = attrition_pop.sample(n=400, weights="YearsAtCompany")
print(attrition_weight)
```

#### **Step 3: Compare Results**

```python
# Compare weighted sample distribution to population
attrition_weight['YearsAtCompany'].hist(bins=np.arange(0, 41, 1))
plt.show()

# Compare means
print(f"Population mean: {attrition_pop['YearsAtCompany'].mean()}")
print(f"Weighted sample mean: {attrition_weight['YearsAtCompany'].mean()}")
```

## Understanding Weighted Sampling Effects

### **How Weighting Changes Your Sample**

When you weight by a variable (like `YearsAtCompany`):

- **Higher values get selected more often**: Employees with more years have higher selection probability
- **Sample becomes skewed**: Toward higher values of the weighting variable
- **Mean increases**: Weighted sample mean will be higher than population mean

### **Answer to the Question**

**Correct Answer: Sample mean**

**Why?** When weighting by `YearsAtCompany`:

- Employees with more years at company have higher selection probability
- This oversamples long-tenure employees
- Therefore, weighted sample mean > population mean

## Comparison of Sampling Methods

| Method                      | Sample Composition             | Use Case                           |
| --------------------------- | ------------------------------ | ---------------------------------- |
| **Proportional Stratified** | Matches population proportions | Representative population analysis |
| **Equal Counts Stratified** | Equal sizes per subgroup       | Subgroup comparisons               |
| **Weighted**                | Biased toward higher weights   | Emphasize certain observations     |

## Technical Details

### **Key Parameters**

```python
# Equal counts stratified
sample = data.groupby('category').sample(n=fixed_number, random_state=123)

# Weighted sampling
sample = data.sample(n=sample_size, weights='weight_column', random_state=123)
```

### **Weight Interpretation**

- **Weights are relative**: A weight of 10 is twice as likely as weight of 5
- **Zero weights**: Rows with weight 0 are never selected
- **Missing weights**: Rows with NaN weights are excluded

## Practical Applications

### **Equal Counts Examples**

```python
# Clinical trials: Equal patients per treatment group
trial_sample = patients.groupby('treatment').sample(n=100, random_state=123)

# Survey research: Equal responses per demographic
survey_sample = population.groupby('age_group').sample(n=50, random_state=456)
```

### **Weighted Sampling Examples**

```python
# Market research: Weight by purchase amount
customer_sample = customers.sample(n=500, weights='annual_spending', random_state=123)

# Quality control: Weight by production volume
product_sample = products.sample(n=200, weights='batch_size', random_state=456)
```

## When to Use Each Method

### **Use Equal Counts When:**

- Comparing subgroups is your primary goal
- Small subgroups would be inadequately represented
- You need equal statistical power for each group
- Population proportions aren't important for your analysis

### **Use Weighted Sampling When:**

- Some observations are more important than others
- You want to oversample specific types of cases
- Correcting for known sampling biases
- Following up on particular patterns or outliers

## Best Practices

1. **Visualize first**: Always plot distributions before and after sampling
2. **Understand the bias**: Know how your sampling method will change the data
3. **Document your reasoning**: Explain why you chose specific weights or equal counts
4. **Check your assumptions**: Verify that the sampling achieved your intended goal
