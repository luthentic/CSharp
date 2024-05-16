


## 🚀 Introduction

My personal on going study project about C# and .NET 

<p align="center">
  <img src = "https://miro.medium.com/max/2728/1*7I6oONv2fGLQJcNEFA4QSw.png" width=800 height=300>
</p>

## 🚩 Table of Contents

- [CLR](#clr)
- [Assembly](#assembly)
- [DLL EXE](dll-exe)
- [Attribute](#attribute)
- [Property vs Field](property-vs-field)
- [Sln](sln)
- [Auto Implemented Properties](auto-implemented-properties)
- [Dependency Injection](dependency-injection)
- [Extension Methods](extension-methods)
- [Stream vs Buffer](stream-vs-buffer)
- [Ref Struct](ref-struct)
- [Dynamic Type](dynamic-type)
- [Records](records)
- [Conventions](conventions)

---

## CLR 
**Compile Time:**

C#: C# (pronounced “C-sharp”) is a high-level programming language developed by Microsoft. It’s commonly used for building Windows applications, web services, and more.

CIL (Common Intermediate Language): When you write C# code, it gets compiled into an intermediate language called CIL. Think of CIL as a platform-independent representation of your code. It’s similar to assembly language but at a higher level.

**.NET Framework:**

The .NET Framework is a set of libraries and tools provided by Microsoft. It includes a large collection of pre-built classes and functions that developers can use to build applications.
When you write C# code, you often rely on these .NET libraries to perform common tasks like file I/O, networking, and database access.

**Runtime:**

CLR (Common Language Runtime): The CLR is a crucial component of the .NET Framework. It takes the compiled CIL code and turns it into native machine code that your computer can execute directly.
The CLR also manages memory, handles exceptions, and provides other runtime services. It ensures that your C# application runs smoothly and efficiently.

## Assembly:

- https://learn.microsoft.com/en-us/dotnet/standard/assembly/
- https://www.youtube.com/watch?v=GHNrje08gQM
- https://www.youtube.com/watch?v=lx2tSY4joDg&t=330s

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
     
  - https://www.youtube.com/watch?v=4ZfFI4zV1Wc
  - https://learn.microsoft.com/en-us/dotnet/csharp/advanced-topics/reflection-and-attributes/
     
## Property vs Field:

  - https://stackoverflow.com/questions/4142867/what-is-the-difference-between-a-property-and-a-variable
  - https://www.youtube.com/watch?v=UyS3ppdT8I4

## Sln File

  - https://stackoverflow.com/questions/7133796/what-are-sln-and-vcproj-files-and-what-do-they-contain
  - https://www.youtube.com/watch?v=L2HCvO8dGVg
  - https://www.reddit.com/r/csharp/comments/10culvq/noob_question_sln_files/

## Auto Implemented Properties

  - https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/auto-implemented-properties
  - https://dev.to/cwl157/6-options-to-implement-auto-implemented-properties-a8o

## Anonymous Types
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


## Dependency Injection:
 - https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection
 - https://www.reddit.com/r/csharp/comments/ru9chz/can_you_explain_what_dependency_injection_is/
 - https://www.youtube.com/watch?v=Hhpq7oYcpGE&t=2337s
 - https://www.youtube.com/watch?v=iQ8cNI7a6mk

## Extension Methods:
 - https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods
 - https://www.tutorialsteacher.com/csharp/csharp-extension-method

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

## Stream vs Buffer:
- https://stackoverflow.com/questions/43935608/difference-between-buffer-stream-in-c-sharp
- https://learn.microsoft.com/en-us/dotnet/api/system.io.stream?view=net-8.0
- https://learn.microsoft.com/en-us/dotnet/api/system.buffer?view=net-8.0

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

## Dynamic Type:
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

## Records:
- https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/records
- https://www.csharptutorial.net/csharp-tutorial/csharp-record/



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

## Conventions:
- https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/style-rules/
- https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/code-style-rule-options
- https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions


