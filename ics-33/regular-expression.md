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
| `(?:R)` | matches R but does not remember capture group |
| `(?:<name> R)` | matches R and remembers the name of the pattern. e.g. `(?P<id>[A-Za-z0-9]+)` |

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

