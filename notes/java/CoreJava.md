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


#### Why does char use 2 bytes in Java, and not 1 byte like in C/C++?

Java uses the Unicode system to support characters from all languages worldwide (e.g., Latin, Greek, Chinese).
But Unicode requires 2 bytes (16 bits) to represent all possible characters.
By contrast, ASCII, used in C/C++, is limited to 1 byte (8 bits), supporting only 256 characters.

### What are wrapper classes in Java, and how do they relate to primitive types?

1. Wrapper classes bridge the gap between primitive types and Java’s object-oriented nature.
2. Wrapper classes in Java are object representations of the primitive data types.
3. They allow primitive values to be treated as objects.
4. They enable primitives to interact with collections and provide utility methods.
5. Autoboxing and auto-unboxing simplify conversions between primitives and objects.
6. Use cases: Collections (e.g., List<Integer> instead of List<int> (since collections work with objects)) and Utility methods (e.g., Integer.parseInt()).

### What are the three types of variables in Java, and how are they different?

| Feature            | Local Variable          | Instance Variable             | Static Variable                 |
|--------------------|-------------------------|-------------------------------|---------------------------------|
| **Scope** | Inside a method/block   | Specific to each instance     | Shared across all instances     |
| **Default Value** | Must be initialized     | Gets default values           | Gets default values             |
| **Memory Location**| Stack memory            | Heap memory                   | Method area (or Class memory)   |
| **Declared As Static** | Not allowed         | Not applicable                | Must be declared `static`       |


### What are the 4 pillars of OOPs?

The four fundamental principles of Object-Oriented Programming (OOP) are commonly referred to as the "Four Pillars of OOP." They are:

#### Encapsulation:

1. Encapsulation refers to the bundling of data (attributes or properties) and methods (functions or procedures) that operate on the data into a single unit, called a class.
2. It allows the internal state of an object to be hidden from outside interference and accessed only through well-defined interfaces (public methods).
3. Encapsulation provides data protection, abstraction, and modularity, enhancing code maintainability and reusability.

#### Inheritance:

1. Inheritance is a mechanism that allows a class (subclass or derived class) to inherit properties and behavior (methods) from another class (superclass or base class).
2. It promotes code reuse by enabling subclasses to inherit and extend the functionality defined in their superclass.
3. Inheritance facilitates the creation of a hierarchy of classes with specialized behavior, promoting code organization and scalability.

#### Polymorphism:

1. Polymorphism enables objects to be treated as instances of their superclass, allowing them to take on multiple forms.
2. It allows different objects to respond to the same message (method call) in different ways, based on their specific implementations.
3. Polymorphism simplifies code by allowing methods to be written to work with objects of a superclass, without needing to know the exact subclass at compile time.
4. Polymorphism is achieved through method overriding and method overloading.

#### Abstraction:

1. Abstraction involves simplifying complex systems by focusing on the essential properties while hiding unnecessary details.
2. It allows developers to create abstract classes and interfaces to define common behavior without specifying implementation details.
3. Abstraction helps in managing complexity, improving code maintainability, and promoting code reuse by providing a clear separation between interface and implementation.

### What is Compile Time Polymorphism and Runtime Polymorphism?

#### Compile-Time Polymorphism (Static Binding):

1. Compile-time polymorphism occurs when the decision about which method to call is made at compile time based on the method signature.
2. It is achieved through method overloading, where multiple methods with the same name but different parameter lists (number or type of parameters) are defined within the same class.
3. The compiler determines the appropriate method to invoke based on the number and types of arguments provided in the method call.
4. Compile-time polymorphism is also known as static binding because the method call is resolved at compile time and cannot be changed at runtime.

```java
lass Calculator {
    // Method overloading
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        int result1 = calc.add(2, 3);          // Calls int add(int a, int b)
        double result2 = calc.add(2.5, 3.5);   // Calls double add(double a, double b)
    }
}
```

#### Runtime Polymorphism (Dynamic Binding):

1. Runtime polymorphism occurs when the decision about which method to call is made at runtime based on the actual type of the object.
2. It is achieved through method overriding, where a subclass provides a specific implementation of a method that is already defined in its superclass.
3. The JVM determines the appropriate method to invoke based on the actual object type (not the reference type) at runtime.
4. Runtime polymorphism allows for flexibility and extensibility in object-oriented designs, as it enables subclass objects to exhibit behavior specific to their type.

