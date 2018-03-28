---
layout: post
title: 'Numpy Basic Overview'
date: 2018-03-28 14:54:34 -0600
categories: numpy python
---

# Numpy Features

**numpy** (aka Numerical Python) is a python package that is used extensively throughout the field of Data Science to efficiently store and manipulate _dense_ numerical datasets. Let's cover some of the basic features and uses below.

### array creation

```python
import numpy as np

jersey_nums = [35, 13, 30, 11, 23]
player_names = [
    'Kevin Durant',
    'James Harden',
    'Stephen Curry',
    'Kyrie Irving',
    'LeBron James'
]

# create a numpy array
np_players = np.array([jersey_nums, player_names])

np_players
```

    array([['35', '13', '30', '11', '23'],
           ['Kevin Durant', 'James Harden', 'Stephen Curry', 'Kyrie Irving',
            'LeBron James']], dtype='<U21')

Sure, that looks good and all, but why not just pythons built-in
lists? Wouldn't the zip function work just as well, including grouping the player's and jersey's number together in a tuple? For example:

```python
zip_players = zip(jersey_nums, player_names)
players_list = list(zip_players)
players_list
```

    [(35, 'Kevin Durant'),
     (13, 'James Harden'),
     (30, 'Stephen Curry'),
     (11, 'Kyrie Irving'),
     (23, 'LeBron James')]

Let's look at the visual differences between the numpy array and above python list. The numpy array is _homogenous_, while the zip*players list is \_heterogenous* **_and_** grouped in tuple pairs. Which one is easier to work with and has more built-in attributes and methods that would be beneficial for our Data Science learning?

As we try to retrieve specific values from each data structure, take note of the time each process takes, as
well as which one would be more preferential day and day out.

Selecting specific players and their jersey numbers are essentially the same:

```python
np_players[:,4]
```

    array(['23', 'LeBron James'], dtype='<U21')

```python
%timeit np_players[:, 4]
```

    290 ns ± 46.2 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

```python
players_list[4][:]
```

    (23, 'LeBron James')

```python
%timeit players_list[4][:]
```

    111 ns ± 6.86 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

What if we just want the player names though?

```python
np_players[1, :]
```

    array(['Kevin Durant', 'James Harden', 'Stephen Curry', 'Kyrie Irving',
           'LeBron James'], dtype='<U21')

```python
%timeit np_players[1, :]
```

    295 ns ± 5.49 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

Hmmm... we can't really do the same for the players_list:

```python
players_list[0][:]
```

    (35, 'Kevin Durant')

```python
just_players_names = [name[1] for name in players_list]
just_players_names
```

    ['Kevin Durant',
     'James Harden',
     'Stephen Curry',
     'Kyrie Irving',
     'LeBron James']

```python
%timeit [name[1] for name in players_list]
```

    548 ns ± 21.7 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

### shape & ndim attribute

The **shape** attribute returns a tuple with the first value as the number of rows and the second value as the number of columns. **ndim** returns the dimension of the array.

```python
np_players.shape
```

    (2, 5)

```python
np_players.ndim
```

    2

### indexing

numpy arrays can be indexed in a fashion similar to python's
built-in lists. We can still use indices and slicing, but we can
be even more precise in selection based on the rows and columns
we want to retrieve from multi-dimensional lists.

ex:
`python # np_players[row, col] np_players[0, 3] # outputs '11' as the 0 is first row (jersey_numbers) and the 3 is the index`

```python
np_players[1, 3]
```

    'Kyrie Irving'

```python
np_players[1, :] # returns all player_names
```

    array(['Kevin Durant', 'James Harden', 'Stephen Curry', 'Kyrie Irving',
           'LeBron James'], dtype='<U21')

```python
np_players[:, :3] # selects both rows but returns only the first three columns in each row
```

    array([['35', '13', '30'],
           ['Kevin Durant', 'James Harden', 'Stephen Curry']], dtype='<U21')

### Useful Methods & Attributes

numpy has a multitude of **_very_** useful and practical methods and attributes, especially so when working with integers.

```python
height_ft = np.array([7.0, 6.5, 6.3, 6.2, 6.8], dtype=float)
height_ft
```

    array([7. , 6.5, 6.3, 6.2, 6.8])

```python
height_ft.mean()
```

    6.56

```python
np.median(height_ft)
```

    6.5

```python
height_ft.min()
```

    6.2

```python
height_ft.max()
```

    7.0

```python
height_ft.std()
```

    0.30066592756745814

### Operations

Mathematical operations can be performed on numpy arrays. These operations are acted on each element in the
array. Although you can use methods such as add(), subtract(), multiply(), and divide(), most likely you will come across
the more oft used symbols: +, -, \*, /.

```python
w = np.array([23, 21])
x = np.array([45, 18])
y = w + x
z = np.multiply(w, x)
print(f'y = {y}    z = {z}')
```

    y = [68 39]    z = [1035  378]

```python
# convert ft to m
height_m = height_ft * 0.3048
height_m.dtype = float
height_m
```

    array([2.1336 , 1.9812 , 1.92024, 1.88976, 2.07264])

```python
heights = np.stack([height_m, height_ft])
heights
```

    array([[2.1336 , 1.9812 , 1.92024, 1.88976, 2.07264],
           [7.     , 6.5    , 6.3    , 6.2    , 6.8    ]])

```python
heights.shape
```

    (2, 5)

### Boolean Masking

In cases where we want to determine which values in our array meet some specific conditions, numpy allows loops over the values
and returns a new array with either True or False, depending on
whether the values meet the condition or not. We can then use that Boolean array (mask) to filter out just the values that pass our condition.

```python
# Get a mask of heights greater than 2m
mask = heights[0] > 2
mask
```

    array([ True, False, False, False,  True])

```python
heights[0,mask]
```

    array([2.1336 , 2.07264])

```python
# The above could also be written in one line
heights[0, heights[0] > 2]
```

    array([2.1336 , 2.07264])

We have just looked at some of the basic features used to create,
index, and manipulate numpy arrays.

![scratch](https://media.giphy.com/media/VYcRNU4P3vyM/giphy.gif)

This is just but a scratch of what the package can be used for, but it's a great start for what the following posts will cover, including some of the packages that integrate the numpy packages, such as **pandas**, **seaborn**, and **matplotlib**. Other than following along with the [official numpy tutorial](https://docs.scipy.org/doc/numpy-dev/user/quickstart.html), I've also made good use of this concise [numpy lecture notes](http://www.scipy-lectures.org/intro/numpy/index.html).
