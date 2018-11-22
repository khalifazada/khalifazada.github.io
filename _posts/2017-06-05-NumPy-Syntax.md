---
layout: post
title: NumPy Syntax
---

![NumPy Logo](http://www.numpy.org/_static/numpy_logo.png)

# NumPy Syntax 

----------

## __Import__

### Importing Module
```python
import numpy as np
```

### Read Data
```python
np.genfromtxt(...)
```
#### Example
```python
data = np.genfromtxt("file_name.csv", delimiter=",", skip_header=1, dtype="U75")
```

## __Arrays__

### Creating Arrays
```python
np.array(...)
```
#### Example
```python
matrix = np.array([[1, 2, 3], [4, 5, 6]])
```

### Array Shape
```python
np_arr.shape
```

#### __Array Data Type__
```python
np_arr.dtype
```

#### __Array Comparison__
```python
# returns bool vector
np_arr02 = (np_arr01 == value)
```
##### _Example_
```python
# selecting rows using bool vector
selected_rows = np_arr03[np_arr02,:]

# replacing values using bool vector
np_arr03[np_arr02,:] = value
```

#### __Converting Data Types__
```python
np_obj.astype(type)
```

#### __Computing with NumPy__
##### Example
```python
# sum|mean|max|min etc, 0|1 = row|col
np_obj.sum(axis=0)
```



