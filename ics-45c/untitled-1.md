---
description: week 2
---

# week2

## Strings&#x20;

you need to include strings before using it&#x20;

```cpp
#include <string>

std::string 
```

{% hint style="info" %}
`std::` is part of the C++ standard library that represents a namespace
{% endhint %}

* string concatenation&#x20;
* using `==` to compare strings - same sequence of characters&#x20;
* you can also use relational operators&#x20;
  * compares lexicographical order
* splicing `s[1]`&#x20;
  * if you go out of bounds, it is an `undefined behavior` (not illegal - compiler wont complain)&#x20;
  * so instead of using `[]` use `s.at(5)` because it will never fail
    * `at` costs more tho!&#x20;

Member functions&#x20;

```cpp
if (s.empty())
{
}

s.size()
s.length() //same as size, they return an unsigned int 

for (unsigned int i = 0; i < s.size(); ++i)
{
}

++i //pre-increment, add value to i and then return
i++ //post-increment, get value of i and add 1 to i
```

Pre-incrementation means the variable is incremented before the expression is set or evaluated. Post-incrementation means the expression is set or evaluated, and then the variable is altered.

```
b = x++;
```

is really:

```
b = x;
x++;
```

and

```
b = ++x;
```

is really:

```
x++;
b = x;
```

pre-incrementing is efficient and faster because you don't use the variable returned.

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

Writing programs in more than one file.&#x20;

* files ending in `.cpp` compiles separately and have amnesia -they forget- after they compile them&#x20;
  * put the signature of the method of module 1 in module 2 where you want to use it
* `.cpp` is source file&#x20;
  * we can think of them as "modules"
* `.hpp` is **header file**
  * just communicates the declarations (function signatures) NOT definitions
  * include the `.hpp` file in module 2 that uses module 1
* objects files are created
* Linking - makes sure that all promises to the compiler are fulfilled
  * adding `.hpp` makes a promise to `main.cpp` that there's a module somewhere in the library that has the implementation of `.hpp`&#x20;



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

* **One definition rule**&#x20;
  * you can only have one definition for a name

## Behind the Scenes

#### How to understand the abstraction layer of your programming language

Other programming languages are considered higher abstraction level because there are fewer details that you have to handle.&#x20;

* What are you allowed to say?
* What are the semantics?
* What effects does the construct have on the performance?
* How to decompose a larger program into smaller pieces? How do reuse pieces?
* Who decides how and when values are organized in memory?
* How do you interact with devices other than the processor and memory?

The abstraction level of C++ abstraction level is close to the abstraction level of your machine (it is of lower level).

To use C++, we need to better understand some eecs stuff:

* Computer organization  (processors, registers, the memory hierarchy)
* The runtime organization of memory&#x20;
* What determines the real cost of the things we're doing

#### The Von Neuman Architecture

* **CPU**: grab one instruction at the time&#x20;
* **Main memory**:&#x20;
* **bus**: wires that connect from the CPU to the main memory&#x20;
* **registers**: scratch paper inside CPU to temporarily remember small stuff&#x20;
  * one of them is called: **instruction pointer**&#x20;
    * tells you where in the main memory are stuff stored

1- Read instruction \
2- Decode it - what bits do you need - processes - what to do\
3- read operands\
4- execute the instruction \
5- store the result&#x20;

\--> every statement and expressions in code is a collection of instructions

#### Implementing C++ features

A C++ compiler has two jobs:  //basically it translates into assembly language\
(1) Check syntax and semantics \
(2) Emit machine instructions that have the same observable effect as your program\
&#x20;         \-- includes optimizations

#### Implementing variables

When you are declaring a variable, you are asking for memory space.&#x20;

#### Implementing control structures&#x20;

CPUs have "jump instructions" (the next thing you need to do is over there) and "branch instructions" (there is a condition)

```cpp
if (???) <--- conditional expression of an integral type
```

If the conditional expression evaluates to 0 then the expression is False, any other value is True (0=FALSE, anything else = TRUE). The C++ if statement is the same.

#### What about functions?

There is a cost for the organization. Run time stacks have activation records that keep track of all the variables and stuff needed to run a function.



