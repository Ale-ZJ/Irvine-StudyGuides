# Doubly-linked lists

A doubly-linked list is a data structure that implements a list Abstract Data Type (ADT).&#x20;

* each node has:
  * data
  * pointer to the next node
  * pointer to the previous node
* the list structure has a pointer to the first node (head) and another pointer to the last node (tail)
* type of _positional list_

## Appending a node to a doubly-linked list

**`Append`** inserts a new node after the list's tail node (last node). Different if the list is empty versus not empty:

* Append to empty list:
  * head pointer is NULL
  * algorithm points the list's head and tail pointers to the new node
* Append to non-empty list:&#x20;
  * algorithm points the tail node's next pointer to the new node
  * points the new node's previous pointer to the list's tail node
  * points list's tail to the new node

```cpp
ListAppend( list, newNode) 
{
    if (list -> head == null) //list is empty
    {
        list -> head = newNode;
        list -> tail = newNode;
    }
    else
    {
        list -> tail -> next = newNode;
        newNode -> prev = list -> tail;
        list->tail = newNode
    }
}
```

## Prepending a node to a doubly-linked list

**`Prepend`** inserts a new node before the list's head node (first node) and update the head pointer to point the new node. Different if it is empty and non-empty:

```cpp
ListPrepend(list, newNode)
{
    if (list -> head == null) //list is empty
    {
        list -> head == newNode;
        list -> tail == newNode;
    }
    else
    {
        newNode -> next = list -> head;
        list -> head -> prev = newNode;
        list -> head = newNode;
    }
}
```

## Insert a node in a doubly-linked list

**`InsertAfter`** inserts a new node after a given _existing_ list node (aka. `curNode`). There are three scenarios: insert as first node, last node, middle node.

```cpp
ListInsertAfter(list, curNode, newNode) 
{
    if (list -> head == null) // list is empty
    {
        list -> head = newNode;
        list -> tail = newNode;
    }
    else if (currNode == list -> tail) // insert after tail
    {
        list -> tail -> next = newNode;
        newNode -> prev = list -> tail;
        list -> tail = newNode;
    }
    else // insert in the middle
    {
        sucNode = curNode -> next;
        newNode -> next = sucNode;
        newNode -> prev = curNode;
        curNode -> next = newNode;
        sucNode -> prev = newNode;
    }
    
}
```

## &#x20;Remove a node in a doubly-linked list

**`Remove`** deletes a given existing list node (aka `curNode`). There are four different checks in this algorithm:&#x20;

* succeasor exists
* predecessor exists
* removing list's head node (first node)&#x20;
* removing list's tail node (last node)&#x20;

when removing in the middle of the list, make sure to connect the other nodes.

when removing the only node, then make the list empty&#x20;

```cpp
ListRemove(list, curNode)
{
    sucNode = curNode -> next;
    predNode = curNode -> prev;
    
    if (sucNode != NULL)
    {
        sucNode -> prev = predNode;
    }
    if (predNode !+ NULL)
    {
        predNode -> next = sucNode;
    }
    if (curNode == list -> head) // remove head
    {
        list -> head = sucNode;
    }
    if (curNode == list -> tail) // remove tail
    {
        list -> tail= predNode;
    }
}
```

