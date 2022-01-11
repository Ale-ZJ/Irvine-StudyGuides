---
description: 01/05/2022 lecture, reference this for hw0 0and hw1
---

# Java

* if you don't add "public" then the default is "package scope"
* all objects in Java are derived from the base class `Object`
* we use Camel case
  * method name starts with lower case, classes start with upper case
* every object has an invisible parameter called `this`
  * same for C++
  * for Python it is `self`&#x20;
* elements is a iterator
* in Java you convert to a string to print
  * C++ inserters and extractor are more efficient
* to print you do System.out.print( "string here")
* if you dont initialize a variable it will de default initialized to `0` or `null`
*

```
class LinkedList{ // you can change the name to Picture for hw1
    void add( Object x ) {}
    int length() {}
    boolean isEmpty() {}
    java.util.Enumeration elements() {}
    public String toString() {}
}
```



```
class ListNode{
    Object info;
    ListNode next;
    
    // this is a constructor
    ListNode( Object info, ListNode next) {
        this.info;
        this.next
    }
}
```

#### Implementation

Java uses references, but they are really a pointer

```
public class LinkedList{
    ListNode head;

    //constructor
    public LinkedList(){
        head = null;
    }
}
```



```
public class LinkedList{

    // adds a new node at the beginning of the list
    public void add( Object x ) {
        head = new ListNode(x, head);
    }
    
    public int length(){ 
        // keeping a member variable is more error prone!
        // you also don't 
        
        int len = 0; //accumulator
        for(ListNode p = headl; p != null; p = p.next )
        {
            len++
        }
        return len;
    }
    
    public boolean isEmpty()
    {
        return head == null;
    }
    
    public java.util.Enumeration elements()
        implements Enumeration
    {
        ListNode head;
        
        public boolean hasMoreElements()
        {
            return head != null;
        }
        
        public Object nextElement()
        {
            Object ret = head.info;
            head = head.next;
            return ret;
        }
        
    }
    
    
}
```



### Inheritance

the name of this file HAS TO BE `MyList.java`

```
public class MyList
    extends LinkedList
{
    //main has to be a static public method in a public class
    public static void main(String args[]) 
    {
        MyList L = new MyList("hello");
    }
}
```
