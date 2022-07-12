# Binary Trees

Both structure and order property

* structure&#x20;
  * every parent has 0,1,2 children&#x20;
* order property
  * values in the left subtree are less than that node&#x20;
  * values in the right subtree are greater than that node&#x20;
  * if there are duplicates, then you choose which side, but be consistent&#x20;
* **size:** number of total nodes&#x20;
* **height:** rows of nodes -1&#x20;

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

![](<../.gitbook/assets/image (2).png>)

