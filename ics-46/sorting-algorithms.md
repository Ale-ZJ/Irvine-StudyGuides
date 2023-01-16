# Sorting Algorithms

## Selection Sort

Two indexes `i` and `j`. For each `i`, traverse all the way right and find the smallest element, keep track of the index of the smallest index element in `j` and swap with the element at `i`.

```
for (i <- 1) to (n-1) do 
    min <- i
    for (j <- i+1) to (n) do 
        if A[j] < A[min] then 
            min <- j
        end if
    end for
    swap A[i] and A[min]
end for
```

**Complexity**

{% hint style="info" %}
Selection Sort runtime is **O(N^2)**.

Outer loop executes **** N - 1 times and the inner loop executes an average of N/2. The total number of comparisons is proportional to (N-1)(N/2).

The algorithm never ends earlier.

There are minimum swaps. This is beneficial if you need to order an array that has heavy elements.&#x20;
{% endhint %}

## Bubble Sort

Basically, for all elements in an array, traverse all the way right and swap adjacent elements if `j+1 < j`. Optimization notes: in every sweep, you don't need to go all the way right, Bubble sort will always put the largest element to the last index after each outer loop traversal (after each sweep), therefore the next outer loop traversal can stop at the element 2nd to last.&#x20;

```
for (i <- 1) to (-1) do 
    for (j <- 1) to (n-i) do
        if A[j+1] < A[j] then 
            swap A[j] and A[j+1]
        end if
    end for
end for
```

#### Complexity

{% hint style="info" %}
Bubble Sort runtime is **O(N^2)**

Can end earlier: when an entire outer loop iteration has no swap, the array is already sorted.
{% endhint %}

## Insertion Sort

Your array has an imaginary partition line. The line goes after the first element. **Any array of size 1 is already sorted.** Move the partition line right by one element. Now the newly displaced element on the left side of the partition line might have ruined the sorted property of the left side, so swap until the newly displaced element reaches the correct spot. This way, the "array" on the left of the partition line will always be sorted.&#x20;

```
for (j <- 2) to (n) do
    key <- A[j]
    i <- j-1
    while i > 0 and A[i] > key do
        A[i+1] <- A[i]
        i = i-1
    end while
    A[i+1] <- key
end for
```

#### Complexity

{% hint style="info" %}
Insertion Sort has a best case of **O(N + I)**, which is usually the average&#x20;

* This means that Insertion Sort takes linear time + the number of additional insertions to make the array sorted&#x20;
* If the list is nearly sorted, then it is fast

The worst-case is of **O(N^2)**
{% endhint %}

### Heap Sort

Take the array you have and transform it into a heap. Remember the definition of a min-heap. At any given node, it's children's value are greater or equal than the node's. So once we have a heap we can use the library `extractMin()` method. There are two ways of doing this, the later one slightly faster than the previous one.

* Method 1. Make a heap with the information from the array (takes _O(n log n)_) and then continuously use `extractMin()` to get the increasing order (takes _O(n log n)_)
* Method 2. This is an in-place implementation. We "heapify" the array (takes _(O(n)_) and then use `extractMin()` (takes _O(n log n)_)

#### Complexity

{% hint style="info" %}
Heap Sort takes **O(N log N)**

There is no worse case
{% endhint %}

### Merge Sort

This is not an in-place algorithm (meaning that we need to create additional data structures) so this is memory expensive. This is a recursive algorithm. A vector of size 1, is already sorted!

```
cut vector in half
MergeSort(left) 
MergeSort(right) 
merge left and right side
```

**Complexity**

{% hint style="info" %}
Merge Sort takes **O(N log N)** because each layer takes N work and there is log N layers (you keep dividing in half)

This algorithm requires **O(N)** extra space
{% endhint %}

### Quick Sort

This is an in-place algorithm. Similar to Merge Sort, we will divide the array, but at a _pivot_ point. A good pivot divides the vector into halves, yes, you get to decide how to implement the pivot. The steps in the partition process are: choose a pivot, place the pivot in the right spot in the array, pivot the rest of the array to where they belong.

```
Partition
    - choose pivot
    - find pivot spot ( and swap
    - make sure all values to the left of the pivot are < pivot, 
      same with the right side
QuickSort(left)
QuickSort(right)
```

**Complexity**

{% hint style="info" %}
QuickSort has different time complexities depending on the case,

* best-case: **O(N log N)**
* average: **O(N log N)**
* worst: **O(N^2)**

There is recursion overhead, takes memory in the stack
{% endhint %}

**------------**

**Note:** All comparison-based algorithms sort omega(N log N). This is no algorithm will be better than N log N
