# Chapter 9: Best Practices

This chapter outlines best practices and coding conventions for Python, Java, and Bash to help you write maintainable, efficient, and readable code.

## Python Best Practices

### Code Style
```python
# Follow PEP 8 style guide
# Use 4 spaces for indentation (not tabs)
# Maximum line length: 79 characters (or 99 for comments)

# Naming conventions
CONSTANT_NAME = "value"           # Constants: UPPER_SNAKE_CASE
variable_name = "value"           # Variables: snake_case
function_name()                   # Functions: snake_case
ClassName                        # Classes: PascalCase
_private_var                     # Private: leading underscore

# Import organization
import os                         # Standard library
import sys

import requests                   # Third-party
import numpy

from mymodule import something    # Local imports
```

### Documentation
```python
def calculate_area(radius: float) -> float:
    """
    Calculate the area of a circle.
    
    Args:
        radius: The radius of the circle in meters.
    
    Returns:
        The area of the circle in square meters.
    
    Raises:
        ValueError: If radius is negative.
    """
    if radius < 0:
        raise ValueError("Radius cannot be negative")
    return 3.14159 * radius ** 2
```

### Error Handling
```python
# Be specific with exceptions
try:
    result = risky_operation()
except ValueError as e:
    # Handle specific error
    logger.error(f"Invalid value: {e}")
except FileNotFoundError as e:
    # Handle file error
    logger.error(f"File not found: {e}")
except Exception as e:
    # Last resort - log and re-raise
    logger.exception("Unexpected error")
    raise

# Use context managers for resources
with open('file.txt', 'r') as f:
    content = f.read()
# File automatically closed
```

### List Comprehensions
```python
# Prefer list comprehensions over loops when appropriate
# Good
squares = [x**2 for x in range(10) if x % 2 == 0]

# Avoid overly complex comprehensions
# Bad
result = [x for x in [y for y in range(10) if y > 5] if x % 2 == 0]

# Good - break into steps
filtered = [y for y in range(10) if y > 5]
result = [x for x in filtered if x % 2 == 0]
```

### Type Hints
```python
from typing import List, Dict, Optional

def process_data(
    items: List[str],
    config: Optional[Dict[str, int]] = None
) -> Dict[str, int]:
    """Process items with optional configuration."""
    if config is None:
        config = {}
    # Process items
    return {"processed": len(items)}
```

## Java Best Practices

### Code Style
```java
// Follow Java Code Conventions
// Use 4 spaces for indentation
// Maximum line length: 80-120 characters

// Naming conventions
public static final String CONSTANT_NAME = "value";  // Constants: UPPER_SNAKE_CASE
private String variableName;                         // Variables: camelCase
public void methodName() {}                          // Methods: camelCase
public class ClassName {}                            // Classes: PascalCase
private String privateField;                        // Private: camelCase

// Package naming: lowercase, reverse domain
package com.example.myproject;
```

### Documentation
```java
/**
 * Calculates the area of a circle.
 * 
 * @param radius The radius of the circle in meters
 * @return The area of the circle in square meters
 * @throws IllegalArgumentException if radius is negative
 */
public double calculateArea(double radius) {
    if (radius < 0) {
        throw new IllegalArgumentException("Radius cannot be negative");
    }
    return Math.PI * radius * radius;
}
```

### Access Modifiers
```java
// Use appropriate access modifiers
public class MyClass {
    // Public: accessible everywhere
    public void publicMethod() {}
    
    // Protected: accessible in package and subclasses
    protected void protectedMethod() {}
    
    // Package-private: accessible in same package
    void packagePrivateMethod() {}
    
    // Private: accessible only in this class
    private void privateMethod() {}
}

// Prefer private, expose through public methods
private int age;

public int getAge() {
    return age;
}

public void setAge(int age) {
    if (age > 0) {
        this.age = age;
    }
}
```

### Resource Management
```java
// Always use try-with-resources
try (FileReader file = new FileReader("file.txt");
     BufferedReader reader = new BufferedReader(file)) {
    // Use resources
    // Automatically closed
} catch (IOException e) {
    // Handle exception
}

// Don't ignore exceptions
try {
    riskyOperation();
} catch (Exception e) {
    // Log it at minimum
    logger.error("Operation failed", e);
    // Don't silently swallow
}
```

### Use Modern Java Features
```java
// Use Stream API (Java 8+)
List<Integer> evens = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());

// Use Optional for nullable values
Optional<String> name = Optional.ofNullable(getName());
String result = name.orElse("Unknown");

// Use var for local variables (Java 10+)
var list = new ArrayList<String>();
```