```java
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Dog();  // Upcasting
        animal.sound();             // Calls Dog's overridden method
    }
}
```

### What is diamond problem in Java?

The diamond problem occurs when a class inherits from two or more classes, each of which inherits from the same superclass. This creates a diamond-shaped inheritance hierarchy, where a subclass inherits the same superclass through multiple paths.

The problem arises when the subclass attempts to inherit or override a method from the common superclass. In such cases, the compiler may not be able to determine which version of the method to use, leading to ambiguity and potential conflicts.

In this diamond-shaped inheritance hierarchy, both classes B and C inherit from class A, and class D inherits from both B and C. If class A has a method foo(), and classes B and C override foo() with different implementations, the compiler may not be able to determine which version of foo() to use in class D.

In Java, the diamond problem is avoided by allowing single inheritance of classes and supporting multiple inheritance only through interfaces. Interfaces do not contain any implementation, so there is no ambiguity in method resolution. 

### What is difference between abstract class and interface?

Abstract classes and interfaces are both used to achieve abstraction and define contracts for classes in Java, but they have some key differences:

#### Abstract Class:

1. An abstract class is a class that cannot be instantiated on its own and may contain abstract methods (methods without implementation) as well as concrete methods (methods with implementation).
2. Abstract classes can have constructors, instance variables, and non-abstract methods, in addition to abstract methods.
3. A subclass must provide implementations for all abstract methods defined in its abstract superclass, or it must itself be declared abstract.
4. Abstract classes are used to define common behavior and characteristics shared among subclasses, serving as a blueprint for creating concrete subclasses.

#### Interface:

1. An interface is a reference type similar to a class but contains only abstract methods, constants (static final fields), and default/static methods (methods with default implementations).
2. Interfaces cannot have constructors or instance variables, only method signatures.
3. A class implements an interface by providing concrete implementations for all the methods declared in the interface.
4. Interfaces are used to define contracts that classes can choose to implement, enabling multiple inheritance of type (a class can implement multiple interfaces).

### When to use abstract class VS when to use interface?

#### Use Interface When:

1. A is capable of [doing this] scenario fits in your requirement. For example: below Programmable interface is capable of doing Programming
interface Programmable {
        void programming();
}
2. You want to define a contract for classes to implement: Interfaces specify a set of methods that implementing classes must provide, defining a clear contract for how classes interact with each other.
3. You want to achieve multiple inheritance of type: Classes can implement multiple interfaces, allowing them to inherit behavior from multiple sources without the restrictions of single inheritance.
4. You want to decouple implementation from interface: Interfaces provide a way to separate the specification of behavior from its implementation, promoting loose coupling and flexibility in design.

#### Use Abstract Class When:

1. A is B or C is B scenario fits in your requirement, for example: Dog is Animal, Cat is Animal..etc.
2. You want to provide a common base implementation for subclasses: Abstract classes can contain both abstract and concrete methods, allowing you to define common behavior that subclasses can inherit.
3. You want to define a class hierarchy: Abstract classes are useful for creating a hierarchical structure of classes where each subclass represents a more specialized version of the superclass.
4. You want to share code among closely related classes: Abstract classes can contain state and behavior that is common to multiple subclasses, reducing code duplication.

### How try/catch/finally works in Java?

In Java, the try, catch, and finally blocks are used for exception handling, allowing you to gracefully manage errors that might occur during the execution of your code.

Here's how they work:

1. try: This block encloses the code that you want to monitor for exceptions. If an exception occurs within this block, it is thrown.

2. catch: This block catches exceptions that are thrown within the corresponding try block. You can specify which type of exception you want to catch by specifying the exception type in the catch block. You can have multiple catch blocks to handle different types of exceptions.

3. finally: This block, if present, is executed regardless of whether an exception is thrown or not. It's typically used to perform cleanup tasks, like closing file handles or releasing other resources, that should always occur, whether an exception is thrown or not.

#### What are the scenarios in which finally block will not get executed?

