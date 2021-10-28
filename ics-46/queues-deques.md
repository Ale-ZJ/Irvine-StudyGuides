# Queues, Deques

## Queues

A **queue** is an Abstract Data Type in which items are inserted at the end of the queue and removed from the front. A queue can be implemented with a linked list or an array.

## Queue Operations 

| Operation           | Description                                                 |
| ------------------- | ----------------------------------------------------------- |
| `Enqueue(queue, x)` | inserts `x` at the end of the queue                         |
| `Dequeue(queue)`    | returns and removes the first element of the queue          |
| `Peek(queue)`       | returns but does NOT remove the first element of the queue  |
| `IsEmpty(queue)`    | returns `true `if queue has no elements                     |
| `GetLength(queue)`  | returns the number of elements in the queue                 |

{% hint style="warning" %}
`Dequeue` and `Peek` should NOT be applied to an empty queue. Undefined behavior.
{% endhint %}

##  Queues using Linked Lists

The head's node of a linked list is the queue's front and the list's tail node is the queue's end.

### Enqueue

```cpp
QueueEnqueue( queue, item )
{
    newNode = Allocate new linked list node
    nextNode -> next = null;
    newNode -> data = item;
    
    // Insert node as list tail (end queue)
    ListAppend( queue, newNode );
}
```

### Dequeue

```cpp
QueueDequeue( queue )
{
    headData = queue -> head -> data;
    ListRemoveAfter( queue, null );
    return headData;
}
```

## Deques

A **deque** is an Abstract Data Type where elements can be inserted and removed from both the front and back.

## Deques Operations

| Operation             | Description                                                |
| --------------------- | ---------------------------------------------------------- |
| `PushFront(deque, x)` | Inserts `x` at the front of the deque                      |
| `PushBack(deque, x)`  | Inserts `x` at the back of the deque                       |
| `PopFront(deque)`     | returns and removes element at the front of the deque      |
| `PopBack(deque)`      | returns and removes element at the end of the deque        |
| `PeekFront(deque)`    | returns but does NOT remove the first element in the deque |
| `PeekBack(deque)`     | returns but does NOT remove the last element in the deque  |
| `IsEmpty(deque)`      | returns `true` if the deque is empty                       |
| `GetLength(deque)`    | returns the number of elements in the deque                |

