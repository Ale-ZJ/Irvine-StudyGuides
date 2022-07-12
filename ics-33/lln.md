# Linked Lists



```python
class LN:
    created = 0
    def __init__(self,value,next=None):
        self.value = value
        self.next  = next
        LN.created  += 1

def list_to_ll(l):
    if l == []:
        return None
    front = rear = LN(l[0])
    for v in l[1:]:
        rear.next = LN(v)
        rear = rear.next
    return front

def str_ll(ll):
    answer = ''
    while ll != None:
        answer += str(ll.value)+'->'
        ll = ll.next
    return answer + 'None'
```

![](<../.gitbook/assets/image (1).png>)

writing recusive functions\


* base case is then LN = None
  * no nodes
* LN.next refers to another LN

```python
def sum_ll_r(ll):
    if ll == None: # Could also test: type(ll) is NoneType
        return 0
    else:
        return ll.value + sum_ll_r(ll.next)
```

{% hint style="info" %}
In tuples/lists, using a slice to skip the first value in a recursive call is INEFFICIENT in both time and space. It must COPY the entire tuple/list. But using ll.next to skip the first value in a recursive call is EFFICIENT in both time and space.
{% endhint %}

