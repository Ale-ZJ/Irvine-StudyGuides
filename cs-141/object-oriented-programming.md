# Object-Oriented Programming

## Three key features:

1. **Class definition mechanism**
   1. interface - seen by clients
   2. implementation - how object stores its state
2. **Class Inheritance**
   1. allows reuse - _you don't need to repeat yourself_ - of code and polymorphism
      1. overriding: a child class has a different implementation of a inherited method
      2. overloading: multiple functions with the same name but different number of parameters
   2. 'single inheritance': at most one parent class
      1. Java
   3. 'multiple inheritance': any number of parents
      1. C++
      2. Java has interfaces, which are classes without implementations (base classes
3. **Dynamic Dispatch**
   1. also called "virtual functions" or "late binding"
      1. compiler figures out what method to use at compile time&#x20;
   2. allows polymorphic programming
   3. design and construction of reusable software frameworks

* Python does not follow these strictly&#x20;
  * they have ducktyping&#x20;

### Static Method

static methods does not have `this`

```
class foo{
private:
    int priv;
    static int dummy;
 public:
    static int bar() {
       foo *p = new foo;
       p->priv = 0;  // legal
       priv = 0;     // illegal
```

what happens if i put `static` in front of a data member `dummy`?

the variable is initialized before main

there is only one static variable even if you have like 30 instances of foo

it is like a shared variable. like a global variable for all iinstances of that class



## SmallTalk

* useful for "message passing"
* a program is a collection of interacting objects
* objects communicate via messages
* message response definitions are called 'methods'
* messages are bound to methods dynamically
* collection of methods defined on an object is its 'method protocol'
* everything is an object (even primitive types)

## C++

in the earlier versions of C++, you could write in C, and the compiler didnt complain. But it slowly shifted to only c++

C used to have a lot of overrun buffers, security concerns&#x20;

* programmer has control over construction, copying, assignment, and destruction
* programmer can overload operators, functions, and member funtions&#x20;
* allocation may be static, stack, or free-store (heap)
* also allows plain functions
* primitive types are not objects&#x20;
  * for efficiency
* programmers must manage storage
  * can be tricky
  * may use smart pointers (shared\_ptr)
* a C struct we call it "plain old data" lol&#x20;
  * it only has data ntohing more

## Java

* designed to allow safe, portable down-loadable object code
* Java source is translated into Java Byte Codes
  * may interpret JBC on Java Virtual Machine
  * or just-in-time (JIT) compiler can translate to native code
* all objects are accessed via pointers
* can overload only methods, not operators
* copying is done via a method called `clone`
  * deep copy
  * you can clone any object
* mutable vs immutable





immutable: allows compiler to make optimization, it allows to make copies of data, but you need to allocate more storage



there is no way to remove data in inheritance, you can only add to it&#x20;



## Implementation of Inheritance

* a derived class gets
* derived class constructor
  * first calls all parent contructors
  * then it does its own construction

```
class Derived: Base {
    int my_data1;  // the constructor will initialize the members by order of declaration
    int my_data2;
    Derived() : 
        my_data2(Base::data), my_data(my_data2), 
```

## Implementation of Virtual Functions

this is the low level implementation of inheritance

* every object has a pointer to it's method table
  * this pointer is called a 'vpointer'
  * this table is called a 'vtable'
* each concrete class containing one or more virtual functions has a unique vtable
  * vtable is an array of pointers to object's virtual functions
  * vtables are built before run-time&#x20;
* each virtual function is assigned an index into the vtable
* vpointers are assigned automatically in the constructor
* virtual functions are called through the vtable
* virtual functions are called through the vtable
  * `this -> vpointer[ vfunctionIndex ](this, regular_arguments)`&#x20;
  * ^^^ SUPER IMPORTANT - QUIZ EXAM QUESTION

in the birth of an object:

* allocation, with malloc
* and there is contruction too&#x20;
