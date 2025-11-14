# Chapter 5: File Operations

File operations are essential for reading, writing, and manipulating files. This chapter compares how Python, Java, and Bash handle file I/O operations.

## Reading Files

### Python
```python
# Read entire file
with open('file.txt', 'r') as f:
    content = f.read()

# Read line by line
with open('file.txt', 'r') as f:
    for line in f:
        print(line.strip())

# Read all lines into list
with open('file.txt', 'r') as f:
    lines = f.readlines()

# Read first N bytes
with open('file.txt', 'r') as f:
    chunk = f.read(100)

# Read with encoding
with open('file.txt', 'r', encoding='utf-8') as f:
    content = f.read()

# Check if file exists
import os
if os.path.exists('file.txt'):
    print("File exists")

# Check file properties
import os
size = os.path.getsize('file.txt')
is_file = os.path.isfile('file.txt')
is_dir = os.path.isdir('path')
```

### Java
```java
import java.io.*;
import java.nio.file.*;

// Read entire file (Java 11+)
String content = Files.readString(Paths.get("file.txt"));

// Read all lines
List<String> lines = Files.readAllLines(Paths.get("file.txt"));

// Read line by line (traditional)
try (BufferedReader br = Files.newBufferedReader(Paths.get("file.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
}

// Read with Scanner
try (Scanner scanner = new Scanner(new File("file.txt"))) {
    while (scanner.hasNextLine()) {
        String line = scanner.nextLine();
        System.out.println(line);
    }
}

// Read with encoding
String content = Files.readString(
    Paths.get("file.txt"),
    StandardCharsets.UTF_8
);

// Check if file exists
if (Files.exists(Paths.get("file.txt"))) {
    System.out.println("File exists");
}

// Check file properties
long size = Files.size(Paths.get("file.txt"));
boolean isFile = Files.isRegularFile(Paths.get("file.txt"));
boolean isDir = Files.isDirectory(Paths.get("path"));
```

### Bash
```bash
#!/bin/bash

# Read entire file
content=$(cat file.txt)

# Read line by line
while IFS= read -r line; do
    echo "$line"
done < file.txt

# Read all lines into array
mapfile -t lines < file.txt
# or
readarray -t lines < file.txt

# Read first N bytes
head -c 100 file.txt

# Read first N lines
head -n 10 file.txt

# Read last N lines
tail -n 10 file.txt

# Check if file exists
if [ -f "file.txt" ]; then
    echo "File exists"
fi

# Check file properties
if [ -r "file.txt" ]; then
    echo "File is readable"
fi

if [ -w "file.txt" ]; then
    echo "File is writable"
fi

if [ -x "file.txt" ]; then
    echo "File is executable"
fi

# Get file size
size=$(stat -f%z file.txt)  # macOS
size=$(stat -c%s file.txt)  # Linux
```

## Writing Files

### Python
```python
# Write entire file
with open('file.txt', 'w') as f:
    f.write("Hello, World!")

# Write multiple lines
lines = ["Line 1", "Line 2", "Line 3"]
with open('file.txt', 'w') as f:
    f.write('\n'.join(lines))

# Append to file
with open('file.txt', 'a') as f:
    f.write("New line\n")

# Write with encoding
with open('file.txt', 'w', encoding='utf-8') as f:
    f.write("Hello, World!")

# Write binary file
with open('file.bin', 'wb') as f:
    f.write(b'\x00\x01\x02\x03')
```

### Java
```java
import java.io.*;
import java.nio.file.*;

// Write entire file (Java 11+)
Files.writeString(Paths.get("file.txt"), "Hello, World!");

// Write multiple lines
List<String> lines = Arrays.asList("Line 1", "Line 2", "Line 3");
Files.write(Paths.get("file.txt"), lines);

// Append to file
Files.writeString(
    Paths.get("file.txt"),
    "New line\n",
    StandardOpenOption.APPEND
);

// Write with encoding
Files.writeString(
    Paths.get("file.txt"),
    "Hello, World!",
    StandardCharsets.UTF_8
);

// Write binary file
byte[] data = {0x00, 0x01, 0x02, 0x03};
Files.write(Paths.get("file.bin"), data);

// Traditional approach
try (PrintWriter pw = new PrintWriter("file.txt")) {
    pw.println("Hello, World!");
}
```

### Bash
```bash
#!/bin/bash

# Write entire file
echo "Hello, World!" > file.txt

# Write multiple lines
cat > file.txt << EOF
Line 1
Line 2
Line 3
EOF

# Append to file
echo "New line" >> file.txt

# Write with here-document
cat >> file.txt << EOF
Additional line 1
Additional line 2
EOF

# Write binary (using printf)
printf '\x00\x01\x02\x03' > file.bin
```

## File Manipulation

### Python
```python
import os
import shutil

# Copy file
shutil.copy('source.txt', 'dest.txt')

# Move/rename file
os.rename('old.txt', 'new.txt')
# or
shutil.move('old.txt', 'new.txt')

# Delete file
os.remove('file.txt')

# Create directory
os.makedirs('newdir', exist_ok=True)

# Delete directory
os.rmdir('emptydir')
shutil.rmtree('dirwithfiles')  # Recursive

# List directory
files = os.listdir('.')
for file in files:
    print(file)

# Walk directory tree
for root, dirs, files in os.walk('.'):
    for file in files:
        print(os.path.join(root, file))
```

