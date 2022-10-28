---
description: 'week 1: "Python Review"'
---

# Python review

## Python in 4 sentences&#x20;

1. **NAMES:** Names (in namespaces) are _**bound**_ to objects.
2. **REFERENCE:** Every object has its own **namespace**.
3. **OBJECTS:** Objects are the fundamental unit with which Python computes.
   1. We can compute with int objects in mathematical operations&#x20;
   2. We can compute with **functions** by calling on them&#x20;
   3. We can compute with a module object by **importing** them&#x20;
   4. We can compute with class objects by constructing **instances** of the class
4. Python has rules about how names are bound and how things work.

### Binding

**Binding:** making a name refer to a value \
\- Using the = symbol\
\- Using imports, function definitions and class definitions

![pictorial representation of binding - adapted from prof. Pattis](<../.gitbook/assets/image (3).png>)

###

#### Using \_ as a variable name

By using \_ your variable is basically 'unnamed' and the interpreter does not expect you to use it.

| not using \_                               | using \_                                                               |
| ------------------------------------------ | ---------------------------------------------------------------------- |
| `kv = [k,v for k, v in [list of 2-tuple]]` | `only_k = [k for k, _ in [list of 2-tuple]]`                           |
|                                            | <p><code>for _ in [1,2,3]:</code></p><p>    <code>print(_)</code> </p> |

### Namespaces (for objects): \_\_dict\_\_

Every object has a special variable named `__dict__` that stores bindings in a dictionary in which keys are objects' names and values are the objects themselves.

`x.a = 1` is equivalent to `x.__dict__['a'] = 1`

### Importing

\#1 Import module-name form\
\- when writing code you use module-name.function

```python
import 'module-name' (, 'more-modules')
import 'module-name' as 'alt-name'
```

\#2 from module-name import\
\- directly use the function name in the code

```python
from module-name import attr-name
from module-name import attr-name as alt-name
from module-name import *
```

### Scope

Visibility of a variable. Can be **local** or **global.** We normally want to avoid using global variables, but if a global variable needs to be rebound, then use the keyword `global` to declare that it is global.

| This is better                                                                                                                                                                                                             | **DON'T**                                                                                                                                                                                         |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><code>x = 1</code></p><p><code>def f():</code><br>   <code>global x</code><br>   <code>y = 2</code></p><p>   <code>print(x,y)</code></p><p>   <code>x = 2</code></p><p><code>f()</code></p><p><code>print(x)</code></p> | <p><code>x = 1</code></p><p><code>def f():</code></p><p>   <code>y = 2</code></p><p>   <code>print(x,y)</code></p><p>   <code>x = 2</code></p><p><code>f()</code></p><p><code>print(x)</code></p> |
| <p>>>> 1 2</p><p>>>> 2</p>                                                                                                                                                                                                 | <p>UnboundLocalError exception: </p><p>local variable 'x' referenced before assignment</p>                                                                                                        |

#### Python looks up / binds names in the following order

1. in the **local** **scope** of the function
2. in any of the **enclosing scopes**&#x20;
3. in the **global scope**
4. in the **builtins scope**
5. raise NameError

### Functions

#### Functions vs Methods

* **Functions:** called by function(args)
* **Methods:** object.method()&#x20;

#### Function Calls

Defining a function is binding a name to it; therefore, you can store a function name (without the '()') into a variable name

```python
def function(x):
    print(1)

one = function #dont include the ()

one('yes')
>>> 1

function('also yes')
>>>1
```

#### Functions can return other functions!

```python
def bigger_than(v) :
    def test(x) :
        return x > v
    return test

old = bigger_than(60)
old(70)
>>> True

bigger_than(60)(70)
>>> True
```

### Lamda

* **lambda:** unnamed function object that represents a very simple function
* **predicate:** a function that returns a boolean

| This                      | Same as                                                             |
| ------------------------- | ------------------------------------------------------------------- |
| `...(lambda x,y: x+y)...` | <p><code>def f(x,y):</code></p><p>    <code>return x + y</code></p> |

examples:&#x20;

```python
print((lambda x : x)('Hello World'))
>>> Hello World

x = prompt.for_int('Enter a value in [0,5]', is_legal = (lambda x : 0<=x<=5))
print(x)
>>> only ints between 0 and 5

def bigger_than(v) :
    return (lambda x : x > v)
```

## Parallel Assignment (aka Sequence unpacking)

```python
l,m,n = (1,2,[3,4])
print(l,m,n)
>>> 1 2 [3, 4]

l,m,(n,o) = (1, 2, [3,4])
print(l,m,n,o)
>>> 1 2 3 4
```

## Statements vs Expressions

* **statements**: cause an effect
  * binding, control structures
* **expressions**: evaluated to compute a result
  * formulas, boolean, etc
* **conditional statement**: use boolean expression to decide which block to execute
* **conditional expression**: use boolean expression to decide what expression to evaluate

```python
#statement
    if x <= y:
          min = x
      else:
          min = y

#expression
    min = (x if x <= y else y)

#statement
    if x % 2 == 0:
          print(x,'is even')
      else:
          print(x,'is odd')

#expression
    print(x, ('is even' if x%2 == 0 else 'is odd'))
```

## Arguments vs Parameters

