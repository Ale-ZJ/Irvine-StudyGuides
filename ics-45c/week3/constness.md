---
description: week 4
---

# Constness

In C++, there are tools to keep things constant. Once the value of a constant variable has been initialize, it's value can never be changed again.&#x20;

```cpp
const int x = 3;
const std::string nmae = "Boo";
const int y = x;
int z = x;
const int w = z;
```

{% hint style="info" %}
Note that `const` is part of the variable's type
{% endhint %}

{% hint style="danger" %}
Be careful with pointers and references. Both structures potentially allow us to change the contents of a constant variable indirectly.
{% endhint %}

### References

The key thing to understand is that a reference to a constant doesn't necessarily guarantee that the value it refers to will never change; it simply guarantees that the reference itself will never be used to change the value.

```cpp
const int x = 3;
int& r = x;       // illegal, because you can change x, by changing r
const int& r = x; // legal, r is a reference to an integer constant

int x = 3;
const int& y = x; // the value is constant 
                  // (and technically references are const by definition)

// use reference asa parameter type when objects are extensiv to copy
void printInReverse(const std::string& s)
{
    for (int i = s.length() - 1; i >= 0; --i)
    {
        std::cout << s[i];
    }
}
```

### Pointers

```cpp
const int x = 3;
int* p = &x; // illegal assigning a new value to *p would change x
const int* q; // q is non-const, *q is const -- west const
int const* r; // r is non-const, *r is const -- east const
const int* const t; // t is const, *t is const
```
