# Sorting Algorithms

## Selection Sort

Two indexes `i` and `j`. For each `i`, traverse all the way right and find the smallest element, keep track of that `j` index and swap with the element at `i`.

```
for (i <- 1) to (n-1) do 
    min <- i
    for (j <- i + 1) to (n) do 
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

Outer loop executes **N - 1** times and inner loop executes an average of **N/2**. The total number of comparisons is proportional to **(N-1)(N/2)**.

Algorithm never ends earlier.
{% endhint %}

## Bubble Sort

Basically, for all elements in array, traverse all the way right and swap adjacent elements if `j+1 < j`. Optimization notes: you don't need to go all the way right, Bubble sort will always put the largest element to the last index adter each outer loop traversal, therefore the next outer loop traversal can stop at the element 2nd to last.&#x20;

```
for (i <- 1) to (n - 1) do 
    for (j <- 1) to (n - i) do
        if A[j+1] < A[j] then 
            swap A[j] and A[j+1]
        end if
    end for
end for
```

#### Complexity

{% hint style="info" %}
Bubble Sort runtime is **O(N^2)**

Can end earlier: when an entire outer loop iteration has no swap.
{% endhint %}

## Insertion Sort

Your array has an imaginary partition line. The line goes after the first element. **Any array of size 1 is already sorted.** Move the partition line right by one element. Now the newly displaced element in the left side of the partition line might have ruined the sorted property of the left side, so swap until the newly displaced element reaches the correct spot. "Array" on the left of the partition line will always be sorted.&#x20;

```
for (j <- 2) to (n) do
    key <- A[j]
    i <- j - 1
    while i > 0 and A[i] > key do
    
```
