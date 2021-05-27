# week 9

## RAII

Imagine having to do this everytime you write a function that can potentially throw exceptions.

```cpp
void doTheJob()
{
    A* a = nullptr;
    B* b = nullptr;
    
    try
    {
        a = buildA();     // dynamically allocates an A
        b = buildB();     // dynamically allocates an B
        
        doThings(a, b);
        doMoreThings(a, b);
        doYetMoreTHings(a, b);
        
        delete b;
        delete a;
    }
    catch (...)
    {
        delete b;
        delete a;
        
        throw;
    }
}
```

That is cringe. So we use RAII: Resource Acquisition Is Initialization. 

```cpp
std::vector<int> getFunctionValues(int n, std::function<int(int)> f)
{
    std::vector<int> v;
    
    for (int i = 0; i < n; ++i)
    {
        v.push_back(f(i));
    }
    
    return v;
}
```

Even tho the function above has the potential to throw exceptions, this function does not leak memory.

1. Prefer to acquire dynamic resources in constructors. 
   1. If that acquisition fails, throw an exception from the constructor.
2. Always release those resources in the corresponding destructors.

If every kind of resource is managed by its own class this way, and if we otherwise endeavor to use statically-allocated objects of these classes when we can, then a lot of what would be error-prone cleanup becomes automatic.

## Smart Pointers 

Pointers are dumb. They only know the address where they point. So let's create a smarter one.

```cpp
// A SmartIntPtr uniquely owns a dynamically-allocated integer
class SmartIntPtr
{
public:
    SmartIntPtr(int* p)
        : p{p}
    {
    }
    
    // Remember that when you don't write a copy constructor, a 
    // default one is provided to you by copying all member variables
    // by putting "= delete" at the end of the signature, we 
    // are telling compiler to not give us any default constructor.
    // This will result in a compile time error
    SmartIntPtr(const SmartIntPtr& p) = delete; 
    SmartIntPtr& operator=(const SmartIntPtr& p) = delete;
    
    ~SmartIntPtr()
    {
        delete p;
    }
    
    int* get() const
    {
        return p;
    }
    
    // return a reference to an integer so that you are returning a
    // r-value which you can read and write to 
    int& operator*() const
    {
        return *p; //return a reference to the integer that p points to
    }
    
private:
    int* p;
};
```

If we can't make copies of `SmartIntPtr` then how are we going to pass it as a parameter? We dont't. We pass a reference \(SmartIntPtr loans the value\) to it.

```cpp
// main.cpp

void blah(int* p)
{
    ...
}

void foo(int& i) 
{
    ...
}

int main()
{
    SmartIntPtr p1{ new int{3}};
    SmartIntPtr p2{ new int{9}};
    
    *p = 6;
    std::cout << *p1 << std::endl;
    std::cout << *p2 << std::endl;
    
    blah(p1.get());
    foo(*p2); 
    std::cout << *p1 << std::endl;
    std::cout << *p1 << std::endl;
    
    return 0;
}
```

Since a `SmartIntPtr` owns the pointer, it will automatically delete the object it points at when an exception is thrown.

```cpp
class SomeException()
{
}

void bar()
{
    SmartIntPtr p{new int{21}};
    throw SomeException{};
}

int main()
{
    try
    {
        bar();
    }
    catch (SomeException) // no memory leak !
    {
    }
    
    return 0;
}
```

## Function Templates

Things that let the compiler build functions. We use the word `template` to create a function template:

```cpp
template <typename T>
void myswap(T& a, T& b)
{
    T temp = a;    // copy constructed a T
    a = b;         // assigned a T into another T
    b = temp;      // assigned a T into another T
}                  // destroyed a T (temp)
```

{% hint style="info" %}
Right after writing the function template, I have potentially declared an infinite set of `myswap()`functions. BUT I have not yet defined any of them; _none of them exist yet_.
{% endhint %}

**Template instantiation:** Taking a template and turning it into something concrete.

```cpp
int main()
{
    int i = 3;
    int j = 5;
    myswap(i, j);      //instantiates myswap<int>
    
    std::string s1 = "Boo";
    std::string s2 = "Alex";
    myswap(s1, s2);     //instantiates myswap<std::string>
}
```

Templates have parameters; parameters have constraints! So be careful what types you pass. Some parameter types might not have an assignment nor a delete nor a copy constructor.

{% hint style="info" %}
Function templates should be written in header files.
{% endhint %}

## Class Template

You can also have class templates! But there is a small tweak. 

When using `template` you get an infinite set of classes. Because you have an infinite set of `Point` classes, you also need an infinite set of `Point` member functions when declaring its body.

```cpp
// point.hpp

#ifndef POINT_HPP
#define POINT_HPP

template <typename Coordinate>
class Point
{
public:
    Point(const Coordinate& x, const Coordinate& y, const Coordinate& z);
    
    template <typename OtherCoordinate>
    Point(const Point<OtherCoordinate>& p);
    
    template <typename OtherCoordinate>
    Point& operator=(const Point<OtherCoordinate>& p);
    
    Coordinate& x();
    const Coordinate& x() const;
    
    Coordinate& y();
    const Coordinate& y() const;
    
    Coordinate& z();
    const Coordinate& z() const;
    

private:
    Coordinate x_;
    Coordinate y_;
    Coordinate z_;

};

// here we are writing the definition of the member functions
template <typename Coordinate>
Point<Coordinate>::Point(const Coordinate& x, const Coordinate& y, const Coordinate& z)
    : x_{x}, y_{y}, z_{z}
{
}

template <typename Coordinate>
template <typename OtherCoordinate>
Point<Coordinate>::Point(const Point<OtherCoordinate>& p)
    : x_(p.x()), y_(p.y()), z_(p.z())  // implicit conversion will kick in
{
}

template <typename OtherCoordinate>
Point<Coordinate>& Point<Coordiante>::operator=(const Point<OtherCoordinate>& p)
{
    x_ = p.x();
    y_ = p.y();
    z_ = p.z();
    return *this
}

template <typename Coordinate>
Coordinate& Point<Coordinate>::x()
{
    return x_;
}

template <typename Coordinate>
const Coordinate& Point<Coordinate>::x()
{
    return x_;
}

template <typename Coordinate>
Coordinate& Point<Coordinate>::y()
{
    return y_;
}

template <typename Coordinate>
const Coordinate& Point<Coordinate>::y()
{
    return y_;
}

template <typename Coordinate>
Coordinate& Point<Coordinate>::z()
{
    return z_;
}

template <typename Coordinate>
const Coordinate& Point<Coordinate>::z()
{
    return z_;
}


#endif
```

