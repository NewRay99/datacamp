# Revision Notes: Implementing Cluster Sampling

## Learning Objective

Master the step-by-step implementation of cluster sampling using the two-stage process: selecting clusters, then sampling within clusters.

## Implementation Steps

### **Stage 1: Select Clusters Randomly**

```python
import random

# Step 1: Get all unique cluster identifiers
job_roles_pop = list(attrition_pop["JobRole"].unique())

# Step 2: Randomly sample a subset of clusters
job_roles_samp = random.sample(job_roles_pop, k=4)
print(job_roles_samp)

# This gives you 4 randomly selected job roles out of all available roles
```

**Key Points:**

- Use `random.sample()` for sampling without replacement
- Parameter `k` specifies how many clusters to select
- Setting `random.seed()` ensures reproducible results

### **Stage 2: Filter and Sample Within Clusters**

```python
# Step 3: Filter data to include only selected clusters
jobrole_condition = attrition_pop["JobRole"].isin(job_roles_samp)
attrition_filtered = attrition_pop[jobrole_condition]
print(attrition_filtered)

# Step 4: Clean up categorical data (remove unused categories)
attrition_filtered['JobRole'] = attrition_filtered["JobRole"].cat.remove_unused_categories()

# Step 5: Sample within each selected cluster
attrition_clust = attrition_filtered.groupby("JobRole").sample(n=10, random_state=2022)
print(attrition_clust)
```

## Technical Details

### **Key Methods Used**

1. **`list(dataframe["column"].unique())`**: Gets all unique values in a column
2. **`random.sample(population, k)`**: Randomly selects k items without replacement
3. **`dataframe["column"].isin(list)`**: Boolean mask for filtering rows
4. **`cat.remove_unused_categories()`**: Cleans up categorical data after filtering
5. **`groupby().sample()`**: Samples within each group

### **Important Notes**

**Random Package vs. Pandas:**

- Use `random.sample()` for selecting clusters (works with lists)
- Use `pandas.sample()` for sampling rows within clusters (works with DataFrames)

**Category Management:**

```python
# After filtering, remove unused categories to avoid issues
filtered_data['categorical_column'] = filtered_data['categorical_column'].cat.remove_unused_categories()
```

**Two Different Random Seeds:**

- `random.seed()` for cluster selection stage
- `random_state` parameter for within-cluster sampling

## Complete Implementation Pattern

```python
import random
import pandas as pd

# Set seed for reproducibility
random.seed(19790801)

# STAGE 1: Select clusters
clusters_population = list(data["cluster_column"].unique())
selected_clusters = random.sample(clusters_population, k=number_of_clusters)

# STAGE 2: Sample within selected clusters
# Filter to selected clusters only
cluster_condition = data["cluster_column"].isin(selected_clusters)
filtered_data = data[cluster_condition]

# Clean up categories
filtered_data["cluster_column"] = filtered_data["cluster_column"].cat.remove_unused_categories()

# Sample within each cluster
final_sample = filtered_data.groupby("cluster_column").sample(n=samples_per_cluster, random_state=seed)
```

## Why This Two-Stage Approach?

### **Stage 1 Benefits:**

- **Reduces scope**: Focus on subset of clusters instead of all
- **Cost efficiency**: Avoid visiting/accessing all possible groups
- **Manageable logistics**: Work with fewer, selected locations/categories

### **Stage 2 Benefits:**

- **Representative sampling**: Get unbiased sample from each selected cluster
- **Controlled sample size**: Ensure adequate representation from each chosen cluster
- **Statistical validity**: Maintain randomness within selected groups

## Real-World Applications

### **Job Role Analysis (Example from Exercise):**

- **Stage 1**: Select 4 job roles out of all available roles
- **Stage 2**: Get 10 employees from each selected role
- **Result**: 40 employees representing 4 job functions

### **Other Examples:**

- **Geographic studies**: Select few regions, then sample households within those regions
- **School research**: Choose subset of schools, then sample students within chosen schools
- **Market research**: Pick few store locations, then survey customers at those stores

## Advantages of This Approach

1. **Cost-effective**: Reduce travel/logistics by limiting locations
2. **Practical**: More feasible than visiting every possible cluster
3. **Still representative**: Maintains randomness at both stages
4. **Scalable**: Works for any number of clusters and sample sizes

## Common Pitfalls

- **Forgetting to remove unused categories**: Can cause errors in subsequent analysis
- **Mixed up random seeds**: Different stages need different random number generators
- **Insufficient cluster selection**: Too few clusters may miss important variation
- **Unequal cluster sizes**: Some selected clusters might be very small
