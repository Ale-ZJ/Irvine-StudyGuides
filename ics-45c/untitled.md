# Untitled

* Python is a dynamically typed language 
  * it is dynamic while the program is running 
  * type checking occurs while the program runs 
* C++ is a statically typed language
  * types are checked before the program runs 
    * won't compile 
  * "stricter"

### Basic built-in data types

* Integral types 
  * int 32 bits -2.1bil ... 2.1bil
  * long 64 bits
  * long long 64 bits
  * short 16 bits -32768 ... 32768
  * char 8 bits -128 ... 128 \(2^8-1\)
  * each type has different size \(memory\)
  * differ per machine 
  * sizeof\(char\) &lt;= sizeof\(short\) &lt;= sizeof\(int\) &lt;= sizeof\(long\) &lt;= sizeof\(long long\)
* Unsigned integral types 
  * unsigned int 0 ... 4.2bil
  * unsigned long 
  * unsigned long long
  * unsigned short 
  * unsigned char
  * always non-negative 
    * doesn't care about the sign 
  * nice: represents more numbers with the same memory space
  * flip side: 0 - 1 rolls back to 4.2 
* Boolean type \(bool\) - true, false
  * you can take a bool type and put it into a int and viceversa 
  * 0 means false 
  * non-zero means true
* Floating-point types - numbers that can have a fractional part
  * float 32bits
  * double 64bits
  * scientific notation but with 2 as base
    * how many bits in front and how many bits as exponent \( base x2^exponent\)

### Expressions and Statements

* Bodies of functions are made up of statements 
* statements are jobs 
* statements can contain expressions 
* expressions calculate a value and return it \(which means they also have a type\)



* What does expression look like?
  * a / b
* What kinds of statements are there?
  * expression statement 
    * a + b;
  * assignment
    * a = 3 
      * this is an expression too tho 
      * so it returns the value three
        * you could do a = \( a + 3 \) + \(b + 2\)
  * compound/block statements
    * { ..// .. // ..}
      * control structures

```cpp
//control structures
if (a < 3 && b > 2)
{
}
else
{
}

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

//loops! 
int a = 3;
while (a < 10)
{
    std::cout << a << std::endl;
    a++;
}

int a = 3;
do
{
//code executes at least ONCE
}
while (a < 10)

for (int a = 3; a<10; a++)
{
}
```

###  Declarations and Definitions 

* declarations introduce a name into a C++ program and associate it with a type \(`name` exists! wow\)
  * variable declaration 
    * int a; 
    * creates a name `a` with type `int` 
  * function declaration 
    * write their signature 
      * int square\(int n\); 
    * declaration of a function doesnt mean that the function has meaning 
* definitions: gives a name life, makes it exist
  * int a; //its variable declaration AND definition 
  * int a = 3; //declaration + definition + initialization 
  * int square\(int n\) { } //a promise
  * int square\(int n\) { return n\*n }

### Lvalues and Rvalues

* lvalue: an expression that would be legal on the left-hand side of an assignment 
  * place to live 
  * piece of storage
* rvalue: an expression that would be legal on their right-hand side of an assignment

### Functions

* order matters! 
* names must be declared before they are used! 
* linker: all promises must be made
* `void` functions 
  * doesn't return anything





