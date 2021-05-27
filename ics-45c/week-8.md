# week 8

## Type Casting 

Changing the type of a variable or object to another variable or object type. In C++ there is implicit type conversion between a lot of types. But there are some conversions that can't be done implicitly, so how do we do it explicitly?

### C-Style Cast 

C-Style Cast is a blunt instrument.

You can cast an integer to be a pointer using c-style casting \(although you won't be able to compile it in the ICS45C VM\)

```cpp
int i = 3;
int* p = (int*) i;
```

You can also cast numeric values and cast up.

```cpp
int x = 3;
int y = 4;
double k = x / (double) y;
```

You can also downcast an object in a hierarchy! But be extremely careful with this one because it will result in a silent error if you try to convert to the wrong type.

```cpp
Shape* ps = new Circle{3.0};
Circle* pc = (Circle*) ps;       // downcasting 
Rectangle* pr = (Rectangle*) ps; // will act as a circle in this case
```

What about this weird thing

```cpp
struct A
{
    int a;
};

struct B
{
    int b;
};

int main
{
    A* a = new A{3};
    B* b = (B*) a;  // take a and pretend like it points to b
    
    std::cout << b->b << std::endl;
    // output: 1.4822e-323 wtf
    
    return 0;
}
```

### C++ style casts

We can use `dynamic_cast` to downcast in an inheritance hierarchy

* returns `nullptr` if the object isn't of the right type
* only works on types that have at least one virtual member function

```cpp
void foo(Shape* s)
{
    Rectangle* r = dynamic_cast<Rectangle*>(s);
    // will return a Rectangle object if the Shape s given is a 
    // rectangle, else returns a nullptr
}
```

We can also use `static_cast` cast that is only checked at compile time. It will give you the pointer you asked for anyways and using the pointer later will fail. 

You can also have explicit conversions amongst built-in types

```cpp
void foo(Shape* s)
{
    Rectangle* r = static_cast<Rectangle*>(s);
    // will return the pointer but subsequent use fails
}

int x = 3;
int y = 4;
double k = x / static_cast<double>(y);
```

Other types of casting, but you rarely will use:

* `reinterpret_cast` sort of like C-style cast.
* `const_cast` lets you remove the "const" but not otherwise change the type

## Static Members

Members that belongs to the class as a whole have the keyword `static` in front of them.

* In a static member function, there is no `this` because the function pertains to all objects and there is not a singlular "this". 
* There is also no direct access to non-static member variables or non-static member functions.

```cpp
// Widget.hpp

class Widget
{
public:
    Widget();
    Widget(const Widget& w);
    ~Widget();
    
    int getInt() const;
    
    // A static member function is one that you call on the class, 
    // not on an object of the class
    static unsigned int getWidgetCount();
    
private:
    int id;
    static int nextId;
    static unsigned int widgetCount;
    
}
```

Initialization of the static member functions must be done in one source file \(One Definition Rule\)

```cpp
// Widget.cpp

// initialization of the static member variable
int Widget::nextId = 1;
unsigned int Widget::widgetCount = 0;

Widget::Widget()
    : id{Widget::nextId++}
{
    ++Widget::WidgetCount;
}

Widget::Widget(const Widget& w)
    id{ w.id }
{
    ++Widget::WidgetCount;
}

Widget::~Widget()
{
    --Widget::widgetCount;
}

int Widget::getId() const 
{
    return id;
}

// static member function 
unsigned int Widget::getWidgetCount()
{
    return widgetCount;
}
```

Rememebr that when creating an object, that object only lives inside a scope and dies when you get outside. In the example above, w is created and then it dies every time you loop through.

```cpp
//main.cpp 

void foo()
{
    for (unsigned int i = 0; i < 10; ++i)
    {
        Widget w;
        std::cout << Widget::getWidgetCount() << std::endl;
        // outputs 1 1 1 1 1 1 1 1 1 1
    }
}
```

```cpp
// main.cpp 

void foo()
{    
    std::vector<Widget> widgets;
    
    for (unsigned int i = 0; i < 10; ++i
    {
        // a Widget is created, a copy is made to be stored in the vector 
        // and the original one dies. So you also have to keep track of 
        // widget count 
        widgets.push_back(Widget{});
        std::cout << Widget::getWidgetCount() << std::endl;
        // output 1 2 3 4 5 6 7 8 9 10 after adding the copy constructor
    }
}
```

## Contracts

Describe how a function should work.

* **Precondition**: things that must be true before you can call the function and see it succeed.
* **Postcondition**: things that will be true after the function finishes
* **Side effects**: things that will happen other than the function coputing and returning a result

Classes also have contracts associated with them. Every member function has a contract.

* **class invariants**: post conditions that apply to all member functions
* for example with `std::vector<int>` 
  * `size <= capacity`
  * `size >= 0`
  * `capacity >= 0`
  * `capacity == number of elemetns in the underlying array` 

**Poposed syntax for contracts in C++20** \(got shot down\) 

```cpp
double sqrt(double n)
    [[expects: n >= 0.0 ]] // <-- attribute that specifies precondition
    [[ensures: sqrt(n) >= 0.0 && std::abs(sqrt(n) * sqrt(n) - n) < 0.00001]];

class ArrayList
{
private:
    std::string* items;
    unsigned int sz;
    unsigned int cap;
}
[[ensures: items != nullptr && cap >= sz]];
```

## Exceptions

The object you `throw` can be anything. In fact, the type you throw is greatly debated and differs depending on the programmer. In this class, a general object that represents an expection will be to write an empty class.

```cpp
class DivideByZeroException
{
};
```

Then you can **statically create a `DivideByZeroException`** and throw it in your code. We statically allocate it so that the error can be carried through. If we were to dynamically allocate it, we would be throwing a pointer.

```cpp
double divide(double n, double m)
{
    if (m == 0.0)
    {
        throw DivideByZeroException{};
    }
    
    return n / m;
}
```

If you don't catch the exception, then the function that called divide will fail. You catch a reference of the exceotion class because 1\) it saves time by not having to copy the class and 2\) exceptions are generally part of hierarchies with more general class.

```cpp
void foo()
{
    do
    {
        double n;
        double m;
        
        std::cin >> n >> m;
        
        try
        {
            std::cout << divide(n,m) << std::endl;
        }
        catch (DivideByZeroException)
        {
            std::cout << "That was division by zero! Try again << std::endl;
        }
        catch (...) // catch anything but you won't know it's type
        {           // blunt instrument! 
            // "catch and re-throw"
            // like Python's "finally"
        }
    }
    while (true);
}
```

Statically allocated variables are destroyed automatically when you throw an exception. BUT dynamically allocated variables are a whole new story. \(tba\)

* for instance `std::bad_alloc` is thrown when dynamic memory allocation fails 

{% hint style="danger" %}
Destructors should never, ever, ever, ever, ever, ever, ever, ever, ever throw exceptions
{% endhint %}

If you throw an exception when there is an exception being propagated already, the program calls `std::terminate`, and the program terminates, the memory is corrupt and you can't recover from it.

### Exceptions Safety

Pinciple of ensuring that we have reasonable outcomes in cases when exceptions are thrown.

Classes in the C++ Standard Library make "guarantees" about what happens when their member functions fail.

* no guarantee 
* basic guarantee
  * if member function fails, the objext is left in a consistent \(i.e. all class invariants are met\).
* strong guarantee
  * if a member function fails, the obejct is left in the state it was in before the member function was called
  * too expensive sometimes
* No throw guarantee
  * this function will never throw an exception
  * almost impossible 
  * for example: getters

