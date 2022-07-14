---
description: week 2
---

# C++ Standard Library

C++ Standard Library is a collection of libraries that contain functions, classes, algorithms,  iterators, etc that are useful when writing programs. C++ Standard Library inherits from C Standard Library with some modifications. These libraries are written to use _system calls_ (see notes) to interact with an operating system.&#x20;

Before using any of the standard libraries in the `std` namespace, we need to include the respective headers using the _preprocessor directive_ `#include`.&#x20;

{% hint style="info" %}
Fun small tiny detail to keep in the back of your head:&#x20;

When including standard libraries in C++, we surround the name with <> . Usually libraries in C end in `.h` (like`math.h)` and libraries in C++ follow this format `cmath`

Librariess that the user write end in .h ([header files](untitled-1.md#separate-compilation)) and are surrounded by `"" .`
{% endhint %}

## Strings&#x20;

you need to include `string` before using it&#x20;

```cpp
#include <string>

std::string 
```

{% hint style="info" %}
`::` is the _scope resolution operator_. In this example it tells the program to look for the name `string` in the namespace `std` &#x20;
{% endhint %}

* string concatenation&#x20;
* using `==` to compare strings - same sequence of characters&#x20;
* you can also use relational operators&#x20;
  * compares lexicographical order
* splicing `s[1]`&#x20;
  * if you go out of bounds, it is an `undefined behavior` (not illegal - compiler wont complain)&#x20;
  * so instead of using `[]` use `s.at(5)` because it will never fail
    * `at()` costs more tho bc it needs to check if you out of boundaries&#x20;

Some member functions are below, find more in the cpp online documentation

```cpp
if (s.empty())
{
}

s.size()
s.length() //same as size, they return an unsigned int 
```

## StandardIO

in linux there are three types of standardIO: output, input and error (not covered)

### Standard Output

* standard output `std::cout`&#x20;
  * the output goes to the shell by default in linux
  * you can redirect the output to another place `./run app >output.txt`&#x20;
* `./run app >output.txt`
* `./run app | xxd` xxd is a program that shows the output in hexadecimal (pipping?)

{% hint style="info" %}
using these commands doesn't change the program
{% endhint %}

* `std::endl`&#x20;
  * tells the console to add a new carriage return at the end&#x20;
  * it _flushes_ the string so that it doesnt get _buffered_ later&#x20;
  * if you dont add it then the carriage will remain at the end of the line of output

```cpp
void outputExample()
{
    int i =3;
    double d = 13.125;
    std::string s = "Boo";
    
    std::cout << i << "  " << d << s << std::endl;
    //3  13.125Boo
}
```

### Standard Input

* standard input `std:cin`&#x20;

```cpp
void inputExample()
{
    int i;
    double d;
    std::string s;
    
    std::cin >> i >> s >> d;
    //8 cmg 475.875
    
    std::string temp;
    std::getline(std::cin, temp); //skips to the second line
    
    std::string nextLine;
    std::getline(std::cin, nextLine);
    
    std::cin.ignore(1); //skips one space or one line
}
```

## Separate Compilation

Writing programs in more than one file. Adding your own libraries.

* the compiler compiles one source file at a time into a single _object file_
  * when the compiler compiles a second source file, it has amnesia and forgets all the declarations and definitions in the previous source file
    * then, if your program hs multiple source file we need a way to stitch them together&#x20;
* `.cpp` is **source file**&#x20;
  * we can think of them as "modules"
* `.hpp` is **header file**
  * just communicates the declarations (function signatures) NOT definitions
  * include the `.hpp` file of module1 into the modules that uses module1
* Linking - makes sure that all promises to the compiler are fulfilled
  * links an _object file_ with libraries
  * adding `.hpp` makes a promise to `main.cpp` that there's a module somewhere in the library that has the implementation of `.hpp`&#x20;

There are some rules tho

* &#x20;You can't include `.hpp` files more than one time.&#x20;
* So we use the following:
* &#x20;`#` preprocessor directive
  * tells the compiler that you want to treat something differently&#x20;

```cpp
#ifndef AGE_HPP //multiple inclusion guards
#define AGE_HPP

int methodNames();

#endif
```

{% hint style="warning" %}
**One Definition Rule (ODR):** you can only have one definition for a name
{% endhint %}
