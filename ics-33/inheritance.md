# Inheritance

1. A derived class should take advantage of the base class and implement only methods and attributes that it needs.
   1. IS-A relationship between a derived class and the base class
   2. HAS-A relationship others
2. Inheritance augments the FEOOP look-up rule for attributes.&#x20;
   1. Python uses the tuple of classes in order `__mro__` (m... resolution order) to dictate the order in which attributes will be looked in namespaces `__dict__`&#x20;
3. When calling a method from a base class in  the derived class use the `BaseClassName.method(self, argsssss)` method
4. Because the derived class calls the `__init__` from the base class, the derived class will often take more args for initialization&#x20;
5. A derived class shouldn't directly access data attributes from any base class&#x20;
   1. The base class should provide methods to access or mutate

## Abstract Base Classes (ABC)

1. Special kind of class&#x20;
2. Directly derived from `abc.ABC`
3. Can't be instantiated because
4. Contains **abstract methods**&#x20;
   1. uses the `@abc.abstractmethod` decorator&#x20;
   2. inherited from another ABC&#x20;
5. Its only purpose is to be derived from&#x20;
6. The class that derives from it must eventually define all those abstract methods.

```python
import math # use pi
import abc # use the class ABC and the function abstractmethod (a decorater)

# Shape is
class Shape(abc.ABC):
    @abc.abstractmethod
    def perimeter(self): pass # will be overridden in some derived class
    
    @abc.abstractmethod
    def area(self): pass # will be overridden in some derived class
    
    def one_dimensional(self): # concrete: works by calling an abstract method
        return self.area() == 0
```

```python
class Square(Shape):
    def __init__(self,side) : self.side = side
    def __repr__(self) : return f'Square(side={self.side})'
    def perimeter(self) : return 4*self.side
    def area(self) : return self.side**2
    
class Circle(Shape):
    def __init__(self,radius): self.radius = radius
    def __repr__(self) : return f'Circle(radius={self.radius})'
    def perimeter(self) : return math.pi * 2*self.radius
    def area(self) : return math.pi * self.radius**2
```
