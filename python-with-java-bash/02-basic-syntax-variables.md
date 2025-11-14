# Chapter 2: Basic Syntax and Variables

This chapter compares the fundamental syntax and variable handling in Python, Java, and Bash. Understanding these basics is crucial for writing effective code in each language.

## Comments

### Python
```python
# Single-line comment
"""
Multi-line comment
or docstring
"""
```

### Java
```java
// Single-line comment
/* Multi-line comment */
/**
 * JavaDoc comment
 */
```

### Bash
```bash
# Single-line comment
# Multi-line comments require # on each line
```

## Variables

### Python
Python uses dynamic typing - variables don't need type declarations.

```python
# Variable assignment
name = "Python"
age = 30
price = 19.99
is_active = True

# Type checking (optional)
name: str = "Python"  # Type hint (Python 3.5+)
age: int = 30

# Multiple assignment
x, y, z = 1, 2, 3
a = b = c = 0

# Variable types
print(type(name))    # <class 'str'>
print(type(age))     # <class 'int'>
print(type(price))   # <class 'float'>
print(type(is_active))  # <class 'bool'>
```

### Java
Java requires explicit type declarations (static typing).

```java
// Variable declaration and assignment
String name = "Java";
int age = 30;
double price = 19.99;
boolean isActive = true;

// Declaration then assignment
int x;
x = 10;

// Multiple variables (same type)
int a, b, c;
a = 1;
b = 2;
c = 3;

// Constants (final keyword)
final double PI = 3.14159;
final String COMPANY = "Oracle";

// Type checking
System.out.println(name.getClass().getName());  // java.lang.String
```

### Bash
Bash variables are untyped strings by default.

```bash
#!/bin/bash

# Variable assignment (no spaces around =)
name="Bash"
age=30
price=19.99

# Accessing variables (use $)
echo $name
echo "Age is $age"

# Better practice: use ${variable}
echo "${name} is ${age} years old"

# Constants (convention: UPPERCASE)
readonly PI=3.14159
COMPANY="GNU"

# Multiple assignment
x=1 y=2 z=3

# Command substitution
current_date=$(date)
file_count=$(ls | wc -l)

# Arithmetic expansion
result=$((10 + 20))
```

## Data Types

### Python
```python
# Numbers
integer_num = 42
float_num = 3.14
complex_num = 3 + 4j

# Strings
single_quote = 'Hello'
double_quote = "World"
triple_quote = """Multi-line
string"""

# Boolean
is_true = True
is_false = False

# None (null equivalent)
value = None

# Collections
my_list = [1, 2, 3]
my_tuple = (1, 2, 3)
my_dict = {"key": "value"}
my_set = {1, 2, 3}
```

### Java
```java
// Primitives
byte byteValue = 127;
short shortValue = 32767;
int intValue = 2147483647;
long longValue = 9223372036854775807L;
float floatValue = 3.14f;
double doubleValue = 3.14159;
char charValue = 'A';
boolean boolValue = true;

// Objects (Wrappers)
Integer integerObj = 42;
Double doubleObj = 3.14;
String stringObj = "Hello";
Boolean boolObj = true;

// Collections
List<Integer> list = new ArrayList<>();
Map<String, String> map = new HashMap<>();
Set<String> set = new HashSet<>();
```

### Bash
```bash
#!/bin/bash

# All variables are strings by default
number="42"
text="Hello World"

# Arithmetic operations require special syntax
num1=10
num2=20
sum=$((num1 + num2))

# Arrays
arr=(1 2 3 4 5)
echo ${arr[0]}  # First element

# Associative arrays (Bash 4+)
declare -A map
map["key1"]="value1"
map["key2"]="value2"
```

## String Operations

### Python
```python
# String concatenation
first = "Hello"
last = "World"
full = first + " " + last
full_template = f"{first} {last}"  # f-string (Python 3.6+)

# String methods
text = "hello world"
print(text.upper())        # HELLO WORLD
print(text.lower())        # hello world
print(text.capitalize())   # Hello world
print(len(text))           # 11
print(text.split())        # ['hello', 'world']
```

### Java
```java
// String concatenation
String first = "Hello";
String last = "World";
String full = first + " " + last;
String fullTemplate = String.format("%s %s", first, last);

// StringBuilder for efficiency
StringBuilder sb = new StringBuilder();
sb.append(first).append(" ").append(last);
String result = sb.toString();

// String methods
String text = "hello world";
System.out.println(text.toUpperCase());  // HELLO WORLD
System.out.println(text.toLowerCase());  // hello world
System.out.println(text.length());       // 11
String[] parts = text.split(" ");
```

### Bash
```bash
#!/bin/bash

# String concatenation
first="Hello"
last="World"
full="$first $last"

# String operations
text="hello world"
echo "${text^^}"           # HELLO WORLD (Bash 4+)
echo "${text,,}"           # hello world (Bash 4+)
echo "${#text}"            # Length: 11
echo "${text// /_}"        # Replace spaces: hello_world

# Substring
text="Hello World"
echo "${text:0:5}"         # Hello
echo "${text:6}"           # World
```

## Constants

### Python
```python
# Convention: UPPERCASE (not enforced)
PI = 3.14159
MAX_SIZE = 100

# True constants (using enum or dataclass)
from enum import Enum
class Constants(Enum):
    PI = 3.14159
    MAX_SIZE = 100
```

### Java
```java
// Constants using final
final double PI = 3.14159;
final int MAX_SIZE = 100;

// Class constants
public class Constants {
    public static final double PI = 3.14159;
    public static final int MAX_SIZE = 100;
}
```

### Bash
```bash
#!/bin/bash

# Read-only variables
readonly PI=3.14159
readonly MAX_SIZE=100

# Convention: UPPERCASE
COMPANY_NAME="MyCompany"
```

## Variable Scope

### Python
```python
# Global scope
global_var = "I'm global"

def my_function():
    # Local scope
    local_var = "I'm local"
    global global_var
    global_var = "Modified globally"
    
    # Nonlocal (for nested functions)
    def inner():
        nonlocal local_var
        local_var = "Modified in inner"
```

### Java
```java
public class ScopeExample {
    // Class-level variable
    private String classVar = "Class variable";
    
    public void method() {
        // Local variable
        String localVar = "Local variable";
        
        {
            // Block scope
            String blockVar = "Block variable";
        }
        // blockVar not accessible here
    }
}
```

### Bash
```bash
#!/bin/bash

# Global by default
global_var="I'm global"

my_function() {
    # Local variable
    local local_var="I'm local"
    
    # Without local, variable is global
    another_var="I'm also global"
}

my_function
echo "$local_var"      # Empty (local to function)
echo "$another_var"    # "I'm also global"
```

## Summary

| Aspect | Python | Java | Bash |
|--------|--------|------|------|
| **Typing** | Dynamic | Static | Untyped (strings) |
| **Declaration** | Not required | Required | Not required |
| **Type Safety** | Runtime | Compile-time | None |
| **Constants** | Convention | `final` keyword | `readonly` keyword |
| **String Interpolation** | f-strings | String.format() | Variable expansion |
| **Scope Control** | `global`, `nonlocal` | Access modifiers | `local` keyword |

---

**Next Chapter**: [Control Structures](./03-control-structures.md)

