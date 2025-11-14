# Chapter 6: Error Handling

Error handling is crucial for writing robust applications. This chapter compares exception handling mechanisms in Python, Java, and Bash.

## Basic Exception Handling

### Python
```python
# Try-except block
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")

# Multiple exceptions
try:
    value = int("abc")
    result = 10 / value
except ValueError:
    print("Invalid number")
except ZeroDivisionError:
    print("Cannot divide by zero")

# Catch all exceptions
try:
    risky_operation()
except Exception as e:
    print(f"Error occurred: {e}")

# Else clause (executes if no exception)
try:
    result = 10 / 2
except ZeroDivisionError:
    print("Division by zero")
else:
    print(f"Result: {result}")

# Finally clause (always executes)
try:
    file = open('file.txt', 'r')
    content = file.read()
except FileNotFoundError:
    print("File not found")
finally:
    file.close()  # Always executed
```

### Java
```java
// Try-catch block
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero");
}

// Multiple catch blocks
try {
    int value = Integer.parseInt("abc");
    int result = 10 / value;
} catch (NumberFormatException e) {
    System.out.println("Invalid number");
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero");
}

// Catch multiple exceptions (Java 7+)
try {
    riskyOperation();
} catch (IllegalArgumentException | NullPointerException e) {
    System.out.println("Error: " + e.getMessage());
}

// Finally block (always executes)
FileReader file = null;
try {
    file = new FileReader("file.txt");
    // read file
} catch (FileNotFoundException e) {
    System.out.println("File not found");
} finally {
    if (file != null) {
        try {
            file.close();
        } catch (IOException e) {
            // handle close error
        }
    }
}

// Try-with-resources (Java 7+)
try (FileReader file = new FileReader("file.txt")) {
    // read file
    // file automatically closed
} catch (FileNotFoundException e) {
    System.out.println("File not found");
}
```

### Bash
```bash
#!/bin/bash

# Basic error handling
set -e  # Exit on error
set -o errexit  # Same as above

# Check exit code
command_that_might_fail || echo "Command failed"

# If statement for error checking
if ! command_that_might_fail; then
    echo "Command failed"
fi

# Trap errors
trap 'echo "Error occurred at line $LINENO"' ERR

# Trap exit
trap 'cleanup_function' EXIT

# Check if command exists
if command -v some_command &> /dev/null; then
    some_command
else
    echo "Command not found"
fi

# Check file operations
if [ ! -f "file.txt" ]; then
    echo "File not found"
    exit 1
fi

# Error handling function
handle_error() {
    echo "Error: $1"
    exit 1
}

# Usage
some_command || handle_error "Command failed"
```

## Custom Exceptions

### Python
```python
# Define custom exception
class CustomError(Exception):
    pass

class ValidationError(Exception):
    def __init__(self, message, value):
        self.message = message
        self.value = value
        super().__init__(f"{message}: {value}")

# Raise exception
def validate_age(age):
    if age < 0:
        raise ValueError("Age cannot be negative")
    if age > 150:
        raise ValueError("Age seems unrealistic")
    return age

# Use custom exception
def process_data(data):
    if not data:
        raise CustomError("Data is required")
    # process data

# Catch and re-raise
try:
    validate_age(-5)
except ValueError as e:
    print(f"Validation failed: {e}")
    raise  # Re-raise the exception
```

### Java
```java
// Define custom exception
class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
}

class ValidationException extends Exception {
    private int value;
    
    public ValidationException(String message, int value) {
        super(message + ": " + value);
        this.value = value;
    }
    
    public int getValue() {
        return value;
    }
}

// Throw exception
public void validateAge(int age) throws ValidationException {
    if (age < 0) {
        throw new ValidationException("Age cannot be negative", age);
    }
    if (age > 150) {
        throw new ValidationException("Age seems unrealistic", age);
    }
}

// Use custom exception
public void processData(String data) throws CustomException {
    if (data == null || data.isEmpty()) {
        throw new CustomException("Data is required");
    }
    // process data
}

// Catch and re-throw
try {
    validateAge(-5);
} catch (ValidationException e) {
    System.out.println("Validation failed: " + e.getMessage());
    throw;  // Re-throw
}
```

### Bash
```bash
#!/bin/bash

# Custom error function
error_exit() {
    echo "$1" >&2
    exit 1
}

# Usage
[ -z "$VAR" ] && error_exit "Variable is empty"

# Error codes
ERROR_FILE_NOT_FOUND=1
ERROR_PERMISSION_DENIED=2

check_file() {
    if [ ! -f "$1" ]; then
        echo "File not found: $1" >&2
        return $ERROR_FILE_NOT_FOUND
    fi
    if [ ! -r "$1" ]; then
        echo "Permission denied: $1" >&2
        return $ERROR_PERMISSION_DENIED
    fi
    return 0
}

# Use error codes
check_file "file.txt"
case $? in
    $ERROR_FILE_NOT_FOUND)
        echo "Handling file not found"
        ;;
    $ERROR_PERMISSION_DENIED)
        echo "Handling permission denied"
        ;;
    0)
        echo "File is OK"
        ;;
esac
```

## Exception Propagation

