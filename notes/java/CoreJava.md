## Core Java

### Why Is String an Immutable Class?

String is immutable in Java, meaning its state cannot be changed after it's created. There are several reasons why String is designed to be immutable:

1. Thread Safety:
Immutable objects are inherently thread-safe because their state cannot be modified once created. This eliminates the need for synchronization when working with Strings in a multi-threaded environment.

2. String Pool Optimization:
Strings are stored in a string pool, which is a pool of unique String literals in memory. Immutable strings allow for string interning, where multiple references to the same string literal point to the same memory location. This reduces memory overhead and improves performance.

3. Security:
Immutable strings are inherently more secure because their values cannot be changed once created. This prevents malicious code from modifying sensitive string data.

4. Caching and Optimization:
Immutable strings can be cached and reused, leading to performance optimizations in applications. Operations like substring, concatenation, and conversion to upper/lower case can be optimized by reusing existing string instances.

5. Hashing and Hashcode Stability:
Immutable strings have a stable hashcode, meaning their hashcode value remains constant throughout their lifetime. This property is essential for objects used in hash-based collections like HashMap and HashSet.

### What happens when you create a String with a new keyword?

When the new keyword is used to create a string object, the JVM creates the object in the heap memory, regardless of whether an equivalent string already exists in the string pool. If the string is not found in the string pool, the JVM creates a new string object in the heap and returns a reference to it.

Here's a simplified explanation of the process: 

When new keyword is used to create a string object, the JVM creates the object in the heap memory. If the string already exists in the string pool, a new object is still created in the heap, and the string is not added to the pool again.

If the string does not exist in the string pool, the JVM adds it to the pool and returns a reference to the newly created object in the heap.

This behavior ensures that each call to new String("...") creates a new string object in the heap, regardless of whether an equivalent string exists in the string pool.

This can be useful in scenarios where you explicitly want to avoid string interning and ensure that a new object is created in the heap. However, it's important to note that using new unnecessarily can lead to unnecessary memory usage and should be done judiciously.

### Java is strictly pass-by-value language?

Yes, Java is strictly pass-by-value language. When you pass arguments to a method in Java, the values of the arguments are copied and passed to the method parameters.

However, it's important to note that the behavior can sometimes be confusing, especially when dealing with objects. When you pass an object to a method, you are passing a reference to the object, not the object itself. This reference is also passed by value, meaning a copy of the reference is passed to the method. This can lead to the misconception that Java uses pass-by-reference, but in reality, it's still pass-by-value.

Here's a brief summary:

For primitive data types (int, double, boolean, etc.), the value itself is passed.
For objects, the reference to the object is passed by value. This means changes to the object's state made inside the method are reflected outside the method, but reassigning the reference inside the method does not affect the reference outside the method.

### Explain about static, abstract and final keywords in Java?

#### static:

1. The static keyword in Java is used to declare members (variables and methods) that belong to the class itself, rather than to instances of the class.
2. Variables declared as static are called class variables or static variables. They are shared among all instances of the class and can be accessed using the class name.
3. Methods declared as static are called class methods or static methods. They belong to the class itself and can be called without creating an instance of the class.
4. static variables and methods can be accessed directly using the class name, without the need to create an object of the class.

#### abstract:

1. The abstract keyword in Java is used to declare abstract classes and methods.
2. Abstract classes cannot be instantiated directly; they serve as templates for concrete subclasses to extend and implement.
3. Abstract methods are declared without an implementation and must be implemented by concrete subclasses.
4. Abstract classes may contain both abstract and concrete methods, but a class with at least one abstract method must be declared abstract.
5. Abstract methods are intended to be overridden by subclasses to provide specific implementations.

#### final:

1. The final keyword in Java is used to restrict the modification of classes, methods, and variables.
2. When applied to a class, the final keyword prevents the class from being subclassed (i.e., it becomes a final class).
3. When applied to a method, the final keyword prevents the method from being overridden by subclasses.
4. When applied to a variable, the final keyword indicates that the variable's value cannot be changed after initialization. For primitive types, it makes the value immutable, while for reference types, it makes the reference immutable (i.e., the reference cannot be changed, but the object's state can).
5. final variables must be initialized either when declared or in a constructor, and they cannot be reassigned.

### Difference between String, StringBuffer, and StringBuilder?

The main difference between String, StringBuffer, and StringBuilder in Java lies in their mutability, thread safety, and performance characteristics:

#### String:

1. String objects are immutable, meaning their values cannot be changed after they are created.
2. Once a String object is created, its value cannot be modified. Any operation that appears to modify a string actually creates a new String object with the modified value.
3. String objects are thread-safe because they are immutable, making them safe for use in multi-threaded environments.
4. Due to their immutability, String objects are safe for sharing across different threads without synchronization overhead.
5. Use String when you need an immutable sequence of characters.

#### StringBuffer:

