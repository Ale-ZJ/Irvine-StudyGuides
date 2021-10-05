# Stacks

## Stack Abstract Data Type \(ADT\)

A **stack** is an ADT where elements can only be inserted or removed from the top of the stack. Also known as **last-in first-out** ADT. Can be implemented using a linked list, an array, or a vector.

There is a pointer that keeps track of where the "top" is.

## Stack Operations

| Operation | Description |
| :--- | :--- |
| `Push(stack, x)` | inserts `x` on top of the stack |
| `Pop(stack)` | returns and removes element at top |
| `Peek(stack)` | returns but does not remove element at the top |
| `IsEmpty(stack)` | `true` if stack has no elements |
| `GetLength(stack)` | returns number of elements in the stack |

## Stacks using Linked Lists

* **Stacks** can be implemented with **linked lists**. The head of the list is the stack's "top". 
* A push creates a new list node, assigns the data,  and _prepends_ the node to the front of the list
* A pop assigns a local variable with the head's node data, removes the head node from the list, and returns the local variable. 

```cpp
StackPush( stack, item )
{
    newNode = Allocate new linked list node;
    newNode -> next = null;
    newNode -> data = item;
    
    // Insert as list head (top of stack) 
    ListPrepend(stack, newNode)
}
```

```cpp
StackPop(stack)
{
    headData = stack -> head -> data;
    ListRemoveAfter(stack, null);
    return headData;
}
```



