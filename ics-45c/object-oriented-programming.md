# Object Oriented Programming

Three main features:

* Classes
* Inheritance 
* Polymorphism

## Inheritance 

* Use `:` to inherit from a class
* Use `public` before the parent class tells the entire program child class to be aware of this inheritance relationship
* Constructors and destructors are not inherited tho

```cpp
// Student.hpp 

#ifndef STUDENT_HPP
#define STUDENT_HPP

#include "Person.hpp" // parent

class Student : public Person
{
public: 
    Student(const std::string& name, unsigned int age, unsigned int id);
    
    unsigned int getId() const;
    
    // you can also overriding 
    std::string toString() const;
    
private: 
    unsigned int id;
}
```

```cpp
// Student.cpp 

#include <sstream>
#include "Student.hpp"

Student::Student(const std::string& name, unsigned int age, unsigned int id)
    : Person{name, age}, id{id}
{
}

unsigned int Student::getId() const
{
    return id;
}

std::string Student::toString() const
{
    std::ostringstream out; 
    out << Person::toString() << " (ID#" << id << ")";
    return out.str();
}
```

A student is a person, but not all people are students. And this is also true in code!

```cpp
Student s{"Some Student", 21, 12345678};
Person p{"Some Person", 34};

Person p2 = s;   // <-- slicing: the Student part can't be copied 
                 // because there is not enough memory

Student s2 = p;  // illegal!

Person* pptr = &p;   // acts as Person
Person& pref = p;    // acts as Person

Person* pptr = &s;   // acts as Student ONLY IF YOU USE THE KEYWORD VIRTUAL IN FXN SIGNATURE
Person& pref = s;    // acts as Studnet ONLY IF YOU USE THE KEYWORD VIRTUAL IN FXN SIGNATURE
                     // otherwise it will behave as a Person

```

### Virtual Keyword

By adding the keyword virtual in a member function signature, you are telling the compiler \(during run-time\) to look for the appropriate version of a member function \( in this case, `toString()` from Student or Person\) based on the type of object. Virtual is ultimately something you choose to use because it comes with a run time cost.

`virtual` means that this member function is called on the basis of the type of the object and not based on the type of the pointer.

```cpp
virtual std::string toString() const;
```

{% hint style="warning" %}
Write `virtual` only on the base \( parent \) class because derived classes will automatically also be virtual.
{% endhint %}

#### Virtual Destructors

Suppose you have a Person pointer that points at a dynamically allocated Student.

```cpp
Person* dynamicPerson = new Student{"Dynamic Student", 20, 23345678};
```

The question comes then when you want to delete the `dynamicPerson`.

```cpp
delete dynamicPerson; // this is going to call Person's destructors
```

So we add a virtual destructor in the base class Person that uses the default destructor implementation that the compiler does for you.

```cpp
virtual ~Person() = default;
```

### Override Specifier

Member functions are meant to be overridden should be written with the `override` keyword at the end of the signature. This tells the compiler that you intend for the function to be overridden, so that if the signatures don't match, then flag it for us.

```cpp
std::string toString() const override;
```

## Abstract Base Classes

