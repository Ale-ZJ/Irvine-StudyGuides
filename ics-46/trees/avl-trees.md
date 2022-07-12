---
description: Balanced Trees
---

# AVL Trees

## AVL Tree

An **AVL tree** is a BST with a height balance on both sides of a node. We determine this balance property based on a **balance factor** that you get by substracting the height of the left child's height minus the height of the right child.&#x20;

* If the child is a `null` you the height of that child is `-1.`
* Leaf children are height `0`

```
balanceFactor = node->left->height - node->right->height
```

![](<../../.gitbook/assets/image (21).png>)

{% hint style="success" %}
A BST is balanced when the balance factor is `1`, `0`, `-1`
{% endhint %}

### Helper Algorithms for AVL Trees

#### Update Height

takes the maximum of the child subtree heights and adding 1.

```
AVLTreeUpdateHeight(node)
{
    leftHeight = -1 // assumes nullptr
    if(node->left != null)
        leftHeight = node->left->height
    
    rightHeight = -1 // assumes nullptr
    if(node->right != null)
        rightHeight = node->right->height
        
    node->height = max(leftHeight, rightHeight) + 1
}
```

#### Set Child

sets a node as the parent's left or right child, updates the child's parent pointer, and updates the parent node's height.

```
AVLTreeSetChild(parent, whichChild, child)
{
    if(whichChild != "left" && whichChild != "right")
        return false
        
    if(whichChild == "left")
        parent->left = child
    else
        parent->right = child
    
    if(child != null)
        child->parent = parent

    AVLTreeUpdateHeight(parent)
    
    return True
}
```

#### Replace Child

replaces one of a node's existing child pointers with a new value, using `setChild` to perform the replacement

```
AVLTreeReplaceChild(parent, currentChild, newChild)
{
    if(parent->left == currentChild)
        return AVLTreeSetChild(parent, "left", newChild)
    else if(parent->right == currentChild)
        return AVLTreeSetChild(parent, "right", newChild)
        
    return false
}
```

#### Get Balance

computes a node's balance factor by substracting the right subtree height from the left subtree height

```
AVLTreeGetBalance(node)
{
    leftHeight = -1 // assumes nullptr
    if(node->left != null)
        leftHeight = node->left->height
    
    rightHeight = -1 // assumes nullptr
    if(node->right != null)
        rightHeight = node->right->height
        
    return leftHeight - rightHeight
}
```

{% hint style="warning" %}
Then what happens if a `insert` or a `delete` causes an unbalance? aha good luck you need to rotate the tree :') (or you can trash out the ridiculously complex rotation algorithm and follow the one from professor Shindler's lecture)
{% endhint %}

### Rotation Algorithm

When an AVL tree node has a balance factor of 2 or -2, which only occurs after an insertion or removal, the node must be rebalanced via rotations.

This is a **right rotation algorithm**

```
AVLTreeRotateRight(tree, node) {
   leftRightChild = node⇢left⇢right
   if (node⇢parent != null)
      AVLTreeReplaceChild(node⇢parent, node, node⇢left)
   else { // node is root
      tree⇢root = node⇢left
      tree⇢root⇢parent = null
   }
   AVLTreeSetChild(node⇢left, "right", node)
   AVLTreeSetChild(node, "left", leftRightChild)
}
```

#### Tree Rebalance

updates the height value at a node, computes the balance factor and rotates if the balance factor is 2 or -2

```
AVLTreeRebalance(tree, node) {
   AVLTreeUpdateHeight(node)        
   if (AVLTreeGetBalance(node) == -2) {
      if (AVLTreeGetBalance(node⇢right) == 1) {
         // Double rotation case.
         AVLTreeRotateRight(tree, node⇢right)
      }
      return AVLTreeRotateLeft(tree, node)
   }
   else if (AVLTreeGetBalance(node) == 2) {
      if (AVLTreeGetBalance(node⇢left) == -1) {
         // Double rotation case.
         AVLTreeRotateLeft(tree, node⇢left)
      }
      return AVLTreeRotateRight(tree, node)
   }        
   return node
}
```

