# Python for Data Science, AI & Development — Complete Course Study Guide

**Course**: Python for Data Science, AI & Development (IBM / Coursera)  
**Learner**: Pandya Shashank / Pandya Nareshbhai  
**Status**: 🏆 PASSED with **100% Final Grade**  

---

## 📋 Table of Contents
1. [Module 1: Python Basics](#module-1-python-basics)
2. [Module 2: Python Data Structures](#module-2-python-data-structures)
3. [Module 3: Python Programming Fundamentals](#module-3-python-programming-fundamentals)
4. [Module 4: Working with Data in Python](#module-4-working-with-data-in-python)
5. [Module 5: APIs and Data Collection](#module-5-apis-and-data-collection)
6. [Course Grade Summary](#course-grade-summary)

---

<a id="module-1-python-basics"></a>
## 🔷 Module 1: Python Basics

### 1. Types & Type Casting
Python supports several primitive data types:
- **Integers (`int`)**: Whole numbers without decimals (positive, zero, or negative), e.g., `-5`, `0`, `42`.
- **Floats (`float`)**: Numbers with decimal points or scientific notation, e.g., `3.14`, `-0.001`, `2.0`.
- **Booleans (`bool`)**: Truth values `True` or `False`.
- **Strings (`str`)**: Sequences of characters enclosed in single or double quotes, e.g., `"Hello"`, `'Python'`.

#### Explicit Type Conversion (Casting)
```python
int(3.99)       # Returns 3 (truncates decimal)
float("3.14")   # Returns 3.14
str(100)        # Returns '100'
int(False)      # Returns 0
int(True)       # Returns 1
bool(1)         # Returns True
```

### 2. Expressions and Mathematical Operations
Python evaluates arithmetic expressions following standard mathematical precedence (PEMDAS):

| Operator | Operation | Example | Result |
|---|---|---|---|
| `+` | Addition | `5 + 3` | `8` |
| `-` | Subtraction | `5 - 3` | `2` |
| `*` | Multiplication | `5 * 3` | `15` |
| `/` | Float Division | `5 / 2` | `2.5` |
| `//` | Integer (Floor) Division | `5 // 2` | `2` |
| `%` | Modulo (Remainder) | `5 % 2` | `1` |
| `**` | Exponentiation | `2 ** 3` | `8` |

> **Key Rule**: Standard division `/` always returns a `float`, even if the numbers divide evenly (e.g., `4 / 2` yields `2.0`).

### 3. Strings & String Manipulation
Strings are **immutable** ordered sequences of characters.

#### Indexing and Slicing
```python
s = "Python"
s[0]       # 'P' (first character)
s[-1]      # 'n' (last character)
s[0:4]     # 'Pyth' (slice from index 0 up to, but not including, 4)
s[::2]     # 'Pto' (stride of 2)
```

#### String Methods & Operations
```python
name = "Michael Jackson"

# Length
len(name)                # 15

# Case Conversion
name.upper()             # "MICHAEL JACKSON"
name.lower()             # "michael jackson"

# Search and Replace
name.find("Jack")        # Returns starting index (8). Returns -1 if not found.
name.replace("Michael", "Janet")  # "Janet Jackson"

# Escape Sequences
print("Line1\nLine2")    # \n adds a new line
print("Tab\tSpace")      # \t adds a tab
print("Backslash: \\")   # \\ escapes a backslash
```

---

<a id="module-2-python-data-structures"></a>
## 🔷 Module 2: Python Data Structures

### 1. Tuples
Tuples are **ordered** and **immutable** (cannot be changed after creation). Enclosed in parentheses `()`.

```python
tuple1 = ("disco", 10, 1.2)
tuple1[0]               # 'disco'
tuple2 = tuple1 + ("hard rock", 10)  # Concatenation creates a new tuple

# Nesting & Indexing inner elements
nested_tuple = (1, 2, ("pop", "rock"), (3, 4))
nested_tuple[2][1]      # 'rock'
```

### 2. Lists
Lists are **ordered** and **mutable** (can be modified in place). Enclosed in square brackets `[]`.

```python
L = ["Michael Jackson", 10.1, 1982]

# Mutability & Modification
L[0] = "Janet Jackson"  # Modifies index 0 in-place

# Adding Elements: append() vs extend()
L.append(["pop", "rock"]) # Adds the list as a SINGLE element at the end
L.extend(["pop", "rock"]) # Adds EACH element individually to the list

# Deleting & Splitting
del L[0]                 # Removes element at index 0
"A,B,C,D".split(",")     # Returns ['A', 'B', 'C', 'D']

# Aliasing vs Copying
A = ["hard rock", 10, 1.2]
B = A                    # Aliasing: B points to the SAME list as A
C = A[:]                 # Cloning: C is a NEW independent copy of A
```

### 3. Sets
Sets are **unordered** collections of **unique** elements. Enclosed in curly braces `{}`.

```python
set1 = {"pop", "rock", "soul", "hard rock", "rock"}  # Automatically removes duplicate 'rock'
# Result: {'pop', 'rock', 'soul', 'hard rock'}

# Converting List to Set
music_set = set(["pop", "rock", "pop"])  # Returns {'pop', 'rock'}

# Set Operations
set1.add("R&B")          # Adds element
set1.remove("pop")       # Removes element

album_set1 = {"AC/DC", "Back in Black", "Thriller"}
album_set2 = {"AC/DC", "Back in Black", "The Dark Side of the Moon"}

# Intersection and Union
intersection = album_set1 & album_set2         # {'AC/DC', 'Back in Black'}
union = album_set1.union(album_set2)           # All unique items from both
album_set1.issubset(union)                     # Returns True
```

### 4. Dictionaries
Dictionaries store data in **key-value pairs**. Keys must be unique and immutable. Enclosed in curly braces `{}`.

```python
dict1 = {
    "title": "Thriller",
    "release_year": 1982,
    "genre": "Pop"
}

# Accessing & Updating
dict1["title"]               # 'Thriller'
dict1["artist"] = "Michael"   # Adds new key-value pair
"title" in dict1             # Returns True (checks if key exists)

# Keys and Values
dict1.keys()                 # dict_keys(['title', 'release_year', 'genre', 'artist'])
dict1.values()               # dict_values(['Thriller', 1982, 'Pop', 'Michael'])
```

---

<a id="module-3-python-programming-fundamentals"></a>
## 🔷 Module 3: Python Programming Fundamentals

### 1. Conditions & Branching
Control execution flow based on logical expressions.

```python
age = 18

if age > 18:
    print("You can enter the concert.")
elif age == 18:
    print("You just turned 18! Show ID.")
else:
    print("Move along to the kid zone.")

# Logical Operators: and, or, not
if age >= 18 and age <= 65:
    print("Working age adult")
```

### 2. Loops & Enumeration

#### `for` Loops
```python
squares = ["red", "yellow", "green"]

for square in squares:
    print(square)

for i in range(len(squares)):
    print(i, squares[i])

# enumerate() yields index and value simultaneously
for i, x in enumerate(['A', 'B', 'C']):
    print(i, 2 * x)
# Output:
# 0 AA
# 1 BB
# 2 CC
```

#### `while` Loops
```python
count = 5
while count > 0:
    print(count)
    count -= 1
```

### 3. Functions & Scope

```python
# Function Definition with Default Parameter
def add_numbers(a, b=10):
    """
    Returns the sum of a and b.
    """
    return a + b

result = add_numbers(5)     # Returns 15 (uses default b=10)

# Local vs Global Scope
total_budget = 1000         # Global Variable

def calculate_remaining(spent):
    total_budget = 500      # Local Variable (shadows global inside function)
    return total_budget - spent

print(calculate_remaining(200)) # Output: 300
```

### 4. Exception Handling
Gracefully catch and process runtime errors without crashing the program.

```python
try:
    file = open("data.txt", "r")
    value = 10 / 0
except FileNotFoundError:
    print("Error: The requested file does not exist.")
except ZeroDivisionError:
    print("Error: Cannot divide by zero.")
except Exception as e:
    print(f"Generic error caught: {e}")
finally:
    print("Execution complete. (Always runs regardless of errors)")
```

### 5. Objects & Classes
Classes act as blueprints for creating objects with attributes (data) and methods (functions).

```python
class Rectangle:
    # Constructor Method
    def __init__(self, width=2, height=3, color='red'):
        self.width = width
        self.height = height
        self.color = color

    # Instance Method
    def calculate_area(self):
        return self.width * self.height

# Object Instantiation
rect1 = Rectangle(4, 5, 'blue')
print(rect1.width)            # Access attribute: 4
print(rect1.calculate_area()) # Invoke method: 20
```

---

<a id="module-4-working-with-data-in-python"></a>
## 🔷 Module 4: Working with Data in Python

### 1. Reading & Writing Files with `open`
Using the `with` statement guarantees that the file is automatically closed.

```python
# Reading Files
with open("Example1.txt", "r") as file1:
    content = file1.read()         # Reads entire file content as string
    
with open("Example1.txt", "r") as file1:
    line1 = file1.readline()       # Reads first single line
    line_chars = file1.readline(4) # Reads first 4 characters of next line

# Writing & Appending
with open("Example2.txt", "w") as file2:
    file2.write("Overwriting line 1\n") # Writes to file (overwrites existing)

with open("Example2.txt", "a") as file2:
    file2.write("Appended line 2\n")   # Appends to end of file
```

### 2. Pandas for Data Analysis
Pandas provides high-performance data structures like `DataFrame` (2D table) and `Series` (1D column).

```python
import pandas as pd

# Reading Files into DataFrames
df = pd.read_csv("data.csv")

# Inspection Methods
df.head()             # View first 5 rows
df.tail()             # View last 5 rows
df.describe()         # Summary statistics for numeric columns
df['ColumnName'].unique() # Array of distinct values in column

# Selection & Indexing
df[['Name', 'Age']]   # Select multiple columns
df.iloc[0, 0]         # First row, first column by integer position
df.loc[0, 'Name']     # First row, column 'Name' by label

# Filtering Data
adults = df[df['Age'] >= 18]
```

### 3. NumPy for Numerical Computing
NumPy provides fast $N$-dimensional arrays (`ndarray`) for mathematical operations.

#### 1D Arrays
```python
import numpy as np

a = np.array([1, 2, 3, 4, 5])
a.ndim               # Array dimension: 1
a.shape              # Shape tuple: (5,)
a.dtype              # Data type, e.g. int64

# Vectorized Operations & Vector Math
b = a + 1            # Array([2, 3, 4, 5, 6]) — scalar addition
c = a * 2            # Array([2, 4, 6, 8, 10]) — scalar multiplication

x = np.array([-1, 1])
y = np.array([1, 1])
dot_product = np.dot(x, y) # (-1*1) + (1*1) = 0
```

#### 2D Arrays & Matrices
```python
A = np.array([[1, 0, 1], [2, 2, 2]])
A.ndim               # 2

# Transpose & Matrix Multiplication
A_transposed = A.T   # Transposes shape (2,3) to (3,2)
B = np.array([[1, 1], [1, 1], [1, 1]])
result = np.dot(A, B)# Performs matrix dot product
```

---

<a id="module-5-apis-and-data-collection"></a>
## 🔷 Module 5: APIs and Data Collection

### 1. HTTP Requests & Simple APIs
APIs allow communication between client applications and web services.

```python
import requests

# Sending HTTP GET request
url = "https://api.example.com/data"
payload = {"name": "John", "ID": "123"}
response = requests.get(url, params=payload)

# HTTP Response Attributes & Status Codes
print(response.status_code) # 200 (Success), 404 (Not Found), 500 (Server Error)
print(response.headers)     # Response HTTP headers dictionary
data_dict = response.json() # Converts JSON response directly to Python dictionary
```

### 2. Web Scraping with BeautifulSoup
Web scraping extracts structured data from raw HTML/XML documents.

#### Key HTML Tags
- `<table>`: Table element
- `<tr>`: Table Row
- `<td>`: Table Cell Data
- `<a>`: Hyperlink anchor
- `<h1>`-`<h6>`: Heading levels

```python
from bs4 import BeautifulSoup

html_doc = "<html><body><table><tr><td>Item 1</td></tr></table></body></html>"
soup = BeautifulSoup(html_doc, "html.parser")

# Finding elements in the HTML tree
table_rows = soup.find_all(name="tr")
for row in table_rows:
    cells = row.find_all(name="td")
    for cell in cells:
        print(cell.text)
```

---

<a id="course-grade-summary"></a>
## 🏆 Course Grade Summary

| Module / Assessment | Status | Weight | Score Achieved |
|---|---|---|---|
| **Module 1 Graded Quiz: Python Basics** | **Success: Passed** | 5% | **100%** |
| **Module 2 Graded Quiz: Python Data Structures** | **Success: Passed** | 5% | **100%** |
| **Module 3 Graded Quiz: Python Programming Fundamentals** | **Success: Passed** | 5% | **100%** |
| **Module 4 Graded Quiz: Working with Data in Python** | **Success: Passed** | 5% | **100%** |
| **Module 5 Graded Quiz: APIs and Data Collection** | **Success: Passed** | 5% | **100%** |
| **Final Exam for the Course** | **Success: Passed** | 75% | **100%** |
| **FINAL OVERALL COURSE RESULT** | **PASSED** | **100%** | **100%** |

---
*Study Guide generated and saved to Downloads folder.*
