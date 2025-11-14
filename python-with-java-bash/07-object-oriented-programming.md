# Chapter 7: Object-Oriented Programming

Object-Oriented Programming (OOP) is a programming paradigm based on objects and classes. Python and Java both support OOP, while Bash is primarily procedural. This chapter focuses on Python and Java OOP features.

## Classes and Objects

### Python
```python
# Simple class
class Person:
    pass

# Create object
person = Person()

# Class with attributes
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

# Create object
person = Person("John", 30)
print(person.name)  # John
print(person.age)   # 30

# Class with methods
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def greet(self):
        return f"Hello, I'm {self.name}"
    
    def have_birthday(self):
        self.age += 1

person = Person("John", 30)
print(person.greet())  # Hello, I'm John
person.have_birthday()
print(person.age)  # 31
```

### Java
```java
// Simple class
public class Person {
}

// Create object
Person person = new Person();

// Class with attributes
public class Person {
    private String name;
    private int age;
    
    // Constructor
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // Getters
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
}

// Create object
Person person = new Person("John", 30);
System.out.println(person.getName());  // John
System.out.println(person.getAge());   // 30

// Class with methods
public class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String greet() {
        return "Hello, I'm " + name;
    }
    
    public void haveBirthday() {
        age++;
    }
    
    public int getAge() {
        return age;
    }
}
```

## Encapsulation

### Python
```python
# Public attributes (convention)
class Person:
    def __init__(self, name):
        self.name = name  # Public

# Private attributes (convention: _ or __)
class Person:
    def __init__(self, name):
        self._name = name      # Protected (convention)
        self.__age = 0         # Name mangling (weakly private)
    
    def get_name(self):
        return self._name
    
    def set_age(self, age):
        if age > 0:
            self.__age = age

# Properties (getters/setters)
class Person:
    def __init__(self, name):
        self._name = name
        self._age = 0
    
    @property
    def name(self):
        return self._name
    
    @name.setter
    def name(self, value):
        if isinstance(value, str) and len(value) > 0:
            self._name = value
    
    @property
    def age(self):
        return self._age
    
    @age.setter
    def age(self, value):
        if isinstance(value, int) and value > 0:
            self._age = value

person = Person("John")
person.age = 30
print(person.age)  # 30
```

### Java
```java
// Encapsulation with access modifiers
public class Person {
    private String name;  // Private
    private int age;      // Private
    
    public Person(String name) {
        this.name = name;
        this.age = 0;
    }
    
    // Public getter
    public String getName() {
        return name;
    }
    
    // Public setter
    public void setName(String name) {
        if (name != null && !name.isEmpty()) {
            this.name = name;
        }
    }
    
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        if (age > 0) {
            this.age = age;
        }
    }
}

// Access modifiers:
// private - only within class
// protected - within package and subclasses
// public - everywhere
// (default/package) - within package
```

## Inheritance

### Python
```python
# Single inheritance
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        return "Some sound"

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)
        self.breed = breed
    
    def speak(self):
        return "Woof!"

# Multiple inheritance
class Flyable:
    def fly(self):
        return "Flying"

class Swimmable:
    def swim(self):
        return "Swimming"

class Duck(Animal, Flyable, Swimmable):
    def __init__(self, name):
        super().__init__(name)

duck = Duck("Donald")
print(duck.fly())    # Flying
print(duck.swim())   # Swimming
```

### Java
```java
// Single inheritance
class Animal {
    protected String name;
    
    public Animal(String name) {
        this.name = name;
    }
    
    public String speak() {
        return "Some sound";
    }
}

class Dog extends Animal {
    private String breed;
    
    public Dog(String name, String breed) {
        super(name);  // Call parent constructor
        this.breed = breed;
    }
    
    @Override
    public String speak() {
        return "Woof!";
    }
}

// Interfaces (multiple inheritance of behavior)
interface Flyable {
    String fly();
}

interface Swimmable {
    String swim();
}

class Duck extends Animal implements Flyable, Swimmable {
    public Duck(String name) {
        super(name);
    }
    
    @Override
    public String fly() {
        return "Flying";
    }
    
    @Override
    public String swim() {
        return "Swimming";
    }
}
```

