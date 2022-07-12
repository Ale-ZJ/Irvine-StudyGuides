# Array-based Lists

An array-based list is a list of Abstract Data Types implemented with an array. Because a static array is initialized with a fixed size, the array-based list dynamically allocates (memory allocated at runtime -> not fixed size) the array with a default capacity (size) and changes the capacity once the number of elements is the same as the capacity.

{% hint style="info" %}
A default size of 1 to 10 is common. Usually, we also double the capacity when the number of elements catches up.
{% endhint %}

### Resize

We must resize the array-based list when the `allocationSize` equals the list's length. A new array with double the capacity is created and the elements from the original array are copied to the new one.

Runtime complexity of O(**N**).

```cpp
ArrayListResize(list, newAllocationSize)
{
    newArray = new array of size newAllocationSize
    
    // copy all elements from list -> array to newArray
    
    list -> array = newArray;
    list -> allocationSize = newAllocationSize;
}
```

### Append

Adds an element at the end of the array-based list.

```cpp
ArrayListAppend(list, newItem)
{
    if (list -> allocationSize == list -> length)
    {
        ArrayListResize(list, list -> length * 2);
    }
    
    list -> array[list -> length] = newItem;
    list -> length = list -> length + 1;
}
```

### Prepend

Inserts a new item at the start of the list.&#x20;

```cpp
ArrayListPrepend(list, newItem)
{
    // make sure to resize if length = capacity
    if (list -> allocationSize == list -> length) 
    {
        ArrayListResize(list, list -> length * 2);
    }
    
    for (i = list -> length; i> 0; i--)
    {
        list -> array[i] = list -> array[i-1];
    }
    
    list -> array[0] = newItem;
    list -> length = list -> length + 1;
}
```

### InsertAfter

Inserts a new element _after_ a given index.&#x20;

```cpp
ArrayListInsertAfter(list, index, newItem)
{
    // make sure to resize if length = capacity
    if (list -> allocationSize == list -> length) 
    {
        ArrayListResize(list, list -> length * 2);
    }
    
    for (i = list -> length; i > index + 1; i--)
    {
        list -> array[i] = list -> array[i-1];
    }
    
    list -> array[index + 1] = newItem;
    list -> length = list -> length + 1;
    
}
```

### Search

Returns the index of the first element whose data matches that of a key. `-1` if not found. O(**N**) complexity.

```cpp
ArrayListSearch(list, item)
{
    for (i = 0; i < list -> length; i++)
    {
        if (list -> array[i] == item)
        {
            return i;
        }
    }
    return -1; //not found
}
```

### Remove At

Removes the item from the list at a given index. O(**N**) complexity.

```cpp
ArrayListRemoveAt(list, index)
{
    if index >= && index < list -> length) 
    {
        for (i = index; i < list -> length -1; i++)
        {
            list -> array[i] - list -> array[i+1];
        }
        list -> length = list -> length - 1;
    }
}
```

