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





  


