# Master Feature Engineering: Polynomial Transformation for Normal Distribution

Feature engineering is crucial in machine learning, especially when dealing with skewed or non-normal distributions. Many models, such as linear regression, assume normally distributed features for optimal performance. In this guide, we explore **polynomial transformation** as a technique to reshape feature distributions to approximate normality. We'll demonstrate this with the **California Housing dataset** using Python.

## Why Normalize Features?
Machine learning models perform better when features follow a normal distribution, as it improves interpretability, optimization, and prediction accuracy. Some benefits include:
- **Improved model performance**: Many algorithms assume normality.
- **Better convergence**: Gradient-based optimizations work efficiently.
- **Enhanced interpretability**: Normalized features make models easier to analyze.

## Polynomial Transformation Explained
Polynomial transformation involves generating higher-order terms (squared, cubic, etc.) of a feature to capture non-linear relationships. It can also help in normalizing skewed distributions.

Mathematically, for a feature \( x \), a polynomial transformation of degree \( d \) generates:
\[ x, x^2, x^3, ..., x^d \]

## Implementation with California Housing Dataset
We'll use `pandas`, `numpy`, `sklearn`, and `matplotlib` to demonstrate how polynomial transformations can help achieve a normal distribution.

### Step 1: Load Dependencies
```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.datasets import fetch_california_housing
from sklearn.preprocessing import PolynomialFeatures
from scipy.stats import skew
```

### Step 2: Load and Inspect the Dataset
```python
data = fetch_california_housing()
df = pd.DataFrame(data.data, columns=data.feature_names)
```
The dataset consists of housing features such as median income (`MedInc`), house age, and population.

### Step 3: Visualize Feature Distribution
```python
feature = 'MedInc'
plt.figure(figsize=(6,4))
plt.hist(df[feature], bins=50, edgecolor='k', alpha=0.7)
plt.title(f'Original Distribution of {feature}')
plt.show()
```
This often reveals a **right-skewed** distribution.

### Step 4: Apply Polynomial Transformation
We apply a polynomial transformation to the `MedInc` feature.
```python
poly = PolynomialFeatures(degree=2, include_bias=False)
df_poly = poly.fit_transform(df[[feature]])
df_poly = pd.DataFrame(df_poly, columns=[feature, f'{feature}^2'])
```

### Step 5: Assess Normality
```python
plt.figure(figsize=(12,5))
plt.subplot(1, 2, 1)
plt.hist(df_poly[feature], bins=50, edgecolor='k', alpha=0.7)
plt.title(f'Histogram of {feature}')

plt.subplot(1, 2, 2)
plt.hist(df_poly[f'{feature}^2'], bins=50, edgecolor='k', alpha=0.7)
plt.title(f'Histogram of {feature}^2')
plt.show()
```
You should observe that **squaring** the feature reduces skewness.

### Step 6: Validate Skewness Reduction
```python
original_skew = skew(df[feature])
transformed_skew = skew(df_poly[f'{feature}^2'])

print(f'Original Skewness: {original_skew:.2f}')
print(f'Transformed Skewness: {transformed_skew:.2f}')
```
A lower skewness value confirms improved normality.

## Conclusion
Polynomial transformation is a powerful technique for normalizing features and capturing non-linear relationships. Applying it to skewed data can significantly enhance model performance, particularly in regression tasks.

Experiment with different polynomial degrees and transformations to find the best fit for your dataset!


