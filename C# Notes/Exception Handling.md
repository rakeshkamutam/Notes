# Exception Handling in C#

## What is Exception Handling?

Exception handling is a programming construct that allows you to catch and handle runtime errors gracefully instead of letting your application crash. It provides a structured way to deal with unexpected situations that occur during program execution.

## Basic Syntax

### Try-Catch Block
```csharp
try
{
    // Code that might throw an exception
}
catch (SpecificExceptionType ex)
{
    // Handle specific exception
}
catch (Exception ex)
{
    // Handle any other exception
}
```

### Try-Catch-Finally Block
```csharp
try
{
    // Code that might throw an exception
}
catch (Exception ex)
{
    // Handle exception
}
finally
{
    // This code always executes (cleanup code)
}
```

## Common Exception Types

| Exception Type | Description | When It Occurs |
|---|---|---|
| `ArgumentException` | Invalid argument passed to method | Wrong parameter value |
| `ArgumentNullException` | Null argument passed when not allowed | Passing null to non-nullable parameter |
| `IndexOutOfRangeException` | Array/collection index is invalid | Accessing array[1] when array has 5 elements |
| `NullReferenceException` | Attempting to use null reference | Calling method on null object |
| `DivideByZeroException` | Division by zero | Mathematical division by 0 |
| `FormatException` | String format is invalid | Converting "abc" to integer |
| `FileNotFoundException` | File not found | Opening non-existent file |
| `InvalidOperationException` | Operation not valid for current state | Calling method when object not ready |

## Practical Examples

### Example 1: Basic Exception Handling
```csharp
using System;

class Program
{
    static void Main()
    {
        try
        {
            Console.WriteLine("Enter a number:");
            int number = int.Parse(Console.ReadLine());
            
            Console.WriteLine("Enter another number:");
            int divisor = int.Parse(Console.ReadLine());
            
            int result = number / divisor;
            Console.WriteLine($"Result: {result}");
        }
        catch (FormatException)
        {
            Console.WriteLine("Error: Please enter a valid number.");
        }
        catch (DivideByZeroException)
        {
            Console.WriteLine("Error: Cannot divide by zero.");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"An unexpected error occurred: {ex.Message}");
        }
    }
}
```

### Example 2: Multiple Catch Blocks
```csharp
using System;
using System.IO;

public class FileProcessor
{
    public static void ProcessFile(string fileName)
    {
        try
        {
            string content = File.ReadAllText(fileName);
            int number = int.Parse(content);
            Console.WriteLine($"Number from file: {number}");
        }
        catch (FileNotFoundException)
        {
            Console.WriteLine($"File '{fileName}' was not found.");
        }
        catch (UnauthorizedAccessException)
        {
            Console.WriteLine($"Access denied to file '{fileName}'.");
        }
        catch (FormatException)
        {
            Console.WriteLine("File content is not a valid number.");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Unexpected error: {ex.Message}");
        }
    }
}
```

### Example 3: Finally Block Usage
```csharp
using System;
using System.IO;

public class DatabaseConnection
{
    public static void ProcessData()
    {
        FileStream fileStream = null;
        
        try
        {
            fileStream = new FileStream("data.txt", FileMode.Open);
            // Process file data
            Console.WriteLine("Processing file data...");
            
            // Simulate an error
            throw new InvalidOperationException("Simulated error");
        }
        catch (FileNotFoundException)
        {
            Console.WriteLine("Data file not found.");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error processing data: {ex.Message}");
        }
        finally
        {
            // Cleanup resources - this ALWAYS executes
            if (fileStream != null)
            {
                fileStream.Close();
                Console.WriteLine("File stream closed.");
            }
            Console.WriteLine("Cleanup completed.");
        }
    }
}
```

### Example 4: Using Statement (Automatic Cleanup)
```csharp
using System;
using System.IO;

public class FileHandler
{
    public static void ReadFileContent(string fileName)
    {
        try
        {
            // 'using' statement automatically disposes resources
            using (FileStream fs = new FileStream(fileName, FileMode.Open))
            using (StreamReader reader = new StreamReader(fs))
            {
                string content = reader.ReadToEnd();
                Console.WriteLine(content);
                
                // No need for finally block - resources auto-disposed
            }
        }
        catch (FileNotFoundException)
        {
            Console.WriteLine($"File '{fileName}' not found.");
        }
        catch (IOException ex)
        {
            Console.WriteLine($"IO Error: {ex.Message}");
        }
    }
}
```

## Custom Exceptions

### Creating Custom Exception Classes
```csharp
using System;

// Custom exception class
public class InsufficientBalanceException : Exception
{
    public decimal CurrentBalance { get; }
    public decimal RequestedAmount { get; }
    
    public InsufficientBalanceException(decimal currentBalance, decimal requestedAmount)
        : base($"Insufficient balance. Current: {currentBalance:C}, Requested: {requestedAmount:C}")
    {
        CurrentBalance = currentBalance;
        RequestedAmount = requestedAmount;
    }
    
    public InsufficientBalanceException(string message) : base(message) { }
    
    public InsufficientBalanceException(string message, Exception innerException) 
        : base(message, innerException) { }
}

// Using custom exception
public class BankAccount
{
    private decimal balance;
    
    public BankAccount(decimal initialBalance)
    {
        balance = initialBalance;
    }
    
    public void Withdraw(decimal amount)
    {
        if (amount > balance)
        {
            throw new InsufficientBalanceException(balance, amount);
        }
        
        balance -= amount;
        Console.WriteLine($"Withdrawal successful. New balance: {balance:C}");
    }
}

// Usage example
class Program
{
    static void Main()
    {
        BankAccount account = new BankAccount(100m);
        
        try
        {
            account.Withdraw(150m);
        }
        catch (InsufficientBalanceException ex)
        {
            Console.WriteLine($"Transaction failed: {ex.Message}");
            Console.WriteLine($"Available balance: {ex.CurrentBalance:C}");
            Console.WriteLine($"Requested amount: {ex.RequestedAmount:C}");
        }
    }
}
```

