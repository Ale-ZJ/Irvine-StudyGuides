# week5

## Functions and Lamdas

In C++, a function object is an object that can be called like a function

What can we do with variables: 

* store them in variables 
* pass them as arguments to functions 
* return them as the result of functions 
* store them in member variables of a class and initialize them in a constructor 

{% hint style="info" %}
function() when using parenthesis, you are telling the compiler to use the function right now. 

function without the parentheses it will return a function pointer
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

### Function literals - lambdas 

**Literal **means an object without a name. So `"Boo"` or `10` or `'A'` are all literals. And there is also function literals called **lambd**a expression.

```cpp
transform(a, 10, []      (int i)      { return i + i });
                 --      -------      -----------------
                 lambda  param list   the function job
```

* \[] tells the compiler the next thing is a lambda expression 
* you don't need a return type because the compiler will figure it out on its own

```cpp
int x = 3;

transform(a, 10, [=]      (int i){ return x + i });
                 ---     
                 = copy     
                 & ref
```

## Linked Data Structures

* Arrays: each element lives contiguously (next to each other) in memory address 
*  

```cpp
// LinkedList.hpp

#ifndef LINKEDLIST_HPP
#define LINKEDLIST_HPP

class LinkedList 
{
public:
    int sum() const;

private:
    struct Node
    {
        int value;
        Node* next; // legal!
    };
    
    Node* head;
};

#endif
```



```cpp
// LinkedList.cpp

#include "LinkedList.hpp"

int LinkedList::sum() const
{
    int total = 0;
    
    const Node* current = head;
    
    while (current != nullptr)
    {
        total += current->value;
        current = current->next;
    }
    
    return total;
}
```

## Unitest

In this course, we are using UNITEST from Google Test.

```cpp
// putting these functions into the "unnamed namespace"
// only visible in this source file
namespace
{
    std::string truncate(const std::string& s, unsigned int length)
    {
        if (s.size() > length)
        {
            return s.substr(0, length);
        }
        else
        {
            return s;
        }
    }
    
    std::string truncateArtist(const std::string& artist)
    {
        return truncate(artist, Song::MAX_ARTIST_LENGTH);
    }
    
    std::string truncateTitle(const std::string& title)
    {
        return truncate(title, Song::MAX_TITLE_LENGTH);
    }
}
```

Inside the `gtest` file, create a new source file with your tests. 

```cpp
// SongTests.cpp

#include <gtest/gtest.h>
#include "Song.hpp" // all files in gtest are seen in app

TEST(SongTests, containArtistGivenWhenCreated)
{
    Song s{"U2", "Breath"};
    ASSERT_EQ("U2", s.getArtist());
}

TEST(SongTests, containTitleGivenWhenCreated)
{
    Song s{"Arcade Fire", "After Life"};
    ASSERT_EQ("After Life", s.getTitle());
}

TEST(SongTests, afterChangingArtist_ContainNewArtist)
{
    Song s{"U2", "Afterlife"};
    s.setArtist("Arcade Fire");
    ASSERT_EQ("Arcade Fire", s.getArtist());
    EXPECT_EQ("Arcade Fire", s.getArtist());
}

TEST(SongTest, truncateArtistAtCreationIfTooLong)
{
    std::string longArtist(Song::MAX_ARTITST_LENGTH + 5, 'x');
    std::string truncatedArtist(Song::MAX_ARTIST_LENGTH,  'x');
    
    Song s{longArtist, "Some Title"};
    ASSERT_EQ(truncatedArtist, s.getArtist());
}
```

{% hint style="info" %}
Anything you **assert**, you can **expect**.
{% endhint %}

* **Assert**: if this isn't true then test fail and the test immediatly stops
* **Expect**: check this, and if it is wrong tell me, and keep running the test