## Insert (Zybook)

Insert sounds very straightforward, you will always insert a leaf node into the tree. But inserting can cause an unbalance and you might need to rotate more than one time ot fix the unbalance.&#x20;

AVL insertion starts with the normal BST insertion algorithm but after inserting anode you must rebalance everything.

{% hint style="info" %}
Insert traverses from the root to a leaf node to find the insertion point, then traverses back up to the root to rebalance. One node is visited per level, and at most 2 rotations are needed for a single node. Each rotation is **O(1)** operation .&#x20;

Therefore:

* the runtime complexity of insertion is **O(logN)**
* the space complexity is **(O1)** since it is a fixed number of temporary pointers
{% endhint %}

```
AVLTreeInsert(tree, node) {
   if (tree⇢root == null) {
      tree⇢root = node
      node⇢parent = null
      return
   }

   cur = tree⇢root
   while (cur != null) {
      if (node⇢key < cur⇢key) {
         if (cur⇢left == null) {
            cur⇢left = node
            node⇢parent = cur
            cur = null
         }
         else {
            cur = cur⇢left
         }
      }
      else {
         if (cur⇢right == null) {
            cur⇢right = node
            node⇢parent = cur
            cur = null
         }
         else
            cur = cur⇢right
      }
   }

   node = node⇢parent
   while (node != null) {
      AVLTreeRebalance(tree, node)
      node = node⇢parent
   }
}
```

## Insert (Lecture)

Just as the Zybook approach, we start by normally inserting a node to a BST. Here is the new procedure:&#x20;

* walking up to the root. check if each node is balanced
  * if not balanced, then:&#x20;
    * `z = first unbalanced node`
    * `y = child of z with greater height`
    * `x = child of y with greater height`
      * if there is a tie then choose x to be ancestor of inserted node
    * then find median between `x`, `y`, `z`
    * new median becomes the middle node and then you arrange y and z

## Removal&#x20;

Good luck removing an internal node :')

{% hint style="info" %}
Same thing as with insertion. Time complexity of removal is **O(logN)** and space complexity is **O(1).**
{% endhint %}

#### Remove Key

```
AVLTreeRemoveKey(tree, key) {
   node = BSTSearch(tree, key)
   return AVLTreeRemoveNode(tree, node)
}
```

#### Remove&#x20;

```
AVLTreeRemoveNode(tree, node) {
   if (node == null) {
      return false
   }

   // Parent needed for rebalancing
   parent = node⇢parent
        
   // Case 1: Internal node with 2 children
   if (node⇢left != null && node⇢right != null) {
      // Find successor
      succNode = node⇢right
      while (succNode⇢left != null) {
         succNode = succNode⇢left
      }

      // Copy the key from the successor node
      node⇢key = succNode⇢key
            
      // Recursively remove successor
      AVLTreeRemoveNode(tree, succNode)
            
      // Nothing left to do since the recursive call will have rebalanced
      return true
   }

   // Case 2: Root node (with 1 or 0 children)
   else if (node == tree⇢root) {
      if (node⇢left != null) {
         tree⇢root = node⇢left
      }
      else {
         tree⇢root = node⇢right
      }
      if (tree⇢root != null) {
         tree⇢root⇢parent = null
      }
      return true
   }

   // Case 3: Internal with left child only
   else if (node⇢left != null) {
      AVLTreeReplaceChild(parent, node, node⇢left)
   }
   // Case 4: Internal with right child only OR leaf
   else {
      AVLTreeReplaceChild(parent, node, node⇢right)
   }

   // node is gone. Anything that was below node that has persisted is already correctly
   // balanced, but ancestors of node may need rebalancing.
   node = parent
   while (node != null) {
      AVLTreeRebalance(tree, node)            
      node = node⇢parent
   }
   return true
}
```

