---
description: >-
  Week 4: "Iterators via Classes" + "Generator Functions (and yield): Functions
  that Return Iterators"
---

# Generators and Decorators

Important terminologies:

* **Iterable:** object that can be iterated over \(you can call `iter` on them\)
* **Iterator:** object which `next()` can be called 
* **Generator function:** function with one or more `yield` statements
* **Generator:** special kind of iterator 
* **Decorator:** returns the 'same kind' of object that its argument but decorated, i.e. with a change in behavior
  * takes in an iterable and returns the same iterable but with diff stuff

<table>
  <thead>
    <tr>
      <th style="text-align:left">function</th>
      <th style="text-align:left">generators</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>return</code> it goes outside the function</td>
      <td style="text-align:left"><code>return</code> ignores return value and raises <code>StopIteration</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">resets in every function call</td>
      <td style="text-align:left"><code>yield</code> is like printing a value and then resuming execution
        in the same line of code when <code>next</code> is called in the generator.</td>
    </tr>
    <tr>
      <td style="text-align:left">binds parameters to matching arguments and then they are forgotten</td>
      <td
      style="text-align:left">
        <p><code>yield</code> args and params are remembered</p>
        <p><code>return</code> they are forgotten</p>
        </td>
    </tr>
  </tbody>
</table>

```python
#will produce 2 3 5 7 11 13 17 19 if primes(20)
def primes(max = None):
    p = 2
    while max == None or p <= max: #short circuit evaluation
        if is_prime(p):
            yield p
        p += 1
```

#### Decorators from Iterators via classes to generators

Because generators can remember information - state, it is easier to write generators to implement iterators compared to writing  `__iter__` and `__next__` methods explicitly.

```python
class Repeat: 
    '''
    Repeat an iterator some fixed number of times (when the second argument for 
    building a Repeat class is an integer) or forever (no second arg).
    '''
    def __init__(self, iterable, max_times=None):
        self._iterable = iterable
        self._max_times = max_times

    def __iter__(self):

        class Repeat_iter(self, iterable, max_times):
            def __init__(self, iterable, max_times):
                self._iterable = iterable
                self._iterator = iter(iterable)
                self._max = max_times

            def __next__(self):
                if self._max != None and self._max <= 0:
                    raise StopIteration
                else:
                    try:
                        return next(self._iterator)
                    except StopIteration: 
                        if self._max:
                            self._max -= 1
                        self._iterator = iter(self._iterable) #restart iterator
                        return next(self)

            def __iter__(self):
                return self

        return Repeat_iter(self, self._iterable, self._max_times)

for i in Repeat("abcde",3):
    print(i,end=''))
#>>> abcdeabcdeabcd


#-------------simpler version-----------
def repeat(iterable):
    '''repeatedly produces value from an iterable over and over again'''
    while True:
        for i in iterable:
            yield i

#same thing as the class   
def repeat(iterable, max_times=None):
    while max_times is None or max_times > 0:
        for i in iterable:
            yield i
        if max_times: max_times -= 1 #not None
```

```python
from collections import defaultdict
class Unique: 
    '''
    Returns all unique values in an iterable. The user gets to decide how
    many times a value can be returned.
    '''
    def __init__(self, iterable, max_times=1):
        self._iterable = iterable
        self._max = max_times

    def __iter__(self):
        class Unique_iter: 
            def __init__(self, iterable, max_times):
                self._times = defaultdict(int)
                self._iterator = iter(iterable)
                self._max = max_times

            def __next__(self):
                answer = next(self._iterator)    #StopIteration?
                while self._times[answer] >= self._max
                    answer = next(self._iterator)    #StopIteration?
                self._timer[answer] += 1
                return answer

            def __iter__(self):
                return self
                
        return Unique_iter(self, self._iterable, self._max)

for i in Unique('Mississippi',2):
    print(i,end='')
#>>> Missipp


#-------------simpler version-----------
def unique(iterable):
'''produce unique values'''
    iterated = set()
    for i in iterable:
        if i not in iterated:
            iterated.add(i)
            yield i
            
#same thing as class
def unique(iterable, max_times=1):
    iterated = defaultdict(int)
    for i in iterable:
        iterated[i] += 1
        if iterated[i] <= max_times:
            yield i
```

```python
class Filter: 
    '''
    The argument predicate dictates whether a value produced with 
    next should be kept or filtered out. When predicate False, then 
    do not return that value and keep iterating. When predicate is True,
    return that value and keep iterating.
    '''
    def __init__(self, iterable, predicate):
        self._iterable = iterable 
        self._predicate = predicate

    def __iter__(self):
        
        class Filter_iter: 
            def __init__(self, iterable, predicate):
                self._iterator = iter(iterable)
                self._predicate = predicate

            def __next__(self):
                answer = next(self._iterator)
                while self._predicate(answer) == False:
                    answer = next(self._iterator)
                return answer 

            def __iter__(self):
                return self 

        return Filter_iter(self._iterable, self._predicate)

for i in Filter('abcdefghijklmnopqrstuvwxyz',lambda x : x not in 'richardpattis'):
    print(i,end='')
#>>> befgjklmnoquvwxyz


#-------------simpler version-----------
def pfilter(iterable, p):
    '''only returns values that returns True when passed to a predicate'''
    for i in iterable:
        if p(i):
            yield i
```

{% hint style="info" %}
Generators embodies a small amount of code and does not often store any large data type. It also produces one value at a time. Therefore generators are space efficient. 
{% endhint %}





