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



