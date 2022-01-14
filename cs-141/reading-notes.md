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



