# Classes

Like other programming languages, a class in C++ is a blueprint for a new kind of object. A class consists of:

* an **interface**: specifies what information is stored in these objects and what can these objects do
  * member variables: information
  * member functions: what jobs it can do
* &#x20;an **implementation**: precisely specifies what happens when you ask objects for a job for you&#x20;

## Data types in C++

Unlike Java, which has primitive and class types, C++ does not make a distinction between different types. All types in C++ are on (more or less) equal footing.&#x20;

Types characteristics:

* A type specifies a certain amount of memory required for its objects.
* A type specifies a layout for that memory
* Some types support "operators" that allow us to manipulate their objects
  * when making a class, you can overload operators&#x20;
  * e.g. the `>>` operator in `std::cin >> x`
* Some types support "member functions" that can be called on objects of those types.
  * `s.length()` or `s.size()`
  * &#x20;`int` do not have member functions
* A type specifies any initialization that will happen when its objects are created
  * `std::string s;` // initialized empty if you don't say otherwise
* A type specifies how resources it holds are released when its objects are destroyed
  * Resources include memory, open files, open connections over a network...
* A type specifies how or whether or not its objects can be _copied_ or _moved_
  * `std::string s = "Boo";`
* What can be done to "const" objects of a type?

Here is a class example:

| Song.hpp                                 | Song.cpp                    |
| ---------------------------------------- | --------------------------- |
| interface                                | implementation              |
| what you need to know in order to use it | how does it work internally |
| use `#ifndef CLASS_HPP`                  |                             |

```cpp
// Song.hpp
// The "interface" of my Song class

#ifndef SONG_HPP
#define SONG_HPP

#include <string>

// A song contains two pieces of information: an artist and a title,
// both strings.

// The inteface of a class consists of two things:
//     1) member variables
//     2) member fucntions

class Song
{
public: // added after Unitest lecture
    static constexpr unsigned int MAX_ARTITST_LENGTH = 40;
    static constexpr unsigned int MAX_TITLE_LENGTH = 40;

public:
    Song(const std::string& initialArtist, const std::string& initialTitle); // constructor
    
    std::string getArtist() const; // <-- legal to call this on a const Song
    std::string getTitle() const;  // <-- legal to call this on a const Song
    
    void setArtitst(const std::string& newArtist);
    void setTitle(const std::string& newTitle);
    
private:
    std::string artist;
    std::string title;
};
```

* `Song` is a constructor
  * initialization&#x20;
  * same name as class&#x20;
  * no return type
* `public`: accessible anywhere in the program&#x20;
  * usually member functions&#x20;
* `private`: only accessible only within this class
  * usually member variables because outsiders can change the information
* `std::string getArtist() const` means that you can call the function using `const`
* `static` belongs to the class as a whole and not to the individual object of the class

```cpp
// Song.cpp 
// The implementation of my Song class 

#include "Song.hpp"

// constructor
Song::Song(const std::string& initialArtist, const std::string& initialTitle)
    // initializers
    : artist{initialArtist}, title{initialTitle}
{
}

std::string Song::getArtist() const
{
    return artist;
}

std::string Song::getTitle() const
{
    return title;
}

void Song::setArtitst(const std::string& newArtist)
{
    artist = newArtist;
}

void Song::setTitle(const std::string& newTitle)
{
    title = newTitle;
}
```

* `Song::Song` tells the compiler that we want the function Song inside the class Song.
  * same as when using`std::string`
* `const std::string&`&#x20;
  * we pass by reference because if not we would have to make a copy every time
  * there is no reason why the initial parameters will change
  * remember to make the same changes in the hpp file - same signature
* **initializers**
  * if you put `artist = initialArtist` inside Song constructor (inside curly braces) you are initializing the value of artist again. You already initialized it to be empty (in hpp) before entering the body of Song.&#x20;
  * instead, after the constructor signature add `: artist{initialArtist}, title{initialTitle}`
    * this way artist and title are "born" with the correct type of initialization

```cpp
// main.cpp

#include <iostream>
#include "Song.hpp"

void printSongInfo(const Song& song)
{
    std::cout << song.getArtist() << " - " << song.getTitle() << std::endl;
}

int main()
{
    Song song{"U2", "Moment of Surrender"}; // curly braces mean initialization
    printSongInfo(song)
    
    return 0;
}
```

{% hint style="warning" %}
curly braces mean initialization
{% endhint %}

* When writing member functions in a class, they always have a `this` parameter!&#x20;