In Java, the finally block is designed to execute regardless of whether an exception is thrown or not. However, there are a few scenarios in which the finally block may not get executed:

1. System.exit():
If the JVM terminates abruptly due to a call to System.exit(), the finally block will not get executed. This is because System.exit() immediately terminates the JVM and does not allow any further code execution.

2. Infinite loop:
If the try or catch block contains an infinite loop or some other code that prevents control from reaching the finally block, then the finally block will not be executed until the loop terminates or control is transferred out of the block.

3. Fatal errors:
If a fatal error occurs, such as a StackOverflowError or an OutOfMemoryError, the JVM may terminate abruptly without executing the finally block.

In all other scenarios, the finally block should execute as expected, even if an exception is thrown or caught within the try block.

#### How to use try-with-resources in Java?

Try-with-resources is a feature for automatic resource management. It's used to automatically close resources that implement the AutoCloseable interface, such as streams, database connections, or sockets, at the end of a try block.

In below code example, the FileReader is automatically closed at the end of the try block, even if an exception is thrown while reading from the file.

```java
try (FileReader fileReader = new FileReader("example.txt")) {
    // Code that reads from the file
    int data = fileReader.read();
    while (data != -1) {
        System.out.print((char) data);
        data = fileReader.read();
    }
} catch (IOException e) {
    // Exception handling
    e.printStackTrace();
}
```

### What Is the Difference Between a Checked and an Unchecked Exception?

#### Checked Exceptions:

1. Checked exceptions are subclasses of Exception (excluding RuntimeException and its subclasses).
2. They must be either caught or declared to be thrown by the method using the throws keyword.
3. Examples of checked exceptions include IOException, ClassNotFoundException, and SQLException.
4. The compiler enforces handling of checked exceptions, which means that you must either catch them or declare that your method throws them. This helps to ensure that developers are aware of potential exceptional conditions and take appropriate action.

#### UncheckedExceptions (RuntimeExceptions):

1. Unchecked exceptions are subclasses of RuntimeException.
2. They do not need to be explicitly caught or declared by the method using the throws keyword.
3. Examples of unchecked exceptions include NullPointerException, ArrayIndexOutOfBoundsException, and ArithmeticException.
4. The compiler does not enforce handling of unchecked exceptions. This means that it's up to the developer to write code to handle or prevent them if necessary, but it's not mandatory to do so.
5. Unchecked exceptions often represent programming errors or unexpected conditions that may occur at runtime, rather than conditions that can be reasonably anticipated and handled by the code.


### What Is the Difference Between an Error and Exception?

In Java, both errors and exceptions are subclasses of the Throwable class, but they serve different purposes and are meant to be handled differently:

#### Exception:

1. Exceptions represent exceptional conditions that occur during the execution of a program. They are typically caused by problems that are outside the control of the program, such as invalid input, network issues, or file I/O errors.
2. Exceptions can be checked or unchecked. Checked exceptions must be either caught or declared in the method signature using the throws keyword, while unchecked exceptions do not need to be caught or declared.
3. Examples of exceptions include IOException, NullPointerException, ArrayIndexOutOfBoundsException, etc.

#### Error:

1. Errors represent abnormal conditions that occur in the Java Virtual Machine (JVM) or in the runtime environment, which are typically not recoverable by the application itself.
2. Errors are typically caused by serious problems that are beyond the control of the programmer, such as system failures, out-of-memory errors, or stack overflow errors.
3. Errors are unchecked, which means they do not need to be caught or declared by the programmer. In fact, it's generally not recommended to catch errors because they often indicate serious problems that the application cannot recover from.
4. Examples of errors include OutOfMemoryError, StackOverflowError, VirtualMachineError, etc.

### How can you create a custom exception in Java?

To create a custom exception in Java, you need to create a new class that extends either Exception (for checked exceptions) or RuntimeException (for unchecked exceptions).

```java
//example of creating a custom checked exception
//Define the custom exception
public class CustomCheckedException extends Exception {
    // Constructor that accepts a message
    public CustomCheckedException(String message) {
        super(message);
    }

    // Constructor that accepts a message and a cause
    public CustomCheckedException(String message, Throwable cause) {
        super(message, cause);
    }

    // Constructor that accepts a cause
    public CustomCheckedException(Throwable cause) {
        super(cause);
    }
}
```

