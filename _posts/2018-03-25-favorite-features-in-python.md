---
layout: post
title: 'Favorite Features In Python'
date: 2018-03-25 18:42:05 -0600
categories: python
---

# Features I Love In Python

Every programming language implement some of the same features, such as functions, loops, if/else statements, etc, but what differents most of them is how they chose to implement them. While some may be quite similar, because most likely they are based off of another language, i.e javascript and c, some of them are quite unique, particulary to me, as in the case of python and ruby.

Lets compare some of my favorite features in python and its approximate equivalent in javascript. The javascript equivalents could potentially be made more terse, but I have written it in a way to higlight python's succintness.

### Comprehensions

```python
# python
even_nums = [i for i in range(0,20,2)]
even_nums
```

    [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

```javascript
// javascript
getEvenNums = () => {
  nums = []
  for (let x = 0; x < 20; x += 2) {
    nums.push(x)
  }
  return nums
}
evenNums = getEvenNums()
evenNums
```

    [ 0, 2, 4, 6, 8, 10, 12, 14, 16, 18 ]

```python
# python
even_keys = {i: val for i, val in enumerate(even_nums) if i % 2 == 0}
even_keys
```

    {0: 0, 2: 4, 4: 8, 6: 12, 8: 16}

```javascript
// javascript
getEvenKeys = () => {
  evenKeysObj = {}
  evenNums.forEach(x => {
    index = evenNums.indexOf(x)
    if (x % 2 == 0 && index % 2 == 0) {
      evenKeysObj[index] = x
    }
  })
  return evenKeysObj
}
even_keys = getEvenKeys()
even_keys
```

    { '0': 0, '2': 4, '4': 8, '6': 12, '8': 16 }

### lambda functions

```python
# python
double = lambda x: x * 2
double(5)
```

    10

```javascript
// javascript
double = x => x * 2
double(5)
```

    10

### snake_case vs camelCase

These aren't really features, but adopted practices. However with much usage of the snake_case I have found
a preference for it, thus I dub it a feature.

### Batteries Come Included

Python is much lauded for it standard library that comes included with such modules as the datetime, collections, fileinput, statistics, and math among others. The entire list can be seen [here](https://docs.python.org/3/library/index.html).

As I stated in my previous post, the upcoming entries will focus on Data Science. Therefore some focus will shed on some of python's packages that were created just for that purpose (among others). The first one discussed will be the **numpy** package.
