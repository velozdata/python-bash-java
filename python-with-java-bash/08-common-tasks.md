# Chapter 8: Common Tasks and Use Cases

This chapter provides practical examples of common programming tasks, comparing implementations in Python, Java, and Bash.

## Working with Lists/Arrays

### Python
```python
# Create list
numbers = [1, 2, 3, 4, 5]
fruits = ["apple", "banana", "cherry"]

# Add elements
numbers.append(6)
numbers.insert(0, 0)
numbers.extend([7, 8, 9])

# Remove elements
numbers.remove(3)
popped = numbers.pop()
del numbers[0]

# Access elements
first = numbers[0]
last = numbers[-1]
slice = numbers[1:4]

# List comprehension
squares = [x**2 for x in range(10)]
evens = [x for x in range(10) if x % 2 == 0]

# Filter and map
filtered = list(filter(lambda x: x > 5, numbers))
mapped = list(map(lambda x: x * 2, numbers))

# Sort
numbers.sort()
sorted_numbers = sorted(numbers, reverse=True)
```

### Java
```java
import java.util.*;

// Create list
List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
List<String> fruits = new ArrayList<>(Arrays.asList("apple", "banana", "cherry"));

// Add elements
numbers.add(6);
numbers.add(0, 0);
numbers.addAll(Arrays.asList(7, 8, 9));

// Remove elements
numbers.remove(Integer.valueOf(3));
int popped = numbers.remove(numbers.size() - 1);
numbers.remove(0);

// Access elements
int first = numbers.get(0);
int last = numbers.get(numbers.size() - 1);
List<Integer> slice = numbers.subList(1, 4);

// Stream API (Java 8+)
List<Integer> squares = IntStream.range(0, 10)
    .mapToObj(x -> x * x)
    .collect(Collectors.toList());

List<Integer> evens = IntStream.range(0, 10)
    .filter(x -> x % 2 == 0)
    .boxed()
    .collect(Collectors.toList());

// Filter and map
List<Integer> filtered = numbers.stream()
    .filter(x -> x > 5)
    .collect(Collectors.toList());

List<Integer> mapped = numbers.stream()
    .map(x -> x * 2)
    .collect(Collectors.toList());

// Sort
numbers.sort(Comparator.naturalOrder());
List<Integer> sorted = numbers.stream()
    .sorted(Comparator.reverseOrder())
    .collect(Collectors.toList());
```

### Bash
```bash
#!/bin/bash

# Create array
numbers=(1 2 3 4 5)
fruits=("apple" "banana" "cherry")

# Add elements
numbers+=(6)
numbers=("0" "${numbers[@]}")

# Remove elements (not directly supported, use array manipulation)
unset numbers[2]  # Remove index 2
numbers=("${numbers[@]}")  # Reindex

# Access elements
first=${numbers[0]}
last=${numbers[-1]}

# Iterate
for num in "${numbers[@]}"; do
    echo "$num"
done

# Filter (using loop)
evens=()
for num in "${numbers[@]}"; do
    if [ $((num % 2)) -eq 0 ]; then
        evens+=("$num")
    fi
done

# Sort (external command)
sorted=($(printf '%s\n' "${numbers[@]}" | sort -n))
```

## Working with Dictionaries/Maps

### Python
```python
# Create dictionary
person = {"name": "John", "age": 30, "city": "NYC"}
person = dict(name="John", age=30, city="NYC")

# Access values
name = person["name"]
age = person.get("age", 0)  # With default

# Add/update
person["email"] = "john@example.com"
person.update({"phone": "123-456-7890"})

# Remove
del person["city"]
email = person.pop("email", None)

# Iterate
for key, value in person.items():
    print(f"{key}: {value}")

# Dictionary comprehension
squares = {x: x**2 for x in range(10)}
```