### What is Java Garbage Collection and how does it work?

Java Garbage Collection (GC) is the process by which Java programs perform automatic memory management. The Java runtime environment deletes objects when they are no longer in use to free up memory. This is done automatically by the garbage collector, which is part of the Java Virtual Machine (JVM).

GC is essential in long-running server applications to prevent memory leaks and ensure efficient memory usage.

Java provides several garbage collection algorithms:

1. Serial GC: Uses a single thread to perform all garbage collection work, suitable for single-threaded applications.
2. Parallel GC: Uses multiple threads for garbage collection, suitable for multi-threaded applications.
3. CMS (Concurrent Mark-Sweep) GC: Performs most of the garbage collection concurrently with the application threads to minimize pause times.
4. G1 (Garbage-First) GC: Divides the heap into regions and prioritizes reclaiming regions with the most garbage.


Here's a breakdown of the typical process:

1. Object Allocation: When you create an object using the new keyword, memory is allocated for it on the heap.

2. Reachability (GC Roots): The GC determines which objects are "live" by starting from a set of known "GC Roots." These roots are special references that are always considered reachable, such as:
    * Local variables on the stack of active threads.
    * Static variables.
    * Active Java threads themselves.
    * JNI (Java Native Interface) references.

3. Mark Phase: 
    * Starting from the GC Roots, the garbage collector traverses the object graph.
    * It identifies and "marks" all objects that are reachable (directly or indirectly) from these roots as "live." Any object that cannot be reached from a GC root is considered eligible for garbage collection.

4. Sweep Phase:
    * After the marking phase, the garbage collector "sweeps" through the heap.
    * It deallocates the memory occupied by all objects that were not marked as live. This memory is then made available for new object allocations.

5. Compact Phase (Optional but common):
    * After sweeping, memory can become fragmented (small, non-contiguous blocks of free space).
    * To optimize for future allocations and prevent OutOfMemoryError due to fragmentation, some garbage collectors perform a "compact" phase. This involves moving live objects closer together, effectively defragmenting the heap.


Modern JVMs use generational garbage collection to optimize the process, based on the empirical observation that most objects are short-lived. The heap is divided into different "generations":

1. Young Generation:

    * Eden Space: Where most new objects are initially allocated.
    * Survivor Spaces (S0 and S1): Objects that survive a minor GC in the Eden space are moved to one of the survivor spaces. Objects are moved between survivor spaces in subsequent minor GCs.
    * Minor GC (Young GC): Occurs frequently to collect garbage in the young generation. It's typically very fast because it deals with a smaller portion of the heap and most objects in this generation are short-lived.

2. Old Generation (Tenured Space):

    * Objects that survive multiple minor GC cycles in the young generation are promoted to the old generation. These are typically longer-lived objects.
    * Major GC (Full GC): Occurs less frequently and cleans up the old generation. Full GCs are generally more time-consuming as they involve scanning a larger memory area.

3. Metaspace (Java 8+):

    * Replaced the PermGen space. Stores class metadata (class structures, method bytecode, static variables, etc.). This space is allocated from native memory (off-heap).

#### Analogy

When your Java program runs, it constantly creates new "boxes" (objects) and puts "stuff" inside them. Over time, some of these "boxes" are no longer needed (your program has finished using them). If you don't throw them out, your storage facility gets full, and you can't put new "boxes" in. This is a memory leak, and your program crashes with an "Out of Memory" error.

This is where the Garbage Collector (GC) comes in. It's like a cleanup crew for your storage facility.

#### Traditional GC (Analogy)

Imagine a traditional cleanup crew:

1. Stop everything! (This is called "Stop-the-World" or STW). Everyone has to freeze.
2. Find all the useful boxes: The crew goes through all the boxes and marks the ones that are still being used.
3. Throw out the useless boxes: All unmarked boxes are thrown away.
4. Reorganize (sometimes): Sometimes, they also move the remaining useful boxes closer together to fill gaps.
5. Resume work! Everyone can go back to their tasks.

The problem with this is that if your storage facility is HUGE, step 2 and 3 can take a very long time, and everyone has to wait! This causes your program to "freeze" for noticeable periods.

