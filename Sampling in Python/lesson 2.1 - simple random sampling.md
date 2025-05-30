# Revision Notes: Simple Random Sampling (SRS)

## Learning Objective

Understand simple random sampling as the fundamental method for selecting representative samples where each row has an equal chance of being picked.

## Key Concepts

### **What is Simple Random Sampling?**

Simple random sampling (SRS) is the most basic sampling method where:

- Each row has the same chance of being selected as any other
- Rows are picked one at a time randomly
- No systematic bias in selection process

### **Implementation**

```python
# Basic syntax for simple random sampling
sample = population.sample(n=sample_size, random_state=seed_value)

# Example from exercise: Employee attrition dataset
attrition_samp = attrition_pop.sample(n=70, random_state=18900217)
print(attrition_samp)
```

## Technical Details

### **Key Parameters**

- `n`: Number of rows to sample (70 in the example)
- `random_state`: Seed for reproducible results (18900217 in the example)

### **Why Use random_state?**

Setting a seed ensures you get the same sample every time you run the code, making your analysis reproducible.

## Context

This method is often applied to real-world datasets like the IBM employee attrition dataset, where "attrition" means employees leaving the company. SRS helps create unbiased samples for analysis.

## Why SRS Matters

- **Eliminates bias**: No human judgment in selection
- **Representative**: Sample should reflect population characteristics
- **Statistical foundation**: Enables valid statistical inference
- **Simple to implement**: Easy to use with pandas `.sample()` method
