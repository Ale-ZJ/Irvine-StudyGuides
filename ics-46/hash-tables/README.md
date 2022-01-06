# Hash Tables

A **hash table** is a data structure that stores unordered items by **mapping** (or **hashing**) each item to a location in an array.&#x20;

What is mapping? Getting an index value (more specifically hash value) from the item that tells you the location in the array where an item should be stored

* we use an item's key, which should be unique, to get a **hash value **(bucket index)
  * the **hash value** is determined by whatever hash function you write or are given
* the hash value tells you the bucket where the item will be stored at&#x20;
  * **bucket **is a hash table array element

{% hint style="info" %}
Searching for an item in the hash table only takes **O(1)**! Only one item is chect
{% endhint %}

## Example of Hash Function

For example,&#x20;

```
hashFunction( int key )
{
    return int % 12;
}
```

This hash function maps keys to bucket indeces 0 to 11. If key of an item is 27, then the hash value is 3. Then this item will live in bucket number 3.

But then what happens when there is another item with key 15. There is now two items that should live in bucket #3. UH OH collision!

## Collision

A **collision **happens when an item inserted into a hash table maps to the same bucket as an existing one. We can get around collisions, these solutions are called **collision resolution techniques.**&#x20;

* **Chaining**: each bucket has a list of items&#x20;
* **Open addressing**: looks for an empty bucket elsewhere in the table.&#x20;

## Hash Table Resizing&#x20;

Maybe during chaining, the lists are getting too big, then you need to resize your hash table while preserving all existing items! Usually, we resize to the next prime number >= N\*2.

{% hint style="info" %}
Resize operation is **O(N)**
{% endhint %}

```cpp
HashResize( hashTable, currentSize )
{
    newSize = nextPrime( currentSize * 2 )
    
    newArray = Allocate new array of size newSize
    Set all entries in newArray to EmptySinceStart
    
    bucket = 0
    while( bucket < currentSize )
    {
        if (hashTable[bucket] is not Empty)
        {
            key = hashTable[bucket]
            HashInsert(newArray, key)
        }
        bucekt++
    }
    return newArray
}

// YOU ALSO NEED TO UPDATE YOUR HASH FUNCTION!
Hash(key, tableSize)
{
    return key % tableSize
}
```

{% hint style="danger" %}
Remember to update your hash function when resizing!!!&#x20;
{% endhint %}

How do we determine when our hash tables are getting too full? - you decide when is it too big! Multiple criterias:

* **Load factor: **number of items in the hash table divided by the number of buckets&#x20;
* Open-addressing: number of collisions during insertion
  * load factor should be < 1.0
* Chaining: size of bucket's linked lists
  * load factor can be > 1.0

## Good Hash Functions

{% hint style="info" %}
A GOOD HASH FUNCTION MINIMIZES COLLISIONS!
{% endhint %}

Less collisiong -> faster hashTable

Criteria for a perfect (or good) hash function:

* maps items to buckets with no collisions
* uniformly distributes items into bucekts
  * short bucket lists in chaining&#x20;
  * avoid hashing mulitple items in consecutive bucekts
* **ideal O(1)** inserts, searches, removes
  * worse-case may require **O(N)**

### Modulo

// see above

### Mid-square hash

* squares key
* extracts R digits from the middle
* return the remainder of the middle digits divided by hash table size N

For example

key = 453

* 453^2 = 205209
* returns 52
* for N buckets, R must be greater or equal than ceiling of log base 10 N to index all buckets

#### implementation using base 2

base 2 because base 2 is superior. binary implementation is faster, you dont need to convert square of num into string like base 10 to extract the middle integers. For base 2 you use shift and bitwise AND operations!! (eecs31 and ics6b all over again)

```
HashMidSquare(int key)
{
    squaredKey = key * key
    
    lowBitsToRemove = (32 - R) / 2
    extractedBits = squaredKey >> lowBitsToRemove
    extractedBits = extractedBits & (0xFFFFFFFF >> (32 - R)) 

    return extractedBits % N
}
```

### Multiplicative string hash function

Repeatedly multiplies the hash value and adds the ASCII value of each character in the string. Starts with a large initial value. For each character, the hash function multiplies the current hash value by a multiplier and adds the character's value. Returns the remainder of the sum divided by the hash table size `N`

Daniel J. Bernstein used initial value of 5381 and multiplier of 33. Hashes well short english strings

```
HashMultiplicative(string key)
{
    stringHash = InitialValue
    
    for (each character strChar in key) 
    {
        stringHash = (stringHash * HashMultiplier) + strChar
    }
    
    return stringHash % N
}
```

## Direct Hashing

use the item's key as the bucket index. This kind of hash table is called ** direct access table.**&#x20;

```
HashSearch(hashTable, key)
{
    if (hashTable[key] is not Empty)
        return hashTable[key]
    else
        return null
}

HashInsert(hashTable, item) {
   hashTable[item⇢key] = item 
}

HashRemove(hashTable, item) {
   hashTable[item⇢key] = Empty
}
```

with direct access there is no collisions because each key is unique (sounds like a dictionary to me aha). But there are limitations:

* all keys must be non-negative integers
* hash table size equals the largest key value plus 1 - which can be hugeeee
