# Deloitte | FS | L2 – Interview Preparation with Code Snippets

## 📜 Table of Contents
1. [How can you design a class with sensitive information so that no developer can work with it or create its object?](#q1)  
2. [How do you implement middleware in .NET Core, and what kind of custom middlewares can you create?](#q2)  
3. [In RESTful APIs which are stateless, if something goes wrong at the API level, how would you log that error?](#q3)  
4. [Have you implemented any design patterns? Explain one with an example.](#q4)  
5. [Can you provide code snippets in Angular for working with arrays?](#q5)  
6. [What is HTTP status code 400, and what does the entire 4xx series represent?](#q6)  
7. [How do you connect to the database using both Entity Framework and Dapper in .NET Core?](#q7)  
8. [What is the difference between Observables and Promises in Angular?](#q8)  
9. [Additional important questions Deloitte may ask](#q9)  
10. [Why do you want to change your job?](#q10)  

---

## <a id="q1"></a>1. How can you design a class with sensitive information so that no developer can work with it or create its object?

To ensure a class is secure and cannot be directly accessed or instantiated by other developers:
- Make the **constructor `private`** so no one can create an object from outside.
- Make the class **sealed** so it cannot be inherited.
- Use **`internal`** access modifiers with assembly-level restrictions if needed.
- Provide only **controlled access** through static methods.

**Example:**
```csharp
public sealed class SensitiveDataHandler
{
    private SensitiveDataHandler() { } // Private constructor

    public static string GetEncryptedValue(string plainText)
    {
        // Example encryption logic
        return Convert.ToBase64String(System.Text.Encoding.UTF8.GetBytes(plainText));
    }
}

// Usage
string encrypted = SensitiveDataHandler.GetEncryptedValue("MySecret");
```
✅ Developers cannot do `new SensitiveDataHandler()`.

[🔼 Back to Top](#📜-table-of-contents)

---

## <a id="q2"></a>2. How do you implement middleware in .NET Core, and what kind of custom middlewares can you create?

Middleware is a component that processes HTTP requests and responses in a pipeline.  
It can either pass control to the next middleware or short-circuit the request.

**Example: Custom Logging Middleware**
```csharp
public class RequestLoggingMiddleware
{
    private readonly RequestDelegate _next;

    public RequestLoggingMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext context)
    {
        Console.WriteLine($"Request: {context.Request.Method} {context.Request.Path}");
        await _next(context); // Pass to next middleware
    }
}

// In Program.cs or Startup.cs
app.UseMiddleware<RequestLoggingMiddleware>();
```
**Common custom middlewares:**
- Logging middleware
- Global exception handling
- Authentication middleware
- Request validation middleware
- Rate-limiting middleware

[🔼 Back to Top](#📜-table-of-contents)

---

## <a id="q3"></a>3. In RESTful APIs which are stateless, if something goes wrong at the API level, how would you log that error?

Since REST APIs are stateless, error logging must happen at the server side (API boundary).  
Use middleware for **centralized error logging**.

**Example:**
```csharp
public class ErrorHandlingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<ErrorHandlingMiddleware> _logger;

    public ErrorHandlingMiddleware(RequestDelegate next, ILogger<ErrorHandlingMiddleware> logger)
    {
        _next = next;
        _logger = logger;
    }

    public async Task Invoke(HttpContext context)
    {
        try
        {
            await _next(context);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "An unhandled exception occurred.");
            context.Response.StatusCode = 500;
            await context.Response.WriteAsync("Internal Server Error");
        }
    }
}
```
**Production tools:**  
- Serilog  
- NLog  
- Azure Application Insights  
- Datadog  

[🔼 Back to Top](#📜-table-of-contents)

---

## <a id="q4"></a>4. Have you implemented any design patterns? Explain one with an example.

One common pattern is the **Repository Pattern**, which abstracts database operations from business logic.

**Example:**
```csharp
public interface IProductRepository
{
    Task<IEnumerable<Product>> GetAllAsync();
}

public class ProductRepository : IProductRepository
{
    private readonly DbContext _context;
    public ProductRepository(DbContext context) => _context = context;

    public async Task<IEnumerable<Product>> GetAllAsync() =>
        await _context.Set<Product>().ToListAsync();
}
```
**Benefits:**
- Loose coupling between data access and business logic
- Easier unit testing
- Better maintainability

[🔼 Back to Top](#📜-table-of-contents)

---

## <a id="q5"></a>5. Can you provide code snippets in Angular for working with arrays?

**Example:**
```typescript
const numbers = [1, 2, 3, 4, 5];

// Filter
const evens = numbers.filter(n => n % 2 === 0);

// Map
const squares = numbers.map(n => n * n);

// Reduce
const sum = numbers.reduce((acc, val) => acc + val, 0);

console.log(evens, squares, sum);
```
**Output:**  
```
[2, 4]
[1, 4, 9, 16, 25]
15
```

[🔼 Back to Top](#📜-table-of-contents)

---

## <a id="q6"></a>6. What is HTTP status code 400, and what does the entire 4xx series represent?

- **400 Bad Request:** The server cannot process the request due to invalid syntax, missing parameters, or incorrect data.
- **4xx series:** Represents **Client Errors** — issues caused by the client’s request.

**Common 4xx codes:**
- **400** – Bad Request  
- **401** – Unauthorized (authentication required)  
- **403** – Forbidden (no permission)  
- **404** – Not Found  
- **409** – Conflict

[🔼 Back to Top](#📜-table-of-contents)

---

## <a id="q7"></a>7. How do you connect to the database using both Entity Framework and Dapper in .NET Core?

**Entity Framework:**
```csharp
public class AppDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }
}

using (var context = new AppDbContext())
{
    var products = await context.Products.ToListAsync();
}
```

**Dapper:**
```csharp
using (var connection = new SqlConnection(connString))
{
    var products = await connection.QueryAsync<Product>("SELECT * FROM Products");
}
```
**Difference:**
- EF → ORM, generates SQL automatically, slower but easy to maintain.
- Dapper → Micro ORM, manual SQL, faster but more code.

[🔼 Back to Top](#📜-table-of-contents)

---

## <a id="q8"></a>8. What is the difference between Observables and Promises in Angular?

| Feature         | Promise | Observable |
|-----------------|---------|------------|
| Multiple values | ❌      | ✅          |
| Lazy execution  | ❌      | ✅          |
| Cancellable     | ❌      | ✅          |
| Operators       | ❌      | ✅ (map, filter, etc.) |

**Example:**
```typescript
// Promise
getData(): Promise<User> {
  return this.http.get<User>('/api/user').toPromise();
}

// Observable
getData(): Observable<User> {
  return this.http.get<User>('/api/user');
}
```

[🔼 Back to Top](#📜-table-of-contents)

---

## <a id="q9"></a>9. Additional important questions Deloitte may ask

- What is the difference between `IEnumerable`, `IQueryable`, and `List<T>` in C#?
- What is Dependency Injection and how is it implemented in .NET Core?
- How would you handle high API load using rate limiting?
- Explain async/await and how it works internally.
- What are some SQL performance optimization techniques you have used?

[🔼 Back to Top](#📜-table-of-contents)

---

## <a id="q10"></a>10. Why do you want to change your job?

> “I have gained strong technical skills in .NET, Angular, and cloud technologies. I’m now looking for an opportunity where I can work on more challenging projects, contribute to large-scale systems, and explore the latest technology stack to accelerate my career growth.”

[🔼 Back to Top](#📜-table-of-contents)
