---
layout: post
title: Pandas Syntax
---

![Pandas Logo](http://pandas.pydata.org/_static/pandas_logo.png)

# Pandas Syntax

-----

## Getting Started

### Importing Module
The module can be imported in the following way
```python
import pandas
```
it can also be renamed when importing to shorten the name
```python
import pandas as pd
```


### Reading CSV
Pandas dataframe object supports multiple data types in each data cell and is specifically designed to work with tabular data. Typical tabular file is a CSV file where a delimiter separating each value is defined by some kind of character (usually a comma, since CSV stands for Comma Separated Values.)

Pandas data frame objects have a header and a row name unlike _NumPy_ objects that are n-dimensional arrays of one particular type of data.
```python
dataframe_obj = pandas.read_csv("file_name.csv")
```

## Exploring the Data Frame
```python
pandas_dataframe_obj.head(n)
```
will output first $n$ rows.
```python
df.shape # outputs a 2-element tuple
df.shape[0] # number of rows
df.shape[1] # number of columns
```
The above code block will output the tuple representing the dimensions of the data frame, number of rows and columns respectfully.
```python
df.columns
```
will output column names. However, `df.rows` will produce an error.

### Working with Rows

#### Selecting Rows

##### `*.loc[]`
There are a few methods to use when selecting rows with Pandas. The first we will lok at is using `*.loc[]` which stands for _location_.
```python
row = df.loc[value]
```
The `value` parameter passed can be an __integer__ or the row __label__ and the resulting `row` variable will be a single vector with headers (column names.)

Selecting multiple rows is just as straight forward:
```python
matrix = df.loc[start:finish]
matrix = df.loc[0,3,10]
```
where `start` and `finish` specify the range.

##### `*.iloc[]`

Another method is using `*.iloc[]` and stands for _integer location_ and just like the explanation suggests can __only__ use integers to specify row selection. Other than that works in exactly the same manner as `*.loc[]`.

__Bracket notation__ is used to select a __slice__ of rows.
To select __individual__ or a custom __list__ of rows we need to use the `*.iloc[]` method on the _Dataframe_ object. With `*.iloc[]` the following methods are acceptable:

* Integer
* List of integers
* Slice object
* Boolean array

##### Boolean

We can also select rows by subsetting with a vector of Boolean values:
```python
bool_vals = (dataframe["column_name"] == value)
rows = dataframe[bool_vals]
```
The code above will create a vector of Boolean values for each element where `dataframe["column_name"] == value` produces a `True` or `False` value. Further, `rows = dataframe[bool_vals]` will select all rows where the value is `True`.

### Working with Columns

#### Column Names
##### Naming Columns
```python
df.columns = ['col_name01', ... , 'col_nameN']
```
##### Renaming Columns
```python
col_rename_dict = {old_name01: new_name01, ... , old_nameN: new_nameN}
df.rename(columns = col_rename_dict)
```

#### Selecting Columns
Selecting a column is even easier as the snippet below demonstrates:
```python
dataframe_column = df["column_name"]
dataframe_column = df[["col_name01", "col_name03"]]
```
By passing a string with the right column name the dataframe will return a Series object. String can also be passed as an object (i.e. `df[string]`)

#### Accessing Values

To generate a list of values from a particular column we can do the following:
```python
list_of_values = dataframe["column_name"].values
```
#### Counting Values
It is sometimes helpful to find out what values are unique and how often they appear in the given column. For that we can use a special method __`Series.value_counts()`__

#### Mapping Values
We can use a dictionary to specify what values in the column we want to convert (**map**). This is most useful when `Yes/No` type values need to be converted to *Boolean* `True/False`. We can achieve this using the **`pd.Series.map()`** method.
```python
# Consider the following string
series = ['Yes', 'Yes', NaN, 'No']

# Define a dictionary
dict = {'Yes': True, 'No': False}

series = series.map(dict)
```
The output of `series` will be **`[True, True, NaN, False]`**.

#### Manipulating a Column
It is possible to define element-wise arithmetic operations on an entire column at once
```python
resulting_column = dataframe["column_name"] / 1000
```
As a result of the above operation all elements of the column will be divided by the specified value

It is also possible to define operations on multiple columns:

```python
resulting_column = dataframe["column01"] + dataframe["column02"]
```
#### Creating a New Column
It is convenient to create a new column in the data frame either by inserting new values or manipulating values from one of the existing columns and assigning it as a new column.
```python
dataframe["new_column_name"] = value
```
Assuming that the __`value`__  variable is a vector.

Creating a new column from an existing one using the `=` assignment operator:

```python
new_column = dataframe["old_column_name"] / 1000
dataframe["new_column_name"] = new_column
```
### Sorting the Dataframe
Sometimes it is necessary to sort values within the dataframe by a particular column. This can be done in the following way:
```python
new_dataframe = dataframe.sort_values("column_name", ascending=True)
```
Specifying `ascending=False` will sort the dataframe in descending order.

#### Resetting Index
After sorting the rows indexes are no longer in sequence. Sometimes it is useful to reset the index which can be done using the `*.reset_index()`. 

This command will retain the old index and add a new column with the new sequential index. If we do not need to retain the old index then `drop=True` has to be specified as a parameter.

#### Custom Index
The dataframe object has a `*.set_index()` method that allows us to pass in the name of the column we want pandas to use as the _Dataframe_ index.
The `*.set_index()` method has a few parameters that allow us to tweak this behavior:

* `inplace`: If set to `True`, this parameter will set the index for the current dataframe, instead of returning a new dataframe.
* `drop`: If set to `False`, this parameter will keep the column we specified as the index, instead of dropping it.

### Working with Missing Data
#### Finding NAs

To create a vector of `True` and `False` values of missing data in a particular column we can use the following method:
```python
column = dataframe["column_name"]
vector_w_bool_vals  = pandas.isnull(column)
```

We can further select the _null_ values:
```python
rows_w_null_vals = column[vector_w_bool_vals]
```
We can also subset the values and select the ones that are __not__ null
```python
rows_w_vals = column[vector_w_bool_vals == False]
```
#### Removing NAs
To remove any rows or columns containing `NA` values we can use `*.dropna()` method on a dataframe.
```python
drop_nas = dataframe.dropna(axis=0)
```
Axis `0` will specify to drop rows and `1` will drop any columns that have `NA` values as entries.

#### Filling NAs
We can fill in missing data in pandas using the **`pandas.DataFrame.fillna()`** method. This method will replace any missing values in a dataframe with the values we specify.

#### Subsetting Dataframe
We can use the `subset` keyword argument to the `dropna()` method so that it only drops rows if there are NA values in certain columns. We will have to specify a *list* of column names.

```python
new_df = df.dropna(subset=["col_nm_01", "col_nm_02",...])
```

### Pivot Tables

#### Parameters

Pivot tables (popularized by _Microsoft Excel_) allow grouping and then applying a calculation. With Pandas we can create a Pivot Table using the following method:

```python
pivot_table = dataframe.pivot_table(index="column_name", values="another_column", aggfunc=func_name)
```

For `aggfunc` we can specify any function or a list of functions. The default parameter is `numpy.mean`.

Similarly, for `values` a list of column name strings can be passed to group similar calculations across several columns for a specified index.

For further information refer to the [documentation](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.pivot_table.html "Pivot Tables").

### Applying Functions to a Dataframe

#### Using a Built-In Function
We can compute the mean of **every** column using the **`pandas.DataFrame.mean()`** method.

#### Using a Custom Function
We can pass a function to a dataframe object and have it apply the function through each column and perform an operation iteratively on each element of the column. The following code illustrates:
```python
new_result = dataframe.apply(function_name)
```
The code above takes the function `function_name`, passes it to the dataframe and has is apply the function to every element assigning the returning result to `new_result`.

#### Using a Lambda Function

We can also apply a _Lambda Function_:
```python
new_result = dataframe.apply(lambda x: np.std(x), axis=1)
```
A new _Dataframe_ object will be returned where the _lambda_ function was applied to each element of every column.
In this case `axis=1` instructs `*.apply()` to perform `np.std()` over __rows__ of the dataframe.

## Pandas Data Structures

### Series 

#### Instanciating
_Series_ is a Pandas object that is basically a collection of values. Series object is constructed as a one-dimensional NumPy ndarray. In order to instanciate a _Series_ object we do the following:
```python
# import the correct module
from pandas import Series

custom_series = Series(data=list_of_values, index=list_of_labels)
```
In the snippet above `list_of_values` and `list_of_labels` must have the __same length__ and will produce a labelled series of values.

#### Accessing Values

When it comes to _Series_ objects we can access values just like in a dictionary by providing a __key__ `series_obj['string_label']` or simply by using the __bracket notation__ just like ordinary lists `series_obj[n:m]`.

#### Re-indexing

_Series_ objects can be re-indexed by calling `*.reindex(index=sorted_list)` method on the object and passing a __sorted__ list as an index.

* Sorting can be done by calling `*.sort()` on a _list_ or passing to the `sorted(unsorted_list)` method

#### Sorting

Additionally Pandas _Series_ objects can be sorted using the built-in `*.sort_index()` or `*.sort_values()` methods that preserve __data alignment__.

#### Comparing and Filtering

We can use vectorized operations on _Series_ objects to compare values or filter
```python
series_greater_than_50 = series_obj[series_obj > value]
```
This will return a _Series_ object that has values bigger than $50$ based on Boolean values that expression `series_obj > value` creates.
We can also define more complex operations using bracket notation:
```python
both_criteria = series_obj[(series_obj > n) & (series_obj < m)]
```

## Dataframe & Series Plots

### Scatter Plots
There is a way to plot _Pandas_ dataframe directly without creating any _Matplotlib_ figures and plots explicitly.
```python
dataframe_obj.plot(x='col_name01', y='col_name02, kind='scatter')
```
The code above will return an _Axes_ object which is essentially a subplot that we can further customize.

### Histograms
We can also quickly visualize histograms of a specific column the following way:
```python
dataframe_obj['col_name'].plot(kind='hist')
```
Another method to generate a histogram of a column showing it's distribution is to call __`Series.hist()`__ method.
```python
dataframe_obj['col_name'].hist(bins=k, range=(n,m))
```

### Scatter Matrix Plots

A __scatter matrix__ plot combines both scatter plots and histograms into one grid of plots and allows us to explore potential relationships and distributions simultaneously. A scatter matrix plot consists of `n` by `n` plots on a grid.
_Pandas_ contains a function named __`scatter_matrix()`__ that generates the plots for us.

* This function is part of the __`pandas.tools.plotting`__ module and needs to be __imported separately__.

```python
from pandas.tools.plotting import scatter_matrix

scatter_matrix(df_obj[['col_01',...,'col_k']], figsize=(n,m))
```
