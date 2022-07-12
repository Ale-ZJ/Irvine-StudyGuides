# week7

## Containers

The C++ Standard Library provides a set of what are called containers (or data structures), some examples of which include:

* **std::vector**, an array-based sequence, in the same spirit as the ArrayList class we wrote in a previous example
* **std::list**, a doubly-linked list with head and tail pointers
* **std::map**, a balanced binary search tree of key/value pairs

## Standard Libraries

* Containers&#x20;
  * are data structures that store other things.&#x20;
* Generic Algorithm&#x20;
* Iterators
  * loop through&#x20;
  * abstract details&#x20;
    * doesnt need to know what data structures they are working with&#x20;

### Vectors

```cpp
#include <vector> // ArrayList we built in week 5 (but better)

// std::vector is not a class, it is a class "template"
// instantiation 

void foo()
{
    std::vector<int> v;
    
    for (int i = 0; i < 10000; ++i)
    {
        v.push_back(i);
    }
    
    // construction with an "initializer list"
    //    - initialize the vector to contain 1, 2, 3, 4, 5
    // the constructor takes an initializer as a parameter 
    std::vector<int> w{1, 2, 3, 4, 5};
    
    // x to contain 10 elements whose values are all 100
    std::vector<int> x(10, 100);
    
    //you can ask vectors for a size or a capacity 
    for (unsigned int i = 0; i < v.size(); ++i)
    {
        v[i] += 1; // indexing is not bounds-checked
        std::cout << v[i] << " ";
        
        v[i] += 1; // bounds-checked
        std::cout << v.at(i) << " ";
    }   
}

void bar(const std::vector<int>& v)
{
    v[3] = 50; // illegal
    
    for (unsigned int i = 0; i < v.size(); ++i) // legal
    {
        v[i] += 1; // indexing is not bounds-checked
        std::cout << v[i] << " ";
    }
}

// this only works in C++17
void notSpecifyingTypes()
{
    std::vector v1; // illegal because std::vector is not a type
                    // compiler can't help you deduce the type it can store
    std::vector v1{1, 2, 3, 4, 5}; // v1's type will be deduced as std::vector<int>
    std::vector v2{"hello", "there"}; // it is actually std::vector<cosnt* char>
}

```

### Iterators

Benefits

* Forward iterators `*` `->` `++` `==` `!=`&#x20;
  * move one by one&#x20;
  * `std::forward_list` (singly-linked list)
* Bidirectional iterators `--`&#x20;
  * like forward but now you can go backwards
  * `std::list` (doubly-linked list)
* Random-access iterators&#x20;
  * pointer arithmetic `+=` `-=`&#x20;
  * you can go forward by 5, 6 , back by 7 , 8, etc

```cpp
std::vector<std::string> v{"Boo", "is", "happy", "today"};
std::vector<std::string>::iterator i = v.begin(); //returns an iterator of a vector of string

// it is like pointers arithmetic! 
// where .begin() is the pointer of the start of your iterable item
// and .end() is a pointer to the last element
for (std::vector<std::string>::iterator j = v.begin(); i != v.end(); ++i)
{
    std::cout << *i << std::endl; // * to de reference a pointer (aka get the value at the end of the pointer)
}
// Output:
// Boo
// is
// happy
// today

for (std::vector<std::string>::iterator j = v.begin(); i != v.end(); ++i)
{
    std::cout << i->length() << std::endl; // you can also call on member functions
}
// Output:
// 3
// 2
// 5
// 5
```

but std::vector::iterator is a mouthful so you can use auto instead

#### Type Inference&#x20;

compiler will deduce what type to assign a variable based on the code you already have.

```cpp
// so this
for (std::vector<std::string>::iterator j = v.begin(); i != v.end(); ++i)
// is equivalent to 
for (auto j = v.begin(); i != v.end(); ++i)
```

When you pass a vector as a parameter, pass it by reference always (and const if you can). But what is the type of `i` inside the for loop if you pass by reference and say that it's content will remain constant.&#x20;

```cpp
void printAll(const std::vector<std::string>& v)
{
    for (std::vector<std::string>::const_iterator i = v.begin(); i !=  v.end(), ++i) ...
}
```

There is a `const_iterator` in every container!

## range-based "for" loop

Ever miss python for loops?&#x20;

```python
a = [1,2,3,4,5]
for i in a:
    print(i)
```

fear not, here is the c++ implementation.

```cpp
for (const std::string& s : v)
{
    std::cout << s << std:: endl;
}
```

## Generic Algorithm

A library in the standard library that has algorithms that implements redundant lines of code for you. For example looping through a list and reading its content, etc

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main
{
    std::vector<int> v1{1, -2, 3, -4, 5, -6};
    
    // loop from     beginning to end             and do what you tell it to do
    std::for_each(v1.begin(), v1.end(), [](int i){std::cout << i << " "; });
    
    
    // loop throu all the elements and see if they all return true
    bool allNonNegative = std::all_of(
        v1.begin(), v1.end(), [](int i){ return i >= 0; });
        
        
    // count how many elements that follow a criteria 
    unsigned int nonNegativeCount = std::count_if(
        v1.begin(), v1.end(), [](int i){ return i >= 0; });
        
    
    std::vector<int> v2;
        
    // transform the given iterable v1 and insert the result at the
    // end of another container called v2
    std::transform(v1.begin(), v1.end(), std::back_inserter(v2), 
        [](int i){ return i * i; });
        
    
}
```

