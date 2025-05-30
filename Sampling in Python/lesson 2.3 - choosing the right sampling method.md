# Revision Notes: Choosing the Right Sampling Method

## Learning Objective

Understand when to use different sampling methods (stratified vs. simple random sampling) based on your research goals and population characteristics.

## Key Decision Framework

### **When to Use Stratified Sampling**

Use stratified sampling when you need to ensure representation of specific subgroups or want to analyze differences between groups.

**Ideal Scenarios:**

- **Blood types for vaccine testing**: Ensure all blood types are represented to test effectiveness across different subgroups
- **Demographics for census**: Match population proportions by race to ensure proper representation
- **Income groups for tax analysis**: Understand how policy changes affect different economic strata
- **Any time subgroup analysis matters**: When you need guaranteed representation of important categories

### **When to Use Simple Random Sampling**

Use simple random sampling when you want a representative sample of the overall population without focusing on specific subgroups.

**Ideal Scenarios:**

- **General population studies**: When you're not particularly interested in subgroup properties
- **Product quality analysis**: Looking at Skittles candy color distribution where all colors appear roughly equally (20% each)
- **Overall trend analysis**: When you want to understand general patterns rather than group differences
- **Exploratory research**: Initial investigations where subgroup structure isn't yet clear

## Key Distinguishing Questions

### **Ask Yourself:**

1. **Do I need to analyze specific subgroups separately?**

   - Yes → Stratified sampling
   - No → Simple random sampling

2. **Are there important categories that might be underrepresented?**

   - Yes → Stratified sampling
   - No → Simple random sampling

3. **Is my main goal overall population estimates or subgroup comparisons?**
   - Overall estimates → Simple random sampling
   - Subgroup comparisons → Stratified sampling

## Practical Examples

### **Stratified Sampling Examples:**

```python
# Medical research: Ensure representation by blood type
sample = population.groupby('blood_type').apply(lambda x: x.sample(n=50))

# Survey research: Match census demographics
sample = population.groupby('race').apply(lambda x: x.sample(frac=0.1))

# Economic analysis: Represent all income brackets
sample = population.groupby('income_bracket').apply(lambda x: x.sample(n=100))
```

### **Simple Random Sampling Examples:**

```python
# General population survey
sample = population.sample(n=1000, random_state=123)

# Product quality check (when subgroups don't matter)
sample = candy_production.sample(frac=0.05, random_state=456)

# Overall trend analysis
sample = customer_data.sample(n=500, random_state=789)
```

## Summary Table

| Factor             | Stratified Sampling                | Simple Random Sampling         |
| ------------------ | ---------------------------------- | ------------------------------ |
| **Purpose**        | Subgroup analysis & representation | Overall population estimates   |
| **Subgroups**      | Important to study separately      | Not the focus                  |
| **Representation** | Guarantees subgroup presence       | May miss small groups          |
| **Complexity**     | More complex implementation        | Simple implementation          |
| **Use when**       | Groups matter for analysis         | Groups don't matter or unknown |

## Best Practices

- **Start with your research question**: What do you want to learn?
- **Consider your population**: Are there important subgroups?
- **Think about analysis goals**: Will you compare groups or focus on overall patterns?
- **When in doubt**: Simple random sampling is often a safe default choice
