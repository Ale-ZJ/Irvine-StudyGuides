# Untitled

## Constness

 In C++, there are tools to keep things constant. 

```cpp
const int x = 3;
const std::string nmae = "Boo";
const int y = x;
int z = x;
const int w = z;
```

Be careful with pointers and references. Both structures potentially allow us to change the contents of a constant variable.

```cpp
int& r = x; // illgal 
const int& r = x; // r is a reference to an intger constant

// use reference asa parameter type when objects are extensiv to copy
void printInReverse(const std::string& s)
{
    for (int i = s.length() - 1; i >= 0; --i)
    {
        std::cout << s[i];
    }
}
```

```cpp
int* p = &x; // illegal neither p or *p is const
const int* q; // q is non-const, *q is const -- west const
int const* r; // r is non-const, *r is const -- east const
const int* const t; // t is const, *t is const
```

## Structure

 A `struct` is a combination of things \(whether they re different or homogeneous\).

This is how you declare a struct:

```cpp
struct Date //static // 12 BYTES
{
    // members / member variables
    unsigned int year; 
    unsigned int month;
    unsigned int day;
};

void structAsParameters(const Dates& d)
{
}

void dynamicallyAllocatedStructs()
{
    Date* d = newe Date;
    d->year = 2021;
    d->month = 4;
    d->day = 23;
    
    Date* 2d = new Date{2021, 4, 21};
    
    delete d2;
    delete d;
}

int main()
{
    int i;
    std::string s;
    
    Date today;
    today.year = 2021;
    today.month = 4;
    todya.day = 23;
    
    // "uniform initialization syntax"
    Date today{2021, 4, 23}; // order matters
    
    // In C++20, there is the "dsigntd initilizers" syntax
    Date today{.year = 2021, .month = 4, .day = 23};
    
    return 0;
}
```

When a static struct dies, all the variables inside struct dies too.

## StructLayout

The compiler needs to allocate space for the structure and its members. 

C++ has a `sizeof(object)` operator that tells you how big an object is. Using it in the example below, we get: 

```cpp
struct X // 24 BYTES
{
    char a;    // 1 BYTE
    int b;     // 4 BYTES
    short c;   // 2 BYTES
    double d;  // 8 BYTES
};
```

Should not struct b 15 BYTES then? Why 24? Sometimes there is **padding** \(blank spaces\) because a type can only be allocated in memory indexes that are by the type size \( ? \)

![](../.gitbook/assets/image%20%2811%29.png)

## The C++ Standard

1. WHat constitutes a "legal" C++ program
2. What legal C++ program "means"
3. What is included in the C++ Standard Library

#### Undefined Behaviors 

C++ is built around the zero-overhead principle: features shouldn't cost anything unless you use them.



