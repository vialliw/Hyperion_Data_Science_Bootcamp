# Supervised Learning: Linear Regression with Insurance Data

Welcome to this blog post where we'll explore the power of supervised learning using Linear Regression. We'll use a real-world dataset from Kaggle to predict insurance charges based on various customer attributes. Let's dive into the process step-by-step!

## Dataset Overview

We'll be using the "Insurance" dataset available on Kaggle, which contains information about different insurance policies. The dataset includes features like age, gender, BMI, smoking status, and the actual insurance charges. You can access the dataset here: [Insurance Dataset](https://www.kaggle.com/datasets/mirichoi0218/insurance).

## Importing Libraries and Data

First, we need to import the necessary libraries and load the dataset into our Jupyter Notebook.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error
```

Now, let's load the dataset:

```python
url = "https://www.kaggle.com/datasets/mirichoi0218/insurance/raw/insurance.csv"
data = pd.read_csv(url)
```

## Data Exploration and Visualization

Let's take a quick look at the dataset and visualize the relationship between age and insurance charges.

```python
# Display the first few rows of the dataset
data.head()

# Scatter plot of age vs. charges
plt.scatter(data['age'], data['charges'])
plt.xlabel('Age')
plt.ylabel('Charges')
plt.title('Age vs. Insurance Charges')
plt.show()
```

The scatter plot shows a clear positive relationship between age and insurance charges, indicating that older individuals might have higher insurance costs.

## Data Preprocessing and Splitting

Before training the model, we need to preprocess the data and split it into training and testing sets.

```python
# Drop rows with missing values
data.dropna(inplace=True)

# Split the data into features (X) and the target variable (y)
X = data[['age', 'sex', 'bmi', 'children', 'smoker']]
y = data['charges']

# Split the data into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

## Training the Linear Regression Model

Now, we'll use the `LinearRegression` class from scikit-learn to fit our model to the training data.

```python
linear_model = LinearRegression()
linear_model.fit(X_train, y_train)
```

## Making Predictions

With the model trained, we can now make predictions on the test data.

```python
predictions = linear_model.predict(X_test)
```

## Evaluating the Model

Let's assess the model's performance using the Mean Squared Error (MSE) metric.

```python
mse = mean_squared_error(y_test, predictions)
print(f"Mean Squared Error: {mse}")
```

## Visualizing the Best-Fit Line

Finally, we'll plot the best-fit line on the scatter plot to visualize the model's predictions.

```python
# Plot the actual charges vs. predicted charges
plt.scatter(y_test, predictions)
plt.plot([y.min(), y.max()], [y.min(), y.max()], color='red', lw=2)
plt.xlabel('Actual Charges')
plt.ylabel('Predicted Charges')
plt.title('Actual vs. Predicted Charges')
plt.show()
```

This plot shows the scatter of actual charges against predicted charges, with the best-fit line indicating the model's trend.

In conclusion, we've demonstrated how to perform Linear Regression using the Insurance dataset. By following these steps, you can build and evaluate a simple yet powerful model to predict insurance charges. Remember that this is just a starting point, and further exploration and feature engineering can lead to even better results!
