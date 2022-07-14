---
description: >-
  week 3 - lost my original notes smh, this is a shorter version bc i am not
  watching 3 hours of lecture again
---

# References, Pointers, Arrays

## References

In C++, when naming a storage location (a variable or parameter) you give that location (think of it as a box) a type (which dictates how much memory is required to store it or how big the box is) and a name.&#x20;

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
2. Once a reference is initialized, it **cannot be changed** to refer to something else (like another object)

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

![](<../.gitbook/assets/image (6).png>)

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

When passing a parameter by a reference, no copies are made. Change made to the parameter will be reflected on the original. It enables functions to alter the arguments that are passed to them.&#x20;

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

### Function overloading&#x20;

Overloading means writing more than one function with the same name. In C++ it is ok to overload functions as long as the compiler has a way to differentiate them (in this case the parameters).&#x20;

Overload resolution rules:&#x20;

* Exact matches have priority&#x20;
* Matches involving promotion&#x20;
  * int -> long
  * int -> double&#x20;
  * float -> double
* matches involving other standard conversions&#x20;
  * double -> int
  * float -> char

### Default Arguments

ALWAYS appear after ALL the named parameters.

```cpp
double vectorLength(double x, double y, double z = 0.0)
{
    return std::sqrt(x * x + y * y + z * z);
}
```

## Static and Dynamic Allocation&#x20;

| Static allocation                               | Dynamic allocation                  |
| ----------------------------------------------- | ----------------------------------- |
| determine things before the program runs        | determine things while program runs |
| set size                                        | size changes during execution       |
| in the run-time stack                           | in the free store or heap           |
| automatically deletes names after function ends | you need to delete them yourself    |

```cpp
{
    std:: string s = "Alex is happy today!";
    s += "because she saw a pic of Boo";
}
```

![](<../.gitbook/assets/image (3).png>)

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

![](<../.gitbook/assets/image (5).png>)

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

![](<../.gitbook/assets/image (4).png>)

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

`new` dynamically allocates a block of memory big enough in the heap to store the type specified and returns a pointer.

```cpp
int* p = new int;
```

`delete` used to de-allocate **the object/value a pointer is pointing at**, it does not get rid of the name of the pointer.

```cpp
delete p; //deletes whatever p points at
```

dangling problem

```cpp
int* p = new int;
*p = 3;
delete p;
std::cout << *p << std::endl; //once deleted you can't find it anymore
```

## Statically Allocated Arrays

To create a single-dimension array of homogeneous collection (a collection of the same type of things):

```cpp
int a[10];
```

* You need to specify how big your array will be before compile time so that our compiler knows to allocate 40 bytes for this array.&#x20;
* Arrays are stored contiguously (they live next to each other) in memory - cells in the array are "indexed" starting from 0.
* The size of an array CANNOT change for as long as it lives&#x20;
* Given an array, you can't ask a static array its size. It is as if they don't know

![](<../.gitbook/assets/image (9).png>)

The compiler uses a "formula" to index into cells. Every cell has a memory address at the beginning. Each subsequent cell memory address can be found by:&#x20;

```cpp
address of cell i = address of cell0 + (sizeof(int) * i)
```

#### Indexing an array

```cpp
a[3] = 4; //write
std::cout << a[3] << std::endl //read
```

An index will give you an **l-value**. This means that you can write and read to it.

{% hint style="danger" %}
**Be careful!** Unlike Python, there are no negative indexes in C++ and you can go out-of-boundaries without failing (compiles and run).&#x20;
{% endhint %}

#### Why can't we statically allocate every array?

* Because you might not know how many elements it needs to have&#x20;
* Because you might need it to outlive the function it's created in
* The run-time stack has a limited size

## Dynamically Allocated Arrays&#x20;

As the descriptor "dynamically" suggests, this array is created in the heap. Hence this array can change in size and outlive a function. As with any object created in the heap, we use the keyword `new` and store it in a pointer.&#x20;

```cpp
int* a = new int[10]; // instead of 10, you can use a variable
```

![](<../.gitbook/assets/image (8).png>)

The expression above allocated a block of memory on the heap large enough to store 10 ints (10 bytes in our VM).&#x20;

{% hint style="info" %}
Notice that `int*` is the syntax we learned to declare a _pointer to an integer_. Then how does the compiler know we are talking about an array of ints and not an int? - sike, it doesn't and can't, you need to keep track yourself. Arrays are implemented as pointer to their first cell (in this case an int)
{% endhint %}

You can treat a dynamically allocated array as a pointer to a static array of integers. Indexing and such is the same.

#### How to delete a dynamic array

Unlike statically allocated arrays, where deletion is automatic, you need to explicitly use the keyword `delete` . The little twist is that you need to tell the compiler that what you are deleting is an array and not an int, so in order to do that we add `[]` at the end of delete:

```cpp
delete[] a; // remember that a is dangling and should no longer be used
```

Regardless of how the array was allocated, the simplest way to pass an array as a parameter is to write `int*` and explicitly pass its size.

```cpp
void zeroFill(int* a, unsigned int size)
{
    for (unsigned int i = 0; i < size; ++i)
    {
        a[i] = 0;
    }
}

int main()
{
    int n;
    std::cin >> n;
    
    int* x = new int[n];
    zeroFill(x, n);
    delete[] x;
    
    int a[20];
    zeroFill(a, 20);
    
    return 0;
}
```

## Pointer arithmetic

Pointers points to memory addresses, which are integers, which implies that we can do arithmetics with pointers :O

```cpp
int a[10];                           // a is effectively a pointer to the first element of the array
int b[10];                           // b is also effectively a pointer
std::cout << a << std::endl;         // writes the address of a[0]
std::cout << (a + 1) << std::endl;   // writes the address of a[1]
*(a + 1) = 3;                        // stores 3 in a[1]
std::cout << (a - 1) << std::endl;   // writes the address of a[-1] - although illegal
std::cout << (a - b) << std::endl;   // writes the distance in memory between a[0] and b[0] (divided by the size of an int)
```

The naive compilation of the `zeroFill` function above performs multiplications each time you need to index into the array. In the example code below, there is only additions, which makes it faster.

```cpp
void zeroFill(int* a, unsigned int size)
{
    int* end = a + size;

    for (int* p = a; p != end; ++p)
    {
        *p = 0;
    }
}
```

#### Optimization&#x20;

Compilers with optimizers do not necessarily translate code exactly the same. It doesn't matter as long as the result is the same. The compiler in the VM we are using is smart and translates both `zeroFill` to a better function that are the same.