## Polymorphism

### Python
```python
# Polymorphism through duck typing
class Cat:
    def speak(self):
        return "Meow!"

class Dog:
    def speak(self):
        return "Woof!"

def make_sound(animal):
    print(animal.speak())

cat = Cat()
dog = Dog()
make_sound(cat)  # Meow!
make_sound(dog)  # Woof!

# Abstract base classes
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def speak(self):
        pass

class Cat(Animal):
    def speak(self):
        return "Meow!"

# Can't instantiate Animal directly
# animal = Animal()  # Error
cat = Cat()  # OK
```

### Java
```java
// Polymorphism through inheritance
abstract class Animal {
    abstract String speak();
}

class Cat extends Animal {
    @Override
    String speak() {
        return "Meow!";
    }
}

class Dog extends Animal {
    @Override
    String speak() {
        return "Woof!";
    }
}

public void makeSound(Animal animal) {
    System.out.println(animal.speak());
}

Cat cat = new Cat();
Dog dog = new Dog();
makeSound(cat);  // Meow!
makeSound(dog);  // Woof!

// Interfaces
interface Animal {
    String speak();
}

class Cat implements Animal {
    @Override
    public String speak() {
        return "Meow!";
    }
}
```

## Special Methods / Operator Overloading

### Python
```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        return f"Vector({self.x}, {self.y})"
    
    def __repr__(self):
        return f"Vector({self.x}, {self.y})"
    
    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)
    
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y
    
    def __len__(self):
        return int((self.x**2 + self.y**2)**0.5)

v1 = Vector(3, 4)
v2 = Vector(1, 2)
v3 = v1 + v2  # Vector(4, 6)
print(v1)     # Vector(3, 4)
print(len(v1))  # 5
```

### Java
```java
// Java doesn't support operator overloading
// Use methods instead
class Vector {
    private int x;
    private int y;
    
    public Vector(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    public Vector add(Vector other) {
        return new Vector(this.x + other.x, this.y + other.y);
    }
    
    @Override
    public String toString() {
        return "Vector(" + x + ", " + y + ")";
    }
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Vector vector = (Vector) obj;
        return x == vector.x && y == vector.y;
    }
}

Vector v1 = new Vector(3, 4);
Vector v2 = new Vector(1, 2);
Vector v3 = v1.add(v2);  // Vector(4, 6)
System.out.println(v1);   // Vector(3, 4)
```

## Static Methods and Class Methods

### Python
```python
class MathUtils:
    PI = 3.14159
    
    @staticmethod
    def add(a, b):
        return a + b
    
    @classmethod
    def from_string(cls, value_str):
        return cls(float(value_str))
    
    def instance_method(self):
        return "Instance method"

# Static method (no self/cls)
result = MathUtils.add(5, 3)

# Class method
obj = MathUtils.from_string("3.14")
```

### Java
```java
class MathUtils {
    public static final double PI = 3.14159;
    
    // Static method
    public static int add(int a, int b) {
        return a + b;
    }
    
    // Instance method
    public String instanceMethod() {
        return "Instance method";
    }
}

// Static method call
int result = MathUtils.add(5, 3);

// Static variable
double pi = MathUtils.PI;
```

## Summary

| Feature | Python | Java | Bash |
|---------|--------|------|------|
| **Classes** | Yes | Yes | No |
| **Inheritance** | Single/Multiple | Single (classes), Multiple (interfaces) | No |
| **Encapsulation** | Convention-based | Access modifiers | No |
| **Polymorphism** | Duck typing | Inheritance/Interfaces | No |
| **Operator Overloading** | Yes | No | No |
| **Abstract Classes** | `abc` module | `abstract` keyword | No |
| **Interfaces** | Protocols (implicit) | `interface` keyword | No |
| **Static Methods** | `@staticmethod` | `static` keyword | No |

**Note**: Bash is a procedural language and doesn't support OOP concepts. For complex object-oriented needs in shell scripting, consider using Python or other languages.

---

**Next Chapter**: [Common Tasks and Use Cases](./08-common-tasks.md)

