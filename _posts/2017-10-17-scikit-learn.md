---
layout: page
title: scikit-learn Cheatsheet
---

# `scikit-learn`

## k-NN Regression/Classification

### Algorithm Logic

Metric formula to calculate *Minkowski* distance between two vectors:

![Minkowski Distance](http://mathurl.com/y9pf8jfx.png)

![](http://mathurl.com/yarbmsz7.png)

Here $\vec v$ is the *feature* vector and $\vec w$ is the *observed* vector, the value of which we need to predict.

First we sort the distance values in an ascending order and selecting $k$ rows will give the required neighbors nearest to $\vec w$.

After this we look up their *true* values and in the case of **classification** choose the most frequent one to be our *prediction*. In the case of **regression** we average out the values to get our prediction.

The *error* in our prediction is calculated by taking the *Root Mean Squared Error*:

![RMSE](http://mathurl.com/yahmv4dq.png)

Here ![](http://mathurl.com/y7lsqj4z.png) is the *prediction* and ![](http://mathurl.com/y83r6xxf.png) is the *true* value vector.

### Importing
```python
from sklrean.neighbors import KNeighborsRegressor
```

### Instanciating a model
```python
knn = KNeighborsRegressor(n_neighbors=5, algorithm='brute', p=2)
```
Parameter `p` specifies the *Minkowski* distance

### Training
```python
col_list = [col_nm_01,...]
train_data = df[col_list]
target_data = df[target_col_nm]

knn.fit(train_data, target_data)
```
Model will *store* training & target data at this point.

### Predicting
```python
test_data = df[col_list]
predictions = knn.predict(test_data)
```
Model will perform the distance calculation,  comparison and *prediction* steps at this point.

### Error
```python
from sklearn.metrics import mean_squared_error
mse = mean_squared_error(predictions, true_values)
rmse = mse ** 0.5
```

## k-Fold Cross Validation

### Algorithm Logic

1. Split the dataset into $k$ partitions.
2. Select one partition, assign as **Test Set**
3. Randomize ordering of other rows and assign as **Train Set**
4. Fit model on Train Set and calculate RMSE
5. Repeat with other partitions in similar fashion.
6. Average all RMSE values

### Importing
Working with *sklearn* will require us to create a **KFold** object that is an *iterator*. This object will instruct how many partitions to create, specified by the `n_folds` argument; whether to `shuffle` the observations; and if so then what is the *seed* parameter for the `random_state`.
```python
from sklearn.model_selection import KFold
kf = KFold(n_folds, shuffle=False, random_state=None)
```

A **KFold** object is used in *conjunction* with **cross_val_score()** function:
```python
from sklearn.model_selection import cross_val_score

cross_val_score(model, x_train, y_train, scoring="string_val", cv=kf)
```
* `model` is the algorithm we want to use to *fit* values (k-NN, Regression, etc.)
* `x_train` is our training set
* `y_train` is the corresponding *true* values
* [`scoring`](http://scikit-learn.org/stable/modules/model_evaluation.html#common-cases-predefined-values) a string describing criteria
* `cv` instance of the `KFold` class describing the cross-validation parameters. Can also be an *integer* value.
