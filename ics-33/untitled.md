---
description: >-
  week 3: 'Python classes' + 'Operator Overloading I' + 'Operator Overloading
  (continued)'
---

# Classes

When we define a class in Python, we bind the class name to an object that represents the entire class. Then, we can create instances of this class by using the given class name.

#### Python does three things when constructing new instance objects:

1. Python calls a special function that _constructs_ a new instance object of the class and has an empty `__dict__`.
2. Python calls the class' `__init__(self, *args, **kargs)` with self as the object created in \(1\). It also _initializes_ attributes by binding their name with their respective values in the namespace `__dict__`.
3. A reference to the object that was **created in \(1\)** and **initialized in \(2\)** is returned.

## Types of variables 

<table>
  <thead>
    <tr>
      <th style="text-align:left">name</th>
      <th style="text-align:left">definition and use</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">local variables</td>
      <td style="text-align:left">
        <ul>
          <li>defined and used inside functions/methods.</li>
          <li>parameters considered local vars too</li>
          <li>local var dies when the function finishes execution</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">instance attributes/variables</td>
      <td style="text-align:left">
        <ul>
          <li>defined in <code>__init__</code> and used inside class methods</li>
          <li><code>self.name</code>
          </li>
          <li>exists while the instance object exists</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">class attributes/variables</td>
      <td style="text-align:left">
        <ul>
          <li>defined in classes (same level as methods)</li>
          <li>stores info common to all objects of a class</li>
          <li>methods are actually<em> class attributes </em>bound to function objects</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">global variables</td>
      <td style="text-align:left">
        <ul>
          <li>defined in modules</li>
          <li>use them with precaution</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

{% hint style="info" %}
**The Fundamental Equation of Object-Oriented Programming \(FEOOP\)** tells us that Python translates the method call `o.m(p)` to the function call `type(o).m(o,p)`
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

* **accessors** \(or query\): o.method\(...\) returns information about o's state without changing it
* **mutator** \(or command\): they change o's state

Python allows us to hide variables or attributes in two ways \(although neither truly prohibits access to the variables' content\):

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

Overloading is giving more than one job to one function name. It usually differentiates between each job based on the arguments. 

Python has special methods used to manage classes. You can think of them as magic methods haha. Their technical name is 'dunder' + method name and they are surrounded by two underscores, e.g. `__init__` . **Remember FEOOP when overloading!** 

<table>
  <thead>
    <tr>
      <th style="text-align:left">purpose</th>
      <th style="text-align:left">dunder method</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">misc</td>
      <td style="text-align:left"><code>__init__, __bool__, __len__, __str__/__repr__</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">relational</td>
      <td style="text-align:left"><code>__lt__ (__gt__, __le__, __ge__, __eq__, __ne__)</code>
        <br />represents: <code>&lt; (&gt;, &lt;=, &gt;=, ==, and !=)</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">arithmetic</td>
      <td style="text-align:left">
        <p><code>__add__, __sub__, __mul__, __truediv__, __floordiv__, __mod__, __divmod__, __pow__, __lshift__, __rshift__, __and__, __or__, __xor__</code> 
        </p>
        <p>represents: <code>+ - * / // % divmod ** &lt;&lt; &gt;&gt; &amp; | ^</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">right arithmetic</td>
      <td style="text-align:left">
        <p>there&apos;s a right form for each of the arithmetic methods so:</p>
        <p><code>__radd__, __rsub__, __rmul__</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">container</td>
      <td style="text-align:left">
        <p><code>__getitem__, __setitem__, __delitem__, __contains__</code> 
        </p>
        <p>represents: <code>l[-1], l[2] = 3, del l[2], 3 in l</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">function call</td>
      <td style="text-align:left"><code>__call__</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">iterators</td>
      <td style="text-align:left"><code>__iter__, __next__, </code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">attributes</td>
      <td style="text-align:left"><code>__getattr__, __setattr__, __delattr__</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">context managers</td>
      <td style="text-align:left"><code>__enter__, __exit__</code>
      </td>
    </tr>
  </tbody>
</table>

example of setattr

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

        self.__dict__[name] = value
```

### Misc 

They are all parameterless except for init

<table>
  <thead>
    <tr>
      <th style="text-align:left">name</th>
      <th style="text-align:left">description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>__init__</code>
      </td>
      <td style="text-align:left">
        <p>cmon you have used it before</p>
        <ul>
          <li>initializes a class by binding attr names to values in <code>__dict__</code>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>__len__</code>
      </td>
      <td style="text-align:left">
        <ul>
          <li>when we call <code>len(object)</code> 
          </li>
          <li>ints do not have a len implementation</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>__bool__</code>
      </td>
      <td style="text-align:left">
        <ul>
          <li>called whenever Python needs to interpret some object as a boolean value</li>
          <li>like test conditions for <code>if </code>or <code>while </code>statements</li>
          <li>if there is no <code>__bool__</code> implementation in the class, Python
            return <code>len(object) != 0</code> . If there is no <code>__len__</code> function
            then Python returns True.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>__str__</code>
      </td>
      <td style="text-align:left">
        <ul>
          <li>called when we call the conversion function <code>str(object)</code> 
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>__repr__</code>
      </td>
      <td style="text-align:left">
        <ul>
          <li>When <code>print()</code> or <code>format()</code> is called and there&apos;s
            no <code>__str__</code> , then we use <code>__repr__</code>as a backup</li>
          <li>if there is no <code>__repr__</code> then Python uses the location of the
            object. e.g. <code>&lt;__main__.C object at [address]&gt;</code> 
          </li>
          <li>convention: it returns a string that when passed to <code>eval()</code> it
            will produce an object with the same state</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

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

  


