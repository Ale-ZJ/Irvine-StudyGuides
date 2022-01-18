# Reading Notes

## 1 Preliminaries

### 1.1 Why study Concepts of Programming Languages

* Express ideas better
  * appreciation for valuable programming language features
* Better background for choosing appropriate languages
* Increased ability to learn new languages
* Better understanding of the significance of implementation&#x20;
  * debugging
  * visualize how a computer executes carious language constructs
* Better use of languages you already know
* Overall advancement of computing

### 1.2 Programming Domains

* **Scientific**
  * first digital computers (late 1940s) were invented for this domain
    * simple data structures
    * large floating point arithmetic computations
    * arrays and matrices
    * counting loops and selections
  * efficiency primary concern
    * Fortan&#x20;
* **Business**
* **Artificial Intelligence**
* **Systems Programming**
  * operating systems&#x20;
* **Web Software**
  * markup (not programming) languages: HTML
  * Java

## 12 Object Oriented Programming

Smalltalk is considered the base model for purely object oriented programming.

Three key features of OO Programming:&#x20;

1. abstract data types
   1. Variables are an abstraction to computer memory&#x20;
2. inheritance
3. dynamic binding&#x20;

### Inheritance

Inheritance solves the minor modifications problem posed by abstract data type reuse and the program organization problem.

* intances of **classes** are called objects
* a class that inherits from another class is called **derived class** or **subclass**
* a class derived from is the **parent** **class** or **superclass**
* **methods** are subprograms that define a job
* calls to methods: **messages**
* collection of methods of one class is the **message protocol/interface**
  * messages are not calls of methods!&#x20;
  * messages to an object is a request to execute one of its methods

A class has two kinds of methods and variables:

* **instance methods** and **instance variables:** every object instantiation of a class has these
* **class methods** and **class variables:** belongs to the class rather than individual objects

One disadvantage of inheritance is that it introduces dependencies. Abstract data types are independent of each other.

### Dynamic Dispatch

polymorphism provided by the dynamic binding of messages to methods definitions ???&#x20;

Dynamic Dispartch is the mechanism by which a call to an overriden method is resolved at run time, rather than compile time&#x20;

* **abstract method:** (_pure virtual method_ in C++) no implementation only definition
* **abstract class:** (_abstract base class_ in C++) class with at least one abstract method
  * CANNOT BE INSTANTIATED because it is missing implementations
  * classes that derive from abstract classes MUST provide missing implementations

### OO Issues

#### Exclusivity of Objects

If all classes were objects, programming would be elegant and pure and uniform. But there is a disadvantage: all operation smust be done through the message-passing process, which can take longer than simple mahcine instruction for primitive simple types. There is no distinction between predefined and user-defined classes.&#x20;

That's why some classes work around this problem by having primitive types. But that would require the need for wrapper classes.

#### Are Subclasses Subtypes?

A derived class is a subtype if it has an **is-a** relationship with its parent class.&#x20;

* The method of that subclass that override parent class methods must be type compatible, have the same number of parameters, and same return type

#### Single and Multiple Inheritance

Multiple inheritance is very useful but it has complex dependencies and maintaining code is hard. It also raises a lot of ambiguiety when a child inherits from two non-related parents.&#x20;

Interfaces are an alternative to multiple inheritance.&#x20;

C++ supports multiple inheritance.

Java only has single inheritance but a way around is to use interfaces

### OO Programming in C++

C++ was the frist videly used object oriented programming. Smalltalk being the first one considered OO.&#x20;

#### General Characteristics&#x20;

* C++ built for efficiency!
  * therefore design choices found in C++ language are often the most efficient (that doesnt mean the most flexible)
* C++ retain the type system of C and adds classes to it
* Supports both prodedural programing and object oriented programming&#x20;
* Objects in C++ can be static, stack dynamic or heap dynamic&#x20;
  * `delete` to deallocate memory stored in the heap&#x20;
* class definitions include a destructor
* programmer can specify whether an object will be static or dynamic bind
  * static is faster (allocated before runtime in the compiler)
  * dynamic is more flexible (allocated at runtime)

#### Inheritance&#x20;

* C++ class can be derived from an existing class or can be stand alone&#x20;
* class are composed of data members and data functions&#x20;
  * can be private, protected or public
* all objects must be initialized before they are used&#x20;
  * classes must have at least one contructor&#x20;
    * if not defined the compiler creates a default one&#x20;

#### Dynamic Binding&#x20;

* Publicly derived subclasses are subtypes if none of the members of the base class are private
* member functions that must be dynamically bound must be declared to be virtual functions with the keyword `virtual` in the function definition

```
Square sq;         // Allocate a Square object on the stack
Rectangle rect;    // Allocate a Rectangle object on the stack
rect = sq;         // Copies the data member values from the Square object
rect.draw();       // Calls the draw from the Rectangle object
```

### Implementation of Object-Oriented Constructs

We will look at storage structures for instance variables and the dynamic bindings of messages to methods.

#### Instance Data Storage

* In C++. classes are defined as extensions of C's `struct`
  * storage structure used is that of a record
    * this is called Class Instance Record (CIR)
    * built at compile time

#### Dynamic Binding of Method Calls to Methods

dynamic methods must have entries in the CIR

```
public class A {
    public int a, b;
    public void draw() { . . . }
    public int area() { . . . }
}

public class B extends A {
    public int c, d;
    public void draw() { . . . }
    public void sift() { . . . }
}
```

![](<../.gitbook/assets/image (15).png>)

```
class A {
public:
    int a;
    virtual void fun() { . . . }
    virtual void init() { . . . }
};

class B {
public:
    int b;
    virtual void sum() { . . . }
};

class C : public A, public B {
public:
    int c;
    virtual void fun() { . . . }
    virtual void dud() { . . . }
};
```

![](<../.gitbook/assets/image (12).png>)

## Variables

A variable is an abstraction of a computer memory cell.&#x20;

A variable has 6 characteristics:&#x20;

* **name:** string that denotest he variable, most variables have a name, some don't, most names start with a character not a number
  * often called identifiers
  * may or may not be case sensitive
  * `keywords`: special names with language meaning
  * reserved keywords in a language may not be used as identifiers
* **address:** machine memory which the variable is associated with
  * sometimes called l-value&#x20;
  * you can have multiple variables that have the same address (aka alias) but it makes code more difficult to maintain
    * two pointer variabl that point to the same memory location for example
* **type**: determines the range of values that the variable can store and te set of operations that it can perform
* **value:** the content of the memory cell
  * also called the r-value

### Binding

Binding is an association between an attribute and an entity, between a variable and its type or value.&#x20;

Binding can take place at: language design time, language implementation time, compile time, load time, link time, or run time.

`count = count + 5`

* type of `count` is bound at compile time
* set of possible values of `count` is bound at compiler design time
* meaning of `+` is bound at compile time
* the internal representation of `5` is bound at compiler design time
* the value of `count` is bound at execution time&#x20;
