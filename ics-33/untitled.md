---
description: >-
  week 3: 'Python classes' + 'Operator Overloading I' + 'Operator Overloading
  (continued)' and week 4: 'Iterators'
---

# Classes

When we define a class in Python, we bind the class name to an object that represents the entire class. Then, we can create instances of this class by using the given class name.

#### Python does three things when constructing new instance objects:

1. Python calls a special function that _constructs_ an empty `__dict__`.
2. Python calls the class' `__init__(self, *args, **kargs)` with self as the object created in (1). It also _initializes_ attributes by binding their name with their respective values in the namespace `__dict__`.
3. A reference to the object that was **created in (1)** and **initialized in (2)** is returned.

## Types of variables&#x20;

| name                          | definition and use                                                                                                                                                                                                                                                           |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| local variables               | <ul><li>defined and used inside functions/methods.</li><li>parameters considered local vars too </li><li>local var dies when the function finishes execution</li></ul>                                                                                                       |
| instance attributes/variables | <ul><li>defined in <code>__init__</code> and used inside class methods</li><li><code>self.name</code></li><li>exists while the instance object exists</li></ul>                                                                                                              |
| class attributes/variables    | <ul><li>defined in classes (same level as methods)</li><li><p>stores info common to all objects of a class </p><ul><li>info shared among all objects created from class</li></ul></li><li>methods are actually <em>class attributes</em> bound to function objects</li></ul> |
| global variables              | <ul><li>defined in modules </li><li>use them with precaution</li></ul>                                                                                                                                                                                                       |

{% hint style="info" %}
**The Fundamental Equation of Object-Oriented Programming (FEOOP)** tells us that Python translates the method call `o.m(p)` to the function call `type(o).m(o,p)`
{% endhint %}

When calling `o.attr`, Python tries to find the attr in instance o's `__dict__` first. If it doesn't find it there, then by FEOOP checks `type(o).attr` and checks for the class' `__dict__`.

#### Bob's finger example

```python
class Person:
    fingers = 10
    def __init__(...):
        #no attribute for self.fingers
        
bob = Person(...)
bob.fingers #returns 10
'''
bob.fingers doesnt exist bc no instance variable fingers in init
type(bob).fingers by FEOOP
Person.fingers returns 10 bc it is the class var
'''

#bob had an accident and lost one finger
bob.fingers = 9 #creates a instance variable in __dict__ 
Person.fingers = 10 #the class fingers doesnt change but the bob intance fingers does


```

## Private variables

We usually don't want clients or outside code to directly access class attributes. Indirect access can be given by writing 'getters' and 'setters':

* **accessors** (or query): o.method(...) returns information about o's state without changing it
* **mutator** (or command): they change o's state

