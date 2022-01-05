---
description: >-
  week 0: "A Quick note on Simple and Efficient File Reading" by proffesor
  Pattis
---

# File Reading

## Reading Files

:x: To write space-efficient code avoid using `.read()` (returns a giant string of lines) or `.readlines()` (returns a list of lines).&#x20;

```python
open_file = open(file_name: str)

for line in open(file_name).read().split('\n'):
    process(line)

for line in open(file_name).readlines():
    line = line.rstrip('\n')
    process(line)
```

:white\_check\_mark: Instead, directly iterate open file objects:

```python
for line in open(file_name: str):
    line = line.rstrip('\n')
    process(line)
```

:white\_check\_mark: Or use a list comprehension to create a list of file lines

```python
line_list = [line.rstrip('\n') for line in open(file_name)]
```

:white\_check\_mark: We can also use `open` as a context manager in a `with` statement.\
\- handles file exceptions \
\- closes the file  when context manager finishes

```python
with open(file_name) as open_file:
    for line in open_file:
        process(line.rstrip('\n'))
```

## Parsing Files

use `parse_lines()`from course's goody module:

```python
def parse_lines(open_file, sep, conversions): 
    for line in open_file: 
        yield [conv(item) for conv,item in \
        zip(conversions,line.rstrip('\n').split(sep))]
```

example:

```python
'''
the file contains:
Bob Smith,75,80
Mary Jones,85,90
'''

for fields in parse_lines( open(file_name), ',' , (str, int, int) ):
    print(fields[0], (fields[1] + fields[2])/2)

#each field will look like
['Bob Smith', 75, 80] and ['Mary Jones', 85, 90]

#BUT DONT USE THE ONE ABOVE PLS!
#IT IS SIMPLER TO UNPACK YOUR VARIABLES!
    #you can unpack values in a tuple into the same amount of variables

for name, test1, test2 in parse_lines(open(file_name), ',', (str,int,int)):
    print(name, (test1 + test2)/2)
```

## Pickling

* **Text files:** composed of ASCII characters - readable
* **Binary files:** 0 and 1's - need to decode

To write binary files we use `pickle.dump(object, open-file)`

```python
import pickle 
adict = dict(a=1,b=2,c=3) # create a dict 

with open('pickletest.dat', 'wb') as storage: # File mode is w(rite)b(inary) 
    pickle.dump(adict,storage) # store dict in a binary file
```

To read binary files we use `pickle.load(open-file)`

```python
with open('pickletest.dat', 'rb') as storage: # File mode is r(ead)b(inary) 
    adict = pickle.load(storage) # restore dict from file 
    
print(adict) # Print orginal/pickled dict 
#which prints: {'a': 1, 'b': 2, 'c': 3}
```
