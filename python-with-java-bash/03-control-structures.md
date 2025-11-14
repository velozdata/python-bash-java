# Chapter 3: Control Structures

Control structures allow programs to make decisions and repeat actions. This chapter compares how Python, Java, and Bash handle conditionals and loops.

## Conditional Statements

### If-Else Statements

#### Python
```python
# Basic if-else
age = 18
if age >= 18:
    print("Adult")
else:
    print("Minor")

# If-elif-else
score = 85
if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
else:
    grade = "F"

# Ternary operator
status = "Adult" if age >= 18 else "Minor"

# Multiple conditions
if age >= 18 and age < 65:
    print("Working age")
```

#### Java
```java
// Basic if-else
int age = 18;
if (age >= 18) {
    System.out.println("Adult");
} else {
    System.out.println("Minor");
}

// If-else if-else
int score = 85;
String grade;
if (score >= 90) {
    grade = "A";
} else if (score >= 80) {
    grade = "B";
} else if (score >= 70) {
    grade = "C";
} else {
    grade = "F";
}

// Ternary operator
String status = (age >= 18) ? "Adult" : "Minor";

// Multiple conditions
if (age >= 18 && age < 65) {
    System.out.println("Working age");
}
```

#### Bash
```bash
#!/bin/bash

# Basic if-else
age=18
if [ $age -ge 18 ]; then
    echo "Adult"
else
    echo "Minor"
fi

# If-elif-else
score=85
if [ $score -ge 90 ]; then
    grade="A"
elif [ $score -ge 80 ]; then
    grade="B"
elif [ $score -ge 70 ]; then
    grade="C"
else
    grade="F"
fi

# Ternary-like (using && and ||)
[ $age -ge 18 ] && status="Adult" || status="Minor"

# Multiple conditions
if [ $age -ge 18 ] && [ $age -lt 65 ]; then
    echo "Working age"
fi
```

## Comparison Operators

### Python
```python
# Comparison operators
a == b  # Equal
a != b  # Not equal
a < b   # Less than
a > b   # Greater than
a <= b  # Less than or equal
a >= b  # Greater than or equal
a is b  # Identity (same object)
a in b  # Membership
```

### Java
```java
// Comparison operators
a == b  // Equal (primitives) or reference equality (objects)
a != b  // Not equal
a < b   // Less than
a > b   // Greater than
a <= b  // Less than or equal
a >= b  // Greater than or equal
a.equals(b)  // Value equality for objects
```

### Bash
```bash
# Numeric comparisons
[ $a -eq $b ]  # Equal
[ $a -ne $b ]  # Not equal
[ $a -lt $b ]  # Less than
[ $a -gt $b ]  # Greater than
[ $a -le $b ]  # Less than or equal
[ $a -ge $b ]  # Greater than or equal

# String comparisons
[ "$a" = "$b" ]   # Equal
[ "$a" != "$b" ]  # Not equal
[ -z "$a" ]       # Empty string
[ -n "$a" ]       # Non-empty string
```

## Switch/Case Statements

### Python
```python
# Python 3.10+ match-case (similar to switch)
def get_day_type(day):
    match day:
        case "Monday" | "Tuesday" | "Wednesday" | "Thursday" | "Friday":
            return "Weekday"
        case "Saturday" | "Sunday":
            return "Weekend"
        case _:
            return "Unknown"

# Traditional approach (dictionary)
def get_day_type_dict(day):
    weekdays = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"]
    return "Weekday" if day in weekdays else "Weekend"
```

### Java
```java
// Switch statement (traditional)
String getDayType(String day) {
    switch (day) {
        case "Monday":
        case "Tuesday":
        case "Wednesday":
        case "Thursday":
        case "Friday":
            return "Weekday";
        case "Saturday":
        case "Sunday":
            return "Weekend";
        default:
            return "Unknown";
    }
}

// Switch expression (Java 14+)
String getDayTypeModern(String day) {
    return switch (day) {
        case "Monday", "Tuesday", "Wednesday", "Thursday", "Friday" -> "Weekday";
        case "Saturday", "Sunday" -> "Weekend";
        default -> "Unknown";
    };
}
```

### Bash
```bash
#!/bin/bash

# Case statement
get_day_type() {
    case $1 in
        Monday|Tuesday|Wednesday|Thursday|Friday)
            echo "Weekday"
            ;;
        Saturday|Sunday)
            echo "Weekend"
            ;;
        *)
            echo "Unknown"
            ;;
    esac
}
```

## Loops

### For Loops