### Python
```python
# Exceptions propagate automatically
def inner_function():
    raise ValueError("Error in inner function")

def outer_function():
    inner_function()  # Exception propagates up

def caller():
    try:
        outer_function()
    except ValueError as e:
        print(f"Caught: {e}")

# Explicit propagation
def might_fail():
    try:
        risky_operation()
    except SomeException:
        print("Handled locally")
        raise  # Re-raise to caller
```

### Java
```java
// Exceptions propagate (checked exceptions must be declared)
public void innerFunction() throws IOException {
    throw new IOException("Error in inner function");
}

public void outerFunction() throws IOException {
    innerFunction();  // Exception propagates up
}

public void caller() {
    try {
        outerFunction();
    } catch (IOException e) {
        System.out.println("Caught: " + e.getMessage());
    }
}

// Unchecked exceptions propagate without declaration
public void innerFunction() {
    throw new RuntimeException("Unchecked exception");
}

public void outerFunction() {
    innerFunction();  // Propagates automatically
}
```

### Bash
```bash
#!/bin/bash

# Exit codes propagate
inner_function() {
    return 1  # Error
}

outer_function() {
    inner_function
    return $?  # Propagate exit code
}

caller() {
    if outer_function; then
        echo "Success"
    else
        echo "Failed with code: $?"
    fi
}

# Set error propagation
set -e  # Exit on any error
set -o pipefail  # Exit on pipe errors
```

## Assertions

### Python
```python
# Assertions for debugging
def divide(a, b):
    assert b != 0, "Divisor cannot be zero"
    return a / b

# Assert with custom message
assert x > 0, f"x must be positive, got {x}"

# Disable assertions in production (use -O flag)
# python -O script.py
```

### Java
```java
// Assertions (disabled by default, enable with -ea)
public int divide(int a, int b) {
    assert b != 0 : "Divisor cannot be zero";
    return a / b;
}

// Assert with message
assert x > 0 : "x must be positive, got " + x;

// Enable assertions: java -ea MyClass
```

### Bash
```bash
#!/bin/bash

# Assertion function
assert() {
    if [ ! "$1" ]; then
        echo "Assertion failed: $2" >&2
        exit 1
    fi
}

# Usage
assert [ "$b" -ne 0 ] "Divisor cannot be zero"
result=$((a / b))

# Simple assertion
[ "$b" -ne 0 ] || { echo "Divisor cannot be zero" >&2; exit 1; }
```

## Logging Errors

### Python
```python
import logging

# Configure logging
logging.basicConfig(
    level=logging.ERROR,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

try:
    risky_operation()
except Exception as e:
    logging.error(f"Operation failed: {e}", exc_info=True)
    # or
    logging.exception("Operation failed")  # Includes traceback
```

### Java
```java
import java.util.logging.Logger;
import java.util.logging.Level;

// Get logger
private static final Logger logger = Logger.getLogger(MyClass.class.getName());

try {
    riskyOperation();
} catch (Exception e) {
    logger.log(Level.SEVERE, "Operation failed", e);
    // or
    logger.severe("Operation failed: " + e.getMessage());
}
```

### Bash
```bash
#!/bin/bash

# Logging function
log_error() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] ERROR: $1" >&2
}

# Usage
if ! some_command; then
    log_error "Command failed: some_command"
    exit 1
fi

# Log to file
log_file="error.log"
echo "[$(date)] ERROR: $1" >> "$log_file"
```

## Best Practices

### Python
```python
# Specific exceptions
try:
    file = open('file.txt')
except FileNotFoundError:  # Specific
    pass
except IOError:  # More general
    pass

# Don't catch all exceptions unless necessary
try:
    risky_operation()
except (ValueError, TypeError):  # Specific exceptions
    handle_error()
except Exception as e:  # Last resort
    log_error(e)
    raise

# Use context managers for resources
with open('file.txt') as f:
    content = f.read()
# File automatically closed
```

### Java
```java
// Specific exceptions first
try {
    file = new FileReader("file.txt");
} catch (FileNotFoundException e) {  // Specific
    // handle
} catch (IOException e) {  // More general
    // handle
}

// Use try-with-resources
try (FileReader file = new FileReader("file.txt")) {
    // use file
} catch (IOException e) {
    // handle
}

// Don't catch and ignore
try {
    riskyOperation();
} catch (Exception e) {
    logger.error("Error", e);  // Log it
    // Don't silently ignore
}
```

### Bash
```bash
#!/bin/bash

# Always check exit codes
if ! command; then
    echo "Error: command failed" >&2
    exit 1
fi

# Use set -e for scripts
set -e
set -o pipefail  # Fail on pipe errors

# Clean up on exit
cleanup() {
    rm -f temp_file
}
trap cleanup EXIT

# Validate inputs
if [ -z "$1" ]; then
    echo "Error: argument required" >&2
    exit 1
fi
```

## Summary

| Feature | Python | Java | Bash |
|---------|--------|------|------|
| **Try-Catch** | `try-except` | `try-catch` | `if` or `set -e` |
| **Finally** | `finally` | `finally` | `trap EXIT` |
| **Custom Exceptions** | Class inheritance | Class inheritance | Error codes/functions |
| **Propagation** | Automatic | Automatic (checked declared) | Exit codes |
| **Assertions** | `assert` | `assert` | Custom function |
| **Logging** | `logging` module | `java.util.logging` | Custom functions |

---

**Next Chapter**: [Object-Oriented Programming](./07-object-oriented-programming.md)

