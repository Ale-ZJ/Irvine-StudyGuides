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

