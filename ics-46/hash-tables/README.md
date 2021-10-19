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

##