#### G1 GC: The Smart, Flexible Cleanup Crew

G1 (Garbage-First) is like a much smarter cleanup crew. Instead of one giant storage facility, they divide it into many small, equal-sized rooms (these are called regions).

Imagine your entire computer memory as a big grid of small squares, like a chessboard. Each square is a "region."

Here's how G1's smart crew works:

1. Flexible Rooms
    
    * New Boxes go here (Eden Regions): When your program creates a new box, it usually goes into an "Eden" room.
    * Survivors (Survivor Regions): If a box survives a quick cleanup, it moves to a "Survivor" room.
    * Old, Long-Term Boxes (Old Regions): Boxes that have been around for a long time and survived many cleanups move to "Old" rooms.
    * Huge Boxes (Humongous Regions): If a box is really, really big, it gets its own special set of rooms (humongous regions).

2. The "Garbage-First" Idea

The name "Garbage-First" means the cleanup crew looks for the rooms that have the most garbage (useless boxes) and cleans those first. Why? Because you get the most space back for the least amount of effort.

3. Two Main Types of Cleanup (GC Cycles)

G1 has two main ways it cleans:

* Quick Cleanups (Young GC / Minor GC)

    1. What it does: These happen frequently. G1 quickly checks the "Eden" and "Survivor" rooms.
    2. How: It still has to "Stop-the-World" (everyone freezes) for a very short time. But because it's only cleaning a small set of "young" rooms, the pause is usually very quick.
    3. Goal: Get rid of lots of short-lived boxes quickly.
    4. The "Copying" Trick: Instead of throwing out boxes in place, G1 copies the useful boxes from the dirty rooms into fresh, empty rooms. This also helps to nicely pack the useful boxes together, avoiding empty spaces (fragmentation).

* Big Cleanups (Mixed GC / Space-Reclamation Phase)

    1. Initial Mark (Very Short Freeze):

        * The cleanup crew quickly looks at some key starting points to see which boxes are currently in use. This is often done during a quick Young GC.
        * Analogy: Like quickly glancing at your desk to see what you're actively working on right now.

    2. Concurrent Mark (Working While You Work!):

        * This is key! While your program is still running and doing its work, the cleanup crew goes through most of the rooms (especially the "Old" rooms) to find all the useful boxes.
        * Analogy: The cleanup crew is quietly wandering around your storage facility, looking at boxes and marking them, while you continue to put new boxes in and take old ones out. They have special notes (called "write barriers") to keep track of changes you make.

    3. Remark (Short Freeze):

        * After their concurrent search, they need a final, brief "Stop-the-World" pause.
        * Analogy: They quickly double-check their notes and make sure they didn't miss any boxes that became useful, or mark any that became useless, right at the very end of their concurrent sweep.

    4. Cleanup & Copying (Mixed GC - Short Freeze):

        * Now, G1 knows which "Old" rooms have a lot of garbage. It picks a few of these "Old" rooms (the ones with the most garbage, hence "Garbage-First") and some "Young" rooms.
        * It then does a "Stop-the-World" pause.
        * Just like the Young GC, it copies the useful boxes from these selected dirty rooms into new, clean, empty rooms. This again helps to compact the memory.
        * Analogy: They focus on cleaning just a few of the messiest rooms completely, rather than trying to clean all the old rooms at once.

    The beauty of G1 is that it breaks down the "big cleanup" into smaller, manageable pieces, and does most of the work while your program is running. This makes the "Stop-the-World" pauses much shorter and more predictable.

### What Is the Difference Between an Inner Class and a Static Nested Class?

#### Inner Classes
Inner classes are non-static nested classes. They have the following characteristics:

1. Associated with an Instance:
Inner classes are tied to an instance of the outer class. This means they can access the outer class's instance variables and methods directly.

2. Syntax
```java
public class OuterClass {
    class InnerClass {
        // Inner class code
    }
}
```

3. Access to Outer Class Members:
Inner classes have direct access to the outer class's instance members, including private ones.