### Java
```java
import java.util.*;

// Create map
Map<String, Object> person = new HashMap<>();
person.put("name", "John");
person.put("age", 30);
person.put("city", "NYC");

// Access values
String name = (String) person.get("name");
int age = person.containsKey("age") ? (Integer) person.get("age") : 0;

// Add/update
person.put("email", "john@example.com");
person.putAll(Map.of("phone", "123-456-7890"));

// Remove
person.remove("city");
String email = (String) person.remove("email");

// Iterate
for (Map.Entry<String, Object> entry : person.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}

// Java 8+ forEach
person.forEach((key, value) -> 
    System.out.println(key + ": " + value));
```

### Bash
```bash
#!/bin/bash

# Associative arrays (Bash 4+)
declare -A person
person["name"]="John"
person["age"]="30"
person["city"]="NYC"

# Access values
name=${person["name"]}
age=${person["age"]:-0}  # With default

# Add/update
person["email"]="john@example.com"

# Remove
unset person["city"]

# Iterate
for key in "${!person[@]}"; do
    echo "$key: ${person[$key]}"
done
```

## String Manipulation

### Python
```python
# Concatenation
full = "Hello" + " " + "World"
full = f"Hello {name}"

# Split and join
parts = "apple,banana,cherry".split(",")
joined = ",".join(parts)

# Replace
text = "Hello World".replace("World", "Python")

# Strip whitespace
text = "  hello  ".strip()

# Case conversion
upper = "hello".upper()
lower = "HELLO".lower()

# Find and replace
import re
text = re.sub(r'\d+', 'NUMBER', 'I have 5 apples')

# Format
formatted = "Name: {}, Age: {}".format("John", 30)
formatted = f"Name: {name}, Age: {age}"
```

### Java
```java
// Concatenation
String full = "Hello" + " " + "World";
String fullTemplate = String.format("Hello %s", name);

// Split and join
String[] parts = "apple,banana,cherry".split(",");
String joined = String.join(",", parts);

// Replace
String text = "Hello World".replace("World", "Java");

// Strip whitespace (Java 11+)
String text = "  hello  ".strip();

// Case conversion
String upper = "hello".toUpperCase();
String lower = "HELLO".toLowerCase();

// Find and replace (regex)
String text = "I have 5 apples".replaceAll("\\d+", "NUMBER");

// Format
String formatted = String.format("Name: %s, Age: %d", "John", 30);
```

### Bash
```bash
#!/bin/bash

# Concatenation
full="Hello $name"
full="Hello ${name}"

# Split
IFS=',' read -ra parts <<< "apple,banana,cherry"
joined=$(IFS=','; echo "${parts[*]}")

# Replace
text="Hello World"
text="${text/World/Bash}"

# Strip whitespace
text="  hello  "
text="${text#"${text%%[![:space:]]*}"}"  # Left trim
text="${text%"${text##*[![:space:]]}"}"  # Right trim

# Case conversion (Bash 4+)
text="hello"
upper="${text^^}"
lower="${text,,}"

# Find and replace (sed)
text=$(echo "I have 5 apples" | sed 's/[0-9]\+/NUMBER/g')

# Format
formatted="Name: $name, Age: $age"
```

## Working with Dates and Time

### Python
```python
from datetime import datetime, timedelta

# Current time
now = datetime.now()

# Format date
formatted = now.strftime("%Y-%m-%d %H:%M:%S")

# Parse date
date_obj = datetime.strptime("2024-01-15", "%Y-%m-%d")

# Add/subtract time
future = now + timedelta(days=7)
past = now - timedelta(hours=2)

# Time difference
diff = future - past
days = diff.days
```

### Java
```java
import java.time.*;
import java.time.format.DateTimeFormatter;

// Current time
LocalDateTime now = LocalDateTime.now();

// Format date
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
String formatted = now.format(formatter);

// Parse date
LocalDate date = LocalDate.parse("2024-01-15");

// Add/subtract time
LocalDateTime future = now.plusDays(7);
LocalDateTime past = now.minusHours(2);

// Time difference
Duration diff = Duration.between(past, future);
long days = diff.toDays();
```