## Nested Try-Catch Blocks

```csharp
using System;

public class NestedExceptionExample
{
    public static void ProcessData()
    {
        try
        {
            Console.WriteLine("Starting outer operation...");
            
            try
            {
                Console.WriteLine("Starting inner operation...");
                // This will throw an exception
                int result = 10 / 0;
            }
            catch (DivideByZeroException ex)
            {
                Console.WriteLine($"Inner catch: {ex.Message}");
                // Re-throw the exception to outer catch
                throw new InvalidOperationException("Inner operation failed", ex);
            }
        }
        catch (InvalidOperationException ex)
        {
            Console.WriteLine($"Outer catch: {ex.Message}");
            if (ex.InnerException != null)
            {
                Console.WriteLine($"Inner exception: {ex.InnerException.Message}");
            }
        }
    }
}
```

## Exception Properties

### Important Exception Properties
```csharp
using System;

public class ExceptionPropertiesExample
{
    public static void DemonstrateExceptionProperties()
    {
        try
        {
            ThrowSampleException();
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Message: {ex.Message}");
            Console.WriteLine($"Source: {ex.Source}");
            Console.WriteLine($"TargetSite: {ex.TargetSite}");
            Console.WriteLine($"StackTrace:\n{ex.StackTrace}");
            
            if (ex.InnerException != null)
            {
                Console.WriteLine($"InnerException: {ex.InnerException.Message}");
            }
        }
    }
    
    private static void ThrowSampleException()
    {
        throw new InvalidOperationException("This is a sample exception for demonstration");
    }
}
```

## Best Practices

### ✅ **DO's**
```csharp
// 1. Catch specific exceptions first
try
{
    // risky code
}
catch (FileNotFoundException ex)
{
    // Handle file not found
}
catch (IOException ex)
{
    // Handle other IO exceptions
}
catch (Exception ex)
{
    // Handle any other exception
}

// 2. Use finally for cleanup or use 'using' statements
using (var resource = new SomeDisposableResource())
{
    // Use resource
} // Automatically disposed

// 3. Provide meaningful error messages
throw new ArgumentException("Product name cannot be empty", nameof(productName));

// 4. Log exceptions properly
catch (Exception ex)
{
    Logger.LogError($"Error in ProcessOrder: {ex.Message}", ex);
    throw; // Re-throw if needed
}
```

### ❌ **DON'Ts**
```csharp
// 1. Don't catch and ignore exceptions
try
{
    // risky code
}
catch (Exception ex)
{
    // Don't do this - ignoring exceptions
}

// 2. Don't catch Exception when specific exception is expected
try
{
    int.Parse(userInput);
}
catch (Exception ex) // Too broad - use FormatException instead
{
    // Handle
}

// 3. Don't use exceptions for control flow
// Bad example:
try
{
    return dictionary[key];
}
catch (KeyNotFoundException)
{
    return defaultValue; // Use TryGetValue instead
}

// Better approach:
if (dictionary.TryGetValue(key, out var value))
    return value;
return defaultValue;
```

## Exception Handling Patterns

### Pattern 1: Try-Parse Pattern
```csharp
public static bool TryParseAge(string input, out int age)
{
    age = 0;
    
    if (string.IsNullOrWhiteSpace(input))
        return false;
        
    if (!int.TryParse(input, out age))
        return false;
        
    return age >= 0 && age <= 150; // Valid age range
}

// Usage
if (TryParseAge(userInput, out int age))
{
    Console.WriteLine($"Valid age: {age}");
}
else
{
    Console.WriteLine("Invalid age entered.");
}
```

### Pattern 2: Guard Clauses
```csharp
public void ProcessUser(User user)
{
    // Guard clauses - fail fast
    if (user == null)
        throw new ArgumentNullException(nameof(user));
        
    if (string.IsNullOrWhiteSpace(user.Name))
        throw new ArgumentException("User name is required", nameof(user));
        
    if (user.Age < 0)
        throw new ArgumentOutOfRangeException(nameof(user), "Age cannot be negative");
    
    // Main logic here
    Console.WriteLine($"Processing user: {user.Name}, Age: {user.Age}");
}
```

## Summary

Exception handling is crucial for creating robust applications. Key points to remember:

- Use **try-catch-finally** blocks to handle exceptions gracefully
- Catch **specific exceptions** before general ones
- Always **clean up resources** using finally blocks or using statements
- Create **custom exceptions** for domain-specific errors
- Follow **best practices** to write maintainable exception handling code
- **Don't ignore exceptions** - always log or handle them appropriately
- Use exceptions for **exceptional circumstances**, not regular program flow