4. Instantiation:
An inner class instance is created using an instance of the outer class:
```java
// Inner Class Example
public class OuterClass {
    private int outerField = 10;

    class InnerClass {
        void display() {
            System.out.println("Outer field: " + outerField);
        }
    }

    public static void main(String[] args) {
        OuterClass outer = new OuterClass();
        OuterClass.InnerClass inner = outer.new InnerClass();
        inner.display(); // Output: Outer field: 10
    }
}
```

#### Static Nested Classes

1. Not Associated with an Instance:
Static nested classes do not have an implicit reference to an instance of the outer class. They can be instantiated without an instance of the outer class.

2. Syntax
```java
// Static Nested Class Example:
public class OuterClass {
    private static int staticOuterField = 20;

    static class StaticNestedClass {
        void display() {
            System.out.println("Static outer field: " + staticOuterField);
        }
    }

    public static void main(String[] args) {
        OuterClass.StaticNestedClass nested = new OuterClass.StaticNestedClass();
        nested.display(); // Output: Static outer field: 20
    }
}
```

3. Access to Outer Class Members:
Static nested classes can only access the static members of the outer class directly. To access instance members, they need an instance of the outer class.

4. Instantiation:
A static nested class instance is created without needing an instance of the outer class
```java
OuterClass.StaticNestedClass nested = new OuterClass.StaticNestedClass();
```

### What Is an Anonymous Class and what is it's Use Case?

An anonymous class in Java is a type of inner class without a name. It is used to instantiate a class defined in the method body, usually for implementing an interface or extending a class with minor modifications. Anonymous classes are often used for short-lived implementations that are not reused elsewhere.

1. Event Handling
Commonly used in GUI applications for handling events (e.g., button clicks).

```java
mport javax.swing.JButton;
import javax.swing.JFrame;

public class AnonymousClassExample {
    public static void main(String[] args) {
        JFrame frame = new JFrame("Anonymous Class Example");
        JButton button = new JButton("Click Me");

        button.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                System.out.println("Button was clicked!");
            }
        });

        frame.add(button);
        frame.setSize(300, 200);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }
}
```

2. Thread Creation
Simplifies thread creation without defining a separate class.

```java
public class AnonymousClassExample {
    public static void main(String[] args) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("Thread is running");
            }
        }).start();
    }
}
```

3. Customizing Behavior:
Allows for quick customization of existing classes or interfaces.

```java
import java.util.ArrayList;
import java.util.List;

public class AnonymousClassExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<String>() {
            @Override
            public boolean add(String s) {
                System.out.println("Adding element: " + s);
                return super.add(s);
            }
        };

        list.add("Hello");
        list.add("World");
    }
}
```

### Marker Interface

A marker interface in Java is an interface with no methods or constants. It serves as a means to signal to the JVM or other code that a class implementing this interface has a particular property or should be treated in a special way. Marker interfaces are used to provide metadata to objects.

* Purpose of Marker Interfaces
Marker interfaces are used for various purposes, including:
1. Type Identification: Identifying the type of an object at runtime.
2. Special Behavior: Indicating that a class should be processed in a special manner by certain Java APIs or libraries.
3. Security and Serialization: Controlling the serialization mechanism or ensuring certain security measures.

Examples of Marker Interfaces in Java: Serializable, Cloneable

### What Is an Immutable Class and how to create it?

An immutable class in Java is a class whose instances cannot be modified after they are created. Once an object is created, its state (fields) cannot be changed. Immutable objects are inherently thread-safe and can be shared freely among multiple threads without synchronization concerns.

#### Characteristics of Immutable Classes
1. Final Class:
The class should be declared as final so that it cannot be subclassed.

2. Private Final Fields:
All fields should be declared as private and final to ensure they are not modified after initialization.

3. No Setter Methods:
The class should not provide any methods that modify fields or objects referred to by fields.

```java
import java.util.Date;

public final class ImmutablePerson {
    private final String name;
    private final int age;
    private final Date birthDate; // mutable object

    public ImmutablePerson(String name, int age, Date birthDate) {
        this.name = name;
        this.age = age;
        // Creating a defensive copy of the mutable Date object
        this.birthDate = new Date(birthDate.getTime());
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public Date getBirthDate() {
        // Returning a defensive copy of the mutable Date object
        return new Date(birthDate.getTime());
    }
}
```

