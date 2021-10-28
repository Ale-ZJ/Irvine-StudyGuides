---
description: 'std::cout << "muffled screams"'
---

# Sanity check for myself

There are so many symbols... :s

```cpp
#include <iostream>
#include <string>

struct Node{
  std::string key; 
  Node* next;
};

int main() {
  std::cout << "Hello from C++!" <<std::endl;

  
  Node* list = new Node[3]; //synamic array of Nodes
  list->key = "hi";
  std::cout << list << std::endl;
  std::cout << list->key << std::endl;
  
  // dynamic array of Node pointers (aka linked lists)
  Node** arrayOfPointers = new Node*[3]; // first allocate the array 
  
  for(int i = 0; i < 3; ++i) { // then allocate the pointers!! else segmentation error
    arrayOfPointers[i] = new Node;
	}
  
  arrayOfPointers[0]->key = "hellow";
  
  arrayOfPointers[0]->next = new Node;
  arrayOfPointers[0]->next->key = "there";
  
  std::cout << arrayOfPointers[0]->key << std::endl; 
  // "hellow"
  std::cout << arrayOfPointers[0]->next->key << std::endl;
  // "there"
  
  return 0;
}
```