* **arguments:** passed to a function when **calling** it&#x20;
  * **positional argument:** an argument NOT preceded by `name=`
  * **named argument**: argument preceded by the `name=` option
    * must be written at the end of the function call
* **parameters:** values in a function **header**
  * **name-only parameter:** a parameter with NO default value
  * **default-argument parameter:** a parameter including a default argument value

#### How matching works?

* M1. match positional arguments to the respective parameter&#x20;
* M2. if \* is an arg, then store all remaining pos args in a tuple and match&#x20;
* M3. match named args to the named parameter&#x20;
* M4. default args&#x20;
* M5. exceptions

### \*args and \*\*kargs

* `*args:`&#x20;
  * puts all positional arguments that have not been bound into a tuple&#x20;
* `**kargs:` keyword arguments
  * there are more named arguments than named parameters, then it will store the extra arguments in a dictionary named kargs

```python
def f(a,b,**kargs):
        print(a,b,kargs)
f(c=3, a=1, b=2, d=4) == 1 2 {'c': 3, 'd': 4}
```

{% hint style="info" %}
The following is the general way to get all kinds of arguments:
{% endhint %}

```python
def g(*args, **kargs):
    print(args, kargs)
    print(*args)
    print(*kargs)
    print(dict(**kargs))
    
g(1,2,c=3,d=4)
>>> (1, 2) {'c': 3, 'd': 4}
>>> 1 2
>>> c d
>>> {'c': 3, 'd': 4}
```

#### Important notes

1. Writing `*` __ and _`**`_ for parameters, binds said parameter to tuple/dict resp.&#x20;
2. A parameter name by itself is using the tuple/dict
3. Using _ `*`_ and `**` followed by parameter name as arguments in fxn calls, _expands_ all the values in the tuple/dict to represent all arguments

## None and pass

* None is a value / instance / object of NoneType
  * all functions return None as default
* pass: inside a body, means do nothing&#x20;

```python
def function() -> None:
    pass
```

## Indexable Objects - slicing

* STL = String, Tuple, and List are indexable objects
  * then they can be _**sliced**_: STL\[start: end: step]

## Else block

It is executed when a loop terminates naturally (without using break)

```python
for i in irange(100):
    if special_property(i):
        print(i,'is the first value with the special property')
        break
    else:
        print('No value in the range had the special property')
```

## Must-know functions

### str.split( ) and str.join()

```python
'ab;c;ef;;jk'.split(';')
# returns ['ab', 'c', 'ef', '', 'jk']

[s for s in 'ab;c;ef;;jk'.split(';') if s != '']
#returns ['ab', 'c', 'ef', 'jk']

';'.join(['ab', 'c', 'ef', '', 'jk']) == 'ab;c;ef;;jk'
```

### all(iterable) and any(iterable)

* `all()` returns True if all values in iterable produce True.
* `any()` returns True if at least one value in iterable produce True

```python
x = all( predicate.is_prime(x) for x in l )
```

### max() and min()

```python
max('abcd','xyz') == 'xyz'
min('abcd','xyz') == 'abcd'
min('abcd','xyz', key= lambda x: len(x)) == 'xyz'
```

### zip(iterables)

Takes iterable objects (can be different lengths) and glues respective elements into a tuple and returns a generator as a result.

```python
z = list(zip( 'abcde', (1, 2, 3) )) == [('a', 1), ('b', 2), ('c', 3)]

for v1,v2 in zip ( ('a','b','c','d','e'), (1,2,3) ):
print(v1,v2)
>>> a 1
>>> b 2
>>> c 3
```

### enumerate(iterable)

Takes one iterable (and an optional starting number) and produces a generator.

```python
e = enumerate(['a','b','c','d'], 5)
print(list(e))
>>> [(5, 'a'), (6, 'b'), (7, 'c'), (8, 'd')]
```

## Exceptions

Exceptions are _raised_ and we _handle_ them. A function raises an exception when it fails to finish its job.&#x20;

* `raise` allows you to throw an exception at any time.
* `assert` verifies if a given condition is met and throws an `AssertionError` exception if it isn’t.

| Good and simple                 | Inefficient                                                                                   |
| ------------------------------- | --------------------------------------------------------------------------------------------- |
| `assert x >= 0, 'not positive'` | <p><code>if x &#x3C; 0:</code></p><p>   <code>raise AssertionError('not positive')</code></p> |

We can also handle exceptions with a `try-except` statement.

* The `try` body is executed normally until an exception is raised.
* `except` catches and handles the exception(s).
* `else` body will run only when the try finishes without any exception&#x20;
* `finally` body will always execute no matter if there's an exception or not

```python
def prompt_for_int(prompt_text):
    while True:
        try:
            response = input(prompt_text+': ')    # response is used in except
            answer = int(response)
            return answer
            
        except ValueError: #handle only this specific exception
            print(f'dummy dummy, {response} is not an int')
            
        #OR you could create an instance of ValueError exception class 
        #and use its attributes and behaviors
        except ValueError as e: 
            print(e) #will print the Python ValueError message
            
        except: #generic 
            #will catch ANY exception
            
        else: 
            #will only execute if the code in try body finishes naturally
            
        finally: 
            #will always execute no matter what
        


print(prompt_for_int('Enter a positive number'))
```

##
