---
description: 'week 1: ''Python review'''
---

# Iterables



## Comprehension

Comprehension is a compact way to generate a list, set, dictionary, tuple. 

{% hint style="warning" %}
Do not use comprehensions when mutating during the comprehension
{% endhint %}

### List and Set Comprehension:

```python
#general form
[f(var)   for var in iterable   if p(var)]
 ------   -------------------   ---------
 1        2                     3 (optional, default = True)
```

Basically: Make a list of iterable \(2\), only when the value iterated \(1\) satisfies predicate \(3\)

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

It returns a generator \(an object that is iterable ONE time\)

```python
x = (i for i in 'abc') returns a generator
x = tuple(i for i in 'abc') returns a tuple
```

