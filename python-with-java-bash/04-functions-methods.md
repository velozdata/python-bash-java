# Chapter 4: Functions and Methods

Functions and methods are fundamental building blocks for code organization and reusability. This chapter compares how Python, Java, and Bash handle function definitions, parameters, and return values.

## Function Definition

### Python
```python
# Simple function
def greet():
    print("Hello, World!")

# Function with parameters
def greet_person(name):
    print(f"Hello, {name}!")

# Function with return value
def add(a, b):
    return a + b

# Function with default parameters
def greet_with_title(name, title="Mr."):
    return f"Hello, {title} {name}!"

# Function with multiple return values
def divide(a, b):
    quotient = a // b
    remainder = a % b
    return quotient, remainder

# Call function
result = add(5, 3)
q, r = divide(10, 3)
```

### Java
```java
// Method in a class
public class Calculator {
    // Simple method
    public void greet() {
        System.out.println("Hello, World!");
    }
    
    // Method with parameters
    public void greetPerson(String name) {
        System.out.println("Hello, " + name + "!");
    }
    
    // Method with return value
    public int add(int a, int b) {
        return a + b;
    }
    
    // Method with default parameters (not directly supported, use overloading)
    public String greetWithTitle(String name) {
        return greetWithTitle(name, "Mr.");
    }
    
    public String greetWithTitle(String name, String title) {
        return "Hello, " + title + " " + name + "!";
    }
    
    // Multiple return values (use a class or array)
    public class DivisionResult {
        int quotient;
        int remainder;
    }
    
    public DivisionResult divide(int a, int b) {
        DivisionResult result = new DivisionResult();
        result.quotient = a / b;
        result.remainder = a % b;
        return result;
    }
}

// Static methods (can be called without instance)
public class MathUtils {
    public static int add(int a, int b) {
        return a + b;
    }
}
```

### Bash
```bash
#!/bin/bash

# Simple function
greet() {
    echo "Hello, World!"
}

# Function with parameters
greet_person() {
    local name=$1
    echo "Hello, $name!"
}

# Function with return value (exit code)
check_file() {
    if [ -f "$1" ]; then
        return 0  # Success
    else
        return 1  # Failure
    fi
}

# Function with default parameters
greet_with_title() {
    local name=$1
    local title=${2:-"Mr."}  # Default to "Mr." if not provided
    echo "Hello, $title $name!"
}

# Function returning multiple values (using global variables or echo)
divide() {
    local a=$1
    local b=$2
    echo $((a / b)) $((a % b))
}

# Call function
greet
greet_person "John"
result=$(divide 10 3)
quotient=$(echo $result | cut -d' ' -f1)
remainder=$(echo $result | cut -d' ' -f2)
```

## Function Parameters

### Python
```python
# Positional arguments
def power(base, exponent):
    return base ** exponent

result = power(2, 3)  # 8

# Keyword arguments
result = power(exponent=3, base=2)  # 8

# Default parameters
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

greet("John")                    # "Hello, John!"
greet("John", "Hi")              # "Hi, John!"
greet(name="John", greeting="Hi") # "Hi, John!"

# Variable arguments (*args)
def sum_all(*numbers):
    return sum(numbers)

total = sum_all(1, 2, 3, 4, 5)  # 15

# Keyword arguments (**kwargs)
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="John", age=30, city="NYC")

# Combined
def complex_function(a, b, *args, **kwargs):
    print(f"a={a}, b={b}")
    print(f"args={args}")
    print(f"kwargs={kwargs}")
```

### Java
```java
// Method overloading (simulates default parameters)
public class Example {
    public void greet(String name) {
        greet(name, "Hello");
    }
    
    public void greet(String name, String greeting) {
        System.out.println(greeting + ", " + name + "!");
    }
}

// Variable arguments (varargs)
public int sumAll(int... numbers) {
    int sum = 0;
    for (int num : numbers) {
        sum += num;
    }
    return sum;
}

int total = sumAll(1, 2, 3, 4, 5);  // 15

// Method with multiple parameters
public void printInfo(String name, int age, String city) {
    System.out.println("Name: " + name);
    System.out.println("Age: " + age);
    System.out.println("City: " + city);
}
```

### Bash
```bash
#!/bin/bash

# Positional parameters ($1, $2, etc.)
greet() {
    local name=$1
    local greeting=${2:-"Hello"}  # Default value
    echo "$greeting, $name!"
}

greet "John"           # "Hello, John!"
greet "John" "Hi"      # "Hi, John!"

# All arguments ($@ or $*)
sum_all() {
    local sum=0
    for num in "$@"; do
        sum=$((sum + num))
    done
    echo $sum
}

total=$(sum_all 1 2 3 4 5)  # 15

# Number of arguments
check_args() {
    echo "Number of arguments: $#"
    echo "All arguments: $@"
}

# Shift to process arguments
process_args() {
    while [ $# -gt 0 ]; do
        echo "Processing: $1"
        shift
    done
}
```