Python allows us to hide variables or attributes in two ways (although neither truly prohibits access to the variables' content):

#### \_ underscore prefix

`_variable` indicates that the variable should not be accessed outside, but Python does not enforce this. So this is a nice convention and is up to the programmer to follow.

```python
class C:
    def __init__(self):
        self._ia = 1
    def _f(self):
        return self._ia == 1
        
o = C()
print(o._ia, o._f())
>>> 1 True
```

#### \_\_ double underscore prefix

`__variable` is harder to access outside the class than with a single underscore because you need to include a **mangled name** that includes the name of the class. In other words, because`__dict__` stores `__vars` as `_Classname__vars` you need to use the second way to access a double underscore attribute.

```python
class C:
    def __init__(self):
        self.__ia = 1
    def __f(self):
        return self.__ia == 1

o = C()
print(o.__ia, o.__f()) 
>>> AttributeError: 'C' object has no attribute '__ia' #or '__f'
#because there is no '__ia' nor '__f' in __dict__

#instead we would need to add a mangled name containing the class name
print(o._C__ia, o._C__f())
>>> 1 True
```

## Operator Overloading

Overloading is giving more than one job to one function name. It usually differentiates between each job based on the arguments.&#x20;

Python has special methods used to manage classes. You can think of them as magic methods haha. Their technical name is 'dunder' + method name and they are surrounded by two underscores, e.g. `__init__` . **Remember FEOOP when overloading!**&#x20;

| purpose                            | dunder method                                                                                                                                                                                                                              |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| misc                               | `__init__, __bool__, __len__, __str__/__repr__`                                                                                                                                                                                            |
| relational                         | <p><code>__lt__ (__gt__, __le__, __ge__, __eq__, __ne__)</code><br>represents: <code>&#x3C; (>, &#x3C;=, >=, ==, and !=)</code></p>                                                                                                        |
| unary                              | `__neg__, __pos__, __invert__` represents: ( - + \~)                                                                                                                                                                                       |
| unary functions                    | `__abs__, __round__, __float__, __floor__, __ceil__, __trunc__`                                                                                                                                                                            |
| arithmetic                         | <p><code>__add__, __sub__, __mul__, __truediv__, __floordiv__, __mod__, __divmod__, __pow__, __lshift__, __rshift__, __and__, __or__, __xor__</code> </p><p>represents: <code>+ - * / // % divmod ** &#x3C;&#x3C; >> &#x26; | ^</code></p> |
| right arithmetic                   | <p>there's a right form for each of the arithmetic methods so:</p><p><code>__radd__, __rsub__, __rmul__</code> ...</p>                                                                                                                     |
| arithmetic incrementing delimeters | `__iadd__, __isub__, __imul__` ... represents: `+=, -=, *=` ...                                                                                                                                                                            |
| container                          | <p><code>__getitem__, __setitem__, __delitem__, __contains__</code> </p><p>represents: <code>l[-1], l[2] = 3, del l[2], 3 in l</code></p>                                                                                                  |
| function call                      | `__call__`                                                                                                                                                                                                                                 |
| iterators                          | `__iter__, __next__,`                                                                                                                                                                                                                      |
| attributes                         | `__getattr__, __setattr__, __delattr__`                                                                                                                                                                                                    |
| context managers                   | `__enter__, __exit__`                                                                                                                                                                                                                      |

### Misc&#x20;

They are all parameterless except for init

| name       | description                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `__init__` | <p>cmon you have used it before</p><ul><li>initializes a class by binding attr names to values in <code>__dict__</code></li></ul>                                                                                                                                                                                                                                                                                                                  |
| `__len__`  | <ul><li>when we call <code>len(object)</code> </li><li><p>ints do not have a len implementation </p><ul><li>TypeError: object of type 'int' has no len()</li></ul></li></ul>                                                                                                                                                                                                                                                                       |
| `__bool__` | <ul><li>called whenever Python needs to interpret some object as a boolean value</li><li>like test conditions for <code>if</code> or <code>while</code> statements</li><li>if there is no <code>__bool__</code> implementation in the class, Python return <code>len(object) != 0</code> . If there is no <code>__len__</code> function then Python returns True.</li></ul>                                                                        |
| `__str__`  | <ul><li>called when we call the conversion function <code>str(object)</code> </li></ul>                                                                                                                                                                                                                                                                                                                                                            |
| `__repr__` | <ul><li>When <code>print()</code> or <code>format()</code> is called and there's no <code>__str__</code> , then we use <code>__repr__</code>as a backup</li><li>if there is no <code>__repr__</code> then Python uses the location of the object. e.g. <code>&#x3C;__main__.C object at [address]></code> </li><li>convention: it returns a string that when passed to <code>eval()</code> it will produce an object with the same state</li></ul> |

```python
class Vector:
    def __init__(self,*args):
        self.coords = args
        
    def __len__(self):
        print('Calling __len__')
        return len(self.coords) #len of the tuple coords
        
    def __bool__(self):
        print('Calling __bool__')
        return any( v!=0 for v in self.coords ) #not in origin\
        
    def __repr__(self):
        return 'Vector(' + ','.join(str(c) for c in self.coords) + ')'
        
    def __str__(self):
        return '(' + str(len(self)) + ')' + str(list(self.coords)) 
        
```

### Relational

{% hint style="info" %}
Technically you can write all the other dunder relational operators using only `< or > or <= or >=`
{% endhint %}

| expression              | description                                                                                               |
| ----------------------- | --------------------------------------------------------------------------------------------------------- |
| `12 < y`                | when evaluating this                                                                                      |
| `12.__lt__(y)`          | Python interprets this by feoop                                                                           |
| `type(12).__lt__(y)`    | which then translates to                                                                                  |
| `int.__lt__(12, y)`     | then uses the `__lt__` defined in the int class                                                           |
| `type(y).__gt__(y, 12)` | if `__lt__` was `NotImplemented` in the int class, then Python will try to evaluate the opposite `y > 12` |

**Relational operations are simetrical/mirrored**, so they serve as 'backups' that can be called when one operation is `NotImplemented`

* `__lt__` and `__gt__`&#x20;
* `__le__` and `__ge__`

&#x20;Except for these special cases. The mirror is not used as the backup.

* `__eq__` and itself&#x20;
  * when no `__eq__` is defined, Python uses the `is` operator
* `__ne__` and itself&#x20;
  * when no `__ne__` defined, Python uses `__eq__` and negates it

{% hint style="info" %}
`is` and `==` are not the same! `is` evaluates if left and right refers to the SAME OBJECT, while `==` compare objects by their state.
{% endhint %}

```python
def __lt__(self,right):
    if type(right) is Vector:
        return self.distance() < right.distance()
        
    elif type(right) in (int,float):
        return self.distance() < right
        
    else:
        return NotImplemented
```

### Arithmetic

For binary arithmetic operator Python can't have a mirror backup operator ( `1-v` not the same as `v-1`) because the commutative property doesn't apply to all operands. Instead Python lets us define a 'right hand side' operation.

{% hint style="danger" %}
Do not mutate `self` and remember to **return** a new Class with the new results
{% endhint %}

| left hand operator                                                                            | right hand operator                                                                                                               |
| --------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| <p><code>def __add__(self, right):</code></p><p>   <code>#some implementation here</code></p> | <p><code>def __radd__(self, left):</code></p><p>   <code>return self + left</code></p><p>*works here because of commutativity</p> |

```python
def __add__(self,right):
    if type(right) is Vector:
        assert len(self) == len(right), 'Vector.__add__: operand self('+str(self)+') has different dimension that operand right('+str(right)+')'
        return Vector( *(c1+c2 for c1,c2 in zip(self.coords,right.coords)) )
    
    elif type(right) in (int,float):
        return Vector( *(c+right for c in self.coords) )
        
    else:
        return NotImplemented

v = Vector(0,0)
v1 = Vector(0,1)
v2 = Vector(2,2)

print(v1+v2)
>>> (2)[2, 3]

print(v1+1.) 
>>> (2)[1., 1.]

print(1+v) # TypeError (comes from the NotImplemented)

def __radd__(self,left):
    if type(left) not in (int,float): 
        return NotImplemented
        
    return Vector( *(left+c for c in self.coords) )
    
print(1+v)
>>> (2)[1, 1]
```

### Containers

```python
class List1:
    def __init__(self,_plist):
        self._plist = list(_plist)
        
    def __str__(self):
        return str(self._plist)
    
    @staticmethod
    def _fix_index(i):
        if i == None:
            return None
        else:
        # for positive indexes, 1 smaller: 1 -> 0
        # for - indexes, the same: -1 (still last), -2 (still 2nd to last
            return (i-1 if i >= 1 else i)
    
    def __getitem__(self,index):
        print('List1.__getitem__('+str(index)+')') # for illumination/debugging
        if type(index) is int:
            return self._plist[List1._fix_index(index)]
        elif type(index) is slice:
            s = slice(List1._fix_index(index.start), List1._fix_index(index.stop), index.step)
            return self._plist[s]
        else:
            raise TypeError('List1.__getitem__ index('+str(index)+') must be int/slice')
    
    def __setitem__(self,index,value):
        print('List1.__setitem__('+str(index)+','+str(value)+')') # for illumination/debugging
        if type(index) is int:
            self._plist[List1._fix_index(index)] = value
        elif type(index) is slice:
            s = slice(List1._fix_index(index.start), List1._fix_index(index.stop), index.step)
            self._plist[s] = value
        else:
            raise TypeError('List1.__setitem__ index('+str(index)+') must be int/slice')
    
    def __delitem__(self,index):
        print('List1.__delitem__('+str(index)+')') # for illumination/debugging
        if type(index) is int:
            del self._plist[List1._fix_index(index)]
        elif type(index) is slice:
            s = slice(List1._fix_index(index.start), List1._fix_index(index.stop), index.step)
            del self._plist[s]
        else:
            raise TypeError('List1.__delitem__ index('+str(index)+') must be int/slice')
   
     def __getattr__(self,attr): # if attr not here, try self._plist
        return getattr(self._plist,attr)
        
    def __len__(self):
        return len(self.thislist)
    
    def __contains__(self,item):
        return item in self._plist
```

### Attributes

{% hint style="danger" %}
**Be careful when overloading** `__setattr__` since it gets called by `__init__` to initialize attributes. \
**ALWAYS** include `self.__dict__[name] = value` somewhere in the `__setattr__` . \
**NEVER** write `pass` in `__setattr__` body.
{% endhint %}

```python
class C:
    '''Class that remembers all its attributes'''
    def __init__(self):
        self._history = defaultdict(list)
        self.s = 0

    def bump(self):
        self.s += 1

    def __setattr__(self, name, value):
        #when you first initialize the class, history doesnt exist yet
        if '_history' in self.__dict__:         
            self._history[name].append(value)

        self.__dict__[name] = value #ALWAYS INCLUDE
```

### Function call

```python
class Product: 
    def __init__(self): 
        print("Instance Created") 
  
    # Defining __call__ method 
    def __call__(self, a, b): 
        print(a * b) 
  
# Instance created 
ans = Product() 
  
# __call__ method will be called 
ans(10, 20) 

#taken from https://www.geeksforgeeks.org/__call__-in-python/
```

### Iteration

Whenever you call something that loops, Python translates it into a while loop.

```python
while True:
    statements
    if test:
        break
    statements
    
while not (test):     # not is HIGH precedence, so I put test in ()
    statements         # not True and False = (not True) and False = False
                        # not (True and False) = True
```

{% hint style="info" %}
In Python "not" has a higher precedence than "and", which has a higher precedence than "or" (think of "not" like unary "-", "and" like "\*", and "or" like "+").&#x20;
{% endhint %}

{% hint style="info" %}
`__next__`is called repeated/automatically, in the while-loop translation of a Python for-loop.
{% endhint %}

* `iter(iterable)` returns an **iterator**, an object which `next()` can be called on.
* `next(it)` returns a value and advances the state of the iteration, until no more values, then raise `StopIteration`&#x20;

#### From for loop to while loop&#x20;

```python
for i in range(1,6): # for the values 1-5
    print(i)
else:
    print('executing else')
    
#is translated into
_hidden = iter(range(1,6))
try:
    while True:
    i = next(_hidden)
    print(i)
except StopIteration:
    pass
else: 
    print('executing else')
finally:
    del _hidden
    
    
def sum_dif(iterable):
    answer = 0
    i = iter(iterable)
    v2 = next(i) # get first value (loop moves v2 to v1)
    try:
        while True: # next gets one new value in each loop
            v1, v2 = v2, next(i) # first time, v1 is 1st value, v2 is 2nd
            answer += abs(v1-v2)
    except StopIteration:
        pass
    return answer
```

#### Class Implementation of iterator

```python
class Countdown:
    def __init__(self,start):
        self.start = start # self.start never changes; see self.n in __iter__

# __iter__ must return an object on which __next__ can be called; it returns
# self, which is an object of the Countdown class, which defines __next__.
# Later we will see a problem with returning self (when the same Countdown
# object is iterated over in a nested structure), and how to solve that
# problem.
    def __iter__(self):
        self.n = self.start # n attribute is added to the namespace here
        return self # (not in __init__) and processed in __next__

    def __next__(self):
        if self.n < 0:
            raise StopIteration # can del self.n here, after exhausting iterator
        else:
            answer = self.n # or, without the temporary, but more confusing
            self.n -= 1 # self.n -= 1
            return answer # return self.n+1
            
            
cd = Countdown(10)

for i in cd:
    print(str(i)+', ',end='')
    print ('blastoff')
    
for i in cd:
    print(str(i)+', ',end='')
    print ('blastoff')
    
It print:
10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0, blastoff
10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0, blastoff


#BUT THERE IS A PROBLEM! 

for a in Countdown(2):
    for b in Countdown(2):
        print(a,b)
'''
prints
2 2 
2 1 
2 0 
1 2
1 1
0 0
0 2
0 1
0 0
'''

c = Countdown(2)
for a in c:
    for b in c:
        print(a,b)
'''
2 2
2 1
2 0
'''
```

to avoid the precious problem we define a class to keep track of information. ?????&#x20;

```python
def __iter__(self):
    class prange_iter:
        def __init__(self,start,stop,step):
            self.n = start
            self.stop = stop
            self.step = step
        
        def __next__(self):
            if self.step > 0 and self.n >= self.stop or \
            self.step < 0 and self.n <= self.stop:
                raise StopIteration
            save = self.n
            self.n += self.step
            return save
        
        def __iter__(self):
            return self
            
return prange_iter(self.start, self.stop ,self.step)
```



