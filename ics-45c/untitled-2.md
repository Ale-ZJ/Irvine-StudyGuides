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

 A `struct` is a combination of things \(whether they are different or homogeneous\).

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

{% hint style="info" %}
A **segmentation** fault is an attempt to access memory that our program is not allowed to access.
{% endhint %}

## Memcheck

Memcheck is a memory error detector tool. This tool is found inside a package called Valgrind. 

* values that were never assigned, which means the outcome is undefined in condition tests for control structures.
* Attempting to use memory that's never been allocated
* Attempting to use memory beyond the boundaries of memory that's been allocated 
* Attempting to deallocate memory that was never allocated, or deallocate the same memory twice
* When a program ends, any memory that's been dynamically allocated and never deallocated is reported as a **memory leak**.

Here is an example code to use memcheck on: 

```cpp
#include <iostream>

void zeroFill(int* a, unsigned int size)
{
    for (unsigned int i = 0; i < size; ++i)
    {
        a[i] = 0;
    }
}

void foo()
{
    int* a = new int[50];
    zeroFill(a, 60);  // undefined behavior
    delete[] a;
}

int main()
{
    std::cout << "Hello Boo!" << std::endl;
    foo();
    return 0;
}
```

Use the following command to run memcheck. The `--memcheck` tag tells the compiler that you want to run the program with memcheck watching step by step. The error are printed in the order they occured: 

```cpp
./run --memcheck app
```

#### Using memory that is not ours

The first line tells us that we were trying to write 4 BYTES of memory somewhere we can't. The next three lines are backtrace of where the error emerged. Line 5 tells us what is invalid about the write.

```cpp
==3151== Invalid write of size 4
==3151==    at 0x401167: zeroFill(int*, unsigned int) (main.cpp:7)
==3151==    by 0x4011A5: foo() (main.cpp:14)
==3151==    by 0x401233: main (main.cpp:21)
==3151==  Address 0x52b1d48 is 0 bytes after a block of size 200 alloc'd
==3151==    at 0x483774F: malloc (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==3151==    by 0x4909A69: operator new(unsigned long) (in /usr/lib/x86_64-linux-gnu/libc++.so.1.0)
==3151==    by 0x401193: foo() (main.cpp:13)
==3151==    by 0x401233: main (main.cpp:21)
```

{% hint style="info" %}
`x0` followed by some digits and the letters A-F, you are seeing a hexadecimal number and this is a memory address.
{% endhint %}

#### Use of uninitialized values

Our program has tried to make a decision based on a value that was never initialized

```cpp
void foo()
{
    int a[50]; // initialize a new array of 50 integers
               // but we didn't assign any values in it 
    std::cout << a[0] << std::endl;
}

//--------------

==3514== Conditional jump or move depends on uninitialised value(s)
==3514==    at 0x5105296: vfprintf (vfprintf.c:1637)
==3514==    by 0x51D2CA8: __vsnprintf_chk (vsnprintf_chk.c:63)
==3514==    by 0x48F0981: ??? (in /usr/lib/x86_64-linux-gnu/libc++.so.1.0)
==3514==    by 0x48F0756: std::__1::num_put<char, std::__1::ostreambuf_iterator<char, std::__1::char_traits<char> > >::do_put(std::__1::ostreambuf_iterator<char, std::__1::char_traits<char> >, std::__1::ios_base&, char, long) const (in /usr/lib/x86_64-linux-gnu/libc++.so.1.0)
==3514==    by 0x48CF1B6: std::__1::basic_ostream<char, std::__1::char_traits<char> >::operator<<(int) (in /usr/lib/x86_64-linux-gnu/libc++.so.1.0)
==3514==    by 0x4013AF: foo() (main.cpp:13)
==3514==    by 0x4013F3: main (main.cpp:20)
==3514==  Uninitialised value was created by a stack allocation
==3514==    at 0x401394: foo() (main.cpp:12)
```

#### Deallocation when it is not allowed

```cpp
void foo(int* a)
{
    delete a;
    delete a; // doesn't exist anymore
}

//--------------

==3638== Invalid free() / delete / delete[] / realloc()
==3638==    at 0x483897B: free (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==3638==    by 0x401447: foo(int*) (main.cpp:13)
==3638==    by 0x401472: main (main.cpp:21)
==3638==  Address 0x52b1db0 is 0 bytes inside a block of size 4 free'd
==3638==    at 0x483897B: free (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==3638==    by 0x401429: foo(int*) (main.cpp:13)
==3638==    by 0x401472: main (main.cpp:21)
==3638==  Block was alloc'd at
==3638==    at 0x483774F: malloc (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==3638==    by 0x4909A69: operator new(unsigned long) (in /usr/lib/x86_64-linux-gnu/libc++.so.1.0)
==3638==    by 0x40146A: main (main.cpp:20)
```

```cpp
void foo(int* a)
{
    delete a;
}

void bar()
{
    int x;
    foo(&x); // deleting stack-allocated memory
}

//---------------

==3691== Invalid free() / delete / delete[] / realloc()
==3691==    at 0x483897B: free (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==3691==    by 0x401469: foo(int*) (main.cpp:12)
==3691==    by 0x401480: bar() (main.cpp:18)
==3691==    by 0x4014A3: main (main.cpp:23)
==3691==  Address 0x1fff00031c is on thread 1's stack
==3691==  in frame #2, created by bar() (main.cpp:22)
```

#### Memory Leaks 

Memory leaks can be found under `HEAP SUMMARY` and `LEAK SUMMARY` that memcheck prints to the console. There are several ways in which a block \(a box\) can be "lost" at the end of a program:

* **Definitely:** blocks with no pointers pointing to them 
* **Indirectly:** you can't access the pointers that are pointing the blocks
* **Possibly:** there are pointers pointing to stuff inside the block but not the box itself. We will treat this like definitely lost.
* **Still reachable:** blocks with pointers pointing to them \(usually global variables since all the run-time stack were destroyed\)
* **Suppressed:** memcheck was explicitly instructed to ignore these blocks. We will ignore them





