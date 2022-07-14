# week4

## Constness

&#x20;In C++, there are tools to keep things constant.&#x20;

```cpp
const int x = 3;
const std::string nmae = "Boo";
const int y = x;
int z = x;
const int w = z;
```

Be careful with pointers and references. Both structures potentially allow us to change the contents of a constant variable.

#### References

The key thing to understand is that a reference to a constant doesn't necessarily guarantee that the value it refers to will never change; it simply guarantees that the reference itself will never be used to change the value.

```cpp
int& r = x; // illegal 
const int& r = x; // r is a reference to an intger constant

int x = 3;
const int& y = x; // the value is constant 
                  // (and technically references are const by definition)

// use reference asa parameter type when objects are extensiv to copy
void printInReverse(const std::string& s)
{
    for (int i = s.length() - 1; i >= 0; --i)
    {
        std::cout << s[i];
    }
}
```

#### Pointers

```cpp
int* p = &x; // illegal neither p or *p is const
const int* q; // q is non-const, *q is const -- west const
int const* r; // r is non-const, *r is const -- east const
const int* const t; // t is const, *t is const
```

## Structure

&#x20;A `struct` is a combination of things (whether they are different or homogeneous).

This is how you declare a struct:

```cpp
struct Date //static // 12 BYTES
{
    // members / member variables
    unsigned int year; 
    unsigned int month;
    unsigned int day;
};

void structAsParameters(const Date& d)
{
}

void dynamicallyAllocatedStructs()
{
    Date* d = new Date;
    d->year = 2021;
    d->month = 4;
    d->day = 23;
    
    Date* d2 = new Date{2021, 4, 21};
    
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

The compiler needs to allocate space for the structure and its members.&#x20;

C++ has a `sizeof(object)` operator that tells you how big an object is. Using it in the example below, we get:&#x20;

```cpp
struct X // 24 BYTES
{
    char a;    // 1 BYTE
    int b;     // 4 BYTES
    short c;   // 2 BYTES
    double d;  // 8 BYTES
};
```

Should not struct b 15 BYTES then? Why 24? Sometimes there is **padding** (blank spaces) because a type can only be allocated in memory indexes that are by the type size ( ? )

![](<../.gitbook/assets/image (11).png>)
