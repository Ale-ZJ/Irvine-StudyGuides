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

## Hash Function with Modulo Operator

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

## Collision Resolution Techniques

How to deal when two or more items are hashed to the same bucket?

### Chaining

Each bucket has a list (we did a linked list for an ics45c project!). This way, one bucket can store multiple items in a list.&#x20;

```cpp
// Inserts uses the item's key to determine the bucket where it belongs
// and inserts a new node containing the item as data at the end of 
// the bucket list
HashInsert(hashTable, item)
{
    // if the item is not in the bucket list already
    if ( HashSearch(hashTable, item->key) == null)
    {
        bucketList = hashTable[ Hash(item->key) ]
        node = Allocate new linked list node
        node->next = null
        node->data = item
        ListAppend(bucketList, node)
    }
}

// Searching determines the bucket where the key is and then looks for the key
// in the list and returns the data that goes with given key
// Basically it tells you whether the key is in the hashTable already
HashSearch(hashTable, key)
{
    bucketList = hashTable[ Hash(key) ]
    itemNode = ListSearch( bucketList, key )
    if(itemNode is not null)
        return itemNode -> data
    else
        return null
}

// Removes an item fron the hashTable only if it is in the table 
hashRemove(hashTable, item)
{
    bucketList = hashTable[ Hash(item->key) ]
    itemNode = ListSearch( bucketList, item->key )
    
    // we only remove the item if IT IS in the list to begin with
    if ( itemNode is not null )
    {
        ListRemove( bucketList, itemNode )
    }
}
```

### Linear Probing

* Handles collisions by storing a collision item at the next subsequent empty bucket.
  * it can loop around
    * ex) if the hashed bucket (where collision happens) is the last one, then you start looking for an empty bucket from the very beginning&#x20;
* when searching for an item it begins at the hashed bucket and continues down the list until an empty bucket is found

#### Empty Bucket types

Linear probing needs to distinguish between two types of empty buckets:

* **empty-since-start**
  * bucket empty since hash table was created
* **empty-after-removal**
  * bucket is empty because the last item was removed

{% hint style="warning" %}
searching only stops for empty-since-start buckets!!
{% endhint %}

#### Insert

Uses item's key to determine initial bucket, checks each bucket and inserts the item in the next empty bucket (the empty kind doesnt matter)

```cpp
hashInsert(hashTable, item)
{
    // Hash function determines initial bucket
    bucket = Hash(item.key)
    bucketsProbed = 0
    N = hashTable's size
    
    while (bucketsProbed < N)
    {
        //Insert item in next empty bucket
        if (hashTable(bucket) is Empty)
        {
            hashTable(bucket) = item
            return true // gets out of the loop
        }
        
        // Increment bucket index
        bucket = (bucket + 1) % N
        
        // Increment number of buckets probed
        ++bucketsProbed
    }
    
    return false // bucketsProbed == N, then the list is full
}
```

#### Remove

uses item's key to determine initial bucket. Probes each bucket until we either:

* find a matching item to remove
  * watch out to mark empty-after-removal
* an empty-since-start bucket
  * because subsequent buckets do not contain an item that was inserted as linear probing&#x20;
    * algorithm will keep probing if it encounters an empty-after-removal
* all buckets have been probed

```cpp
HashRemove(hashTable, key)
{
    // Hash function determines the initial bucket
    bucket = Hash(key)
    bucketsProbed = 0
    
    while ( (hashTable[bucekt] is not EmptySinceStart) and (bucketsProbbed < N) 
    {
        if ( (hashTable[bucket] is not Empty and (hashTable[bucket]->key == key )
        {
            hashTable[bucket] = EmptyAfterRemoval //remove and mark
            return
        }    
        
        // Increment bucket index
        bucket = (bucket + 1) % N

        // Increment number of buckets probed
        ++bucketsProbed
    }
}
```

#### Search

item's key to determine initial buckets. probes until:

* matching item found (returns item)
* empty-since-start (returns null)
* al buckets probed (returns null)

```cpp
HashSearch(hashTable, key) 
{
   // Hash function determines initial bucket
   bucket = Hash(key)
   bucketsProbed = 0

   while ((hashTable[bucket] is not EmptySinceStart) and (bucketsProbed < N)) 
   {
      if ((hashTable[bucket] is not Empty) and (hashTable[bucket]⇢key == key)) 
      {
         return hashTable[bucket]
      }

      // Increment bucket index
      bucket = (bucket + 1) % N

      // Increment number of buckets probed
      ++bucketsProbed
   }

   return null  // Item not found
}

```
