---
description: >-
  Be smart and test code little by little, otherwise you will have a million
  errors and you will cry
---

# Memcheck, Unitest

## The C++ Standard

1. WHat constitutes a "legal" C++ program
2. What legal C++ program "means"
3. What is included in the C++ Standard Library

#### Undefined Behaviors&#x20;

C++ is built around the zero-overhead principle: features shouldn't cost anything unless you use them.

{% hint style="info" %}
A **segmentation** fault is an attempt to access memory that our program is not allowed to access.
{% endhint %}

## Memcheck

Memcheck is a memory error detector tool. This tool is found inside a package called Valgrind.&#x20;

* values that were never assigned, which means the outcome is undefined in condition tests for control structures.
* Attempting to use memory that's never been allocated
* Attempting to use memory beyond the boundaries of memory that's been allocated&#x20;
* Attempting to deallocate memory that was never allocated, or deallocate the same memory twice
* When a program ends, any memory that's been dynamically allocated and never deallocated is reported as a **memory leak**.

Here is an example code to use memcheck on:&#x20;

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

Use the following command to run memcheck. The `--memcheck` tag tells the compiler that you want to run the program with memcheck watching step by step. The error are printed in the order they occured:&#x20;

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

#### Memory Leaks&#x20;

Memory leaks can be found under `HEAP SUMMARY` and `LEAK SUMMARY` that memcheck prints to the console. There are several ways in which a block (a box) can be "lost" at the end of a program:

* **Definitely:** blocks with no pointers pointing to them&#x20;
* **Indirectly:** you can't access the pointers that are pointing the blocks
* **Possibly:** there are pointers pointing to stuff inside the block but not the box itself. We will treat this like definitely lost.
* **Still reachable:** blocks with pointers pointing to them (usually global variables since all the run-time stack were destroyed)
* **Suppressed:** memcheck was explicitly instructed to ignore these blocks. We will ignore them

## Unitest

In this course, we are using UNITEST from Google Test.

```cpp
// putting these functions into the "unnamed namespace"
// only visible in this source file
namespace
{
    std::string truncate(const std::string& s, unsigned int length)
    {
        if (s.size() > length)
        {
            return s.substr(0, length);
        }
        else
        {
            return s;
        }
    }
    
    std::string truncateArtist(const std::string& artist)
    {
        return truncate(artist, Song::MAX_ARTIST_LENGTH);
    }
    
    std::string truncateTitle(const std::string& title)
    {
        return truncate(title, Song::MAX_TITLE_LENGTH);
    }
}
```

Inside the `gtest` file, create a new source file with your tests.&#x20;

```cpp
// SongTests.cpp

#include <gtest/gtest.h>
#include "Song.hpp" // all files in gtest are seen in app

TEST(SongTests, containArtistGivenWhenCreated)
{
    Song s{"U2", "Breath"};
    ASSERT_EQ("U2", s.getArtist());
}

TEST(SongTests, containTitleGivenWhenCreated)
{
    Song s{"Arcade Fire", "After Life"};
    ASSERT_EQ("After Life", s.getTitle());
}

TEST(SongTests, afterChangingArtist_ContainNewArtist)
{
    Song s{"U2", "Afterlife"};
    s.setArtist("Arcade Fire");
    ASSERT_EQ("Arcade Fire", s.getArtist());
    EXPECT_EQ("Arcade Fire", s.getArtist());
}

TEST(SongTest, truncateArtistAtCreationIfTooLong)
{
    std::string longArtist(Song::MAX_ARTITST_LENGTH + 5, 'x');
    std::string truncatedArtist(Song::MAX_ARTIST_LENGTH,  'x');
    
    Song s{longArtist, "Some Title"};
    ASSERT_EQ(truncatedArtist, s.getArtist());
}
```

{% hint style="info" %}
Anything you **assert**, you can **expect**.
{% endhint %}

* **Assert**: if this isn't true then test fail and the test immediatly stops
* **Expect**: check this, and if it is wrong tell me, and keep running the test