### Bash
```bash
#!/bin/bash

# Current time
now=$(date)

# Format date
formatted=$(date "+%Y-%m-%d %H:%M:%S")

# Parse date (limited)
date_obj=$(date -d "2024-01-15" "+%Y-%m-%d")

# Add/subtract time
future=$(date -d "+7 days" "+%Y-%m-%d")
past=$(date -d "-2 hours" "+%Y-%m-%d %H:%M:%S")

# Time difference (in seconds)
date1=$(date -d "2024-01-01" +%s)
date2=$(date -d "2024-01-15" +%s)
diff=$((date2 - date1))
days=$((diff / 86400))
```

## Command-Line Arguments

### Python
```python
import sys

# Basic arguments
args = sys.argv
script_name = args[0]
first_arg = args[1] if len(args) > 1 else None

# Using argparse (recommended)
import argparse

parser = argparse.ArgumentParser(description='Process some integers.')
parser.add_argument('integers', metavar='N', type=int, nargs='+',
                    help='an integer for the accumulator')
parser.add_argument('--sum', dest='accumulate', action='store_const',
                    const=sum, default=max,
                    help='sum the integers (default: find the max)')

args = parser.parse_args()
print(args.accumulate(args.integers))
```

### Java
```java
// Basic arguments
public class Main {
    public static void main(String[] args) {
        String scriptName = args.length > 0 ? args[0] : "";
        String firstArg = args.length > 1 ? args[1] : null;
    }
}

// Using Apache Commons CLI or similar library
// Example with manual parsing
public class ArgsParser {
    public static void main(String[] args) {
        for (int i = 0; i < args.length; i++) {
            if (args[i].equals("--sum")) {
                // Handle --sum flag
            } else if (args[i].startsWith("--")) {
                // Handle other flags
            }
        }
    }
}
```

### Bash
```bash
#!/bin/bash

# Basic arguments
script_name=$0
first_arg=$1
second_arg=$2
all_args="$@"
arg_count=$#

# Process arguments
while [[ $# -gt 0 ]]; do
    case $1 in
        -h|--help)
            echo "Usage: $0 [options]"
            exit 0
            ;;
        -v|--verbose)
            verbose=true
            shift
            ;;
        *)
            positional_args+=("$1")
            shift
            ;;
    esac
done

# Using getopts
while getopts "hv:" opt; do
    case $opt in
        h)
            echo "Help"
            ;;
        v)
            verbose=$OPTARG
            ;;
        \?)
            echo "Invalid option"
            ;;
    esac
done
```

## HTTP Requests

### Python
```python
import requests

# GET request
response = requests.get("https://api.example.com/data")
data = response.json()

# POST request
response = requests.post("https://api.example.com/data",
                        json={"key": "value"})

# With headers
headers = {"Authorization": "Bearer token"}
response = requests.get("https://api.example.com/data", headers=headers)
```

### Java
```java
import java.net.http.*;
import java.net.URI;

// GET request (Java 11+)
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.example.com/data"))
    .build();
HttpResponse<String> response = client.send(request, 
    HttpResponse.BodyHandlers.ofString());

// POST request
String json = "{\"key\":\"value\"}";
HttpRequest postRequest = HttpRequest.newBuilder()
    .uri(URI.create("https://api.example.com/data"))
    .header("Content-Type", "application/json")
    .POST(HttpRequest.BodyPublishers.ofString(json))
    .build();
```

### Bash
```bash
#!/bin/bash

# GET request (using curl)
response=$(curl -s "https://api.example.com/data")
echo "$response"

# POST request
curl -X POST "https://api.example.com/data" \
     -H "Content-Type: application/json" \
     -d '{"key":"value"}'

# With headers
curl -H "Authorization: Bearer token" \
     "https://api.example.com/data"

# Using wget
wget -qO- "https://api.example.com/data"
```

## Summary

Each language has its strengths for common tasks:

- **Python**: Excellent for data manipulation, string processing, and rapid development
- **Java**: Strong for complex data structures, type safety, and enterprise applications
- **Bash**: Best for system operations, file processing, and command-line automation

Choose the language that best fits your specific use case and environment.

---

**Next Chapter**: [Best Practices](./09-best-practices.md)