#### Python
```python
# Iterate over range
for i in range(5):
    print(i)  # 0, 1, 2, 3, 4

# Iterate over list
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)

# With index
for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")

# Dictionary iteration
person = {"name": "John", "age": 30}
for key, value in person.items():
    print(f"{key}: {value}")

# List comprehension
squares = [x**2 for x in range(10)]
```

#### Java
```java
// Traditional for loop
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}

// Enhanced for loop (for-each)
String[] fruits = {"apple", "banana", "cherry"};
for (String fruit : fruits) {
    System.out.println(fruit);
}

// With index
for (int i = 0; i < fruits.length; i++) {
    System.out.println(i + ": " + fruits[i]);
}

// Map iteration
Map<String, Integer> person = new HashMap<>();
person.put("name", 30);
for (Map.Entry<String, Integer> entry : person.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

#### Bash
```bash
#!/bin/bash

# C-style for loop
for ((i=0; i<5; i++)); do
    echo $i
done

# Iterate over list
fruits=("apple" "banana" "cherry")
for fruit in "${fruits[@]}"; do
    echo "$fruit"
done

# With index
for i in "${!fruits[@]}"; do
    echo "$i: ${fruits[$i]}"
done

# Iterate over range
for i in {1..5}; do
    echo $i
done

# Iterate over files
for file in *.txt; do
    echo "Processing $file"
done
```

### While Loops

#### Python
```python
# Basic while loop
count = 0
while count < 5:
    print(count)
    count += 1

# While-else
count = 0
while count < 5:
    print(count)
    count += 1
else:
    print("Loop completed")

# Infinite loop with break
while True:
    user_input = input("Enter 'quit' to exit: ")
    if user_input == "quit":
        break
```

#### Java
```java
// Basic while loop
int count = 0;
while (count < 5) {
    System.out.println(count);
    count++;
}

// Do-while loop
int count = 0;
do {
    System.out.println(count);
    count++;
} while (count < 5);

// Infinite loop with break
Scanner scanner = new Scanner(System.in);
while (true) {
    String input = scanner.nextLine();
    if ("quit".equals(input)) {
        break;
    }
}
```

#### Bash
```bash
#!/bin/bash

# Basic while loop
count=0
while [ $count -lt 5 ]; do
    echo $count
    count=$((count + 1))
done

# Read from file
while IFS= read -r line; do
    echo "$line"
done < file.txt

# Infinite loop with break
while true; do
    read -p "Enter 'quit' to exit: " input
    if [ "$input" = "quit" ]; then
        break
    fi
done
```

## Loop Control

### Break and Continue

#### Python
```python
# Break - exit loop
for i in range(10):
    if i == 5:
        break
    print(i)

# Continue - skip iteration
for i in range(10):
    if i % 2 == 0:
        continue
    print(i)  # Only odd numbers

# Nested loops with labels (Python doesn't have labels, use flags)
found = False
for i in range(5):
    for j in range(5):
        if i * j == 12:
            found = True
            break
    if found:
        break
```

#### Java
```java
// Break - exit loop
for (int i = 0; i < 10; i++) {
    if (i == 5) {
        break;
    }
    System.out.println(i);
}

// Continue - skip iteration
for (int i = 0; i < 10; i++) {
    if (i % 2 == 0) {
        continue;
    }
    System.out.println(i);  // Only odd numbers
}

// Labeled break/continue
outer: for (int i = 0; i < 5; i++) {
    for (int j = 0; j < 5; j++) {
        if (i * j == 12) {
            break outer;
        }
    }
}
```

#### Bash
```bash
#!/bin/bash

# Break - exit loop
for i in {0..9}; do
    if [ $i -eq 5 ]; then
        break
    fi
    echo $i
done

# Continue - skip iteration
for i in {0..9}; do
    if [ $((i % 2)) -eq 0 ]; then
        continue
    fi
    echo $i  # Only odd numbers
done
```

## Summary

| Feature | Python | Java | Bash |
|---------|--------|------|------|
| **If Syntax** | Indentation-based | Braces | `if-then-fi` |
| **Ternary** | `x if cond else y` | `cond ? x : y` | `[ cond ] && x \|\| y` |
| **Switch** | `match-case` (3.10+) | `switch` | `case-esac` |
| **For Loop** | `for x in iterable` | `for (init; cond; inc)` | `for x in list` |
| **While Loop** | `while condition:` | `while (condition)` | `while [ condition ]` |
| **Break/Continue** | Yes | Yes | Yes |
| **Labels** | No (use flags) | Yes | No |

---

**Next Chapter**: [Functions and Methods](./04-functions-methods.md)

