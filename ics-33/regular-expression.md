---
description: 'week 2: ''Regular Expressions'''
---

# Regular Expression

A regular expression specifies a sequence for which a string must match. We will always use _greedy_ matching \(matching the most letters\).

Generally, **characters match themselves** except this:

| Symbol | what they mean in RegEx |
| :--- | :--- |
| `.` | any char except `\n` |
| `[]` | matches one char inside the brackets e.g. `[aeiou]` |
| `-` | range e.g. `[0-9]` |
| `[^]` | matches any character NOT inside the brackets `[^aeiou]` |
| `^` | beginning of line/sequence  |
| `$` | end of the line  |

Let R be a regex sequence, there are more rules:

| More | regex meaning |
| :--- | :--- |
| `RaRb` | Rb follows Ra right after |
| `Ra|Rb` | either Ra or Rb |
| `R?` | match R 1 or 0 time |
| `R*` | matches R 0 or more time |
| `R+` | matches R 1 or more time |
| `R{m}` | matches R exactly m times |
| `R{m,n}` | matches R at least m and at most n times |

**Parenthesis** are used for:

1. grouping 
2. remember text matching subpattern names i.e. capturing group

| group | what it means |
| :--- | :--- |
| `(R)` | matches R and enumerates a capturing group. Starts on the outermost parenthesis |
| `(?:R)` | matches R but the group there is NO group number |
| `(?P<name> R)` | matches R and remembers the name of the pattern. e.g. `(?P<id>[A-Za-z0-9]+)` |

Escape characters

| symbol | meaning |
| :--- | :--- |
| `\` | Used before `.|[]-?*+{}()^$` \(and others\) to specify a special character |
| `\t` | tab |
| `\n` | new line |
| `\r` | carriage return |
| `\d` | `[0-9]` digit |
| `\D` | `[^0-9]` non-digit |
| `\s` | `[ \t\n\r\f\v]` white space |
| `\S` | `[^ \t\n\r\f\v]` non-white space |
| `\w` | `[a-zA-Z0-9_]` alphanumeric or underscore |
| `\W` | `[^a-zA-Z0-9_]` non-alphanumeric |

## Regular Expression \(re\) functions

<table>
  <thead>
    <tr>
      <th style="text-align:left">function</th>
      <th style="text-align:left">what it does</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>re.compile(pattern)</code>
      </td>
      <td style="text-align:left">Creates a <b>regular expression object</b> from the pattern.</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">**when the same pattern is used to match lots of sequences, then it is
        the most efficient to compile regex and save it into a variable for later
        use <code>p=re.compile(pattern)</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><code>re.match(pattern, str)</code>
        </p>
        <p><code>p.match(str)</code>
        </p>
      </td>
      <td style="text-align:left">returns a <b>match object</b> if str matches the pattern at the beginning
        of the str, else returns None.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>re.search(pattern, str)</code>
      </td>
      <td style="text-align:left">same as match but a match can start anywhere in the str</td>
    </tr>
  </tbody>
</table>

#### re.split\(\)

`re.split(pattern, str)`  
Splits the string by the occurrences of the pattern. Similar to `str.split()`

```python
re.split(';+' ,'abc;d;;e') 
['abc', 'd', 'e']

re.split('(;+)','abc;d;;e') 
['abc', ';', 'd', ';;', 'e'] 
```

#### re.sub\(\)

`re.sub(pattern, repl, str)`.  
If a match is found in the str, build a string that replaces the `pattern` by `repl` \(which is a string that can refer to matched groups\), else return the str unchanged.

```python
re.sub('(a+)','{as}','aabcaaadaf') 
{as}bc{as}d{as}f

re.sub('(a+)','(\g<1>)','aabcaaadaf') 
(aa)bc(aaa)d(a)f
```

## Match Object functions 

<table>
  <thead>
    <tr>
      <th style="text-align:left">function</th>
      <th style="text-align:left">what it does</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><code>m.group(int)</code>
        </p>
        <p><code>m.group(str)</code>
        </p>
      </td>
      <td style="text-align:left">returns the matched group pattern that corresponds to the given index
        (int) or name (str)</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>m.groupdict()</code>
      </td>
      <td style="text-align:left">puts all groups into a dictionary with keys as the index or name of the
        group and the values are the matched pattern</td>
    </tr>
  </tbody>
</table>

## Example

taken from phonecall.py written by prof. Pattis

```python
import re

def place_call(area_code, exchange, number):
    #does something with the stuff given

def parse_phone_numbered(number):
    pat = r"(\((\d{3})\))?((\d\d)?\d)-(\d{4})$"
    m = re.match(pat, number)
    assert m, number + 'is not legal phone number'
    return place_call(m.group(2), m.group(3), m.group(5))

def parse_phone_named(number):
    pat = r"(\((?P<area_code>\d{3})\))?(?P<exchange>(\d\d)?\d)-(?P<number>\d{4})$"
    m = re.match(pat,number)
    assert m, number + " is not a legal phone number"
    return place_call(**m.groupdict())
    #or place_call(m.group('area_code'), m.group('exchange'), m.group('number'))

number = input('Enter phone number: ')
```