## Bash Best Practices

### Shebang and Options
```bash
#!/bin/bash
# Always include shebang

# Set options for safer scripts
set -e          # Exit on error
set -u          # Exit on undefined variable
set -o pipefail # Exit on pipe failure

# Or combine
set -euo pipefail
```

### Quoting
```bash
# Always quote variables
name="John Doe"
echo "$name"        # Good
echo $name          # Bad (breaks with spaces)

# Quote in conditionals
if [ "$var" = "value" ]; then
    echo "Match"
fi

# Quote array expansions
for item in "${array[@]}"; do
    echo "$item"
done
```

### Error Handling
```bash
#!/bin/bash
set -euo pipefail

# Check if command exists
if ! command -v some_command &> /dev/null; then
    echo "Error: some_command not found" >&2
    exit 1
fi

# Check file operations
if [ ! -f "file.txt" ]; then
    echo "Error: file.txt not found" >&2
    exit 1
fi

# Use functions for error handling
error_exit() {
    echo "$1" >&2
    exit 1
}

# Usage
some_command || error_exit "Command failed"
```

### Functions
```bash
#!/bin/bash

# Use local variables in functions
my_function() {
    local local_var="value"
    # Use local_var
}

# Return values via echo, exit codes, or global variables
get_value() {
    echo "result"
}

result=$(get_value)

# Use meaningful function names
process_file() {
    # Process file
}

# Not: pf() { ... }
```

### Variable Naming
```bash
#!/bin/bash

# Use descriptive names
file_path="/path/to/file"
user_count=10

# Not: fp, uc

# Constants in UPPERCASE
readonly MAX_SIZE=100
readonly DEFAULT_PORT=8080

# Use readonly for constants
readonly COMPANY_NAME="MyCompany"
```

### Comments
```bash
#!/bin/bash

# Use comments to explain why, not what
# Calculate total by summing all values
total=$((value1 + value2 + value3))

# Bad comment (obvious)
# Add value1 and value2
total=$((value1 + value2))
```

## General Best Practices (All Languages)

### Version Control
- Use meaningful commit messages
- Don't commit sensitive data (passwords, API keys)
- Use `.gitignore` appropriately
- Keep commits focused and atomic

### Testing
```python
# Python - Use unittest or pytest
def test_calculate_area():
    assert calculate_area(5) == 78.53975
    assert calculate_area(0) == 0
```

```java
// Java - Use JUnit
@Test
public void testCalculateArea() {
    assertEquals(78.53975, calculateArea(5), 0.0001);
    assertEquals(0, calculateArea(0), 0.0001);
}
```

```bash
# Bash - Use test framework or manual tests
test_calculate_area() {
    result=$(calculate_area 5)
    if [ "$result" = "78.53975" ]; then
        echo "Test passed"
    else
        echo "Test failed"
        exit 1
    fi
}
```

### Code Organization
- Keep functions/methods small and focused
- Follow Single Responsibility Principle
- Avoid deep nesting (max 3-4 levels)
- Use meaningful variable and function names
- Remove dead code and commented-out code

### Performance
- Profile before optimizing
- Use appropriate data structures
- Avoid premature optimization
- Cache expensive operations when appropriate

### Security
- Validate all inputs
- Sanitize user input
- Use parameterized queries (for databases)
- Don't hardcode secrets
- Keep dependencies updated

## Language-Specific Recommendations

### Python
- Use virtual environments (`venv`)
- Manage dependencies with `requirements.txt`
- Use type hints for better IDE support
- Prefer `pathlib` over `os.path`
- Use `f-strings` for string formatting (Python 3.6+)

### Java
- Use Maven or Gradle for dependency management
- Follow SOLID principles
- Use design patterns appropriately
- Prefer composition over inheritance
- Keep methods under 50 lines

### Bash
- Keep scripts under 100 lines (consider Python for longer scripts)
- Use `bash -n script.sh` to check syntax
- Use `shellcheck` for linting
- Prefer functions over repeated code
- Document complex logic

## Summary

Following best practices helps create:
- **Maintainable** code that's easy to understand and modify
- **Reliable** code that handles errors gracefully
- **Efficient** code that performs well
- **Secure** code that protects against vulnerabilities
- **Readable** code that other developers can understand

Remember: **Code is read more often than it's written**. Write for your future self and your teammates.

---

**End of Book**

Thank you for reading! We hope this comparative guide helps you understand the strengths and use cases of Python, Java, and Bash.

