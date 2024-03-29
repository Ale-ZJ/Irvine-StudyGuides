---
description: 'week 1: ''Python review'''
---

# List, Set, Tuple, Dictionary

## Mutable vs immutable

* **mutable:** values can change later&#x20;
* **immutable:** values can't change once created

## Hashable vs unhashable

Think of hashability as indexability\
\- a key needs to be unique\
\- thus an object needs to be hashable in order for Python to create a unique hash (index) to use as the key\
\- since immutable objects do not change, then immutable objects are hashable

{% hint style="info" %}
All immutable objects are hashable but not all hashable objects are immutable.
{% endhint %}

## Comparing Iterable objects&#x20;

* **Sequence types**: **list** (mutable) and **tuples** (immutable)
* **Set type**: **set** (mutable) and **frozenset** (immutable)
* **Mapping type**: **dict** (mutable) and **defaultdict** (mutable)

### defaultdict

A defaultdict gives a default value to a key that does not exist, thus never raises a KeyError.&#x20;

```python
from collections import defaultdict

letters = ['a', 'x', 'b', 'x', 'f', 'a', 'x']

freq_dict = defaultdict(int) #defaults to 0
for l in letters:
    freq_dict[1] += 1
print(freq_dict)
```

## Sorting

* `list.sort()` **** = returns None, it mutates the list
* `sorted()` = returns a sorted list from a given ITERABLE object

when sorting a list of tuples (from a dict), python never compares the 2nd value because all keys in a dictionary are unique

```python
votes = [('Charlie', 20), ('Able', 10), ('Baker' ,20), ('Dog', 15)]

#sort list by value without changing the original list
sorted(votes, key=(lambda t : t[1]), reverse=True)
# returns [('Charlie', 20), ('Baker', 20), ('Dog', 15), ('Able', 10)]


# swaps key with value -> sort list by value and aphabetical order
sorted(votes, key=lambda t : (-t[1],t[0]) )
# return [('Baker', 20), ('Charlie', 20), ('Dog', 15), ('Able', 10)]
```

## Comprehension

Comprehension is a compact way to generate a list, set, dictionary, tuple.&#x20;

{% hint style="danger" %}
Do not use comprehensions when mutating during the comprehension
{% endhint %}

### List and Set Comprehension:

```python
#general form
[f(var)   for var in iterable   if p(var)]
 ------   -------------------   ---------
 1        2                     3 (optional, default = True)
```

Basically: Make a list of iterable (2), only when the value iterated (1) satisfies predicate (3)

```python
x = [i**2 for i in range(1,11) if i%2==0]
print(x)
>>> [4, 16, 36, 64, 100]
```

### Dictionary Comprehension

```python
#general form
{k(var) : v(var) for var in iterable if p(var)}

# ex1)
x = {k : len(k) for k in ['one', 'two', 'three', 'four', 'five']}
>>> {'four': 4, 'three': 5, 'one': 3, 'five': 4, 'two': 3}

# ex2) #a dictionaary with key word and values sets
{word : {c for c in word} for word in ['i', 'love', 'new', 'york']}
{'i': {'i'}, 'love': {'e', 'o', 'v', 'l'}, 'new': {'e', 'n', 'w'}, /
'york': {'k', 'o', 'y', 'r'}}
```

### Tuple Comprehension

{% hint style="info" %}
Tuple comprehensions are special because they return a _**generator**_.
{% endhint %}

```python
x = (i for i in 'abc') returns a generator
x = tuple(i for i in 'abc') returns a tuple
```
