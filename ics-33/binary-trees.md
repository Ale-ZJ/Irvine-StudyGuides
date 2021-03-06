# Binary Trees

Both structure and order property

* structure 
  * every parent has 0,1,2 children 
* order property
  * values in the left subtree are less than that node 
  * values in the right subtree are greater than that node 
  * if there are duplicates, then you choose which side, but be consistent 
* **size:** number of total nodes 
* **height:** rows of nodes -1 

```python
class TN:
    def __init__(self,value,left=None,right=None):
        self.value = value
        self.left  = left
        self.right = right

def list_to_tree(alist):
    if alist == None:
        return None
    else:
        return TN(alist[0],list_to_tree(alist[1]),list_to_tree(alist[2])) 
    
def str_tree(atree,indent_char ='.',indent_delta=2):
    def str_tree_1(indent,atree):
        if atree == None:
            return ''
        else:
            answer = ''
            answer += str_tree_1(indent+indent_delta,atree.right)
            answer += indent*indent_char+str(atree.value)+'\n'
            answer += str_tree_1(indent+indent_delta,atree.left)
            return answer
    return str_tree_1(0,atree) 
```

![](../.gitbook/assets/image%20%282%29.png)



