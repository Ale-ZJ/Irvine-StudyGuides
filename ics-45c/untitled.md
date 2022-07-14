---
description: week 1
---

# C++ Basics

## C++ is a static compiled language

C++ is a subset of the C programming language that comes with classes. C++ is one of the fastest and most efficient programming languages.&#x20;

| Python                                                                  | C++                                                                                                   |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| _dynamically_ typed language: checks for types _while_ the program runs | _statically_ typed language: checks for types _before_ the program runs                               |
| you don't need to specify the type of variables nor functions           | strict type checking, you need to specify the type of variable and function to pass, store or  return |
| two-step process: edit and run                                          | three-step process: edit, compile and run                                                             |
| no pointers, every variable is a reference to an object                 | objects can be passed as references or pointers ([see notes](week3.md#pointers))                      |

Since C++ is a compiled language, it needs a compiler to convert the source code into executable code (machine language). Linux has a preinstalled compiler C compiler called GCC, and a C++ compiler called G++. We use the commands `gcc` and `g++` respectively.

## Basic built-in data types

* Integral types&#x20;
  * `int` 32 bits -2.1bil ... 2.1bil
  * `long` 64 bits
  * `long long` 64 bits
  * `short` 16 bits -32768 ... 32768
  * `char` 8 bits -128 ... 128 (2^8-1)
  * each type has different size (memory)
  * differ per machine&#x20;
  * sizeof(char) <= sizeof(short) <= sizeof(int) <= sizeof(long) <= sizeof(long long)
* Unsigned integral types&#x20;
  * `unsigned int` 0 ... 4.2bil
  * `unsigned long`
  * `unsigned long long`
  * `unsigned short`
  * `unsigned char`
  * always non-negative &#x20;
  * nice: represents more numbers with the same memory space
  * not so nice side: 0 - 1 rolls back to 4.2bil&#x20;
* Boolean type (`bool`) - true, false
  * you can take a bool type and put it into a int and viceversa&#x20;
  * 0 means false&#x20;
  * non-zero means true
* Floating-point types - numbers that can have a fractional part
  * `float` 32bits
  * `double` 64bits
  * scientific notation but with 2 as base
    * base x 2^exponent
  * amount of bits correlates to the precision of the decimal

## Expressions and Statements

* Functions are made up of statements&#x20;
* Statements perform a job
* Statements can contain expressions&#x20;
* An expression evaluates to a value and returns it (which means they also have a type)

Here are some examples:

* What does expression look like?
  * `a / b`
* What kinds of statements are there?
  1. expression statement&#x20;
     * `a + b;`
  2. assignment
     * `a = 3;`&#x20;
       * this is an expression too&#x20;
       * so it returns the value `3`
         * you could do `a = (a + 3) + (b + 2)`
  3. compound/block statements
     * `{ ..// .. // ..}`
       * such as control and loop structures

#### Control Structures

```cpp
// if statements
if (a < 3 && b > 2)
{
}
else
{
}

// switch case statements
switch (a) //controlling expression in parenthesis, must be integral
{
case 0:
    std::cout << "zero" << std::endl;
    //with no BREAK statement, it just continues to the next line
    
case 1:
    std::cout << "zero" << std::endl;
    break
    
default: //technically a FLAW
    std::cout << "zero" << std::endl;
    break
}
```

#### Loops

```cpp
// while loop
int a = 3;
while (a < 10)
{
    std::cout << a << std::endl;
    a++;
}


// do while loop
int a = 3;
do
{
    //code executes at least ONCE
}
while (a < 10)


// for loop
for (int a = 3; a<10; a++)
{
} 
```

#### Post and Pre increment&#x20;

```cpp
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

### Declarations and Definitions&#x20;

* A declaration introduces a name into a C++ program and associates the name with a type
  1. **variable declaration**&#x20;
     * `int a;`&#x20;
     * creates a name `a` of type `int`&#x20;
     * it gives a block of memory of size(int) where values can be stored
     * the value of `a` is _**undefined**_ unless you assigned a value to `a` later or you _**initialize**_ it when you declare `a`
  2. **function declaration** &#x20;
     * `int square(int n);`&#x20;
       * creates a definition of a function named `square` of return type `int` that takes a parameter with name `n` and type `int`
       * this is called a function's _**signature**_
       * note the `;` at the end
     * declaration of a function doesnt mean that the function has meaning&#x20;
       * you need to define them too!
* definitions: gives a name life, makes it exist
  * `int a;`&#x20;
    * //variable declaration AND definition&#x20;
  * `int a = 3;`&#x20;
    * //declaration + definition + initialization&#x20;
  * `int square(int n) { }`&#x20;
    * //a promise
  * `int square(int n) { return n*n }`
    * //declaration + definition

### Lvalues and Rvalues

* **lvalue**: an expression that would be legal on the left-hand side of an assignment&#x20;
  * place to live&#x20;
  * piece of storage
  * location in memory
    * later you will learn that this is a [_reference_](week3.md)__
* **rvalue**: an expression that would be legal on their right-hand side of an assignment
  * usually a temporary value that is not stored anywhere
  * e.g. a constant

## Functions

* order matters!&#x20;
* names must be declared before they are used!&#x20;
* linker: checks that all promises are made
  * promises of defining a function declaration later on for example
* `void` functions&#x20;
  * doesn't return anything

## First C++ program

A program in C++ begins with a call to `main` and ends when `main` returns. &#x20;

```
int main()
{
    return 0;
}
```

The C++ Standard requires `main` to return an integer value. We usually return 0 because an exit code of 0 is used to indicate program success. Other integers indicate other kinds of failures (not covered).&#x20;

{% hint style="info" %}
`main()` can also have two parameters:

`int main(int argc, char *argv[]) {}`

* **`argc`**`:`amount of argument
* **`argv`**`:`holds all the arguments that are "passed to the main() function through the command line when executing a program
{% endhint %}

For this course, use `main()` with no parameters.
