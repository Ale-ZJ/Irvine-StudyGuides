---
description: week 5
---

# Linked Data Structures

* Arrays: each element lives contiguously (next to each other) in memory address&#x20;
* &#x20;

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

