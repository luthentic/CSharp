
<p align="center">
  <img src = "https://miro.medium.com/max/2728/1*7I6oONv2fGLQJcNEFA4QSw.png" width=800 height=300>
</p>

## 🚩 Table of Contents

- [Program Structure](#program-structure)
- [Types](#types)
- [Namespace](#namespace)
- [Class](#class)
- [Records](#records)
- [Generics](#generics)
- [Anonymous Types](#anonymous-types)
- [OOP](#oop)
- [Discards](#discards)
- [Exceptions](#exceptions)
- [Conventions](#conventions)
- [LINQ](#linq)
- [Asynchronous](#asynchronous)
- [Memory Management](#memory-management)
- [Libraries](#libraries)
- [CLR](#clr)
- [Assembly](#assembly)
- [DLL EXE](#dll-exe)
- [Attribute](#attribute)
- [Property vs Field](#property-vs-field)
- [Sln](#sln)
- [Auto Implemented Properties](#auto-implemented-properties)
- [Dependency Injection](#dependency-injection)
- [Extension Methods](#extension-methods)
- [Stream vs Buffer](#stream-vs-buffer)
- [Ref Struct](#ref-struct)
- [AOT](#aot)
- [Class vs Struct](#class-vs-struct)
- [Glossaries](#glossaires)
  
---


## Program Structure:

C# programs consist of one or more files.
Each file contains zero or more namespaces.
A namespace can contain various types, such as classes, structs, interfaces, enumerations, and delegates, or other namespaces.

### Main() Method: 
- The Main() method is the entry point of a C# application.
- It’s where program control starts and ends.
- Must be declared inside a class or struct.
- Must be static (and need not be public).
- Can have a void, int, Task, or Task<int> return type.
- Can be declared with or without a string[] args parameter for command-line arguments.

**Example signatures:**

public static void Main()

public static int Main()

public static async Task Main()

public static async Task<int> Main()

### Command-Line Arguments: 
- Parameters are read as zero-indexed command-line arguments.
- Unlike C/C++, the program name is not treated as the first argument.
- Use args.Length to get the number of command-line arguments.



### Top Level Statement:

With top-level statements, you can avoid the extra ceremony of placing your program’s entry point in a static method within a class.

```csharp
Console.WriteLine("Hello World");
```

```csharp
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddRazorPages();
builder.Services.AddControllersWithViews();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();

app.UseAuthorization();

app.MapGet("/hi", () => "Hello!");

app.MapDefaultControllerRoute();
app.MapRazorPages();

app.Run();
```


**References:**
- https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/program-structure/
- https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/program-structure/top-level-statements

---












## Types:

### Equality Comparisons: ###

**Reference Equality:**

Reference equality means that two object references refer to the same underlying object.
Use System.Object.ReferenceEquals(a, b) to check if two references point to the same object.
Applies only to reference types (not value types).
Value type objects cannot have reference equality.

**Value Equality:**

Value equality means that two objects contain the same value or values.
For primitive value types (e.g., int, bool), use the == operator for value equality.
For other types (classes, structs), value equality depends on how the type defines it (usually all fields or properties having the same value).


### Value Types:

1. Derived from System.ValueType, which itself derives from System.Object.
2. Special behavior in the Common Language Runtime (CLR).
3. Variables directly contain their values.

**Structure Type:**

A structure type (or struct type) is a value type that encapsulates data and related functionality.
You use the struct keyword to define a structure type.
Structure types have value semantics, meaning they contain an instance of the type directly.
All the rules that apply to structs also apply to enums.

**Value Types:**

- Value types (e.g., struct) are sealed and cannot be derived from.
- You cannot create a struct that inherits from a user-defined class or another struct.
- Value types inherit directly from System.ValueType.

**Interfaces and Boxing:**

- A struct can implement one or more interfaces.
- When you cast a struct to an interface type, it undergoes boxing (wrapped inside a reference type object on the managed heap).
- Boxing occurs when passing a value type to a method that takes System.Object or any interface type as a parameter.

**Default Behavior:**

Variable values of structure types are copied on assignment, method arguments, and method return.
Use structure types for small data-centric types with little or no behavior.

**Immutable Structs:**

Declare a readonly struct to make it immutable.
All data members must be read-only or init-only.

**Readonly Instance Members:**

Mark instance members as readonly if they don’t modify the struct’s state.
Readonly members can call non-readonly members but can’t assign to instance fields.

**Enumeration Type:** 

1. An enum type is a value type defined by named constants.
2. Enum members have an associated constant value (by default, of type int).
3. You can specify any integral numeric type as the underlying type.


**Nullable value type** - T? represents all values of its underlying value type T and an additional null value

**const** - keyword to declare a constant field or a local constant

**Built-in types:**

1. bool - System.Boolean
2. byte	- System.Byte
3. sbyte - System.SByte
4. char	- System.Char
5. decimal - System.Decimal
6. double	- System.Double
7. float	- System.Single
8. int	- System.Int32
9. uint	- System.UInt32
10. nint	- System.IntPtr
11. nuint	- System.UIntPtr
12. long	- System.Int64
13. ulong	- System.UInt64
14. short	- System.Int16
15. ushort - System.UInt16


### Custom types:
Custom types refer to user-defined types created using class, struct, enum, or interface.

- class MyClass { }
- struct MyStruct { }
- enum MyEnum { }
- interface IMyInterface { }

### Reference Types:
Reference types store references (memory addresses) to the actual data.
Objects of reference types are allocated on the heap.

- Class instances
- string
- Arrays

### Common type system:

**Inheritance:**

Types can derive from other types (base types).
Derived types inherit methods, properties, and other members from their base type.
A base type can itself derive from another type, creating an inheritance hierarchy.
All types, including built-in numeric types, ultimately derive from System.Object.

**Common Type System (CTS):**

The CTS defines a unified type hierarchy for all types in .NET.
Custom types (user-defined) and built-in types (e.g., int) are part of the CTS.

**Value Types vs. Reference Types:**

Value types (e.g., struct) store their data directly.
Reference types (e.g., class) store references to their data.
Different compile-time rules and run-time behavior apply to each type.

<p align="center">
<img src = "https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/types/media/index/value-reference-types-common-type-system.png" width=700 height=400>
</p>

### Tuples vs System.Tuple:
The tuples feature provides concise syntax to group multiple data elements in a lightweight data structure. 

```charp
(double, int) t1 = (4.5, 3);
Console.WriteLine($"Tuple with elements {t1.Item1} and {t1.Item2}.");
// Output:
// Tuple with elements 4.5 and 3.

(double Sum, int Count) t2 = (4.5, 3);
Console.WriteLine($"Sum of {t2.Count} elements is {t2.Sum}.");
// Output:
// Sum of 3 elements is 4.5.
```

C# tuples, which are backed by System.ValueTuple types, are different from tuples that are represented by System.Tuple types. The main differences are as follows:

1. System.ValueTuple types are value types. System.Tuple types are reference types.
2. System.ValueTuple types are mutable. System.Tuple types are immutable.
3. Data members of System.ValueTuple types are fields. Data members of System.Tuple types are properties.

**Tuples as out parameters**

```csharp
var limitsLookup = new Dictionary<int, (int Min, int Max)>()
{
    [2] = (4, 10),
    [4] = (10, 20),
    [6] = (0, 23)
};

if (limitsLookup.TryGetValue(4, out (int Min, int Max) limits))
{
    Console.WriteLine($"Found limits: min is {limits.Min}, max is {limits.Max}");
}
// Output:
// Found limits: min is 10, max is 20
```

### Dynamic Type:
The dynamic type in C# was introduced in version 4.0 (.NET 4.5). It allows you to bypass compile-time type checking and resolve the type at runtime. Here are the key points about dynamic 

**Definition**: A dynamic type variable is defined using the dynamic keyword.

**Type Checking**: Unlike other static types, the compiler doesn’t perform type checking on dynamic variables during compilation.

**Runtime Resolution**: The actual type of a dynamic variable is determined at runtime.

**Operations**: You can perform any operation on a dynamic element without the need to know its exact type.

**Errors**: If the code isn’t valid, errors surface at runtime rather than compile time.

```csharp
class ExampleClass
{
    public void ExampleMethod1(int i) { /* ... */ }
    public void ExampleMethod2(string str) { /* ... */ }
}

static void Main(string[] args)
{
    ExampleClass ec = new ExampleClass();

    // Compiler error (if ExampleMethod1 has only one parameter)
    // ec.ExampleMethod1(10, 4);

    dynamic dynamic_ec = new ExampleClass();

    // No compiler error (but causes a runtime exception)
    dynamic_ec.ExampleMethod1(10, 4);

    // These calls also don't cause compiler errors
    dynamic_ec.SomeMethod("some argument", 7, null);
    dynamic_ec.NonexistentMethod();
}
```

**References:**

- https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/built-in-types
- https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/ref-struct
- https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/struct
- https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/
- https://www.reddit.com/r/csharp/comments/15i6m8b/very_confused_about_structs/
- https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-tuples#code-try-3

---



## Namespace:
- They organize large code projects.
- They're delimited by using the . operator.
- The using directive obviates the requirement to specify the name of the namespace for every class.
- The global namespace is the "root" namespace: global::System will always refer to the .NET System namespace.

**References:**

- https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/namespaces

---


## Class:

- Beginning with C# 12, you can define a primary constructor as part of the class declaration:
  
  ```csharp
      public class Container(int capacity)
    {
        private int _capacity = capacity;
    }
  ```
- You can also use the required modifier on a property and allow callers to use an object initializer to set the initial value of the property:

  ```csharp
  public class Person
  {
      public required string LastName { get; set; }
      public required string FirstName { get; set; }
  }
  
  var p1 = new Person(); // Error! Required properties not set
  var p2 = new Person() { FirstName = "Grace", LastName = "Hopper" };
  ```

**References:**

- https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/classes

  
---













## Records:

### Records Overview: ###
Records are reference types (or value types) designed for encapsulating data.
They provide built-in functionality for common scenarios.

### Syntax: ###

Use the record modifier to define a reference type (record class) or a value type (record struct).
Records simplify creating immutable data models.

```csharp
// A simple record with positional parameters
public record Person(string FirstName, string LastName);

// Usage
var person = new Person("John", "Doe");
Console.WriteLine($"Full Name: {person.FirstName} {person.LastName}");
```

### Positional Parameters: ###
Records have a primary constructor with positional parameters.
The compiler generates public properties for these parameters.
Positional properties are read-only.

**Deconstruction of Records:**

```csharp
var (first, last) = person; // Deconstructing into separate variables
Console.WriteLine($"First Name: {first}, Last Name: {last}");

```

### Immutability: ###
Records are immutable by default.
Once created, their property values cannot change.

### Value Equality: ###
Records support value-based equality checking.
Two records with the same values are considered equal.

```csharp
var person1 = new Person("John", "Doe");
var person2 = new Person("John", "Doe");

// Value equality (true)
bool areEqual = person1 == person2;
Console.WriteLine($"Are equal: {areEqual}");
```

### Customization: ###
Records provide default formatting (customizable via ToString()).
Use records for API response models, configuration settings, and domain models.

```csharp
public record Point(int X, int Y)
{
    public override string ToString() => $"({X}, {Y})";
}

var point = new Point(10, 20);
Console.WriteLine($"Point: {point}"); // Prints "(10, 20)"

```

---










## Generics:

### What Are Generics?

Generics allow you to create classes, methods, and interfaces that work with different data types without sacrificing type safety.
They provide a way to write reusable code by parameterizing types.

### Syntax and Usage:
Define a generic type using angle brackets (<>).

Example: List<T> represents a list of elements of type T.

### Benefits: ###

**Type safety:** Compile-time checks ensure correct usage of generic types.

**Code reusability:** Write one implementation for multiple data types.

**Performance:** Avoid boxing/unboxing overhead for value types.

### Constraints: ###
Specify constraints on generic type parameters (e.g., where T : class or where T : struct).
Constraints allow you to restrict the types that can be used with a generic type.

### Common Use Cases: ###
- Collections (e.g., lists, dictionaries) with various element types.
- Algorithms that work with different data types.
- Custom data structures.

### Generic Class: ###
Create a class that works with different data types using generics.

```csharp
public class Student<T>
{
    public T Data { get; set; }
}

// Usage:
var studentName = new Student<string>();
studentName.Data = "Avicii";

var studentId = new Student<int>();
studentId.Data = 23;
```

### Generic Method: ###
Create a method that can handle any data type using generics.

```csharp
public static void DisplayData<T>(T data)
{
    Console.WriteLine("Data passed: " + data);
}

// Usage:
DisplayData(34); // int
DisplayData("Tim"); // string
```
---



















## Anonymous Types

### What Are Anonymous Types? ###
Anonymous types allow you to create objects without explicitly defining a class.
They are useful for temporary data structures or projections.

### Syntax: ###
Create an anonymous type using the new keyword and an object initializer.

Example: 
  ```csharp
    var person = new { Name = "John", Age = 30 };
  ```

### Properties: ###
Anonymous types have read-only properties inferred from the initializer.
You can access properties using dot notation (e.g., person.Name).

### Common Use Cases: ###
- Query results from LINQ expressions.
- Data transformations or projections.
- Simplified code for temporary data.

### Basic Example: 

```csharp
var person = new { Name = "John Doe", Age = 25 };
// Access properties:
Console.WriteLine($"Name: {person.Name}, Age: {person.Age}");
```

### Using LINQ:

```csharp
var products = new List<Product>
{
    new Product { Color = "Red", Price = 50 },
    new Product { Color = "Blue", Price = 40 }
};

var productQuery = from prod in products
                   select new { prod.Color, prod.Price };

foreach (var v in productQuery)
{
    Console.WriteLine($"Color: {v.Color}, Price: {v.Price}");
}
```

---





















## OOP:

### What Is Object-Oriented Programming (OOP)?
OOP is a programming paradigm that organizes code around objects.
Objects encapsulate data (attributes) and behavior (methods).

### Key Concepts:
**Classes:** Blueprint for creating objects.

**Objects:** Instances of classes.

### Benefits of OOP:
**Modularity:** Divide code into manageable components.

**Reusability:** Reuse existing classes and build upon them.
**Flexibility:** Easily modify or extend functionality.

**Examples:**
Define a Person class with properties like Name and Age.

Create instances of Person and call methods (e.g., person.SayHello()).

**Encapsulation**: 

  It involves bundling related data (state) and behavior (methods) into a single unit (usually a class).
  
  Encapsulation ensures that the internal details of an object are hidden from external code.

  **Why Use Encapsulation?**

  **Data Protection:** Encapsulation allows you to protect data by restricting direct access to it.
  
  **Abstraction:** It simplifies the interface for using an object, exposing only essential methods and properties.
  
  **Modularity:** Encapsulated classes are easier to maintain and extend.

  **Implementation Techniques:**

  Use private, protected, or internal access modifiers to control visibility of class members.
  
  - **Private:** Only accessible within the class.
  - **Protected:** Accessible within the class and derived classes.
  - **Internal:** Accessible within the same assembly.

  ```csharp
      class BankAccount
    {
        private decimal balance;
    
        public decimal Balance
        {
            get { return balance; }
            set { balance = value; }
        }
    }
  ```

**What is the use of encapsulation when I'm able to change the property values with setter methods?**

- https://stackoverflow.com/questions/16418571/what-is-the-use-of-encapsulation-when-im-able-to-change-the-property-values-wit



**Inheritance:**

  - Inheritance allows one class (the derived class) to acquire properties and behaviors from another class (the base class).
  - It promotes code reuse and hierarchy.

  **Single Inheritance:**
  
  C# supports single inheritance, meaning a class can inherit from only one base class.
  However, inheritance can be transitive, forming an inheritance hierarchy.
  
  **Base Class and Derived Class:**
  
  The base class provides common functionality.
  The derived class extends or modifies that functionality.
  
    Example: class Dog : Animal { ... }
    
  **Access Modifiers:**
  
  Use access modifiers (e.g., public, protected, private) to control member visibility.
  Inherited members can be overridden or accessed based on their visibility.
  
  **Method Overriding:**
  
  Derived classes can override base class methods.
  Use the override keyword to provide a new implementation.

  ```csharp
        class Circle : Shape
    {
        public override double Area() { ... }
    }
  ```

  **Base Keyword:**
  
  Use base to call base class methods or constructors.
  
  Example: base.Area();


**Polymorphism:**

  It allows you to treat objects of different types as if they were the same type.

  - **Runtime Polymorphism:** Objects of derived classes can be treated as objects of the base class. The declared type no longer matches the runtime type.
  - **Method Overriding:** Derived classes can provide their own implementation for methods defined in the base class.


  **Method Overloading:**
  
  ```csharp
    abstract class Shape
    {
        public abstract double Area();
    }
    
    class Circle : Shape
    {
        public override double Area()
        {
            // Calculate circle area
        }
    }
    
    class Rectangle : Shape
    {
        public override double Area()
        {
            // Calculate rectangle area
        }
    }
    
    // Usage:
    Shape shape = new Circle();
    double area = shape.Area(); // Polymorphic call

  ```
  
  **Operator Overloading:**
    
  ```csharp
        class ComplexNumber
    {
        public double Real { get; set; }
        public double Imaginary { get; set; }
    
        public static ComplexNumber operator +(ComplexNumber a, ComplexNumber b)
        {
            // Add complex numbers
        }
    }
    
    // Usage:
    ComplexNumber c1 = new ComplexNumber();
    ComplexNumber c2 = new ComplexNumber();
    ComplexNumber result = c1 + c2; // Polymorphic operator overloading
  ```


## Pattern Matching:
  - Technique to test expressions for specific characteristics.
  - C# provides concise syntax for matching expressions.
  - Two main expressions: “is expression” and “switch expression.”

  **is Expression**

    ```csharp
    int? maybe = 12;
    if (maybe is int number)
    {
        Console.WriteLine($"The nullable int 'maybe' has the value {number}");
    }
    
    ```
  **switch Expression**
  
    ```csharp
    public static class SwitchExample
    {
        public enum Direction
        {
            Up, Down, Right, Left
        }
    
        public enum Orientation
        {
            North, South, East, West
        }
    
        public static Orientation ToOrientation(Direction direction) =>
            direction switch
            {
                Direction.Up => Orientation.North,
                Direction.Right => Orientation.East,
                Direction.Down => Orientation.South,
                Direction.Left => Orientation.West,
                _ => throw new ArgumentOutOfRangeException(nameof(direction), $"Not expected direction value: {direction}"),
            };
    
        public static void Main()
        {
            var direction = Direction.Right;
            Console.WriteLine($"Map view direction is {direction}");
            Console.WriteLine($"Cardinal orientation is {ToOrientation(direction)}");
            // Output:
            // Map view direction is Right
            // Cardinal orientation is East
        }
    }
    
    ```
---


















## Discards

### What Are Discards?
Discards are intentionally unused variables in your code.
They act as placeholders for values you don’t need or want to explicitly create variables for.
Discards are equivalent to unassigned variables; they don’t have a value.

### Use Cases for Discards:
Ignoring Expression Results:

When you want to ignore the result of an expression (e.g., method return value), use a discard.

Example: (_, _, area) = city.GetCityInformation(cityName);

Tuple and Object Deconstruction:

When working with tuples, you can use discards for elements you don’t care about.

Example: (_, _, _, pop1, _, pop2) = QueryCityDataForYears("New York City", 1960, 2010);

### Syntax:
Indicate a variable as a discard by assigning it the underscore (_) as its name.
Attempting to retrieve its value generates a compiler error because it’s intentionally unused.
    
```csharp
var (_, _, _, pop1, _, pop2) = QueryCityDataForYears("New York City", 1960, 2010);
Console.WriteLine($"Population change, 1960 to 2010: {pop2 - pop1:N0}");

static (string, double, int, int, int, int) QueryCityDataForYears(string name, int year1, int year2)
{
    int population1 = 0, population2 = 0;
    double area = 0;
    if (name == "New York City")
    {
        area = 468.48;
        if (year1 == 1960)
        {
            population1 = 7781984;
        }
        if (year2 == 2010)
        {
            population2 = 8175133;
        }
    }
    // Other logic...
    return (name, area, year1, population1, year2, population2);
}
```

---














## Exceptions:

  ### Use try/catch/finally Blocks:
    - Wrap potentially exception-prone code in a try block.
    - Use catch blocks to handle specific exceptions or log errors.
    - Clean up resources (e.g., close files, release connections) in a finally block.
  ### Order Exceptions from Most Derived to Least Derived:
    - In catch blocks, order exceptions from the most specific (derived) to the least specific (base).
    - This ensures that more specific exceptions are caught before more general ones.
  ### Avoid Swallowing Exceptions:
    - Don’t catch exceptions if you can’t handle them effectively.
    - Let exceptions propagate up the call stack to higher-level handlers.
  ### Check Common Conditions to Avoid Exceptions:
    - Use conditional checks to prevent exceptions when possible. For example, verify connection states before closing them.
    - Use using Statements for Resource Cleanup:
    - Leverage using statements to automatically clean up resources (e.g., file handles, database connections).
    - Resources are disposed of even if exceptions occur.
  ### Log Exceptions:
    - Log exception details (type, message, stack trace) for debugging and diagnostics.
    - Use a logging framework (e.g., Serilog, NLog) to capture exceptions.
  ### Create Custom Exceptions:
    - Define custom exception classes for specific scenarios in your application.
    - Derive from Exception or other relevant base classes.
    - Include additional properties or context information.
  ### Provide User-Friendly Messages:
    - When displaying exceptions to users, provide clear and informative error messages.
    - Avoid exposing technical details that might confuse users.

### Basic Exception Handling with try and catch:
```csharp
using System;

class Program
{
    static void Main()
    {
        try
        {
            int numerator = 5;
            int denominator = 0;
            int result = numerator / denominator; // Throws DivideByZeroException
            Console.WriteLine($"Result: {result}");
        }
        catch (DivideByZeroException e)
        {
            Console.WriteLine($"Error: {e.Message}");
        }
    }
}
// Output: Error: Attempted to divide by zero.  
```

### Custom Exception Class:

```csharp
using System;

class CustomException : Exception
{
    public CustomException(string message) : base(message) { }
}

class Program
{
    static void Main()
    {
        try
        {
            throw new CustomException("Something went wrong!");
        }
        catch (CustomException e)
        {
            Console.WriteLine($"Custom Exception: {e.Message}");
        }
    }
}
// Output: Custom Exception: Something went wrong!
```

### Using finally Block:
```csharp
using System;

class Program
{
    static void Main()
    {
        try
        {
            // Code that may raise an exception
            Console.WriteLine("Inside try block");
        }
        catch (Exception e)
        {
            Console.WriteLine($"Exception: {e.Message}");
        }
        finally
        {
            // This code always executes, whether there's an exception or not
            Console.WriteLine("Inside finally block");
        }
    }
}
// Output:
// Inside try block
// Inside finally block

```

### Compiler-generated exceptions:

**ArithmeticException:**

Base class for exceptions during arithmetic operations (e.g., DivideByZeroException and OverflowException).

**ArrayTypeMismatchException:**

Thrown when an array can’t store an element due to incompatible types.

```csharp
try
{
    int[] numbers = new int[3];
    object obj = "Hello";
    numbers[0] = (int)obj; // Throws ArrayTypeMismatchException
}
catch (ArrayTypeMismatchException ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}

```

**DivideByZeroException:**

Occurs when dividing an integral value by zero.


```csharp
try
{
    int result = 10 / 0; // Throws DivideByZeroException
}
catch (DivideByZeroException ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}

```

**IndexOutOfRangeException:**

Thrown when attempting to index an array with an invalid index (less than zero or outside array bounds).

```csharp
try
{
    int[] numbers = new int[3];
    int value = numbers[5]; // Throws IndexOutOfRangeException
}
catch (IndexOutOfRangeException ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}

```

**InvalidCastException:**

Happens when explicit conversion from a base type to an interface or derived type fails at runtime.

```csharp
try
{
    object obj = "Hello";
    int intValue = (int)obj; // Throws InvalidCastException
}
catch (InvalidCastException ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}

```

**NullReferenceException:**

Raised when referencing a null object.

```csharp
try
{
    byte[] bigArray = new byte[int.MaxValue]; // Throws OutOfMemoryException
}
catch (OutOfMemoryException ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}

```

**OutOfMemoryException:**

Indicates memory allocation failure using the new operator.

```csharp
try
{
    byte[] bigArray = new byte[int.MaxValue]; // Throws OutOfMemoryException
}
catch (OutOfMemoryException ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}

```

**OverflowException:**

Arises during arithmetic operations in a checked context when overflow occurs.

```csharp
try
{
    byte value = checked((byte)(200 + 200)); // Throws OverflowException
}
catch (OverflowException ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}

```

**StackOverflowException:**

Indicates exhausted execution stack due to deep or infinite recursion.

```csharp
void RecursiveMethod()
{
    RecursiveMethod(); // Throws StackOverflowException
}

```

**TypeInitializationException:**

Thrown when a static constructor fails without a compatible catch clause.

```csharp
class MyClass
{
    static MyClass()
    {
        throw new Exception("Static constructor failed.");
    }
}

try
{
    var instance = new MyClass(); // Throws TypeInitializationException
}
catch (TypeInitializationException ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}

```
---











## Conventions:

### Naming Rules:
- Identifiers must start with a letter or underscore (_).
- They can contain Unicode letter characters, decimal digit characters, connecting characters, combining characters, or formatting characters.
- You can use the @ prefix to declare identifiers that match C# keywords.
- For a complete definition of valid identifiers, refer to the C# Language Specification.

### Naming Conventions:
- By convention, use PascalCase for type names, namespaces, and public members.
- Interface names start with a capital I.
- Attribute types end with the word Attribute.
- Enum types use singular nouns for non-flags and plural nouns for flags.
- Avoid consecutive underscores (__) in identifiers (reserved for compiler-generated names).
- Choose meaningful and descriptive names.
- Use PascalCase for class and method names, and camelCase for method parameters and local variables.
- Private instance fields start with an underscore (_).
- Static fields start with s_.
- Avoid single-letter names except for simple loop counters.

### Namespace Naming:
- Use meaningful and descriptive namespaces.
- Follow a hierarchical structure (e.g., MyApp.Services, MyApp.Models).
- Braces and Indentation:
- Use K&R style (opening brace on the same line as the method/class).
- Indent code blocks consistently (usually 4 spaces or a tab).

### Comments and Documentation:
- Document public APIs using XML comments (///).
- Explain the purpose of classes, methods, and parameters.
- Avoid excessive comments within methods (code should be self-explanatory).

### Using Directives:
- Organize using directives alphabetically.
- Remove unnecessary using statements.

### Constants and Readonly Fields:
- Use const for compile-time constants.
- Prefer readonly for runtime constants (initialized at runtime).

### Avoid Magic Numbers and Strings:
- Define constants or enums instead of hardcoding values.
- Avoid using raw strings directly in code.

### Exception Handling:
- Use meaningful exception types.
- Handle exceptions appropriately (try-catch blocks).

**References:**
- https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/style-rules/
- https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/code-style-rule-options
- https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions

---














## LINQ

### Unified Querying:
LINQ provides a consistent way to query and manipulate data from collections, databases, XML, and other sources.
Developers can use a single querying interface for various data types.

### Syntax Similar to SQL:
LINQ queries resemble SQL queries, making them easy to read and understand.
You express queries using familiar language constructs.

### Strongly Typed:
Variables in LINQ queries are strongly typed.

```csharp
// Sample data source (a sequence of strings)
        List<string> names = new List<string> { "Alice", "Bob", "Charlie" };
```

Queries are not executed until you iterate over the result (e.g., in a foreach loop).

```csharp
// LINQ query: Select names starting with 'B'
        var query = from name in names
                    where name.StartsWith("B")
                    select name;

        // Query is not executed until we iterate over the result
        foreach (var result in query)
        {
            Console.WriteLine(result);
        }
```

### Multiple LINQ Providers:
LINQ supports different data sources:
LINQ to Objects: Querying in-memory collections.
LINQ to SQL: Querying relational databases.
LINQ to XML: Querying XML data.

### Examples:

**Query Syntax:**

```csharp
int[] numbers = { 5, 10, 8, 3, 6, 12 };
var evenNumbersQuery = from num in numbers
                       where num % 2 == 0
                       select num;
foreach (var num in evenNumbersQuery)
{
    Console.Write(num + " ");
}

```

**Method Syntax:**

```csharp
var evenNumbersMethod = numbers.Where(num => num % 2 == 0);
foreach (var num in evenNumbersMethod)
{
    Console.Write(num + " ");
}

```

---



















## Asynchronous

### Async and Await Keywords:
- Use the async keyword to mark a method as asynchronous.
- The await keyword allows you to wait for asynchronous operations without blocking the thread.
- Together, they simplify writing code that appears sequential but executes asynchronously.
### Task Asynchronous Programming (TAP):
- TAP provides an abstraction over asynchronous code.
- You write code as a sequence of statements, but some statements return a Task representing ongoing work.
- The goal is to enable code that reads like a sequence of statements but executes in a more complex order based on external resource allocation and task completion.
- Think of it like following instructions for making breakfast asynchronously: start tasks (e.g., warming a pan), then move on to other tasks (frying bacon) while they run concurrently.
### Async and Await Keywords:
- Use the async keyword to mark a method as asynchronous.
- The await keyword allows you to wait for asynchronous operations without blocking the thread.
- Together, they simplify writing code that appears sequential but executes asynchronously.
### I/O-Bound vs. CPU-Bound:
- Asynchronous programming is essential for I/O-bound tasks (e.g., reading files, making network requests).
- For CPU-bound tasks (heavy computations), consider parallelism (multiple threads).
### Benefits of Asynchronous Programming:
- Improved responsiveness: The main thread remains free to handle other tasks.
- Efficient resource utilization: Waiting for I/O doesn’t block threads.
- Scalability: Asynchronous code can handle many concurrent operations.

**Async Method Example**

```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        await DoWorkAsync();
        Console.WriteLine("Work completed!");
    }

    static async Task DoWorkAsync()
    {
        await Task.Delay(2000); // Simulate async work (e.g., I/O)
        Console.WriteLine("Async work done!");
    }
}

```

**Parallel Execution**

```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        var task1 = Task.Run(() => DoTask("Task 1"));
        var task2 = Task.Run(() => DoTask("Task 2"));

        await Task.WhenAll(task1, task2);
        Console.WriteLine("All tasks completed!");
    }

    static void DoTask(string name)
    {
        Console.WriteLine($"Starting {name}");
        Task.Delay(1000).Wait(); // Simulate work
        Console.WriteLine($"{name} completed");
    }
}

```

**Async and Await in UI Applications**

```csharp
using System;
using System.Threading.Tasks;
using System.Windows;

namespace AsyncWpfApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private async void StartButton_Click(object sender, RoutedEventArgs e)
        {
            await LongRunningTaskAsync();
            ResultTextBlock.Text = "Task completed!";
        }

        private async Task LongRunningTaskAsync()
        {
            await Task.Delay(3000); // Simulate work
        }
    }
}

```

---












```csharp

```





## Memory Management:

**Memory Compaction:**

Memory compaction happens during garbage collection.
If a collection finds many unreachable objects, it compacts memory.
Compaction rearranges objects to minimize fragmentation.
Large objects (e.g., arrays) are allocated separately to avoid fragmentation.

**Large Object Heap (LOH) Compaction:**

LOH contains large objects (e.g., arrays > 85 KB).
Compacting LOH can be expensive.
You can manually compact LOH using GCLargeObjectHeapCompactionMode.CompactOnce.
However, it’s typically done automatically during full garbage collections.

**Logical Address:**

It is a virtual address generated by the CPU while a program is running. It is referred to as a virtual address because it does not exist physically. Using this address, the CPU access the actual address or physical address inside the memory, and data is fetched from there.

**Physical Address**

Physical Address is the actual address of the data inside the memory. The logical address is a virtual address and the program needs physical memory for its execution. The user never deals with the Physical Address. The user program generates the logical address and is mapped to the physical address by the Memory Management Unit(MMU).

The hardware device called Memory Management Unit (MMU) is used for mapping this logical address to the physical address. The set of all logical addresses generated by the CPU for a program is called the logical address space.


<p align="center">
<img src = "https://wat-user-images.s3.amazonaws.com/1634560374197%2FN6hj9g0G6p%2FScreenshot_(324).png" width=700 height=400>
</p>


### Memory Management in C# and .NET:
Memory management is crucial for efficient software development. In C# and .NET, automatic memory management is provided by the Common Language Runtime (CLR).

When a garbage collection is triggered, the garbage collector reclaims the memory that's occupied by dead objects. The reclaiming process compacts live objects so that they're moved together, and the dead space is removed, thereby making the heap smaller. This process ensures that objects that are allocated together stay together on the managed heap to preserve their locality.

### States: ###

**Free:**	The block of memory has no references to it and is available for allocation.

**Reserved**	The block of memory is available for your use and can't be used for any other allocation request. However, you can't store data to this memory block until it's committed.

**Committed**	The block of memory is assigned to physical storage.


**Benefits:**

1. Frees developers from having to manually release memory.
2. Allocates objects on the managed heap efficiently.
3. Reclaims objects that are no longer being used, clears their memory, and keeps the memory available for future allocations. Managed objects automatically get clean content to start with, so their constructors don't have to initialize every data field.
4. Provides memory safety by making sure that an object can't use for itself the memory allocated for another object.


<p align="center">
<img src = "https://www.telerik.com/sfimages/default-source/blogs/7figure-2-png" width=700 height=400>
</p>

**Garbage Collector (GC):**

1. The CLR’s garbage collector automatically manages memory for managed applications.
2. Developers don’t need to manually allocate or free memory; the GC handles it.
3. This eliminates common issues like memory leaks or accessing freed memory.

**Managed Heap:**

1. When a process starts, the runtime reserves a contiguous region called the “managed heap.”
2. All reference types (objects) are allocated on this heap.
3. Objects are allocated consecutively, and their memory is contiguous.

**Allocating Memory:**

1. When you create a reference type (e.g., an object), memory is allocated on the managed heap.
2. The GC maintains a pointer to the next available memory location.
3. New objects are allocated sequentially, as long as address space is available.

**Releasing Memory:**

1. The GC determines when to perform a collection based on allocations.
2. It examines the application’s roots (e.g., static fields, local variables) to find reachable objects.
3. Unreachable objects are released, freeing memory.

```csharp
using System;

class Program
{
    static void Main()
    {
        // Allocate memory for an object
        MyClass myObject = new MyClass();
        
        // Use myObject...
        
        // No need to explicitly free memory; GC handles it
    }
}

class MyClass
{
    // Class definition
}
```


### Generations: ###

The garbage collector (GC) divides memory into three generations: 0, 1, and 2.
Each generation represents different object lifetimes.

**Generation 0 (Youngest):**

Contains short-lived objects (e.g., temporary variables).
Frequent garbage collection occurs here.
Objects that survive move to generation 1.

**Generation 1 (Buffer):**

Serves as a buffer between short-lived and long-lived objects.
Promotes objects with longer lifetimes.
If not enough memory, GC collects gen 1 and then gen 2.

**Generation 2 (Long-Lived):**

Holds long-lived objects (e.g., server data).
Surviving objects remain here.
Large objects (LOH) are also collected here.


### Performance considerations: ### 

**Workstation GC:**

Used for client apps (e.g., desktop apps).
Runs on the user thread that triggered it.
Competes with other threads for CPU time.
Always used on single-CPU computers.

**Server GC:**
For server apps (high throughput).
Multiple dedicated threads (high priority).
Each logical CPU has a heap.
Faster than workstation GC.
Can be resource-intensive.

**Differences:**

Background workstation garbage collection uses one dedicated background garbage collection thread, whereas background server garbage collection uses multiple threads. Typically, there's a dedicated thread for each logical processor.

Unlike the workstation background garbage collection thread, the background server GC threads do not time out.

<p align="center">
<img src = "https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/media/fundamentals/background-workstation-garbage-collection.png" width=700 height=400>
</p>


<p align="center">
<img src = "https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/media/fundamentals/background-server-garbage-collection.png" width=700 height=400>
</p>

### Generations and Heap Segments:
Imagine the managed heap as a storage area for objects in a .NET application. 

Objects are grouped into different generations: 0, 1, and 2.

The heap is divided into segments: Small Object Heap (SOH) and Large Object Heap (LOH).

If an object is small (less than 85,000 bytes), it goes to the SOH segment.

If an object is large (85,000 bytes or more), it goes to the LOH segment.

### Object Promotion: ### 
Objects survive garbage collection (GC) cycles.

Survivors from generation 0 become generation 1 objects.

Survivors from generation 1 become generation 2 objects.

Objects in the oldest generation (gen2) remain there.

### Large Object Heap (LOH): ###

The LOH stores large objects.

Compacting the LOH (rearranging objects) is expensive.

Instead, the GC sweeps the LOH, creating a free list of dead objects.

These dead objects can be reused for new large object allocations

<p align="center">
<img src = "https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/media/loh/loh-figure-1.jpg" width=700 height=400>
</p>

<p align="center">
<img src = "https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/media/loh/loh-figure-2.jpg" width=700 height=400>
</p>

**References:**
 - https://learn.microsoft.com/en-us/dotnet/standard/automatic-memory-management
 - https://learn.microsoft.com/en-us/dotnet/api/system.gc?view=net-8.0
 - https://workat.tech/core-cs/tutorial/logical-and-physical-address-os-8abv46w3k0bu#:~:text=Physical%20Address%20is%20the%20actual,deals%20with%20the%20Physical%20Address.
 - https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)
 - https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/
 - https://www.telerik.com/blogs/understanding-net-garbage-collection
 - https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/background-gc


---











## Libraries
 - https://stackoverflow.com/questions/807880/bcl-base-class-library-vs-fcl-framework-class-library
 - https://learn.microsoft.com/en-us/dotnet/api/system?view=net-8.0
 - https://learn.microsoft.com/en-us/dotnet/standard/class-library-overview

### Base Class Library (BCL): ###
The BCL contains fundamental types like System.String and System.DateTime.
It’s the foundation—basic building blocks for .NET applications.

### Framework Class Library (FCL): ###
The FCL is broader—it includes the BCL.
Contains libraries for ASP.NET, WinForms, XML handling, ADO.NET, and more.
Developers use FCL classes to simplify their work.

### Runtime Libraries: ### 
These are part of the .NET runtime.
They provide essential functionality for executing .NET applications.
Think of them as the toolbox for developers.

**System namespace:**

The System namespace is the root namespace for fundamental types in .NET. This namespace includes classes that represent the base data types used by all applications, for example, Object (the root of the inheritance hierarchy), Byte, Char, Array, Int32, and String. Many of these types correspond to the primitive data types that your programming language uses. When you write code using .NET types, you can use your language's corresponding keyword when a .NET base data type is expected.

---













## CLR:

<p align="center">
<img src = "https://media.geeksforgeeks.org/wp-content/uploads/Overview-of-the-.NET-Framework-min.png" width=700 height=400>
</p>

<p align="center">
<img src = "https://media.geeksforgeeks.org/wp-content/uploads/Working_CLR.jpg" width=700 height=400>
</p>

**Compile Time:**

C#: C# (pronounced “C-sharp”) is a high-level programming language developed by Microsoft. It’s commonly used for building Windows applications, web services, and more.

CIL (Common Intermediate Language): When you write C# code, it gets compiled into an intermediate language called CIL. Think of CIL as a platform-independent representation of your code. It’s similar to assembly language but at a higher level.

**.NET Framework:**

The .NET Framework is a set of libraries and tools provided by Microsoft. It includes a large collection of pre-built classes and functions that developers can use to build applications.
When you write C# code, you often rely on these .NET libraries to perform common tasks like file I/O, networking, and database access.

**Runtime:**

CLR (Common Language Runtime): The CLR is a crucial component of the .NET Framework. It takes the compiled CIL code and turns it into native machine code that your computer can execute directly.
The CLR also manages memory, handles exceptions, and provides other runtime services. It ensures that your C# application runs smoothly and efficiently.


**References:**
- https://barcelonageeks.com/common-language-runtime-clr-en-c-1/

---









## Assembly:


**References:**
- https://learn.microsoft.com/en-us/dotnet/standard/assembly/
- https://www.youtube.com/watch?v=GHNrje08gQM
- https://www.youtube.com/watch?v=lx2tSY4joDg&t=330s

---







## DLL EXE:

- DLL (Dynamic Link Library):
  
  A DLL is a shared library that contains reusable code, functions, and classes.
  
  It cannot be directly executed; instead, it needs a host executable (usually an EXE) to run.
  
  DLLs are designed for code reusability across multiple applications.
  
  When a DLL is loaded, it becomes part of the address space of an existing process.
  
  Common use cases for DLLs include utility functions, custom controls, and database access.
  
  The file format of DLLs and EXEs is essentially the same, but Windows distinguishes them through the PE (Portable Executable) header in the file.
  
  
- EXE (Executable):
  
  An EXE file is an independent executable program that runs in its own address space.
  
  It launches a separate application process.
  
  When you run an EXE, it typically starts a new process with its own memory space.
  
  EXEs are used for standalone applications, such as desktop applications or console programs.
  
  The entry point for an EXE is the main method or function.
  
  EXEs can be executed directly by the operating system.

- https://www.geeksforgeeks.org/creating-and-using-dll-class-library-in-c/
- https://stackoverflow.com/questions/3672920/two-different-dll-with-same-namespace



---



## Attribute:

  Attributes provide a way to associate metadata or additional information with code elements. They allow me to add declarative information to classes, methods, properties, and other program entities.

  Attributes are a powerful way to add metadata and behavior to your code, and they play a crucial role in various scenarios such as serialization, validation, and more

  - Built-in Attributes:

    C# provides several built-in attributes that you can use directly.

    ```csharp
      [Serializable] // Marks a class as serializable
      public class SampleClass
      {
          // Class implementation...
      }
    ```
  - Creating Custom Attributes:
    
    You can also create your own custom attributes by defining a new class that inherits from the Attribute base class.
    
    Custom attributes allow you to add specific behavior or information to your code.

    ```csharp
      [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method)]
      public class MyCustomAttribute : Attribute
      {
          public string Description { get; }
      
          public MyCustomAttribute(string description)
          {
              Description = description;
          }
      }
    ```
    
  - Applying Custom Attributes:
    
    To apply an attribute, use square brackets ([]) above the declaration of the entity (class, method, etc.) to which it applies.

    ```csharp
      [MyCustom("This is a custom attribute")]
      public class MyClass
      {
          // Class implementation...
      }

    ```
   - Retrieving Attribute Information at Runtime:

     You can retrieve attribute information programmatically using reflection.

     ```csharp
      var attribute = typeof(MyClass).GetCustomAttributes(typeof(MyCustomAttribute), false)
                                .FirstOrDefault() as MyCustomAttribute;
      if (attribute != null)
      {
          Console.WriteLine($"Description: {attribute.Description}");
      }

     ```

**References:**   
  - https://www.youtube.com/watch?v=4ZfFI4zV1Wc
  - https://learn.microsoft.com/en-us/dotnet/csharp/advanced-topics/reflection-and-attributes/


---









## Property vs Field:

**References:**
  - https://stackoverflow.com/questions/4142867/what-is-the-difference-between-a-property-and-a-variable
  - https://www.youtube.com/watch?v=UyS3ppdT8I4

---









## Sln

**References:**
  - https://stackoverflow.com/questions/7133796/what-are-sln-and-vcproj-files-and-what-do-they-contain
  - https://www.youtube.com/watch?v=L2HCvO8dGVg
  - https://www.reddit.com/r/csharp/comments/10culvq/noob_question_sln_files/


---







## Auto Implemented Properties


**References:**
  - https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/auto-implemented-properties
  - https://dev.to/cwl157/6-options-to-implement-auto-implemented-properties-a8o


---










## Anonymous Types:
 ### What are Anonymous Types?
  Anonymous types are nameless types that allow you to bundle a set of read-only properties into a single object without explicitly defining a type beforehand.
  These types are particularly useful when you want to create a temporary object with specific properties without having to create a formal class or struct.
  
  - Creating an Anonymous Type:
  To create an anonymous object, use the new keyword along with an object initializer (curly braces {}) to specify the desired properties.
  The compiler infers the type name, and you don’t need to explicitly declare it.
  
  Example:

  ```csharp
  var person = new { Name = "Alice", Age = 30 };
  ```

 ### Usage Scenarios:
  Query Expressions: Anonymous types are commonly used in the select clause of LINQ query expressions to return a subset of properties from each object in a source sequence.
  Reducing Data: When querying data, using anonymous types allows you to return only the necessary properties, reducing the amount of data retrieved.
  Read-Only Properties: Anonymous types contain only public read-only properties. No other members (methods, events, etc.) are allowed.
  Property Initialization: You can initialize properties from other types. 
  
  For example:

  ```csharp
  var products = new List<Product>(); // Assume Product class has Color and Price properties
  var productQuery = from prod in products
                     select new { prod.Color, prod.Price };
  ```

Anonymous types are lightweight and convenient for short-lived scenarios.
They cannot contain functions, constructors, or other class members.
In summary, anonymous types provide a quick way to create small, temporary objects with specific properties. They’re especially handy when working with LINQ queries or when I need a simple data structure on the fly! 

**References:**

---















## Dependency Injection:

**References:**
 - https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection
 - https://www.reddit.com/r/csharp/comments/ru9chz/can_you_explain_what_dependency_injection_is/
 - https://www.youtube.com/watch?v=Hhpq7oYcpGE&t=2337s
 - https://www.youtube.com/watch?v=iQ8cNI7a6mk


---
















## Extension Methods:




**References:**
 - https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods
 - https://www.tutorialsteacher.com/csharp/csharp-extension-method


---













## Scoped Ref:

### What Is a Ref Struct?

A ref struct is a special type in C# that lives on the stack (rather than the heap) and is designed for performance-critical scenarios.
It’s often used for things like buffers, memory pools, or other low-level operations.

### The Problem with Passing Ref Structs

When you pass a ref struct as an argument to a method, there’s a concern about its lifetime.
If the method captures or returns the ref struct, it could lead to unexpected behavior because the lifetime of the ref struct isn’t guaranteed.

### Enter the Scoped Keyword

The scoped keyword was introduced in C# 11 to address this issue.
When you mark a ref struct parameter as scoped, it means that the method can accept a locally declared ref struct without capturing it or returning it.
In other words, the method won’t hold onto the ref struct beyond its own scope.

### Use Cases for Scoped Ref

Imagine you have a ref struct representing a buffer, and you want to pass it to a method for processing.
By using scoped, you ensure that the method won’t accidentally capture the buffer or return it, avoiding lifetime issues.

**References:**
---













## Stream vs Buffer:



**References:**
- https://stackoverflow.com/questions/43935608/difference-between-buffer-stream-in-c-sharp
- https://learn.microsoft.com/en-us/dotnet/api/system.io.stream?view=net-8.0
- https://learn.microsoft.com/en-us/dotnet/api/system.buffer?view=net-8.0


---











## Ref Struct:
### What is a ref struct?
A ref struct is a special type of structure (or struct) in C# that has some unique characteristics:

Instances of a ref struct are allocated on the stack rather than the managed heap.
They cannot escape to the managed heap, ensuring that they remain within the stack frame.
The purpose of ref struct is to allow types that can only exist on the stack, improving memory efficiency and performance.

### Why is it important?
**Memory Safety**: By restricting ref struct instances to the stack, C# ensures memory safety. These types cannot be accidentally moved to the heap, preventing potential memory leaks or unsafe behavior.

**Performance**: Since ref struct instances reside on the stack, they avoid the overhead of heap allocation and garbage collection. This makes them more efficient in terms of memory usage and access time.

**Span and ReadOnlySpan**: One of the most common use cases for ref struct is with Span<T> and ReadOnlySpan<T>. These types allow efficient manipulation of contiguous memory regions (e.g., arrays, buffers) without unnecessary copying. Both Span<T> and ReadOnlySpan<T> are implemented as ref struct types1.

**Stack-Only Behavior**: By using ref struct, you can enforce stack-only behavior for certain types. This is particularly useful when dealing with low-level memory operations or when you want to avoid heap allocations.

**Avoiding Heap Pressure**: When working with large datasets or performance-critical code, using ref struct can help reduce heap pressure and improve overall application performance.

Example: Defining a ref struct

```csharp
public ref struct CustomRef
{
    public bool IsValid;
    public Span<int> Inputs;
    public Span<int> Outputs;
}
```

Readonly ref struct

```csharp
public readonly ref struct ConversionRequest
{
    public ConversionRequest(double rate, ReadOnlySpan<double> values)
    {
        Rate = rate;
        Values = values;
    }

    public double Rate { get; }
    public ReadOnlySpan<double> Values { get; }
}
```

**References:**

---















## Primary Consturctors:
primary constructors were introduced in version 12. They provide a concise syntax for declaring constructors whose parameters are available throughout the body of a type.

**Declaration:**

1. Can add parameters to a class or struct declaration to create a primary constructor.
2. Primary constructor parameters are in scope throughout the class definition.
3. They are similar to method parameters but have specific rules.

**Rules for Primary Constructor Parameters:**

1. They may not be stored if not needed.
2. They aren’t members of the class.
3. They can be assigned to.
4. They don’t become properties (except in record types).

**Common Uses:**

1. As arguments to a base() constructor invocation.
2. To initialize member fields or properties.
3. Referencing them in instance members.

**Record Type with Primary Constructor**
```csharp
public record Person(string FirstName, string LastName);
```

**Class with Primary Constructor and Initialization**
```csharp
public class Point
{
    public double X { get; }
    public double Y { get; }

    public Point(double x, double y) => (X, Y) = (x, y);
}
```

**Struct with Primary Constructor and Computed Properties**
```csharp
public readonly struct Circle
{
    public double Radius { get; }
    public double Area => Math.PI * Radius * Radius;

    public Circle(double radius) => Radius = radius;
}
```

**References:**

---


















## AOT:
**Ahead-of-Time (AOT) compilation** in the context of C# refers to a process where C# code is compiled to native machine code before the application runs.

**Purpose:**

AOT compilation contrasts with the traditional method where code is compiled to native code during runtime (Just-In-Time compilation, or JIT).
AOT aims to improve startup time and reduce memory overhead.

**How It Works:**

During AOT, the C# code is compiled to native machine code on the developer’s machine.
The resulting native binary can run directly without relying on a JIT compiler during execution.

**Benefits:**

Faster Startup: AOT-compiled apps start more quickly because there’s no JIT compilation delay.
Smaller Memory Footprint: Native binaries are often smaller than their JIT-compiled counterparts.
Restricted Environments: AOT apps can run in environments where JIT compilation isn’t allowed.

**Limitations:**
AOT support varies across different versions of .NET.
Not all libraries are fully compatible with AOT.
Some platform-specific limitations exist.

**References:**

---











## Class vs Struct:
### Reference Types (Classes):
**Lives on the heap**: Objects of reference types (instances of classes) are allocated on the heap.

**Contains a reference**: A variable of a reference type holds a reference (or pointer) to the actual data stored elsewhere in memory.

**Nullable**: Reference types can be null.

**Used for complex scenarios**: Classes are suitable for more complex data structures, inheritance, and polymorphism.
Can have default constructors and destructors.

### Value Types (Structs):
**Lives inline:** Structs are value types, and their instances are stored directly where the variable or field is defined (usually on the stack).

**Contains the entire value**: A variable of a value type contains the entire value type value.

**Non-nullable**: Value types cannot be null.

**Used for lightweight data**: Structs are ideal for small, lightweight data structures (e.g., coordinates, simple data).
Cannot have default constructors or destructors


**References:**


---




## Glossaries:

1. Buffer: a sequence of bytes in memory
2. Buffering: manipulation of that data
3. Primitive Types: the most basic data types that the language supports and the compiler predefines
4. Offset: integer that represents the distance between the beginning of an array or data structure object and a specific point or element


