---
description: >-
  lost my original notes smh, this is a shorter version bc i am not watching 3
  hours of lecture again
---

# week3

## References

In C++, when naming a storage location \(a variable or parameter\) you give that location \(think of it as a box\) a type \(which dictates how much memory is required to store it or how big the box is\) and a name. 

A reference can be thought of as an "alternative name" for a variable, like a synonym. This means that changes made to a reference are reflected on the original.

To create a reference, we use the `&` symbol after a type

```cpp
int& r = i;  
```

Behind the scenes, a reference refers to a location in memory. This means that they refer to an **l-value.**

```cpp
int& x = 4; // illegal because 4 is a r-value
```

There are two important rules about references:

1. References must be **initialized explicitly** when they're **defined**
2. Once a reference is initialized, it **cannot be changed** to refer to something else \(like another object\)

Here is an example of everything above:

```cpp
int i = 3;
int& r = i;                     // r now refers to i
std::cout << r << std::endl;    // writes 3
r = 4;                          // a change to r is also a change to i
std::cout << i << std::endl;    // writes 4
i = 5;                          // a change to i is also a change to r
std::cout << r << std::endl;    // writes 5
```

Another example:

```cpp
int i = 3;
int* p = i;
int* q = p;
int** r = q
```

![](../.gitbook/assets/image%20%286%29.png)

### Pass-by-value parameters

When passing a parameter by value, a copy of the parameter is created and lives in the function. Any change or usage of the copy does not affect the original. When the function ends, the copy dies.

```cpp
void foo(int x)
{
    x++;
    std::cout << x << std::endl;
}

int main()
{
    int i = 3;
    foo(i);
    std::cout << i << std::endl;
    return 0;
}

// console:
// 4
// 3
```

### Pass-by-reference parameters

When passing a parameter by a reference, no copies are made. Change made to the parameter will be reflected on the original. It enables functions to alter the arguments that are passed to them. 

```cpp
void foo(int& x)
{
    x++;
    std::cout << x << std::endl;
}

int main()
{
    int i = 3;
    foo(i);
    std::cout << i << std::endl;
    return 0;
}

// console 
// 4
// 4
```

## Compatibility - basic conversions

### Implicit type conversion

Some types can be implicitly converted to another if they are _compatible_ when there is a known conversion between them.

```cpp
int i1 = 4;
double d1 = i1;    // 4

double d2 = 3.5;
int i2 = d2;       // 3 truncated

long l3 = 5000000000;
int i3 = l3;       // 705032704
```

### Function overloading 

Overloading means writing more than one function with the same name. In C++ it is ok to overload functions as long as the compiler has a way to differentiate them \(in this case the parameters\). 

Overload resolution rules: 

* Exact matches have priority 
* Matches involving promotion 
  * int -&gt; long
  * int -&gt; double 
  * float -&gt; double
* matches involving other standard conversions 
  * double -&gt; int
  * float -&gt; char

### Default Arguments

ALWAYS appear after ALL the named parameters.

```cpp
double vectorLength(double x, double y, double z = 0.0)
{
    return std::sqrt(x * x + y * y + z * z);
}
```

## Static and Dynamic Allocation 

| Static allocation | Dynamic allocation |
| :--- | :--- |
| determine things before the program runs | determine things while program runs |
| set size | size changes during execution |
| in the run-time stack | in the free store or heap |
| automatically deletes names after function ends | you need to delete them yourself |

```cpp
{
    std:: string s = "Alex is happy today!";
    s += "because she saw a pic of Boo";
}
```

![](../.gitbook/assets/image%20%283%29.png)

But what is that blue arrow? It can't be a reference because references can't change the object they are referring to.

## Pointers

A **pointer** in C++ is another way to store components loation.

In other words, a pointer won't just say "I'm pointing to memory location x." It'll say "I'm pointing to an integer stored at memory location x."

How to create a pointer:

```cpp
int i = 3;
int* p = &i; // & here is a pre-operator that give you the address of i
```

```cpp
int i = 3;
int* p = &i;
*p = 4; // dereferencing - * is a pre-operator 
        // that gives you the value at the end of the arrow

```

![](../.gitbook/assets/image%20%285%29.png)

```cpp
int* p = new int;
*p = 3;

int& q = *p;
q = 4;

int* r = &q;
*r = 5;

int*& s = p;
*s = 6;
```

![](../.gitbook/assets/image%20%284%29.png)

There are also null pointers. Pointers that point at the absence of something. Useful in linked lists.

```cpp
int* p = nullptr; //this is bueno
int* p = 0;    
int* p = NULL; // C library

if (p != nullptr)...

int* p = nullptr;
*p = 3;            // CRASH! - no bueno
```

## New / Delete

`new` dynamically allocates a block of memory big enough to store the type specified.

```cpp
int* p = new int;
```

`delete` used to de-allocate the value a pointer is pointing at, it does not get rid of the name of the pointer.

```cpp
delete p;
```

dangling problem

```cpp
int* p = new int;
*p = 3;
delete p;
std::cout << *p << std::endl; //once deleted you can't find it anymore
```

