# week 8

## Standard Libraries

* Containers 
  * are data structures that store other things. 
* Generic Algorithm 
* Iterators
  * loop through 
  * abstract details 
    * doesnt need to know what data structures they are working with 

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

```cpp
#include iterators

```

