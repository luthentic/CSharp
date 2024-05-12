


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


## Sln File

