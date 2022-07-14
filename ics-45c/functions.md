---
description: week 3 and week 5
---

# Functions

## Parameter types

There are many ways to pass an argument in a function call. We will cover two main ones: pass-by-value and pass-by-reference.

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

## Compatibility

When assigning a value to a variable, the types of the value and the variable must be compatible. Even if the types are not exactly the same, there are compatible types.&#x20;

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

## Function overloading&#x20;

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

## Default Arguments

ALWAYS appear after ALL the named parameters.

```cpp
double vectorLength(double x, double y, double z = 0.0)
{
    return std::sqrt(x * x + y * y + z * z);
}
```

## Functions and Lamdas

In C++, a function object is an object that can be called like a function

What can we do with variables:&#x20;

* store them in variables&#x20;
* pass them as arguments to functions&#x20;
* return them as the result of functions&#x20;
* store them in member variables of a class and initialize them in a constructor&#x20;

{% hint style="info" %}
`function()` when using parenthesis, you are telling the compiler to use the function right now.&#x20;

`function` without the parentheses it will return a function pointer
{% endhint %}



```cpp
#include <functional>

std::string duplicate(const std::string& s)
{
    return s + s;
}

int square(int n)
{
    return n * n;
}

int multiply(int n, int m)
{
    reutrn n * m;
}

int main()
{
    int i = 3;
    int* p = &i
    
    std::cout << i << std::endl;
    std::cout << duplicate("Boo") << std::endl;
    
    std::function<std::string(const std::string&)> f1 = duplicate;
    std::function<int(int)> f2 = square;
    std::function<int(int, int)> f3 = multiply;
    
    std::cout << f1("Boo") << std::endl;
    std::cout << f2(3) << std::endl;
    std::cout << f3(3,4) << std::endl;
}
```

{% hint style="info" %}
`<>` talks about the return and paremeter types
{% endhint %}

### Higher-order Functions

Higher order functions are functions that take other functions as parameters and/or return functions as their result. This is an essential feature in any functional programming language. Gives flexibility.

```cpp
void transform(int* a, unsigned int size, std::function<int(int)> f)
{
    for (unsigned int i = 0; i < size; ++i)
    {
        a[i] = f(a[i]);
    }
}

int main()
{
    int a[10] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    
    transform(a, 10, square);
//                   ------ 
//                   can be any function!
    
    return 0;
}
```

### Function literals - lambdas&#x20;

**Literal** means an object without a name. So `"Boo"` or `10` or `'A'` are all literals. And there is also function literals called **lambd**a expression.

```cpp
transform(a, 10, []      (int i)      { return i + i });
                 --      -------      -----------------
                 lambda  param list   the function job
```

* \[] tells the compiler the next thing is a lambda expression&#x20;
* you don't need a return type because the compiler will figure it out on its own

```cpp
int x = 3;

transform(a, 10, [=]      (int i){ return x + i });
                 ---     
                 = copy     
                 & ref
```
