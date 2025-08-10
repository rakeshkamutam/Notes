# C# Object-Oriented Programming (OOP) Notes

## Table of Contents
1. [Introduction to OOP](#introduction-to-oop)
2. [Classes and Objects](#classes-and-objects)
3. [Encapsulation](#encapsulation)
4. [Inheritance](#inheritance)
5. [Polymorphism](#polymorphism)
6. [Abstraction](#abstraction)
7. [Interfaces](#interfaces)
8. [Constructors](#constructors)
9. [Static Members](#static-members)
10. [Method Overloading](#method-overloading)
11. [Method Overriding](#method-overriding)
12. [Access Modifiers](#access-modifiers)
13. [Properties](#properties)
14. [Sealed Classes and Methods](#sealed-classes-and-methods)

***

## Introduction to OOP

Object-Oriented Programming (OOP) is a programming paradigm that organizes code into **objects** containing both data and methods. It improves code reusability, maintainability, and modularity.

### The Four Pillars of OOP:
1. **Encapsulation** - Hiding internal details and controlling access
2. **Inheritance** - Creating new classes based on existing ones
3. **Polymorphism** - Same interface, different implementations
4. **Abstraction** - Showing only essential features

```csharp
// Basic OOP example
public class Animal
{
    public string Name { get; set; }
    public virtual void MakeSound() => Console.WriteLine("Animal makes a sound");
}
```

[⬆ Back to Top](#table-of-contents)

***

## Classes and Objects

A **class** is a blueprint that defines the structure and behavior of objects. An **object** is an instance of a class.

### Key Concepts:
- **Class** = Template/Blueprint
- **Object** = Instance created from the class
- **Fields** = Variables that store data
- **Methods** = Functions that define behavior

```csharp
public class Car
{
    // Fields
    public string Brand;
    public string Model;
    
    // Method
    public void Start()
    {
        Console.WriteLine($"{Brand} {Model} is starting...");
    }
}

// Creating objects
Car myCar = new Car();
myCar.Brand = "Toyota";
myCar.Model = "Camry";
myCar.Start(); // Output: Toyota Camry is starting...
```

[⬆ Back to Top](#table-of-contents)

***

## Encapsulation

Encapsulation means hiding the internal state of an object and only allowing access through public methods or properties. This protects data from unauthorized access and maintains object integrity.

### Benefits:
- **Data Protection** - Prevents direct access to sensitive data
- **Validation** - Controls how data is modified
- **Maintainability** - Changes to internal implementation don't affect external code

```csharp
public class BankAccount
{
    private decimal balance; // Private field - hidden from outside
    
    public decimal Balance => balance; // Read-only property
    
    public void Deposit(decimal amount)
    {
        if (amount > 0)
            balance += amount;
    }
    
    public bool Withdraw(decimal amount)
    {
        if (amount > 0 && amount  Console.WriteLine($"{Name} is eating");
}

// Derived class
public class Dog : Animal
{
    public void Bark() => Console.WriteLine($"{Name} is barking");
}

// Usage
Dog myDog = new Dog { Name = "Buddy" };
myDog.Eat();  // Inherited from Animal
myDog.Bark(); // Dog's own method
```

[⬆ Back to Top](#table-of-contents)

***

## Polymorphism

Polymorphism allows objects of different types to be treated as objects of a common base type. The same method can behave differently depending on the object that calls it.

### Types:
- **Runtime Polymorphism** - Method overriding with virtual/override
- **Compile-time Polymorphism** - Method overloading

```csharp
public class Animal
{
    public virtual void MakeSound() => Console.WriteLine("Some sound");
}

public class Dog : Animal
{
    public override void MakeSound() => Console.WriteLine("Woof!");
}

public class Cat : Animal
{
    public override void MakeSound() => Console.WriteLine("Meow!");
}

// Polymorphic behavior
Animal[] animals = { new Dog(), new Cat() };
foreach (Animal animal in animals)
{
    animal.MakeSound(); // Calls appropriate override method
}
```

[⬆ Back to Top](#table-of-contents)

***

## Abstraction

Abstraction hides implementation details while showing only essential features. It's achieved using **abstract classes** and **interfaces**.

### Abstract Classes:
- Cannot be instantiated directly
- Can contain both abstract and concrete methods
- Abstract methods must be implemented by derived classes

```csharp
public abstract class Shape
{
    public abstract double CalculateArea(); // Must be implemented
    
    public void Display() // Concrete method
    {
        Console.WriteLine($"Area: {CalculateArea()}");
    }
}

public class Circle : Shape
{
    public double Radius { get; set; }
    
    public override double CalculateArea()
    {
        return Math.PI * Radius * Radius;
    }
}
```

[⬆ Back to Top](#table-of-contents)

***

## Interfaces

An interface defines a contract that implementing classes must follow. It contains method signatures without implementation (until C# 8.0).

### Key Features:
- All members are public by default
- No implementation (except default implementations in C# 8.0+)
- Classes can implement multiple interfaces
- Supports multiple inheritance of behavior

```csharp
public interface IDrawable
{
    void Draw();
    void Move(int x, int y);
}

public interface IPrintable
{
    void Print();
}

public class Rectangle : IDrawable, IPrintable
{
    public void Draw() => Console.WriteLine("Drawing rectangle");
    public void Move(int x, int y) => Console.WriteLine($"Moving to ({x}, {y})");
    public void Print() => Console.WriteLine("Printing rectangle");
}
```

[⬆ Back to Top](#table-of-contents)

***

## Constructors

Constructors are special methods used to initialize objects when they are created. They have the same name as the class and no return type.

### Types:
- **Default Constructor** - No parameters
- **Parameterized Constructor** - Takes parameters
- **Copy Constructor** - Creates object from another object
- **Static Constructor** - Initializes static members

```csharp
public class Student
{
    public string Name { get; set; }
    public int Age { get; set; }
    
    // Default constructor
    public Student()
    {
        Name = "Unknown";
        Age = 0;
    }
    
    // Parameterized constructor
    public Student(string name, int age)
    {
        Name = name;
        Age = age;
    }
}

// Usage
Student s1 = new Student(); // Uses default constructor
Student s2 = new Student("John", 20); // Uses parameterized constructor
```

[⬆ Back to Top](#table-of-contents)

***

## Static Members

Static members belong to the class itself rather than to any specific instance. They are shared among all instances of the class.

### Types:
- **Static Fields** - Shared variables
- **Static Methods** - Can be called without creating an instance
- **Static Properties** - Shared properties
- **Static Constructors** - Initialize static members

```csharp
public class Counter
{
    private static int count = 0; // Static field
    
    public static int Count => count; // Static property
    
    public static void Increment() // Static method
    {
        count++;
    }
    
    public Counter()
    {
        Increment(); // Called for each new instance
    }
}

// Usage
Console.WriteLine(Counter.Count); // 0
Counter c1 = new Counter();
Counter c2 = new Counter();
Console.WriteLine(Counter.Count); // 2
```

[⬆ Back to Top](#table-of-contents)

***

## Method Overloading

Method overloading allows multiple methods with the same name but different parameters in the same class.

### Rules:
- Same method name
- Different parameter list (number, type, or order)
- Return type alone cannot differentiate overloaded methods

```csharp
public class Calculator
{
    public int Add(int a, int b) => a + b;
    
    public double Add(double a, double b) => a + b;
    
    public int Add(int a, int b, int c) => a + b + c;
    
    public string Add(string a, string b) => a + b;
}

// Usage
Calculator calc = new Calculator();
int result1 = calc.Add(5, 3);        // Calls Add(int, int)
double result2 = calc.Add(5.5, 3.2); // Calls Add(double, double)
string result3 = calc.Add("Hello", " World"); // Calls Add(string, string)
```

[⬆ Back to Top](#table-of-contents)

***

## Method Overriding

Method overriding allows a derived class to provide a specific implementation of a method that is already defined in its base class.

### Requirements:
- Base class method must be `virtual`, `abstract`, or `override`
- Derived class method must use `override` keyword
- Method signature must be identical

```csharp
public class Vehicle
{
    public virtual void Start() => Console.WriteLine("Vehicle starting...");
}

public class Car : Vehicle
{
    public override void Start() => Console.WriteLine("Car engine starting...");
}

public class Motorcycle : Vehicle
{
    public override void Start() => Console.WriteLine("Motorcycle engine revving...");
}

// Usage
Vehicle[] vehicles = { new Car(), new Motorcycle() };
foreach (Vehicle vehicle in vehicles)
{
    vehicle.Start(); // Calls appropriate overridden method
}
```

[⬆ Back to Top](#table-of-contents)

***

## Access Modifiers

Access modifiers control the visibility of classes, methods, and other members.

| Modifier | Accessibility |
|----------|---------------|
| `public` | Accessible from anywhere |
| `private` | Accessible only within the same class |
| `protected` | Accessible within the same class and derived classes |
| `internal` | Accessible within the same assembly |
| `protected internal` | Accessible within the same assembly or derived classes |
| `private protected` | Accessible within derived classes in the same assembly |

```csharp
public class MyClass
{
    public int PublicField;        // Accessible everywhere
    private int privateField;      // Only within MyClass
    protected int protectedField;  // MyClass and derived classes
    internal int internalField;    // Same assembly
    
    public void PublicMethod() { }
    private void PrivateMethod() { }
    protected void ProtectedMethod() { }
}
```

[⬆ Back to Top](#table-of-contents)

***

## Properties

Properties provide a way to access private fields with additional logic for validation, calculation, or formatting.

### Types:
- **Auto-implemented Properties** - Simple get/set
- **Read-only Properties** - Only getter
- **Computed Properties** - Calculate value on access

```csharp
public class Person
{
    private string name;
    private int age;
    
    // Auto-implemented property
    public string Email { get; set; }
    
    // Property with validation
    public string Name
    {
        get => name;
        set => name = string.IsNullOrEmpty(value) ? "Unknown" : value;
    }
    
    // Read-only property
    public int Age => age;
    
    // Computed property
    public string AgeGroup => age = 0 && newAge  Console.WriteLine("Final implementation");
}

// Base class with virtual method
public class BaseClass
{
    public virtual void Display() => Console.WriteLine("Base display");
}

// Derived class with sealed override
public class DerivedClass : BaseClass
{
    public sealed override void Display() => Console.WriteLine("Sealed display");
}

// Cannot inherit from FinalClass or override Display in further derived classes
```

[⬆ Back to Top](#table-of-contents)

***

### Summary

These OOP concepts work together to create maintainable, reusable, and scalable code. Understanding these fundamentals is essential for effective C# programming and building robust applications.

**Key Takeaways:**
- **Encapsulation** protects data integrity
- **Inheritance** promotes code reuse
- **Polymorphism** enables flexible design
- **Abstraction** manages complexity
- **Proper access control** ensures security
- **Properties** provide controlled data access
