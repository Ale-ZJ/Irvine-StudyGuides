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

{% hint style="warning" %}
`Pop` and `Peek` should NOT be applied to an empty Stack. Undefined behavior.
{% endhint %}

## What problems does Stack solve?

* web browser "back" button 
* postfix notation 
  * \( \(5+2\) \* \(8-3\)\) / 4
  * 5 2 + 8 3 - \* 4 /
  * if it is a number then push it to a stack
  * if you see an operation, then perform the operation and replace the top of the stack with the solution

## Stacks using Array

Here is a **template** to implement Stacks with Arrays. Remember that a template is a **set of classes** and the template is implemented in `.hpp`

###  .hpp

```cpp
// .hpp file

template<typename Object>
class ArrayStack
{
private:
    enum { CAPACITY = 1000};
    size_t capacity; 
    Object* elements; // array of elements
    int topIndex;
    
public: 
    // CONSTRUCTOR
    explicit ArrayStack(int cap = CAPACITY);
    // explicit prevents you from chagning atributes 
    // of an Object directly, you are forced to use a 
    // a constructor
    //
    // ex) w/out: ArrayStack<double> a = 5;
    //     w/:    ArrayStack<double> a(5);
    
    
    // COPY CONSTRUCTORS
    ArrayStack(const ArrayStack& st);
    ArrayStack& operator=(const ArrayStack& st);
    
    // DESTRUCTORS
    ~ArrayStack()
    {
        delete [] elements;
    }
    
    
    
    // MORE FUNCTIONS WE WILL CALL
    size_t size() const noexcept; // noexcept says that you don't expect any exceptions
    bool isEmpty() const noexcept;
    
    Object& top();
    const Object& top() const;
    
    void push(const Object& elem);
    
    Object pop();
    
    
};
```

### .cpp

```cpp
// .cpp file

template<typename Object>
ArrayStack<Object>::ArrayStack(int cap)
{
    capacity = cap;
    elements = new Object[capacity];
    topIndex = -1;
}

template<typename Object>
ArrayStack<Object>::size( const noexcept
{
    return topIndex + 1;
}

template<typename Object>
bool ArrayStack<Object>::isEmpty() const noexcept
{
    return (topIndex < 0);
}

```

### Unit Testing

```cpp
TEST(StackTests, StackIsEmptyAtStart1)
{
    ArrayStack<int> st;
    EXPECT_TRUE( st.isEmpty() ) << "foo"; // you can add messages to your unit testing
}

TEST(StackTests, StackIsEmptyAtStart2)
{
    ArrayStack<int> st;
    EXPECT_TRUE( st.size(), 0 );
}
```

## TODO

write ArrayQueue functions for 



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



