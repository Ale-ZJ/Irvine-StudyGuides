---
description: week 6
---

# Well Behaved Classes

## Well Behaved classes

Objects of a well-behaved class have these properties:

1. Statically-allocated portion is small and fits on the run-time stack&#x20;
2. Clean up after themselves when they die&#x20;
3. Can be passed by value and preserve the original stuff&#x20;
4. Can be assigned to objects of the same type, with respective copying and clean up.
5. Can be **const** while preserving the ability to perform any const operation
6. Are efficient: no unnecessary work.

## Unnamed Namespace

The professor explained this in a unit test example, but I can't find the lecture or notes so this is straight from StackOverflow.&#x20;

An unnamed namespace is a utility to make a name _translation unit_ local. A translation unit is a file that is generated after compilation that is used to generate an _object file,_ library, or an excutable program. It consists of the contents of one source file + included header files.

```cpp
#include <iostream>

using namespace std;

namespace {
   const int i = 4;
   int variable;
   void boo(int i) { cout << i << endl; }
}

int main()
{
   cout << i << endl;
   variable = 100;
   boo(variable);
   return 0;
}
```

This means that you can have multiple functions `boo()` that exist in multiple translation units, and they won't clash at linking time.

{% hint style="info" %}
The effect of using an unnamed namespace is the same as using the `static` keyword.&#x20;

&#x20;   `namespace{int a1;}`

&#x20;   `static int a2;`

The only difference is that an unnamed namespace is superior because `a1` gets an unique name.
{% endhint %}

## Constructors

If you don't declare a constructor, one will be created for you. The default constructor will use the default initialization for the member variables' type. (For `std::string` the default initialization is an empty string, but for `int` and `pointer` it is an unknown behavior). So we use **initializers** for the member variables after the construction signature.

```cpp
ArrayList::ArrayList()
    : items{new std::string[initialCapacity]}, sz{0}, cap{initialCapacity}
{
//    std::cout << "ArrayList::ArrayList()" << std::endl;
}
```

## The Big Three

Classes whose objects manage resources that live outside of themselves — such as dynamically-allocated memory, open files, open connections to networks, etc. — generally require three new kinds of functions to be implemented. They come together, don't forget them!

### Destructor&#x20;

Member function that will be called right before the object dies. Its job is to perform cleanup of dynamically allocated memory (_not_ member variables, as they are handled automatically).

* takes NO parameters&#x20;
* Same name as constructor&#x20;
* begins with `~`

```cpp
// in header file
~ArrayList();

// in cpp file
ArrayList::~ArrayList()
{
    delete[] items; // deallocate the array
}
```

### Copy Constructor

It initializes new objects that are created as _copies of existing ones._

* object has been passed by value&#x20;
  * `foo(ClassType& object);`
* object has been used to explicitly initialize another
  * `ClassType object1;`
  * `ClassType object2 = object1;`

```cpp
// in header file 
ArrayList(const ArrayList& other);

// in cpp file 
ArrayList::ArrayList(const ArrayList& other)
    : items {new std::string[initialCapacity]}, sz{other.sz}, cap{other.cap}
{
    // copy the array in other
    for (unsigned int i = 0; i < sz; ++i)
    {
        items[i] = other.item[i];
    }
}
```

### Assignment Operator

It is called every time an _existing object_ is assigned into _another existing object._ (Note that both objects already exists).

* `ClassType object1;`
* `ClassType object2;`
* `object1 = object2;` this is a copy assignment&#x20;

The default assignment will copy all the objects' member variables one by one. This is a problem for the pointer `items` because the copy pointer `items` will also point to what the original `items` points.

```cpp
// in header file
ArrayList& operator=(const ArrayList& other);

// in cpp file 
ArrayList& ArrayList::operator=(const ArrayList& other)
{
    // you only do this when the two objects (left and right-other)
    // that uses the operator are different - which you can check by 
    // comparing the address in memory
    if (this != &other) // "this" is a pointer
    {
        std::string* newItems = new std::string[other.cap];
        
        for (unsigned int i = 0; i < other.sz; ++i)
        {
            newItems[i] = other.items[i];
        }
        
        sz = other.sz;
        cap = other.cap;
        
        delete[] items;
        items = newItems;
    }
    
    return *this; // assignments returns a value! 
                  // which is a reference to the obejcts you assign into
}
```
