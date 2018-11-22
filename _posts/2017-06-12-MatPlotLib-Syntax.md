---
layout: post
title: MatPlotLib Syntax
---

![MatPlotLib Logo](https://matplotlib.org/_static/logo2.svg)

# MatPlotLib Syntax

-----

## Getting Started

### Introduction
_Matplotlib_ is a library for making 2D plots of arrays in Python. It makes heavy use of _NumPy_ and other extension code to provide good performance even for large arrays.

### Importing Module
The module can be imported in the following way
```python
import matplotlib.pyplot as plt
```

### Set-up

In order to use _Matplotlib_ within _Jupyter Notebooks_ and display plots __inline__ with our code in each cell we need to run `%matplotlib` [magic code](http://ipython.readthedocs.io/en/stable/interactive/plotting.html#id1) __after__ we import the library.
Basically, a typical _Jupyter Notebook_ in it's first code cell will have:
```python
%matplotlib inline
import matplotlib.pyplot as plt
```


## Display Plots

### `plt.plot()`
[Documentation](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.plot)

This function __generates__ the plot and accepts a _NumPy_ array or a _list_ for the domain and range of the plot. 

```python
plt.plot([1, 2, 3], [1.1, 1.8, 3.3])
```

### `plt.show()`
[Documentation](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.show)

This function is more general and __shows__ complex objects like figures with multiple subplots for example. To display the objects we just call the method __without__ any inputs.

```python
plt.plot([1, 2, 3], [1.1, 1.8, 3.3])
plt.show()
```

### Dataframe Plots

There is a way to plot _Pandas_ dataframe directly without creating any figures and plots explicitly.

```python
dataframe_obj.plot(x='col_name01', y='col_name02, kind='scatter')
```

The code above will return an _Axes_ object which is essentially a subplot that we can further customize.


## Plot Labels & Title

### Labels
To label the axes we can call __`plt.xlabel()`__ and __`plt.ylabel()`__ methods that both accept _String_ as input.

### Title
Labeling the plot with a title is just as straightforward by calling __`plt.title()`__ and passing a _String_.


## Figure & Subplots

### Figure

A __figure__ acts as a container for all of our plots and has methods for customizing the appearance and behavior for the plots within that container.

```python
fig = plt.figure()
```
#### Figure Dimensions

To tweak the dimensions of the plotting area, we need to use the `figsize` parameter when we call __`plt.figure()`__. This parameter takes in a _tuple_ of floats:

```python
fig = plt.figure(figsize=(width, height))
```
Where `width` and `height` are specified in _inches_.


### Subplots

To add a new subplot to an existing figure, use __`Figure.add_subplot()`__.

```python
subplot_obj = fig.add_subplot(nrows, ncols, plot_number)
```

#### Lables

To add labels to subplots we can use:

```python
subplot_obj.set_xlabel('x_label')
subplot_obj.set_ylabel('y_label')
```

#### Axis Limits

```python
subplot_obj.set_xlim(n,m)
subplot_obj.set_ylim(n,m)
```


#### Example

Here's an example of a figure having 2 subplots arranged in 2x1 format:

```python
import matplotlib.pyplot as plt

fig = plt.figure()

ax1 = fig.add_subplot(2,1,1)
ax2 = fig.add_subplot(2,1,2)

plt.show()
```
#### Adding Data to Subplots

Subplots are _Axis_ objects. To generate a line chart within an _Axes_ object, we need to call __`Axes.plot()`__ and pass in the data.

```python
subplot_obj.plot(x,y)
```
_Axis_ objects will accept _NumPy_ arrays and _Pandas.Series_ objects.

#### Simple Subplot

We can create a quick and simple subplot the following way:

```python
fig, ax = plt.subplots()
```
This will return _Figure_ and _Axes_ objects so that we can go straight to specifying subplot parameters that we need.

### Overlaying Plots
Calling __`plt.plot()`__ multiple times will overlay charts onto one single plot. We can also differentiate each chart by specifying __`c='color_name'`__.

### Legend

Adding a legend is straight forward. We just need to specify the __`label`__ attribute with a _String_ when calling the __`plt.plot()`__ function and then calling __`plt.legend()`__ and specifying the location with the __`loc`__ attribute.

```python
plt.plot(x, y, c="red", label="line")
plt.legend(loc="upper right")
```

## Bar & Scatter Plots

### Bar Plots

#### Specifications
To plot bar charts we have to specify the following attributes:

* Position of the bars
* Width of the bars
* Position of the axis labels

There are $2$ methods to create a bar plot with _Matplotlib_. One is to call __`plt.bar()`__ and the other is to call __`Axes.bar()`__. We will focus on the latter which has $2$ required parameters and $1$ optional: __`left`__, __`height`__ that accept _list_ like objects are the required and __`width`__ is the optional parameter:

```python
# generate a single subplot by returning a Figure and Axes object
fig, ax = plt.subplots()

ax.bar(x_coordinates, bar_heights, width_of_bar)
```

#### Horizontal Bar Plot
To create a horizontal bar plot we call __`Axes.barh()`__ method and use the same input parameters as described above.

#### Label Alignment

##### Vertical Bars
* To change the position of ticks spanning the x-axis we can use __`Axis.set_xticks()`__.
* Then we can specify tick labels by using __`Axis.set_xticklabels()`__.
* We can also specify the orientation of labels by including a `rotation` parameter.

Both methods will take _list_ like objects with _String_ values as names.

##### Horizontal Bars

* __`Axes.set_yticks()`__
* __`Axes.set_yticklabels()`__

### Scatter Plots

#### Specifications

To generate a scatter plot, we use __`Axes.scatter()`__. It has 2 required parameters, __`x`__ and __`y`__.

```python
fig, ax = plt.subplots()
ax.scatter(x,y)
```

## Histograms & Box Plots

### Histograms

#### Specifications

To create a _Histogram_ plot in _Matplotlib_ we use the __`Axes.hist()`__ method. 

```python
subplot_obj.hist(list_of_values)
```

#### Bin Range

While only one parameter is required, an iteratable array of values, comparison of plots can become problematic since each can use different binning strategy. To avoid this we can specify __`range`__ of values by passing a _tuple_.

```python
subplot_obj.hist(list_of_values, range=(n,m))
```

#### Number of Bins

We can also specify the number of bins to be plotted.

```python
subplot_obj.hist(list_of_values, bins=k, range=(n,m))
```

### Box Plots

#### Specifications

To generate a box plot we use __`Axes.boxplot()`__ method.

```python
subplot_obj.boxplot(list_of_values)
```
_Matplotlib_ will sort through the values, calculate the quartiles and generate the box and whisker diagram.

#### Multiple Box Plots

To create multiple box plot diagrams in one plot we can do the following:

```python
subplot_obj.boxplot(dataframe_obj[col_names].values)
```
We can further use the same list `col_names` with column name strings to specify x-axis tick labels.

```python
subplot_obj.set_xticklabels(col_names, rotation=degree)
```

## Plot Aesthetics

### Hiding Tick Marks

In order to improve plots visually we should remove all unnecessary clutter. Lets start by hiding tick marks.

__`Axes.tick_params()`__

* `bottom=off`
* `top=off`
* `left=off`
* `right=off`

```python
subplot_obj.tick_params(bottom=off, top=off, left=off, right=off)
```

### Hiding Spines

Chart _Spines_ are objects within the subplot _Axes_ object and they need to be accessed separately to be turned on/off.

```python
direction = 'left'
bool = False

subplot_obj.spines[direction].set_visible(bool)
```
The code above will turn off spines on the left. Similar logic follows for all other directions.

### Colors & RGB

To specify color ranges with _Matplotlib_ we need to scale every __RGB__ value to the range $[0, 1]$ and represent it as a __3-tuple__.

```python
clr = (0/255,107/255,164/255)
```

### Line Width

To set the __width__ of the line on a chart we need to provide an additional parameter __`linewidth`__ when we call the __`Axes.plot()`__ method.

```python
clr = (0/255,107/255,164/255)
chart_label = "Chart Label"
n = 5

subplot_obj.plot(x, y, label=chart_label, c=clr, linewidth=n)
```

### Text & Annotating

To display text or annotate a chart we call the __`Axes.text()`__ method. Tis method has a few required parameters:

* `x` coordinate (_float_)
* `y` coordinate (_float_)
* `s` text (_string_)

```python
subplot_obj.text(x,y, 'text_to_display')
```

### Y-axis Labels

To set the labels on the `y` axis of plots we can use __`ax.set_yticks(list_of_vals)`__ method

```python
subplot_obj.set_yticks([0,33,66,100])
```

### Horizontal Line
To add a horizontal line to a plot we can use __`Axes.axhline()`__ method with following parameters:

* `y` axis coordinate
* RGB color in $[0,1]$
* `alpha` transparency

```python
subplot_obj.axhline(50, c=(128/255,128/255,128/255), alpha=0.5)
```

## Exporting to a File
Once plots are generated we would want to export it to an image file and save locally. This can be done using the method __`pyplot.savefig()`__ or __`Figure.savefig()`__.

* Note that these have to be called __before__ we display the figure using __`pyplot.show()`__.

```python
fig.savefig('plot.png')
```
