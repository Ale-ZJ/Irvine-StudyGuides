# Recursion

## Proof Rules

1. Base case 
2. Smaller input 
3. Combine everything while assuming all recursion works

## Divide and Conquer

* BASE CASE
  * smallest thing you can plug in 
    * a lil brute force
    * often element = 1 or empty
* DIVIEDE - SMALLER INPUT
  * number decreased somhow 
    * slicing, splitting 
  * leave behind the first element of a list \(inefficient but always works\)
  * sorting \(n/2\) \(efficient but not always work\)
    * divide in halves
      * takes one element as pivot
* MERGE AND CONQUER
  * assume recursive function works 
  * how to combine things together 
  * **ONE LEVEL APPROACH - do not think about all the layers**
    * non trivial instance of a problem
    * compute what your final answer should be \(no code\)
    * make your split and apply recursive functions appropriately

Tail recursive 

## Functional Programming

* no local variable and no mutation
* kinda like comprehension

Three higher-order functions that are common in functional programming are:

### Map 

Also known as Transform. 

Takes a unary function and some list/iterable and produces a list/iterable of transformed values by substituting each value with the result of calling the function on values

```python
#with recursion
def map_l(f,alist):
    if alist == []:
        return []
    else:
        return [f(alist[0])] + map_l(f,alist[1:])

#with comprehension        
def map_l(f,alist):
    return [f(i) for i in alist]
    
#with generators
def map_l(f,iterable):
    for i in iterable: 
        yield f(i)
        
#the actual Python code looks more like
def map(f,*iterables):
    for args in zip(*iterables):
        yield f(*args)
```

### Filter

Takes in a predicate and some list/iterable and returns only values that evalues to True.

```python
#with recursion 
def filter_l(p,alist):
    if alist == []:
        return []
    else:
        if p(alist[0]):
            return [alist[0]] + filter_l(p,alist[1:])
        else:
            return filter_l(p,alist[1:])
            
#with comprehension 
def filter_l(p,alist):
    return [i for i in alist if p(i)]
    
#with generator #Python code looks like this one
def filter_i(p,iterable):
    for i in iterable:
        if p(i):
            yield i
```

### Reduce

Different from the ones above. Takes in an iterable

```python
reduce(lambda x,y: x-y, [1,2,3])
>>> (((1-2)-3)-4)
>>> -7
```

```python
#reducing from the left
def reduce(f,iterable):
    i = iter(iterable)         # create iterator
    a = next(i)                 # get its first value
    while True:                # while (to process more values in iterator)
        try:                     #try (to determine if more values)
            a = f(a,next(i))    #get next:f combines with previous result
        except StopIteration:     #when no more values in iterator
            return a            #return reduced/accumulated result
            
#reducing from the right            
def right_reduce_l(f,alist,unit):
    if alist = []:
        return unit
    else:
        return f( alist[0], right_reduce_l(f,alist[1:],unit) )
```