## Return Values

### Python
```python
# Single return value
def square(x):
    return x * x

# Multiple return values (tuple)
def get_name_age():
    return "John", 30

name, age = get_name_age()

# No return (returns None)
def print_message(msg):
    print(msg)
    # Implicitly returns None

# Early return
def find_positive(numbers):
    for num in numbers:
        if num > 0:
            return num
    return None
```

### Java
```java
// Single return value
public int square(int x) {
    return x * x;
}

// Multiple return values (use a class)
public class PersonInfo {
    String name;
    int age;
}

public PersonInfo getNameAge() {
    PersonInfo info = new PersonInfo();
    info.name = "John";
    info.age = 30;
    return info;
}

// Void (no return)
public void printMessage(String msg) {
    System.out.println(msg);
    // No return statement needed
}

// Early return
public Integer findPositive(int[] numbers) {
    for (int num : numbers) {
        if (num > 0) {
            return num;
        }
    }
    return null;
}
```

### Bash
```bash
#!/bin/bash

# Return exit code (0 = success, non-zero = failure)
check_file() {
    if [ -f "$1" ]; then
        return 0
    else
        return 1
    fi
}

# Return value via echo (capture with $())
get_square() {
    local x=$1
    echo $((x * x))
}

result=$(get_square 5)  # 25

# Multiple return values
get_name_age() {
    echo "John 30"
}

result=$(get_name_age)
name=$(echo $result | cut -d' ' -f1)
age=$(echo $result | cut -d' ' -f2)

# Using global variables
name=""
age=0

set_name_age() {
    name="John"
    age=30
}
```

## Lambda Functions / Anonymous Functions

### Python
```python
# Lambda function
square = lambda x: x * x
result = square(5)  # 25

# Lambda with map
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, numbers))  # [1, 4, 9, 16, 25]

# Lambda with filter
evens = list(filter(lambda x: x % 2 == 0, numbers))  # [2, 4]

# Lambda with sorted
people = [("John", 30), ("Jane", 25), ("Bob", 35)]
sorted_by_age = sorted(people, key=lambda x: x[1])
```

### Java
```java
// Lambda expressions (Java 8+)
Function<Integer, Integer> square = x -> x * x;
int result = square.apply(5);  // 25

// Lambda with Stream API
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> squared = numbers.stream()
    .map(x -> x * x)
    .collect(Collectors.toList());

// Lambda with filter
List<Integer> evens = numbers.stream()
    .filter(x -> x % 2 == 0)
    .collect(Collectors.toList());

// Lambda with Comparator
List<Person> people = Arrays.asList(
    new Person("John", 30),
    new Person("Jane", 25)
);
people.sort((p1, p2) -> Integer.compare(p1.getAge(), p2.getAge()));
```

### Bash
```bash
#!/bin/bash

# Bash doesn't have true lambda functions, but functions can be passed
# Using command substitution and functions
square() {
    echo $(($1 * $1))
}

result=$(square 5)  # 25

# Processing arrays
numbers=(1 2 3 4 5)
for num in "${numbers[@]}"; do
    squared=$(square $num)
    echo "$squared"
done
```

## Recursion

### Python
```python
# Recursive function
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)

result = factorial(5)  # 120

# Fibonacci
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)
```

### Java
```java
// Recursive method
public int factorial(int n) {
    if (n <= 1) {
        return 1;
    }
    return n * factorial(n - 1);
}

int result = factorial(5);  // 120

// Fibonacci
public int fibonacci(int n) {
    if (n <= 1) {
        return n;
    }
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

### Bash
```bash
#!/bin/bash

# Recursive function
factorial() {
    local n=$1
    if [ $n -le 1 ]; then
        echo 1
    else
        local prev=$(factorial $((n - 1)))
        echo $((n * prev))
    fi
}

result=$(factorial 5)  # 120

# Fibonacci
fibonacci() {
    local n=$1
    if [ $n -le 1 ]; then
        echo $n
    else
        local a=$(fibonacci $((n - 1)))
        local b=$(fibonacci $((n - 2)))
        echo $((a + b))
    fi
}
```

## Summary

| Feature | Python | Java | Bash |
|---------|--------|------|------|
| **Definition** | `def function():` | `public void method()` | `function() { }` |
| **Default Params** | Yes | Overloading | `${var:-default}` |
| **Varargs** | `*args` | `Type... args` | `$@` |
| **Return** | `return value` | `return value` | Exit code or `echo` |
| **Multiple Returns** | Tuple unpacking | Class/Array | Global vars or echo |
| **Lambda** | `lambda x: x*2` | `x -> x*2` | Functions only |
| **Recursion** | Yes | Yes | Yes (limited) |

---

**Next Chapter**: [File Operations](./05-file-operations.md)

