# Revision Notes: Exact vs. Approximate Sampling Distributions

## Learning Objectives

1. Understand the difference between exact and approximate sampling distributions
2. Learn when computational complexity makes exact calculations impractical
3. Master simulation techniques for approximating sampling distributions

## Key Concepts

### **What is a Sampling Distribution?**

A sampling distribution shows:

- **All possible values** a sample statistic can take
- **How often** each value occurs
- **The pattern of variability** inherent in sampling

### **Two Types of Sampling Distributions**

#### **Exact Sampling Distribution**

- Calculates **every possible** sample and its statistic
- Shows the **true** distribution with complete accuracy
- Requires **manageable** number of possible outcomes

#### **Approximate Sampling Distribution**

- Uses **simulation** to estimate the distribution
- Repeats the sampling process many times
- Approaches the true distribution as simulations increase

## Exact Sampling Distribution Implementation

### **Step 1: Generate All Possible Outcomes**

```python
# Create all possible combinations of five 8-sided dice
dice = expand_grid({
    'die1': [1, 2, 3, 4, 5, 6, 7, 8],
    'die2': [1, 2, 3, 4, 5, 6, 7, 8],
    'die3': [1, 2, 3, 4, 5, 6, 7, 8],
    'die4': [1, 2, 3, 4, 5, 6, 7, 8],
    'die5': [1, 2, 3, 4, 5, 6, 7, 8]
})
print(dice)
```

**Result**: All 8^5 = 32,768 possible combinations

### **Step 2: Calculate Sample Statistic for Each**

```python
# Calculate mean for each possible combination
dice['mean_roll'] = (dice['die1'] + dice['die2'] +
                     dice['die3'] + dice['die4'] +
                     dice['die5']) / 5

# Convert to categorical for proper ordering
dice['mean_roll'] = dice['mean_roll'].astype('category')
print(dice)
```

### **Step 3: Visualize the Exact Distribution**

```python
# Plot the exact sampling distribution
dice["mean_roll"].value_counts(sort=False).plot(kind="bar")
plt.show()
```

**What This Shows**: The true, complete distribution of all possible sample means

## Approximate Sampling Distribution Implementation

### **Step 1: Simulate One Sample**

```python
# Sample five dice rolls with replacement
five_rolls = np.random.choice(list(range(1, 9)), size=5, replace=True)
print(five_rolls.mean())
```

### **Step 2: Repeat Many Times**

```python
# Replicate the sampling process 1000 times
sample_means_1000 = []
for i in range(1000):
    rolls = np.random.choice(list(range(1, 9)), size=5, replace=True)
    sample_means_1000.append(rolls.mean())

print(sample_means_1000[0:10])
```

### **Step 3: Visualize the Approximate Distribution**

```python
# Plot the approximate sampling distribution
plt.hist(sample_means_1000, bins=20)
plt.show()
```

**What This Shows**: An estimate of the sampling distribution based on simulation

## When to Use Each Method

### **Use Exact Sampling Distribution When:**

- **Small number of outcomes**: Computationally feasible
- **Simple scenarios**: Like dice rolls, coin flips
- **Complete accuracy needed**: Want the true distribution
- **Educational purposes**: Understanding theoretical concepts

**Example Scenarios:**

- Rolling 2-3 dice (8^2 = 64, 8^3 = 512 outcomes)
- Sampling from small populations (n < 100)
- Binary outcomes with small samples

### **Use Approximate Sampling Distribution When:**

- **Large number of outcomes**: Exact calculation impossible
- **Complex populations**: Real-world datasets
- **Computational constraints**: Limited time/resources
- **Practical applications**: Most real-world scenarios

**Example Scenarios:**

- Sampling from employee datasets (thousands of employees)
- Customer satisfaction surveys (hundreds of responses)
- Medical studies (complex measurements)

## Computational Complexity

### **Why Exact Becomes Impractical**

```python
# Number of possible outcomes grows exponentially
dice_2 = 8**2      # 64 outcomes - feasible
dice_5 = 8**5      # 32,768 outcomes - manageable
dice_10 = 8**10    # 1+ billion outcomes - problematic
dice_20 = 8**20    # 1+ quintillion outcomes - impossible
```

### **Real-World Example**

Sampling 50 employees from 1000-person company:

- Number of possible samples = C(1000, 50)
- This equals approximately 10^126 possible combinations
- **Impossible to calculate exactly**

## Answer to the Question

**Correct Answer: "No, the computational time and resources needed to look at the population of values could be too much for our problem."**

### **Why This is Correct**

- **Exponential growth**: Number of outcomes increases exponentially with complexity
- **Resource limitations**: Computing power and time are finite
- **Practical constraints**: Real problems involve large, complex datasets
- **Diminishing returns**: Exact calculation may be unnecessarily precise

### **Why Other Options Are Wrong**

**"Exact distribution is always unknown for small die tosses"**

- ❌ False: We can calculate exact distributions for small cases
- The exercise shows exact calculation for 5 dice is possible

**"Population always known, so one extra calculation is no problem"**

- ❌ False: Knowing the population doesn't make calculation trivial
- Number of possible samples can be astronomically large

**"For loops can generate exact distribution in all circumstances"**

- ❌ False: For loops don't solve computational complexity
- Some problems have too many combinations to enumerate

## Comparison of Methods

| Aspect            | Exact Distribution     | Approximate Distribution        |
| ----------------- | ---------------------- | ------------------------------- |
| **Accuracy**      | Perfect                | Very good with many simulations |
| **Computation**   | Can be prohibitive     | Always manageable               |
| **Use cases**     | Simple, small problems | Complex, real-world problems    |
| **Time required** | May be infinite        | Controllable                    |
| **Resources**     | May exceed capacity    | Fits available resources        |

## Best Practices

### **For Exact Distributions:**

- **Calculate complexity first**: Estimate number of possible outcomes
- **Set reasonable limits**: If > 1 million outcomes, consider approximation
- **Use for verification**: Check approximate methods on simple cases

### **For Approximate Distributions:**

- **Use sufficient simulations**: 1000+ for basic understanding, 10000+ for precision
- **Check convergence**: More simulations should give similar results
- **Set random seeds**: Ensure reproducible results
- **Validate when possible**: Compare to exact distributions for simple cases

### **Choosing Simulation Size:**

```python
# Quick exploration: 100-1000 simulations
# Standard analysis: 1000-10000 simulations
# High precision: 10000+ simulations
# Research/publication: 100000+ simulations
```

## Practical Applications

### **Exact Distribution Examples:**

- Quality control with small batches
- Clinical trials with few participants
- Educational examples and theoretical understanding

### **Approximate Distribution Examples:**

- Market research surveys
- Employee satisfaction studies
- Medical research with large populations
- A/B testing in technology
- Financial risk assessment

## Key Takeaways

1. **Exact distributions are theoretically perfect** but computationally limited
2. **Approximate distributions are practical** for real-world problems
3. **Computational complexity grows exponentially** with problem size
4. **Simulation provides excellent approximations** with sufficient replications
5. **Choose method based on computational feasibility**, not just theoretical preference
6. **Most real-world applications require approximation** due to complexity