1. StringBuffer is a mutable sequence of characters, designed for use in multi-threaded environments.
2. StringBuffer provides synchronized methods for appending, inserting, and modifying characters, making it thread-safe.
3. Modifications to StringBuffer objects are performed in-place, avoiding unnecessary memory allocation and string copying.
4. Use StringBuffer when you need a mutable sequence of characters in a multi-threaded environment.

#### StringBuilder:

1. StringBuilder is similar to StringBuffer in that it provides a mutable sequence of characters. However, it is not inherently thread-safe.
2. Unlike StringBuffer, StringBuilder does not provide synchronized methods, making it more efficient in single-threaded scenarios.
3. StringBuilder is recommended for use in single-threaded environments or when synchronization is not required.
4. Use StringBuilder when you need a mutable sequence of characters in a single-threaded environment.

### What Are Two Types of Casting in Java?

In Java, implicit casting (widening) is a form of upcasting, while explicit casting (narrowing) is a form of downcasting.

#### Upcasting (Implicit Casting):

1. Upcasting involves converting an object of a subclass type to an object of its superclass type.
2. It is performed automatically by the compiler when assigning a subclass object to a superclass reference.
3. Upcasting is considered safe because it involves moving from a more specific type to a more general type.

#### Downcasting (Explicit Casting):

1. Downcasting involves converting an object of a superclass type to an object of its subclass type.
2. It requires an explicit cast operator and may result in a ClassCastException if the object being casted is not actually an instance of the target subclass.
3. Downcasting is considered risky because it involves moving from a more general type to a more specific type, which may lead to runtime errors if not done properly.

```java
int num1 = 10;
 double num2 = num1; // Implicit casting from int to double
 System.out.println(num2); //10.0

double num3 = 10.5;
int num4 = (int) num3; // Explicit casting from double to int
System.out.println(num4); //10
```

### Does var keyword makes Java as a dynamically typed language?

Java introduced the var keyword in Java 10, which allows you to declare local variables without explicitly specifying their type. However, this does not mean Java has become a dynamically typed language. It is still statically and strongly typed, even with var.

Basically var is a reserved type name introduced to simplify local variable declarations.
The compiler infers the type of the variable from the value assigned to it during initialization.

With var, the type is determined at compile-time, not at runtime. The compiler infers the type based on the assigned value and enforces type-checking.

```java
var number = 42;      // Compiler infers as `int`
number = "Hello";     // Compilation Error: Type mismatch
```

#### Key Restrictions of var:

1. Cannot be used without initialization: The compiler needs the right-hand side value to infer the type.
2. Type is fixed after inference: Once the type is inferred, it cannot change.
3. Only for local variables: var cannot be used for Class fields, Method parameters and Return types
4. Not allowed with null as an initializer

```java
var num;  // Compilation Error: Cannot infer type for variable

var value = 10;  // Inferred as int
value = "Hello"; // Compilation Error: Type mismatch

var value = null; // Compilation Error: Cannot infer type for variable
```

### What are the two main categories of data types in Java?

1. Primitive Data Types

These are the basic building blocks for data manipulation. They store simple values and include:

boolean: true/false values.
Numeric types like byte, short, int, long, float, and double.
char: A single Unicode character.

2. Non-Primitive (Reference) Data Types

These store references (memory addresses) rather than the actual data. Examples:

Strings: Represent sequences of characters.
Arrays: Collections of elements of the same type.
Classes and Objects: Represent complex structures with attributes and methods.

#### What are the 8 primitive data types in Java, and what does each represent?

| Data Type| Size (in bytes) | Default Value | Range                                                           | Example Literal        |
|----------|-----------------|---------------|-----------------------------------------------------------------|------------------------|
| `byte`   | 1               | `0`           | -128 to 127                                                     | `(byte) 100`           |
| `short`  | 2               | `0`           | -32,768 to 32,767                                               | `(short) 20000`        |
| `int`    | 4               | `0`           | -2<sup>31</sup> to 2<sup>31</sup>-1                             | `123456789`            |
| `long`   | 8               | `0L`          | -2<sup>63</sup> to 2<sup>63</sup>-1                             | `1234567890123456789L` |
| `float`  | 4               | `0.0f`        | Approximately ±3.40282347E+38F (6-7 significant decimal digits)  | `3.14f` or `2.718F`    |
| `double` | 8               | `0.0d`        | Approximately ±1.79769313486231570E+308 (15 significant decimal digits) | `3.14159` or `2.71828d` |
| `char`   | 2               | `'\u0000'`    | 0 to 65,535 (Unicode characters)                                | `'A'` or `'\u0041'`    |
| `boolean`| JVM specific (logically 1 bit) | `false`       | `true` or `false`                                               | `true` or `false`      |   


### Why does char use 2 bytes in Java, and not 1 byte like in C/C++?

Java uses the Unicode system to support characters from all languages worldwide (e.g., Latin, Greek, Chinese).
But Unicode requires 2 bytes (16 bits) to represent all possible characters.
By contrast, ASCII, used in C/C++, is limited to 1 byte (8 bits), supporting only 256 characters.

