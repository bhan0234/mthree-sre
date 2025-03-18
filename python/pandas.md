**Pandas**

Pandas is used for data manipulation and analysis. It offers data structures like Series and DataFrame, which are designed for handling tabular data and provide functionality similar to spreadsheets. Pandas integrates seamlessly with NumPy and offers a variety of tools for data cleaning, aggregation, transformation, and visualization.

---

**Series**

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

**DataFrames**

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

**`head()`** Method:

```python
print(df.head())
```

The `head()` function on the **DataFrame** object returns the first 5 rows of the **DataFrame** by default.

---

**Load Data from Files:**

```python
import pandas as pd

# Replace 'file.csv' with the actual file path
df = pd.read_csv('file.csv')
print(df.head())
```

---

**DataFrame Indexing**

**Accessing Data Using Labels and Integer Indexing:**

Indexing in Pandas allows for efficient selection, slicing, and manipulation of data within DataFrames. There are two primary methods:

- **Label-based Indexing (********`loc`********\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*)**: Access data using explicit row and column labels.
- **Integer-based Indexi**\*\*\*\*\*\*`iloc`\*\*\*\*****\*\*\*\*)******\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\`**)\*\*: Access data using numerical positions.

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

**Selecting Rows with ****************************************************************************************************************************`loc`**************************************************************************************************************************** and ****************************************************************************************************************************`iloc`****************************************************************************************************************************:**

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

**Slicing of DataFrame**

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

**Adding Columns and Rows**

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

**Filtering, Sorting, and Updating DataFrames**

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
df.loc[less_than_70k, 'salary'] *= 1.1

# David has been promoted, but we have forgotten his index.
# No problem, we can find him by name, and update his salary to 85000
print(df['name'] == 'David')
df.loc[df['name'] == 'David', 'salary'] = 85000

# Print the updated DataFrame
print(df)
```
