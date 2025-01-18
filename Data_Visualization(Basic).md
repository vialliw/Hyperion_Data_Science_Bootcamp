### 1. Box plot for the Revs Per Mile for the Audi, Hyundai, Suzuki, and Toyota car manufacturers.  


```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load data
df = pd.read_csv('Cars93.csv')
df1 = df[df['Manufacturer'].isin(['Audi', 'Hyundai', 'Suzuki', 'Toyota'])]

# Create a boxplot with filled colors
plt.figure(figsize=(12, 6))
box = df1.boxplot(column='Rev.per.mile', by='Manufacturer', patch_artist=True)

plt.title('Boxplot of Rev Per Mile by Manufacturer')
plt.suptitle('')  # Remove the default title to avoid overlap
plt.xlabel('Manufacturer')
plt.ylabel('Rev.per.mile')
plt.show()

```


    <Figure size 1200x600 with 0 Axes>



    
![output_1_1](https://github.com/vialliw/Hyperion_Data_Science_Bootcamp/blob/main/image/output_1_1.png?raw=true)    


### The boxplot indicates that Toyota achieves the highest revolutions per mile. 
--- 

### 2. Histograms of MPG in the city and MPG on the highway.  


```python
# Create histograms
plt.figure(figsize=(12, 6))
# Create histograms on the same axis
plt.figure(figsize=(12, 6))

plt.hist(df['MPG.highway'], bins=10, color='blue', alpha=0.7, label='MPG.highway')
plt.hist(df['MPG.city'], bins=10, color='orange', alpha=0.7, label='MPG.city')

plt.title('Histograms of MPG.highway and MPG.city')
plt.xlabel('Value')
plt.ylabel('Frequency')
plt.legend()

plt.show()


```


    <Figure size 1200x600 with 0 Axes>




![output_4_1](https://github.com/vialliw/Hyperion_Data_Science_Bootcamp/blob/main/image/output_4_1.png?raw=true)    


### The histograms indicate that MPG Highway (blue) typically demonstrates greater fuel efficiency compared to MPG City (orange).
--- 

### 3. Line plot of Wheelbase vs Turning Circle 


```python
# Create a line plot
df = df.sort_values('Wheelbase')
plt.figure(figsize=(10, 6))
plt.plot(df['Wheelbase'], df['Turn.circle'], marker='o', linestyle='-')
plt.title('Turn.circle vs Wheelbase')
plt.xlabel('Wheelbase')
plt.ylabel('Turn.circle')
plt.grid(True)
plt.show()

```


    
![output_7_0](https://github.com/vialliw/Hyperion_Data_Science_Bootcamp/blob/main/image/output_7_0.png?raw=true)    


### The line plot illustrates that the turning circle is directly related to the wheelbase. 
--- 

### 4. Bar Chart of average horsepower for different car types.


```python
# Load your data
df = pd.read_csv('Cars93.csv')

# Group by car type and calculate the average horsepower
average_horsepower = df.groupby('Type')['Horsepower'].mean()

# Create a bar chart
plt.figure(figsize=(12, 6))
bars = plt.bar(average_horsepower.index, average_horsepower, color='skyblue')

# Add values on the bars
for bar in bars:
    yval = bar.get_height()
    plt.text(bar.get_x() + bar.get_width()/2, yval, round(yval, 2), ha='center', va='bottom')

plt.title('Average Horsepower by Car Type')
plt.xlabel('Car Type')
plt.ylabel('Average Horsepower')
plt.xticks(rotation=45)
plt.grid(axis='y', linestyle='--', linewidth=0.7)
plt.show()

```



![output_10_0](https://github.com/vialliw/Hyperion_Data_Science_Bootcamp/blob/main/image/output_10_0.png?raw=true)



### The bar chart shows large car type has the highest average horsepower (179.45).


```python

```
