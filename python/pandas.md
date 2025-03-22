**Pandas🎯📊🐼**

Pandas is used for data manipulation and analysis. It offers data structures like Series and DataFrame, which are designed for handling tabular data and provide functionality similar to spreadsheets. Pandas integrates seamlessly with NumPy and offers a variety of tools for data cleaning, aggregation, transformation, and visualization.


### **Table of Contents**
- [Concept 2: Create Series](#concept-2-create-series)
- [Concept 3: Creating DataFrames](#concept-3-creating-dataframes)
- [Concept 4: Loading Data from Files](#concept-4-loading-data-from-files)
- [Concept 5: DataFrame Indexing](#concept-5-dataframe-indexing)
- [Concept 6: DataFrame Slicing](#concept-6-dataframe-slicing)
- [Concept 7: Adding Columns and Rows](#concept-7-adding-columns-and-rows)
- [Concept 8: Filtering, Sorting, and Updating DataFrames](#concept-8-filtering-sorting-and-updating-dataframes)
- [Concept 9: Cleaning Data](#concept-9-cleaning-data)
- [Concept 10: Descriptive Statistics](#concept-10-descriptive-statistics)
- [Concept 11: Grouping and Aggregation](#concept-11-grouping-and-aggregation)
- [Concept 12: Plotting with Pandas](#concept-12-plotting-with-pandas)

---

**Series📌📉📊**

A **Series** is a one-dimensional labeled array capable of holding any data type. It can be thought of as a column in a table or an Excel spreadsheet.

- A Series is an object.
- The index of a Series is a set of labels that correspond to each element in the Series.
- Here, A, B, and C are labels in the index.
- The index itself is `Index(['A', 'B', 'C'], dtype='object')`.

**Key Differences:**

- **Index** refers to the entire set of row identifiers.
- **Label** refers to a single identifier within the index.

**Syntax of Series:**

```python
import pandas as pd

data = [1, 2, 3, 4, 5]
labels = ['A', 'B', 'C', 'D', 'E']

series = pd.Series(data, index=labels)

print(series)
```

---

**DataFrames🏛️📋🔢**

While a **Series** is essentially a one-dimensional labeled array, a **DataFrame** can be thought of as a two-dimensional table with labeled axes.

A **DataFrame** is a collection of **Series**, where each column or row is a **Series**. You can pass a dictionary to `pd.DataFrame` to create a **DataFrame**, where the dictionary keys are column names, and the values are lists of values for the column.

Note:  that all lists used as values must be the same length.

Note using [[]] returns a DataFrame.

**Syntax and Example of DataFrame:**

```python
import pandas as pd

data = {
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35],
    'City': ['New York', 'San Francisco', 'Los Angeles']
}

#index can also be added, index = [a,c,b] etc
df = pd.DataFrame(data)
print(df)

# another syntax
df = pd.DataFrame([[1, 2], [4, 5], [7, 8]],
                  index=['cobra', 'viper', 'sidewinder'],
                  columns=['max_speed', 'shield'])
df

#output
            max_speed  shield
cobra               1       2
viper               4       5
sidewinder          7       8
```

**`head()`** Method 🧐📖🔍:

```python
print(df.head())
```

The `head()` function on the **DataFrame** object returns the first 5 rows of the **DataFrame** by default.

---

**Load Data from Files:📂📝🔄**

```python
import pandas as pd

# Replace 'file.csv' with the actual file path
df = pd.read_csv('file.csv')
print(df.head())
```

---

**DataFrame Indexing  🎯🔢📌**

**Accessing Data Using Labels and Integer Indexing:**

Indexing in Pandas allows for efficient selection, slicing, and manipulation of data within DataFrames. There are two primary methods:

- **Label-based Indexing `loc`: Access data using explicit row and column labels.
- **Integer-based `iloc`: Access data using numerical positions.

These methods enable retrieving entire columns, specific rows, or even individual elements within a DataFrame.

```python
import pandas as pd

data = {
    'name': ['Alice', 'Bob', 'Charlie'],
    'age': [30, 25, 22],
    'city': ['New York', 'San Francisco', 'Los Angeles']
}

df = pd.DataFrame(data)

# Using labels to access columns
print(df['name'])

# Using integer indexing with iloc to access rows
print(df.iloc[1])

# Using a combination of both to access a specific element
print(df['city'].iloc[2])
```

**Selecting Rows with `loc`and `iloc`:**

```python
df = pd.DataFrame({'A': [0, 1, 2], 'B': [3, 4, 5], 'C': [6, 7, 8]}, index=['X', 'Y', 'Z'])

# Select rows with loc i.e the label
row_loc = df.loc[['Y']]
print(row_loc)

# Select rows with iloc, with the default row num
row_iloc = df.iloc[1]
print(row_iloc)




#Single label for row and column
df.loc['cobra', 'shield']
#output
2
```

---

**Slicing of DataFrame✂️📏📋**

Slicing allows selecting specific portions of a DataFrame based on row and column indices.

```python
import pandas as pd

data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'Patrick'],
    'Age': [25, 30, 35, 22],
    'City': ['New York', 'San Francisco', 'Los Angeles', 'Shanghai']
}

df = pd.DataFrame(data)

# Selecting specific columns 
# Note:  should pass a list inside []
print(df[['Name', 'City']])

# Slicing rows from index 2 onwards
print(df.iloc[2:])
```

---

**Adding Columns and Rows➕📊🔧**

Adding new columns in a DataFrame is very easy; it's much like adding a new key-value pair in a dictionary. You can either specify values directly, or you can use existing data in the DataFrame.

Adding new rows is a little bit more involved. We must use the `pd.concat` method and pass a list of DataFrame objects that we want concatenated. Usually, we also want to use the `ignore_index=True` keyword argument to automatically generate a new index for the concatenated rows; otherwise, the index of the new rows will start at 0.

```python
import pandas as pd

data = {
    "name": ["Alice", "Bob", "Charlie"],
    "salary": [50000, 60000, 70000],
    "bonus": [10000, 12000, 15000],
}
df1 = pd.DataFrame(data)

# Make new DataFrame for new rows
new_rows = {
    "name": ["David", "Echo"],
    "salary": [80000, 88000],
    "bonus": [20000, 16000],
}
df2 = pd.DataFrame(new_rows)

# Add new rows to previous DataFrame, passing a list inside concat
df1 = pd.concat([df1, df2], ignore_index=True)

#adding a column like adding in dictionaries
df1["age"] = [32, 34, 35, 65, 34]

# Add new column
df1["total_compensation"] = df1["salary"] + df1["bonus"]

print(df1)
```

---

**Filtering, Sorting, and Updating DataFrames🧐🔍📊**

Filtering involves selecting a subset of rows or columns from a DataFrame based on some condition(s).

The most common way to filter rows is by using boolean indexing, which involves indexing using a boolean expression as a condition and selecting the rows that meet the condition.

Sorting, on the other hand, involves arranging the rows in a DataFrame based on one or more columns. Pandas provides the sort\_values() method to sort a DataFrame by one or more columns.

Together, filtering and sorting are powerful tools for exploring, manipulating, and visualizing data.

```python
import pandas as pd

data = {
    'title': ['The Great Gatsby', 'To Kill a Mockingbird', 'Pride and Prejudice', '1984', 'Animal Farm'],
    'author': ['F. Scott Fitzgerald', 'Harper Lee', 'Jane Austen', 'George Orwell', 'George Orwell'],
    'year_published': [1925, 1960, 1813, 1949, 1945]
}
df = pd.DataFrame(data)

print(df)
print(df['year_published'] < 1950) #returns bool
print(df[[True, False, True, True, True]])
before_1950 = df[df['year_published'] < 1950] # accessing by bools
print(before_1950)

# default ascending = True
sorted_df = df.sort_values(by='year_published', ascending = False)
print(sorted_df)
```

```python
import pandas as pd

data = {
    'name': ['Alice', 'Bob', 'Charlie', 'David', 'Eva'],
    'salary': [50000, 60000, 70000, 75000, 80000],
    'years_experience': [3, 5, 7, 9, 12]
}

df = pd.DataFrame(data)

# Filter for employees with salaries less than 70000
less_than_70k = df['salary'] < 70000

print(less_than_70k)
# Give them a 10% raise
# df.loc[[True, True, False, False, False], 'salary'] *= 1.1, same as
#multiple coloumn lables then pass as a list [ salary, authpr]
df.loc[less_than_70k, 'salary'] *= 1.1

# David has been promoted, but we have forgotten his index.
# No problem, we can find him by name, and update his salary to 85000
print(df['name'] == 'David')
df.loc[df['name'] == 'David', 'salary'] = 85000

# Print the updated DataFrame
print(df)
```
**Cleaning of data🧹📊**
- The `drop_duplicates` method, which filters rows which have an identical row somewhere else in the DataFrame
- The `dropna` method, which drops any rows with "missing" data, which in this case will be the NaN values that pandas creates from Python None values.
- The `fillna` method, which replaces "missing" values with something else.
- `drop()`: Removes rows or columns.

```python
import pandas as pd

data = {
    'order_id': [1001, 1002, 1003, 1004, 1005, 1004],
    'product_name': ['apple', 'banana', 'orange', 'apple', 'banana', 'apple'],
    'quantity': [3, 4, None, 2, 5, 2],
    'price': [1.2, 2.5, 1.8, 1.2, 2.5, 1.2],
    'total': [3.6, 10, None, 2.4, 12.5, 2.4],
    'customer_name': ['Alice', 'Bob', 'Charlie', 'David', None, 'David']
}

df = pd.DataFrame(data)
print(df)
print(" ")

# Example of removing duplicates
df_no_duplicates = df.drop_duplicates()
print(df_no_duplicates)
print(" ")
# Example of dealing with missing data
df_no_missing = df.dropna()
print(df_no_missing)
print(" ")

# Examples of changing data types
df['quantity'] = df['quantity'].fillna(5) #filling nan with 0
df['total'] = df['total'].fillna(df['quantity'] * df['price'])
df['customer_name'] = df['customer_name'].fillna('Unknown')
print(df)
```


**Descriptive Statistics📊**
Pandas provides a range of functions for computing descriptive statistics, such as mean, standard deviation, and percentiles, using numerical data in DataFrames and Series. 
`describe()`

```python
import pandas as pd

data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'David', 'Eve', 'Falco'],
    'Math': [90, 85, 72, 80, 95, 88],
    'Science': [95, 80, 85, 70, 90, 91],
    'English': [80, 75, 85, 90, 95, 79]
}

df = pd.DataFrame(data)
print(df.describe())
#Output:

            Math    Science    English
count   6.000000   6.000000   6.000000
mean   85.000000  85.166667  84.000000
std     8.099383   9.064583   7.483315
min    72.000000  70.000000  75.000000
25%    81.250000  81.250000  79.250000
50%    86.500000  87.500000  82.500000
75%    89.500000  90.750000  88.750000
max    95.000000  95.000000  95.000000

```


**Grouping and Aggregation📊**
In pandas, grouping is done using the `groupby()` function, which creates a GroupBy object that contains information about the groups. Once we have a GroupBy object, we can apply aggregation functions like `sum(), mean(), min(), max(), and count()` to compute summary statistics for each group.


```python
import pandas as pd

data = {
    'Year': [2010, 2010, 2010, 2010, 2011, 2011, 2011, 2011, 
             2012, 2012, 2012, 2012, 2013, 2013, 2013, 2013],
    'Gender': ['M', 'F', 'M', 'F', 'M', 'F', 'M', 'F', 
               'M', 'F', 'M', 'F', 'M', 'F', 'M', 'F'],
    'Count': [101, 153, 120, 140, 214, 257, 180, 220, 
              309, 350, 290, 330, 435, 457, 400, 420]
}

df = pd.DataFrame(data)

# Example of grouping: group by year and gender and compute the sum of counts for each group
grouped_df = df.groupby(['Year', 'Gender']).sum()

print(grouped_df)

#Output:

             Count
Year Gender       
2010 F         293
     M         221
2011 F         477
     M         394
2012 F         680
     M         599
2013 F         877
     M         835

```
**Plotting📊**
When you plot data using Pandas, you are essentially using Matplotlib in the background. Pandas plot() method is a wrapper around Matplotlib's plotting functions, so it simplifies the syntax for creating common plots. However, Matplotlib is still being used to generate the plot.
- We first create a DataFrame with the x and y values.
- We then plot the data by calling plot on the DataFrame and specifying the x and y columns with keyword arguments.
- We also add a label to the plot using the label parameter.
- Finally, we add a title and axis labels using the title, xlabel, and ylabel methods the on matplotlib.pyplot module.
  
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Create some data
x = np.array([0.0, 0.3, 1.2, 2.1, 2.8, 3.6, 4.1, 4.8, 5.3, 6.0])
y = np.sin(x)

# Create a DataFrame
df = pd.DataFrame({'x': x, 'y': y})
print(df)

# Plot the data, default line graph
df.plot(x='x', y='y', label='sin(x)', kind="bar")

# Add a title and axis labels
plt.title('Plot of sin(x)')
plt.xlabel('x')
plt.ylabel('sin(x)')

# Show the plot
plt.show()

# using matplotlib
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Create some data
x = np.array([0.0, 0.3, 1.2, 2.1, 2.8, 3.6, 4.1, 4.8, 5.3, 6.0])
y = np.sin(x)

# Create a DataFrame
df = pd.DataFrame({'x': x, 'y': y})
print(df)

# Use Matplotlib directly to create the plot
plt.bar(df['x'], df['y'], label='sin(x)')  # Bar plot using Matplotlib

# Add a title and axis labels
plt.title('Plot of sin(x)')
plt.xlabel('x')
plt.ylabel('sin(x)')

# Show the plot
plt.show()
```



**Pandas Concepts Summary**

### **Concept 2: Create Series**
A **Series** is a one-dimensional labeled array capable of holding any data type.

**Syntax:**
```python
import pandas as pd

data = [1, 2, 3, 4, 5]
labels = ['A', 'B', 'C', 'D', 'E']
series = pd.Series(data, index=labels)
print(series)
```

---

### **Concept 3: Creating DataFrames**
A **DataFrame** is a two-dimensional labeled table with columns of potentially different types.

**Syntax:**
```python
import pandas as pd

data = {
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35],
    'City': ['New York', 'San Francisco', 'Los Angeles']
}
df = pd.DataFrame(data)
print(df)
```

---

### **Concept 4: Loading Data from Files**
Pandas allows loading data from various file formats like CSV, Excel, and JSON.

**Syntax:**
```python
import pandas as pd
df = pd.read_csv('file.csv')
print(df.head())
```

---

### **Concept 5: DataFrame Indexing**
Indexing in Pandas allows selection of specific rows or columns.

**Syntax:**
```python
# Using labels
print(df['Name'])
# Using integer index
print(df.iloc[1])
# Specific element
print(df.loc[1, 'City'])
```

---

### **Concept 6: DataFrame Slicing**
Extracting specific portions of a DataFrame.

**Syntax:**
```python
# Selecting specific columns
df[['Name', 'City']]
# Slicing rows
df.iloc[2:]
```

---

### **Concept 7: Adding Columns and Rows**
New columns can be added like dictionary keys, while rows are added using `pd.concat`.

**Syntax:**
```python
# Adding a column
df['Salary'] = [50000, 60000, 70000]
# Adding rows
df2 = pd.DataFrame({'Name': ['David'], 'Age': [40], 'City': ['Boston']})
df = pd.concat([df, df2], ignore_index=True)
```

---

### **Concept 8: Filtering, Sorting, and Updating DataFrames**
Filtering allows selecting specific rows based on conditions.

**Syntax:**
```python
# Filtering
df[df['Age'] > 30]
# Sorting
df.sort_values(by='Age', ascending=False)
# Updating
df.loc[df['Name'] == 'Alice', 'Age'] = 26
```

---

### **Concept 9: Cleaning Data**
Handling missing or incorrect data using Pandas.

**Syntax:**
```python
# Dropping missing values
df.dropna()
# Filling missing values
df.fillna(value=0)
# Removing duplicates
df.drop_duplicates()
```

---

### **Concept 10: Descriptive Statistics**
Provides summary statistics for numerical data.

**Syntax:**
```python
# Summary statistics
df.describe()
# Mean
df['Age'].mean()
# Standard deviation
df['Age'].std()
```

---

### **Concept 11: Grouping and Aggregation**
Grouping allows aggregating data by categories.

**Syntax:**
```python
# Grouping
df.groupby('City').mean()
# Aggregation
df.groupby('City').agg({'Age': 'mean', 'Salary': 'sum'})
```

---

### **Concept 12: Plotting with Pandas**
Pandas integrates with Matplotlib for data visualization.

**Syntax:**
```python
import matplotlib.pyplot as plt

df['Age'].plot(kind='bar')
plt.show()
```

| **Category**               | **Method**                                  | **Description** |
|----------------------------|---------------------------------------------|----------------|
| **Creating Data Structures** | `pd.Series(data, index=labels)`          | Creates a Series (one-dimensional labeled array). |
|                            | `pd.DataFrame(data)`                        | Creates a DataFrame (two-dimensional table). |
|                            | `pd.read_csv('file.csv')`                   | Loads data from a CSV file. |
| **Indexing & Selecting Data** | `df['column_name']`                       | Selects a column by label. |
|                            | `df.iloc[index]`                            | Selects rows using integer index. |
|                            | `df.iloc[start:end]`                        | Slices rows using integer index. |
|                            | `df.loc[row_label]`                         | Selects rows by label. |
|                            | `df.loc[row_condition, column_name]`        | Selects specific columns based on condition. |
| **Adding Rows & Columns**  | `df['new_col'] = df['col1'] + df['col2']`   | Adds a new column. |
|                            | `df = pd.concat([df1, df2], ignore_index=True)` | Adds new rows by concatenating DataFrames. |
| **Filtering Data**         | `df[df['year_published'] < 1950]`           | Filters rows based on condition. |
|                            | `df.loc[df['year_published'] < 1950, 'year_published']` | Returns a specific column for filtered rows. |
|                            | `df.loc[df['salary'] < 70000, 'salary'] *= 1.1` | Updates values based on condition. |
| **Sorting Data**           | `df.sort_values(by='column_name', ascending=False)` | Sorts DataFrame by column. |
| **Handling Missing Data**  | `df.drop_duplicates()`                      | Removes duplicate rows. |
|                            | `df.dropna()`                               | Removes rows with missing values. |
|                            | `df.fillna(value)`                          | Replaces missing values with a specified value. |
| **Descriptive Statistics** | `df.describe()`                            | Provides summary statistics. |
|                            | `df.mean()`                                 | Calculates mean. |
|                            | `df.std()`                                  | Calculates standard deviation. |
|                            | `df.median()`                               | Finds median. |
|                            | `df.min()`                                  | Finds minimum value. |
|                            | `df.max()`                                  | Finds maximum value. |
| **Grouping & Aggregation** | `df.groupby(['Year', 'Gender']).sum()`     | Groups by columns and applies sum function. |
|                            | `df.groupby('column_name').mean()`         | Computes mean for each group. |
|                            | `df.groupby('column_name').agg({'col1': 'mean', 'col2': 'sum'})` | Applies multiple aggregations. |
