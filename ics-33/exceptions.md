---
description: 'week 1: ''Python review'''
---

# Exceptions

Exceptions are _raised_ and we _handle_ them. A function raises an exception when it fails to finish its job. 

* `raise` allows you to throw an exception at any time.
* `assert` verifies if a given condition is met and throws an `AssertionError` exception if it isn’t.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Good and simple</th>
      <th style="text-align:left">Inefficient</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>assert x &gt;= 0, &apos;not positive&apos;</code>
      </td>
      <td style="text-align:left">
        <p><code>if x &lt; 0:</code>
        </p>
        <p><code>   raise AssertionError(&apos;not positive&apos;)</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>

We can also handle exceptions with a `try-except` statement.

* The `try` body is executed normally until an exception is raised.
* `except` catches and handles the exception\(s\).
* `else` body will run only when the try finishes without any exception 
* `finally` body will always execute no matter if there's an exception or not

```python
def prompt_for_int(prompt_text):
    while True:
        try:
            response = input(prompt_text+': ')    # response is used in except
            answer = int(response)
            return answer
            
        except ValueError: #handle only this specific exception
            print(f'dummy dummy, {response} is not an int')
            
        #OR you could create an instance of ValueError exception class 
        #and use its attributes and behaviors
        except ValueError as e: 
            print(e) #will print the Python ValueError message
            
        except: #generic 
            #will catch ANY exception
            
        else: 
            #will only execute if the code in try body finishes naturally
            
        finally: 
            #will always execute no matter what
        


print(prompt_for_int('Enter a positive number'))
```

