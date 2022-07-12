# Arrays

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
