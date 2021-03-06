---
layout: post
title: Seaborn Syntax
---

![Seaborn](https://seaborn.pydata.org/_images/hexbin_marginals.png "Seaborn Graphics Example")

# Seaborn - Statistical Data Visualization

## Introduction
Seaborn is a Python visualization library based on matplotlib. It provides a high-level interface for drawing attractive statistical graphics.

## Documentation

* [API Reference](https://seaborn.pydata.org/api.html)
* [Tutorials](https://seaborn.pydata.org/tutorial.html)

## Importing Module
_Seaborn_ is commonly imported as __`sns`__.
```
import seaborn as sns
```

## Displaying Plots
When we are ready to display the plots we call __`pyplot.show()`__.
```
import seaborn as sns
import matplotlib.pyplot as plt
. 
. some code with seaborn to create the plot
.
plt.show()
```

## Histograms & KDE
We can generate a histogram using __`seaborn.distplot()`__ function.
```
sns.distplot(dataframe_obj[col_name])
plt.show()
```
### Kernel Density Plot
In order to generate just the _Kernel Density Plot (KDE)_ we use the __`sns.kdeplot()`__ function.
```
sns.kdeplot(dataframe_obj[col_name], shade=True)
```
The `shade` parameter will shade the area under the graph when set to `True`.


## Plot Appearance

### Styles
We can use the __`seaborn.set_style()`__ function to change the default _Seaborn_ style sheet. _Seaborn_ comes with a few style sheets:

* `darkgrid`: Coordinate grid displayed, dark background color
* `whitegrid`: Coordinate grid displayed, white background color
* `dark`: Coordinate grid hidden, dark background color
* `white`: Coordinate grid hidden, white background color
* `ticks`: Coordinate grid hidden, white background color, ticks visible

```
sns.set_style('whitegrid')
```

### De-spine the Plot
To remove the axis spines for the top and right axes, we use the seaborn.despine() function:
```
sns.despine()
```

By default, only the `top` and `right` axes will be despined, or have their spines removed. To despine the other two axes, we need to set the `left` and `bottom` parameters to `True`.

## Small Multiple - FacetGrid

[`seaborn.FacetGrid()` Reference](http://seaborn.pydata.org/generated/seaborn.FacetGrid.html#seaborn.FacetGrid)

_Small Multiple_ is a group of similar plots on the same scale that can be effectively compared side-by-side. In _Seaborn_ this can be achieved in 2 steps with the __`seaborn.FacetGrid()`__ function first to subset the data and create the __grid__, and then with __`FacetGrid.map()`__ method to specify the __type of plot__ to be generated and any shared parameters for all the subplots.
```
facetgrid_obj = sns.FacetGrid(dataframe_obj, col='col_name_to_condition', size=n)

facetgrid_obj.map(plot_func, col_name, shade=bool)
```
* The first line creates a _FacetGrid_ object that uses `col_name_to_condition` column name as a _String_ to extract values from `dataframe_obj` dataframe. Parameter `size` specifies the height of the plot in inches.
* Second line of code uses the `facetgrid_obj` to map values between `col_name_to_condition` and `col_name` visualized according to what `plot_func` used. The plotting function can be any __valid__ _Matplotlib_ or _Seaborn_ plotting function such as `sns.kdeplot` or `plt.hist`
* When subsetting data we can specify __`three`__ conditions __`col`__, __`row`__ and __`hue`__.
* Adding a legend is done by __`facegrid_obj.add_legend()`__.

