# Revision Notes: Cluster Sampling

## Learning Objective
Understand cluster sampling as a two-stage technique and identify when it's preferable to stratified sampling, particularly for cost and logistical considerations.

## Key Concepts

### **What is Cluster Sampling?**
Cluster sampling is a **two-stage sampling technique**:
1. **Stage 1**: Randomly sample which subgroups (clusters) to include
2. **Stage 2**: Randomly sample rows within each selected subgroup

### **How It Differs from Stratified Sampling**

| Aspect | Stratified Sampling | Cluster Sampling |
|--------|-------------------|------------------|
| **Subgroup selection** | Sample from ALL subgroups | Sample from SOME subgroups |
| **Focus** | Ensure representation of each group | Reduce costs and logistical complexity |
| **When groups are missed** | Never (all groups included) | Some groups excluded entirely |

## When to Use Cluster Sampling

### **Cluster Sampling is Preferable When:**
**Cost and logistics are major concerns**, especially when:
- **Geographic dispersion**: Groups are spread across different locations
- **Travel costs**: Moving between groups is expensive or time-consuming
- **Resource limitations**: Can't afford to sample from every subgroup
- **Accessibility**: Some groups are easier to reach than others

### **Example Scenario:**
"Collecting an overall sample requires lots of travel from one group to another to collect samples within each group."

**Why this favors cluster sampling:**
- Instead of visiting ALL geographic regions (expensive)
- Randomly select a FEW regions, then thoroughly sample within those regions
- Significantly reduces travel time and costs

### **When NOT to Use Cluster Sampling:**

1. **Ensuring rare group representation**: 
   - Cluster sampling might exclude rare groups entirely
   - Stratified sampling guarantees all groups are included

2. **Unlimited resources**:
   - If cost/time aren't limitations, stratified sampling is usually better
   - More precise representation of all subgroups

3. **Comparing specific subgroups**:
   - Need all subgroups present for comparison
   - Cluster sampling might miss important groups

## Practical Examples

### **Good for Cluster Sampling:**
- **School surveys across districts**: Sample few districts completely rather than visiting all
- **Rural health studies**: Sample few villages thoroughly rather than visiting all villages
- **Market research across cities**: Focus on selected cities rather than nationwide sampling

### **Bad for Cluster Sampling:**
- **Medical trials by condition**: Need all rare conditions represented
- **Demographic studies**: Must include all ethnic/age groups
- **A/B testing**: Need controlled representation of user types

## Implementation Concept

```python
# Conceptual cluster sampling approach:

# Stage 1: Randomly select clusters
selected_clusters = all_clusters.sample(n=5, random_state=123)

# Stage 2: Sample within selected clusters
cluster_sample = selected_clusters.groupby('cluster_id').apply(
    lambda x: x.sample(n=30, random_state=456)
)
```

## Key Decision Factors

### **Choose Cluster Sampling When:**
- ✅ **Cost/logistics are primary concerns**
- ✅ **Groups are geographically dispersed**
- ✅ **Some missing subgroups are acceptable**
- ✅ **Need efficient data collection**

### **Choose Stratified Sampling When:**
- ✅ **All subgroups must be represented**
- ✅ **Comparing subgroups is the goal**
- ✅ **Resources allow comprehensive sampling**
- ✅ **Precision is more important than cost**

## Answer to the Question
**Correct Answer: Option 3**
"Collecting an overall sample requires lots of travel from one group to another to collect samples within each group."

This scenario highlights the main advantage of cluster sampling: **reducing logistical costs and complexity** when groups are geographically or practically dispersed.