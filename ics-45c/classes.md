# Classes

Like other programming languages, a class in C++ is a blueprint for a new kind of object. A class consists of:

* an **interface**: specifies what information is stored in these objects and what can these objects do
* an **implementation**: precisely specifies what happens when you ask objects for a job for you 

## Data types in C++

Unlike Java, which has primitive and class types, C++ does not make a distinction between different types. All types in C++ are on \(more or less\) equal footing. 

Types characteristics:

* A type specifies a certain amount of memory required for its objects.
* A type specifies a layout for that memory
* Some types support "operators" that allow us to manipulate their objects
  * when making a class, you can overload operators 
  * `std::cin >> x`
* Some types support "member functions" that can be called on objects of those types.
  * `s.length()` or `s.size()`
  *  `int` do not have member functions
* A type specifies any initialization that will happen when its objects are created
  * `std::string s;` // initialized empty if you don't say otherwise
* A type specifies how resources it holds are released when its objects are destroyed
  * Resources include memory, open files, open connections over a network...
* A type specifies how or whether or not its objects can be _copied_ or _moved_
  * `std::string s = "Boo";`
* What can be done to "const" objects of a type?



