# Collision Resolution Techniques

How to deal when two or more items are hashed to the same bucket?

## Chaining

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

## Linear Probing

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

## Quadratic probing

Handles collisions by starting at the key's mapped bucket and quadratically searches a subsequent bucket until an empty bucket is found.

#### How to determine the item's index in the hash table?

* With `H` = item's mapped bucket
* `c1` and `c2` are programmer-defined constants for quadratic probing

```
(H + c1 * i + c2 * i^2)mod(tablesize)
```

Inserting a key uses the formula, starting with `i = 0` to search the hash table until an empty bucket is found. Each time an empty bucket is not found, i is incremented by 1.

![](<../../.gitbook/assets/image (13).png>)

#### Insert

```
HashInsert(hashTable, item)
{
    i = 0
    bucketsProbed = 0
    
    // Hash function idetermines initial bucket
    bucket = Hash(item->key) % N
    
    while (bucketsProbed < N)
    {
        // Insert item in next empty bucket
        if (hashTable[bucket] is Empty)
        {
            hashTable[bucket] = item
            return true
        }
        
        // Increment i and recompute bucket index
        // c1 and c2 are constants
        i = i + 1
        bucket = (Hash(item->key) + c1 + i + c2 + i * i) % N
        
        // Increment number of buckets probed
        bucketsProbed = bucketsProbbed + 1
    }
    
    return false
}
```

**Remove**

Probes until the target key is found or empty-since-start bucket is found. After removing the target key, the algorithm marks the bucket as empty-after-removal

```cpp
HashRemove(hashTable, key)
{
    i = 0
    bucketsProbed = 0
    
    // Hash function determines initial bucket
    bucket = Hash(key) % N
    
    while( (hashTable[bucket] is not EmptySinceStart) and (bucketsProbed < N))
    {
        if( (hashTable[buckets] is Occupied) and (hashTable[bucket]->key == key))
        {
            hashTable[bucket] = EmptyAfterRemoval
            return true
        }
        
        // Increment i and recompute bucket index
        // c1 and c2 are constants
        i++
        bucket = (Hash(key) + c1 * i + c2 * i * i) % N
        
        // Increment number of buckets probed
        bucketsProbed++
    }
    
    return false //key not found
}
```

#### Search

```cpp
HashSearch(hashTable, key)
{
    i = 0
    bucketsProbed = 0 
    
    // Hash function determines initial bucket
    bucket = Hash(key) % N
    
    while( (hashTable[bucket] is not EmptySinceStart) and (bucketsProbed < N))
    {
        if( (hashTable[bucket] is Occupied) and (hashTable[bucket]->key = key))
        {
            return hashTable[bucket]
        }
        
        // Increment i and recompute bucket index
        // c1 and c2 are programmer-defined constants for quadratic probing
        i++
        bucket = (Hash(key) + c1 * i + c2 * i * i) % N

        // Increment number of buckets probed
        bucketsProbed = bucketsProbed + 1
    }
    
    return false //key not found
}
```
