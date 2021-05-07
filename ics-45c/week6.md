# week6

## Well Behaved classes

Objects of a well-behaved class have these properties:

1. Statically-allocated portion is small and fits on the run-time stack 
2. Clean up after themselves when they die 
3. Can be passed by value and preserve the original stuff 
4. Can be assigned to objects of the same type, with respective copying and clean up.
5. Can be **const** while preserving the ability to perform any const operation
6. Are efficient: no unnecessary work.

## Unnamed Namespace



## Constructors

If you don't declare a constructor, one will be created for you. The default constructor will use the default initialization for the member variables' type. \(For `std::string` the default initialization is an empty string, but for `int` and `pointer` it is an unknown behavior\). So we use **initializers** for the member variables after the construction signature.

```cpp
ArrayList::ArrayList()
    : items{new std::string[initialCapacity]}, sz{0}, cap{initialCapacity}
{
//    std::cout << "ArrayList::ArrayList()" << std::endl;
}
```

## The Big Three

Classes whose objects manage resources that live outside of themselves — such as dynamically-allocated memory, open files, open connections to networks, etc. — generally require three new kinds of functions to be implemented. They come together, don't forget them!

### Destructor 

Member function that will be called right before the object dies. Its job is to perform cleanup of dynamically allocated memory \(_not_ member variables, as they are handled automatically\).

* takes NO parameters 
* Same name as constructor 
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

* object has been passed by value 
  * `foo(ClassType& object);`
* object has been used to explicitly initialize another
  * `ClassType object1;`
  * `ClassType object2 = object 1;`

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

It is called every time an _existing object_ is assigned into _another existing object._ \(Note that both objects already exists\).

* `ClassType object1;`
* `ClassType object2;`
* `object1 = object2;` this is a copy assignment 

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

