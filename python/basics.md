**Python Basics**

### Number Data Types
Python supports multiple number data types:
- **Integers** (whole numbers, e.g., `42`)
- **Floats** (decimal numbers, e.g., `3.14`)
- **Complex numbers** (e.g., `3 + 4j`)

### Arithmetic Operations
- Division (`/`) always results in a float:
  ```python
  12 / 3  # result is 4.0
  12 / 5  # result is 2.4
  ```
- Integer division (`//`) rounds down:
  ```python
  12 // 5   # result is 2
  -1 // 2   # result is -1
  ```
- Exponentiation (`**`):
  ```python
  6 ** 2    # result is 36
  ```

### Order of Operations
Python follows **PEMDAS** (Parentheses, Exponents, Multiplication/Division, Addition/Subtraction).

### Built-in Math Functions
- `pow(a, b)`: Equivalent to `a ** b`
- `round(a)`: Rounds `a` to the nearest integer
- `round(a, b)`: Rounds `a` to `b` decimal places
- `bin(a)`: Converts `a` to binary

For advanced operations, import the `math` module:
```python
import math
math.trunc(3.75)  # Returns 3
math.floor(3.75)  # Returns 3
math.ceil(3.75)   # Returns 4
```

### Strings in Python
- **Indexing:** Strings are indexed from `0`.
  ```python
  "Sweden"[:3]   # returns 'Swe'
  "Sweden"[3:]   # returns 'den'
  "Sweden"[0:4]  # returns 'Swed'
  ```
- **String Methods:**
  ```python
  text = "hello world"
  text.upper()       # 'HELLO WORLD'
  text.lower()       # 'hello world'
  text.capitalize()  # 'Hello world'
  text.split()       # ['hello', 'world']
  ```
- **Membership Operators:**
  ```python
  "sage" in "sausage"  # True
  10 in [14, 1, 96]    # False
  ```

### Variables and Identity
- Two variables can reference the same object:
  ```python
  his_moods = ["happy", "sad"]
  her_moods = his_moods
  his_moods.append("excited")
  print(her_moods)  # ['happy', 'sad', 'excited']
  ```
- **Comparison Operators:**
  ```python
  first_list = [1, 2, 3]
  second_list = [1, 2, 3]
  second_list_1 = first_list
  print(first_list is second_list)    # False (different objects)
  print(first_list is second_list_1)  # True (same object)
  print(first_list == second_list)    # True (same values)
  ```

### f-Strings (String Formatting)
```python
cheese = "Gouda"
price = 8.29
print(f"A pound of {cheese} will be ${price:.2f}.")
```
**Output:**
```
A pound of Gouda will be $8.29.
```

### If-Elif-Else Statements
```python
weather = 'sunny'
if weather == 'rainy':
    print('Bring an umbrella!')
elif weather == 'sunny':
    print('Wear sunscreen!')
elif weather == 'windy':
    print('Hold onto your hat!')
else:
    print('Enjoy the weather!')
```

### Loops
**For Loop:**
```python
shopping_list = ['apples', 'bread', 'milk']
for item in shopping_list:
    print('- ' + item)
```

**Using range():**
```python
for i in range(5):
    print('yes')
```

### Functions
#### `*args` and `**kwargs`
- `*args` allows passing a variable number of arguments as a tuple.
  ```python
  def print_args(*args):
      for arg in args:
          print(arg)
  print_args('hello', 'world', '!')
  ```
- `**kwargs` allows passing keyword arguments as a dictionary.
  ```python
  def print_kwargs(**kwargs):
      for key, value in kwargs.items():
          print(f"{key} = {value}")
  print_kwargs(name='John', age=30)
  ```

#### Type Hinting
```python
def calculate_total_cost(price: float, tax: float = 0.10, shipping: float = 5.00) -> float:
    return price + price * tax + shipping
```

### Decorators
- **Basic decorator:**
  ```python
  def uppercase(func):
      def wrapper(*args, **kwargs):
          result = func(*args, **kwargs)
          return result.upper()
      return wrapper

  @uppercase
  def greet(name):
      return f"Hello, {name}!"

  print(greet("William"))
  ```
  **Output:** `HELLO, WILLIAM!`

- **Parameterized decorator:**
  ```python
  def add_prefix(prefix):
      def decorator(func):
          def wrapper(*args, **kwargs):
              result = func(*args, **kwargs)
              return f"{prefix}{result}"
          return wrapper
      return decorator

  @add_prefix("Result: ")
  def greet(name):
      return f"Hello, {name}!"

  print(greet("William"))
  ```
  **Output:** `Result: Hello, William!`

### Line Continuation (Backslash Operator)
If a statement spans multiple lines, use `\` to continue:
```python
print("This is a long string \
that continues on the next line.")
```

### User Input
```python
age = int(input("Please enter your age: "))
print(type(age))  # <class 'int'>
```

### Boolean Values
```python
bool1 = True
bool2 = False
print(type(bool1))  # <class 'bool'>
```

---
This document serves as a quick reference for fundamental Python concepts and syntax.

