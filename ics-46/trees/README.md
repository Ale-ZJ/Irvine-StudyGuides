---
description: programming trees not real trees. programming trees are upside down.
---

# Trees

## Binary Tree

each node has two children: left and right.

some vocab about binary trees:&#x20;

* **leaf: **a tree node with no children
* **internal node: **at least one child
* **parent: **obviously a node that has a child
* **ancestors: **node's parents, parent's parent, etc until root
* **root: **node without a parent that you can find at the top of the tree

![](<../../.gitbook/assets/image (20).png>)

more vocab:&#x20;

* **edge: **link between node to child node
* **depth: **number of edges on the path from the root to the node.
  * root node has depth 0
  * **level: **nodes on the same depth
* **height: **largest depth of any node

![](<../../.gitbook/assets/image (18).png>)

### Types of Binary Trees

* **full: **every node has 0 or 2 children
* **complete: **all levels contain all possible nodes and last level is to the far left as possible
* **perfect: **internal nodes have 2 children and all lead nodes are at the same level

![](<../../.gitbook/assets/image (19).png>)

## Binary Seach Trees

A BST is a binary tree where the left side of a root is smaller than the root and the right side bigger than the root. Allows for fast searching.

#### Search

```
BSTSearch(tree, key) {
   cur = tree⇢root
   while (cur is not null) {
      if (key == cur⇢key) {
         return cur // Found
      }
      else if (key < cur⇢key) {
         cur = cur⇢left
      }
      else {
         cur = cur⇢right
      }
   }
   return null // Not found
}
```

#### Insert

```
BSTInsert(tree, node) {
   if (tree⇢root is null) {
      tree⇢root = node
   }
   else {
      currentNode = tree⇢root
      while (currentNode is not null) {
         if (node⇢key < currentNode⇢key) {
            if (currentNode⇢left is null) {
               currentNode⇢left = node
               currentNode = null
            }
            else {
               currentNode = currentNode⇢left
            }
         }
         else {
            if (currentNode⇢right is null) {
               currentNode⇢right = node
               currentNode = null
            }
            else {
               currentNode = currentNode⇢right
            }
         }
      }
   }
}
```

#### Remove

```
BSTRemove(tree, key) {
   par = null
   cur = tree⇢root
   while (cur is not null) { // Search for node
      if (cur⇢key == key) { // Node found 
         if (cur⇢left is null && cur⇢right is null) { // Remove leaf
            if (par is null) // Node is root
               tree⇢root = null
            else if (par⇢left == cur) 
               par⇢left = null
            else
               par⇢right = null
         }
         else if (cur⇢right is null) {                // Remove node with only left child
            if (par is null) // Node is root
               tree⇢root = cur⇢left
            else if (par⇢left == cur) 
               par⇢left = cur⇢left
            else
               par⇢right = cur⇢left
         }
         else if (cur⇢left is null) {                // Remove node with only right child
            if (par is null) // Node is root
               tree⇢root = cur⇢right
            else if (par⇢left == cur) 
               par⇢left = cur⇢right
            else
               par⇢right = cur⇢right
         }
         else {                                      // Remove node with two children
            // Find successor (leftmost child of right subtree)
            suc = cur⇢right
            while (suc⇢left is not null)
               suc = suc⇢left
            successorData = Create copy of suc's data
            BSTRemove(tree, suc⇢key)     // Remove successor
            Assign cur's data with successorData
         }
         return // Node found and removed
      }
      else if (cur⇢key < key) { // Search right
         par = cur
         cur = cur⇢right
      }
      else {                     // Search left
         par = cur
         cur = cur⇢left
      }
   }
   return // Node not found
}
```

## Traversals

### In-Order&#x20;

```
inOrder(Node* n) 
{
    inOrder(n->left)
    std::cout << n -> key
    inOrder(n->right)
}
```

### Pre-Order&#x20;

If you insert into a BST with this sequence, it will giv you an exact BST tree.

```
preOrder(Node* n)
{
    std::cout << n->key
    preOrder(n->left)
    preOrder(n->right)
}
```

### Post-Order

Applications: syntax diagrams, compilers

```
postOrder(node* n)
{
    postOrder(n->left)
    postOrder(n->right)
    std::cout << n->key
}
```