### Java
```java
import java.io.*;
import java.nio.file.*;

// Copy file
Files.copy(
    Paths.get("source.txt"),
    Paths.get("dest.txt"),
    StandardCopyOption.REPLACE_EXISTING
);

// Move/rename file
Files.move(
    Paths.get("old.txt"),
    Paths.get("new.txt"),
    StandardCopyOption.REPLACE_EXISTING
);

// Delete file
Files.delete(Paths.get("file.txt"));

// Create directory
Files.createDirectories(Paths.get("newdir"));

// Delete directory
Files.delete(Paths.get("emptydir"));
Files.walkFileTree(Paths.get("dirwithfiles"), 
    new SimpleFileVisitor<Path>() {
        @Override
        public FileVisitResult visitFile(Path file, 
                BasicFileAttributes attrs) throws IOException {
            Files.delete(file);
            return FileVisitResult.CONTINUE;
        }
        @Override
        public FileVisitResult postVisitDirectory(Path dir, 
                IOException exc) throws IOException {
            Files.delete(dir);
            return FileVisitResult.CONTINUE;
        }
    });

// List directory
try (Stream<Path> paths = Files.list(Paths.get("."))) {
    paths.forEach(System.out::println);
}

// Walk directory tree
try (Stream<Path> paths = Files.walk(Paths.get("."))) {
    paths.forEach(System.out::println);
}
```

### Bash
```bash
#!/bin/bash

# Copy file
cp source.txt dest.txt

# Copy directory recursively
cp -r sourcedir destdir

# Move/rename file
mv old.txt new.txt

# Delete file
rm file.txt

# Delete directory
rmdir emptydir
rm -rf dirwithfiles  # Recursive

# Create directory
mkdir newdir
mkdir -p path/to/newdir  # Create parent directories

# List directory
ls -la

# List files only
ls -p | grep -v /

# Walk directory tree
find . -type f -print

# Find and process files
find . -name "*.txt" -exec echo {} \;
```

## File Path Operations

### Python
```python
import os
from pathlib import Path

# Join paths
path = os.path.join('dir', 'subdir', 'file.txt')
# or with pathlib
path = Path('dir') / 'subdir' / 'file.txt'

# Get directory name
dirname = os.path.dirname('/path/to/file.txt')

# Get filename
basename = os.path.basename('/path/to/file.txt')

# Get file extension
name, ext = os.path.splitext('file.txt')

# Get absolute path
abs_path = os.path.abspath('file.txt')

# Check if path exists
exists = os.path.exists('file.txt')

# Normalize path
normalized = os.path.normpath('dir/../file.txt')
```

### Java
```java
import java.nio.file.*;

// Join paths
Path path = Paths.get("dir", "subdir", "file.txt");

// Get directory name
Path dir = path.getParent();

// Get filename
String filename = path.getFileName().toString();

// Get file extension
String ext = "";
String name = path.getFileName().toString();
int dotIndex = name.lastIndexOf('.');
if (dotIndex > 0) {
    ext = name.substring(dotIndex);
}

// Get absolute path
Path absPath = path.toAbsolutePath();

// Check if path exists
boolean exists = Files.exists(path);

// Normalize path
Path normalized = path.normalize();
```

### Bash
```bash
#!/bin/bash

# Join paths
path="dir/subdir/file.txt"

# Get directory name
dirname "$path"

# Get filename
basename "$path"

# Get file extension
filename="file.txt"
extension="${filename##*.}"
name="${filename%.*}"

# Get absolute path
realpath file.txt
# or
readlink -f file.txt  # Linux

# Check if path exists
if [ -e "$path" ]; then
    echo "Path exists"
fi

# Normalize path (remove .. and .)
cd "$(dirname "$path")" && pwd
```

## Text Processing

### Python
```python
# Search and replace in file
with open('file.txt', 'r') as f:
    content = f.read()
    content = content.replace('old', 'new')

with open('file.txt', 'w') as f:
    f.write(content)

# Count lines
with open('file.txt', 'r') as f:
    line_count = sum(1 for _ in f)

# Filter lines
with open('file.txt', 'r') as f:
    filtered = [line for line in f if 'keyword' in line]
```

### Java
```java
// Search and replace in file
String content = Files.readString(Paths.get("file.txt"));
content = content.replace("old", "new");
Files.writeString(Paths.get("file.txt"), content);

// Count lines
long lineCount = Files.lines(Paths.get("file.txt")).count();

// Filter lines
List<String> filtered = Files.lines(Paths.get("file.txt"))
    .filter(line -> line.contains("keyword"))
    .collect(Collectors.toList());
```

### Bash
```bash
#!/bin/bash

# Search and replace in file
sed -i 's/old/new/g' file.txt

# Count lines
line_count=$(wc -l < file.txt)

# Filter lines
grep "keyword" file.txt

# Extract specific columns
cut -d',' -f1,3 file.csv

# Sort file
sort file.txt

# Remove duplicates
sort -u file.txt
```

## Summary

| Operation | Python | Java | Bash |
|-----------|--------|------|------|
| **Read File** | `open().read()` | `Files.readString()` | `cat` or `read` |
| **Write File** | `open().write()` | `Files.writeString()` | `echo >` or `cat >` |
| **Append** | `open('a')` | `APPEND` option | `>>` |
| **Copy** | `shutil.copy()` | `Files.copy()` | `cp` |
| **Move** | `os.rename()` | `Files.move()` | `mv` |
| **Delete** | `os.remove()` | `Files.delete()` | `rm` |
| **List Dir** | `os.listdir()` | `Files.list()` | `ls` |
| **Path Join** | `os.path.join()` | `Paths.get()` | String concat |

---

**Next Chapter**: [Error Handling](./06-error-handling.md)

