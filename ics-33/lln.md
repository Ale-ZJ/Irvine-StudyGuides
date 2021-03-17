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

