# Infosys | FS | L1 Interview Questions & Answers

## Table of Contents
1. [5 ways of component communication in Angular](#1-give-me-5-ways-of-component-communication-in-angular)
2. [Subject and BehaviorSubject](#2-what-is-subject-and-behaviorsubject-why-we-need-them)
3. [Handle errors Globally](#3-how-do-you-handle-errors-globally)
4. [Store JWT token in Angular](#4-where-do-you-store-the-jwt-token-in-angular)
5. [Pass JWT token](#5-how-do-you-pass-jwt-token)
6. [SQL 2nd highest salary](#6-write-a-sql-query-for-2nd-highest-salary-in-employee-table)
7. [Implement JWT in .NET Core](#7-how-do-you-implement-jwt-in-net-core)
8. [Project and roles](#8-can-you-tell-me-about-your-project-and-roles-and-responsibilities)
9. [Cache](#9-what-is-cache)
10. [Exception filters](#10-what-are-exception-filters)
11. [HTTP Status codes](#11-what-are-different-types-of-http-status-codes)
12. [Action verbs](#12-what-are-action-verbs-http-methods)
13. [Web API vs RESTful APIs](#13-what-is-difference-between-web-api-and-restful-apis)
14. [SOAP](#14-what-is-soap)
15. [Upgrade .NET 3.1 to 8.0 challenges](#15-while-upgrading-net-31-to-net-80-have-you-faced-any-challenges)
16. [.NET and Angular versions](#16-what-is-net-and-angular-version-using-in-your-project)

---

## 1. Give me 5 ways of component communication in Angular?

### 1. Parent to Child Communication (@Input)
```typescript
// Parent Component
@Component({
  template: `<child-component [childData]="parentData"></child-component>`
})
export class ParentComponent {
  parentData = "Hello from parent";
}

// Child Component
@Component({
  selector: 'child-component',
  template: `<p>{{childData}}</p>`
})
export class ChildComponent {
  @Input() childData: string;
}
```

### 2. Child to Parent Communication (@Output)
```typescript
// Child Component
@Component({
  selector: 'child-component',
  template: `<button (click)="sendToParent()">Send to Parent</button>`
})
export class ChildComponent {
  @Output() messageEvent = new EventEmitter<string>();
  
  sendToParent() {
    this.messageEvent.emit("Hello from child");
  }
}

// Parent Component
@Component({
  template: `<child-component (messageEvent)="receiveMessage($event)"></child-component>`
})
export class ParentComponent {
  receiveMessage(message: string) {
    console.log(message);
  }
}
```

### 3. Service with Observables
```typescript
// Shared Service
@Injectable({
  providedIn: 'root'
})
export class DataService {
  private messageSubject = new BehaviorSubject<string>('Initial Message');
  public message$ = this.messageSubject.asObservable();
  
  updateMessage(message: string) {
    this.messageSubject.next(message);
  }
}

// Component 1
export class Component1 {
  constructor(private dataService: DataService) {}
  
  sendMessage() {
    this.dataService.updateMessage("Hello from Component 1");
  }
}

// Component 2
export class Component2 {
  message: string;
  
  constructor(private dataService: DataService) {
    this.dataService.message$.subscribe(msg => this.message = msg);
  }
}
```

### 4. ViewChild/ViewChildren
```typescript
// Parent Component
@Component({
  template: `
    <child-component #childRef></child-component>
    <button (click)="accessChild()">Access Child</button>
  `
})
export class ParentComponent {
  @ViewChild('childRef') childComponent: ChildComponent;
  
  accessChild() {
    this.childComponent.childMethod();
  }
}

// Child Component
export class ChildComponent {
  childMethod() {
    console.log("Method called from parent");
  }
}
```

### 5. Template Reference Variables
```typescript
@Component({
  template: `
    <input #userInput>
    <button (click)="processInput(userInput.value)">Process</button>
  `
})
export class MyComponent {
  processInput(value: string) {
    console.log(value);
  }
}
```

**[⬆ Back to Top](#table-of-contents)**

---

## 2. What is Subject and BehaviorSubject? Why we need them?

### Subject
A Subject is both an Observable and Observer. It can multicast to multiple observers.

```typescript
import { Subject } from 'rxjs';

const subject = new Subject<string>();

// Subscribe to subject
subject.subscribe(value => console.log('Observer A:', value));
subject.subscribe(value => console.log('Observer B:', value));

// Emit values
subject.next('Hello');
subject.next('World');
```

### BehaviorSubject
BehaviorSubject requires an initial value and always holds the current value.

```typescript
import { BehaviorSubject } from 'rxjs';

const behaviorSubject = new BehaviorSubject<string>('Initial Value');

// Late subscriber gets the current value immediately
behaviorSubject.subscribe(value => console.log('Observer 1:', value));

behaviorSubject.next('Updated Value');

// New subscriber gets the current value
behaviorSubject.subscribe(value => console.log('Observer 2:', value));
```

### Why we need them?
- **Communication**: Enable communication between components
- **State Management**: Store and share application state
- **Real-time Updates**: Push updates to multiple subscribers
- **Loose Coupling**: Components don't need direct references

**[⬆ Back to Top](#table-of-contents)**

---

## 3. How do you handle errors globally?

### Global Error Handler
```typescript
import { ErrorHandler, Injectable } from '@angular/core';

@Injectable()
export class GlobalErrorHandler implements ErrorHandler {
  handleError(error: any): void {
    console.error('Global error:', error);
    
    // Send to logging service
    this.logError(error);
    
    // Show user-friendly message
    this.showUserNotification();
  }
  
  private logError(error: any) {
    // Log to external service
    console.log('Logging error to service:', error);
  }
  
  private showUserNotification() {
    // Show toast or notification
    alert('Something went wrong. Please try again.');
  }
}

// Register in app.module.ts
@NgModule({
  providers: [
    { provide: ErrorHandler, useClass: GlobalErrorHandler }
  ]
})
export class AppModule {}
```

### HTTP Error Interceptor
```typescript
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpErrorResponse } from '@angular/common/http';
import { catchError, throwError } from 'rxjs';

@Injectable()
export class ErrorInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler) {
    return next.handle(req).pipe(
      catchError((error: HttpErrorResponse) => {
        if (error.status === 401) {
          // Handle unauthorized
          console.log('Unauthorized access');
        } else if (error.status === 500) {
          // Handle server error
          console.log('Server error');
        }
        
        return throwError(error);
      })
    );
  }
}
```

**[⬆ Back to Top](#table-of-contents)**

---

## 4. Where do you store the JWT token in Angular?

### Options for storing JWT tokens:

#### 1. Local Storage (Most Common)
```typescript
// Store token
localStorage.setItem('jwt_token', token);

// Retrieve token
const token = localStorage.getItem('jwt_token');

// Remove token
localStorage.removeItem('jwt_token');
```

#### 2. Session Storage
```typescript
// Store token
sessionStorage.setItem('jwt_token', token);

// Retrieve token
const token = sessionStorage.getItem('jwt_token');
```

#### 3. HTTP-Only Cookies (Most Secure)
```typescript
// Set via server response
// Accessed automatically by browser
```

#### 4. In-Memory Storage
```typescript
@Injectable({
  providedIn: 'root'
})
export class TokenService {
  private token: string | null = null;
  
  setToken(token: string) {
    this.token = token;
  }
  
  getToken(): string | null {
    return this.token;
  }
}
```

**Recommendation**: Use HTTP-Only cookies for production applications for better security.

**[⬆ Back to Top](#table-of-contents)**

---

## 5. How do you pass JWT token?

### HTTP Interceptor (Recommended)
```typescript
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler } from '@angular/common/http';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler) {
    const token = localStorage.getItem('jwt_token');
    
    if (token) {
      const authReq = req.clone({
        headers: req.headers.set('Authorization', `Bearer ${token}`)
      });
      return next.handle(authReq);
    }
    
    return next.handle(req);
  }
}

// Register in app.module.ts
@NgModule({
  providers: [
    { provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true }
  ]
})
export class AppModule {}
```

### Manual Header Setting
```typescript
import { HttpClient, HttpHeaders } from '@angular/common/http';

@Injectable()
export class ApiService {
  constructor(private http: HttpClient) {}
  
  getData() {
    const token = localStorage.getItem('jwt_token');
    const headers = new HttpHeaders({
      'Authorization': `Bearer ${token}`
    });
    
    return this.http.get('/api/data', { headers });
  }
}
```

**[⬆ Back to Top](#table-of-contents)**

---

## 6. Write a SQL query for 2nd highest salary in employee table

### Method 1: Using LIMIT and OFFSET
```sql
SELECT salary 
FROM employee 
ORDER BY salary DESC 
LIMIT 1 OFFSET 1;
```

### Method 2: Using Subquery
```sql
SELECT MAX(salary) as second_highest_salary
FROM employee 
WHERE salary < (SELECT MAX(salary) FROM employee);
```

### Method 3: Using ROW_NUMBER()
```sql
SELECT salary
FROM (
    SELECT salary, ROW_NUMBER() OVER (ORDER BY salary DESC) as rn
    FROM employee
) ranked
WHERE rn = 2;
```

### Method 4: Using DISTINCT (if duplicate salaries exist)
```sql
SELECT salary
FROM (
    SELECT DISTINCT salary
    FROM employee
    ORDER BY salary DESC
    LIMIT 2
) temp
ORDER BY salary ASC
LIMIT 1;
```

**[⬆ Back to Top](#table-of-contents)**

---

## 7. How do you implement JWT in .NET Core?

### Step 1: Install NuGet Packages
```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.JwtBearer" Version="7.0.0" />
<PackageReference Include="System.IdentityModel.Tokens.Jwt" Version="6.32.1" />
```

### Step 2: Configure JWT in Program.cs/.NET 6+
```csharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.IdentityModel.Tokens;
using System.Text;

var builder = WebApplication.CreateBuilder(args);

// Add JWT Authentication
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidateAudience = true,
            ValidateLifetime = true,
            ValidateIssuerSigningKey = true,
            ValidIssuer = builder.Configuration["Jwt:Issuer"],
            ValidAudience = builder.Configuration["Jwt:Audience"],
            IssuerSigningKey = new SymmetricSecurityKey(
                Encoding.UTF8.GetBytes(builder.Configuration["Jwt:SecretKey"]))
        };
    });

builder.Services.AddAuthorization();

var app = builder.Build();

app.UseAuthentication();
app.UseAuthorization();
```

### Step 3: JWT Service
```csharp
public interface IJwtService
{
    string GenerateToken(string userId, string email);
}

public class JwtService : IJwtService
{
    private readonly IConfiguration _configuration;

    public JwtService(IConfiguration configuration)
    {
        _configuration = configuration;
    }

    public string GenerateToken(string userId, string email)
    {
        var securityKey = new SymmetricSecurityKey(
            Encoding.UTF8.GetBytes(_configuration["Jwt:SecretKey"]));
        var credentials = new SigningCredentials(securityKey, SecurityAlgorithms.HmacSha256);

        var claims = new[]
        {
            new Claim(ClaimTypes.NameIdentifier, userId),
            new Claim(ClaimTypes.Email, email),
            new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString())
        };

        var token = new JwtSecurityToken(
            issuer: _configuration["Jwt:Issuer"],
            audience: _configuration["Jwt:Audience"],
            claims: claims,
            expires: DateTime.Now.AddMinutes(Convert.ToDouble(_configuration["Jwt:ExpireMinutes"])),
            signingCredentials: credentials);

        return new JwtSecurityTokenHandler().WriteToken(token);
    }
}
```

### Step 4: Login Controller
```csharp
[ApiController]
[Route("api/[controller]")]
public class AuthController : ControllerBase
{
    private readonly IJwtService _jwtService;

    public AuthController(IJwtService jwtService)
    {
        _jwtService = jwtService;
    }

    [HttpPost("login")]
    public IActionResult Login([FromBody] LoginRequest request)
    {
        // Validate user credentials here
        if (ValidateUser(request.Email, request.Password))
        {
            var token = _jwtService.GenerateToken("userId", request.Email);
            return Ok(new { Token = token });
        }

        return Unauthorized();
    }
}
```

### Step 5: Protect Controllers
```csharp
[ApiController]
[Route("api/[controller]")]
[Authorize] // Requires JWT token
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IActionResult Get()
    {
        // This endpoint requires valid JWT token
        return Ok("Protected data");
    }
}
```

### appsettings.json
```json
{
  "Jwt": {
    "SecretKey": "your-super-secret-key-here-make-it-long",
    "Issuer": "your-app",
    "Audience": "your-app-users",
    "ExpireMinutes": 60
  }
}
```

**[⬆ Back to Top](#table-of-contents)**

---

## 8. Can you tell me about your project and roles and responsibilities?

### Sample Answer Structure:

**Project Overview:**
"I worked on an e-commerce web application built with Angular 15 frontend and .NET Core 6 Web API backend, with SQL Server database."

**Key Responsibilities:**
- Developed responsive UI components using Angular, HTML5, CSS3, and Bootstrap
- Implemented user authentication and authorization using JWT tokens
- Created RESTful APIs using .NET Core Web API with Entity Framework Core
- Implemented CRUD operations and business logic
- Integrated third-party payment gateways (Stripe/PayPal)
- Performed unit testing using Jasmine/Karma for Angular and xUnit for .NET
- Used Git for version control and Azure DevOps for CI/CD

**Technical Achievements:**
- Optimized application performance resulting in 40% faster load times
- Implemented lazy loading reducing initial bundle size by 30%
- Created reusable components reducing code duplication by 50%

**[⬆ Back to Top](#table-of-contents)**

---

## 9. What is Cache?

Cache is a temporary storage mechanism that stores frequently accessed data in memory for quick retrieval.

### Types of Caching in .NET Core:

#### 1. In-Memory Caching
```csharp
// Startup configuration
services.AddMemoryCache();

// Usage in controller
public class ProductController : ControllerBase
{
    private readonly IMemoryCache _cache;

    public ProductController(IMemoryCache cache)
    {
        _cache = cache;
    }

    [HttpGet("{id}")]
    public async Task<IActionResult> GetProduct(int id)
    {
        string cacheKey = $"product_{id}";
        
        if (_cache.TryGetValue(cacheKey, out Product product))
        {
            return Ok(product);
        }

        // Get from database
        product = await _productService.GetByIdAsync(id);
        
        // Cache for 10 minutes
        _cache.Set(cacheKey, product, TimeSpan.FromMinutes(10));
        
        return Ok(product);
    }
}
```

#### 2. Distributed Caching (Redis)
```csharp
// Startup configuration
services.AddStackExchangeRedisCache(options =>
{
    options.Configuration = "localhost:6379";
});

// Usage
public class ProductController : ControllerBase
{
    private readonly IDistributedCache _cache;

    public ProductController(IDistributedCache cache)
    {
        _cache = cache;
    }

    [HttpGet("{id}")]
    public async Task<IActionResult> GetProduct(int id)
    {
        string cacheKey = $"product_{id}";
        var cachedProduct = await _cache.GetStringAsync(cacheKey);
        
        if (cachedProduct != null)
        {
            var product = JsonSerializer.Deserialize<Product>(cachedProduct);
            return Ok(product);
        }

        // Get from database and cache
        var newProduct = await _productService.GetByIdAsync(id);
        var serializedProduct = JsonSerializer.Serialize(newProduct);
        
        await _cache.SetStringAsync(cacheKey, serializedProduct, 
            new DistributedCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(10)
            });
        
        return Ok(newProduct);
    }
}
```

### Benefits:
- **Performance**: Faster data access
- **Reduced Load**: Less database queries
- **Cost Effective**: Reduces expensive operations

**[⬆ Back to Top](#table-of-contents)**

---

## 10. What are Exception Filters?

Exception filters in .NET Core handle unhandled exceptions that occur during action execution.

### Custom Exception Filter
```csharp
public class CustomExceptionFilter : IExceptionFilter
{
    private readonly ILogger<CustomExceptionFilter> _logger;

    public CustomExceptionFilter(ILogger<CustomExceptionFilter> logger)
    {
        _logger = logger;
    }

    public void OnException(ExceptionContext context)
    {
        _logger.LogError(context.Exception, "An unhandled exception occurred");

        var response = new
        {
            Message = "An error occurred while processing your request",
            StatusCode = 500,
            Details = context.Exception.Message
        };

        context.Result = new JsonResult(response)
        {
            StatusCode = 500
        };

        context.ExceptionHandled = true;
    }
}
```

### Global Exception Filter
```csharp
public class GlobalExceptionFilter : IExceptionFilter
{
    public void OnException(ExceptionContext context)
    {
        var statusCode = context.Exception switch
        {
            NotFoundException => 404,
            UnauthorizedAccessException => 401,
            ArgumentException => 400,
            _ => 500
        };

        context.Result = new JsonResult(new
        {
            Error = context.Exception.Message,
            StatusCode = statusCode
        })
        {
            StatusCode = statusCode
        };

        context.ExceptionHandled = true;
    }
}

// Register globally in Program.cs
builder.Services.AddControllers(options =>
{
    options.Filters.Add<GlobalExceptionFilter>();
});
```

### Action-Level Exception Filter
```csharp
[ApiController]
public class ProductController : ControllerBase
{
    [HttpGet]
    [TypeFilter(typeof(CustomExceptionFilter))]
    public IActionResult GetProducts()
    {
        // Action logic
        throw new Exception("Test exception");
    }
}
```

**[⬆ Back to Top](#table-of-contents)**

---

## 11. What are different types of HTTP Status Codes?

### 1xx - Informational
- **100** Continue
- **101** Switching Protocols

### 2xx - Success
```csharp
return Ok(data);                    // 200 OK
return Created("location", data);   // 201 Created
return NoContent();                 // 204 No Content
return Accepted();                  // 202 Accepted
```

### 3xx - Redirection
- **301** Moved Permanently
- **302** Found (Temporary Redirect)
- **304** Not Modified

### 4xx - Client Errors
```csharp
return BadRequest("Invalid data");           // 400 Bad Request
return Unauthorized();                       // 401 Unauthorized
return Forbid();                            // 403 Forbidden
return NotFound();                          // 404 Not Found
return Conflict("Resource already exists"); // 409 Conflict
return UnprocessableEntity(modelState);     // 422 Unprocessable Entity
```

### 5xx - Server Errors
```csharp
return StatusCode(500, "Internal server error");  // 500 Internal Server Error
return StatusCode(502, "Bad Gateway");             // 502 Bad Gateway
return StatusCode(503, "Service Unavailable");    // 503 Service Unavailable
```

### Custom Status Code Example
```csharp
[HttpPost]
public IActionResult CreateUser([FromBody] User user)
{
    if (!ModelState.IsValid)
        return BadRequest(ModelState);  // 400

    if (_userService.UserExists(user.Email))
        return Conflict("User already exists");  // 409

    var createdUser = _userService.Create(user);
    return Created($"api/users/{createdUser.Id}", createdUser);  // 201
}
```

**[⬆ Back to Top](#table-of-contents)**

---

## 12. What are Action Verbs (HTTP Methods)?

### GET - Retrieve Data
```csharp
[HttpGet]
public IActionResult GetAllUsers()
{
    var users = _userService.GetAll();
    return Ok(users);
}

[HttpGet("{id}")]
public IActionResult GetUser(int id)
{
    var user = _userService.GetById(id);
    return user == null ? NotFound() : Ok(user);
}
```

### POST - Create New Resource
```csharp
[HttpPost]
public IActionResult CreateUser([FromBody] User user)
{
    if (!ModelState.IsValid)
        return BadRequest(ModelState);

    var createdUser = _userService.Create(user);
    return Created($"api/users/{createdUser.Id}", createdUser);
}
```

### PUT - Update Entire Resource
```csharp
[HttpPut("{id}")]
public IActionResult UpdateUser(int id, [FromBody] User user)
{
    if (id != user.Id)
        return BadRequest();

    var updatedUser = _userService.Update(user);
    return Ok(updatedUser);
}
```

### PATCH - Partial Update
```csharp
[HttpPatch("{id}")]
public IActionResult UpdateUserPartial(int id, [FromBody] JsonPatchDocument<User> patchDoc)
{
    var user = _userService.GetById(id);
    if (user == null) return NotFound();

    patchDoc.ApplyTo(user);
    _userService.Update(user);
    return NoContent();
}
```

### DELETE - Remove Resource
```csharp
[HttpDelete("{id}")]
public IActionResult DeleteUser(int id)
{
    var user = _userService.GetById(id);
    if (user == null) return NotFound();

    _userService.Delete(id);
    return NoContent();
}
```

### HEAD - Get Headers Only
```csharp
[HttpHead]
public IActionResult CheckUserExists()
{
    // Returns headers without body
    return Ok();
}
```

### OPTIONS - Get Allowed Methods
```csharp
[HttpOptions]
public IActionResult GetOptions()
{
    Response.Headers.Add("Allow", "GET,POST,PUT,DELETE");
    return Ok();
}
```

**[⬆ Back to Top](#table-of-contents)**

---

## 13. What is difference between Web API and RESTful APIs?

### Web API
- **Definition**: General term for any API accessible over HTTP
- **Architecture**: Can follow any architectural style
- **Protocol**: Primarily HTTP, but can use others
- **Data Format**: XML, JSON, or any format
- **State**: Can be stateful or stateless

```csharp
// Web API Example (not necessarily RESTful)
[ApiController]
public class UserApiController : ControllerBase
{
    [HttpPost("GetUserByEmailAndPassword")]
    public IActionResult GetUser([FromBody] LoginRequest request)
    {
        // This violates REST principles
        var user = _userService.AuthenticateUser(request.Email, request.Password);
        return Ok(user);
    }
}
```

### RESTful API
- **Definition**: Web API that follows REST architectural constraints
- **Architecture**: Must follow REST principles
- **Protocol**: HTTP/HTTPS
- **Data Format**: Usually JSON
- **State**: Must be stateless

#### REST Principles:
1. **Stateless**: Each request contains all information needed
2. **Client-Server**: Separation of concerns
3. **Cacheable**: Responses should be cacheable
4. **Uniform Interface**: Consistent interface design
5. **Layered System**: Can have intermediate layers

```csharp
// RESTful API Example
[ApiController]
[Route("api/[controller]")]
public class UsersController : ControllerBase
{
    // GET api/users (Get all users)
    [HttpGet]
    public IActionResult GetUsers()
    {
        var users = _userService.GetAll();
        return Ok(users);
    }

    // GET api/users/5 (Get specific user)
    [HttpGet("{id}")]
    public IActionResult GetUser(int id)
    {
        var user = _userService.GetById(id);
        return user == null ? NotFound() : Ok(user);
    }

    // POST api/users (Create user)
    [HttpPost]
    public IActionResult CreateUser([FromBody] User user)
    {
        var createdUser = _userService.Create(user);
        return Created($"api/users/{createdUser.Id}", createdUser);
    }

    // PUT api/users/5 (Update user)
    [HttpPut("{id}")]
    public IActionResult UpdateUser(int id, [FromBody] User user)
    {
        if (id != user.Id) return BadRequest();
        var updatedUser = _userService.Update(user);
        return Ok(updatedUser);
    }

    // DELETE api/users/5 (Delete user)
    [HttpDelete("{id}")]
    public IActionResult DeleteUser(int id)
    {
        _userService.Delete(id);
        return NoContent();
    }
}
```

### Key Differences:

| Aspect | Web API | RESTful API |
|--------|---------|-------------|
| Architecture | Any style | Must follow REST |
| URL Structure | Can be anything | Resource-based URLs |
| HTTP Methods | Any methods | Standard HTTP verbs |
| Statelessness | Optional | Required |
| Caching | Optional | Should support |

**[⬆ Back to Top](#table-of-contents)**

---

## 14. What is SOAP?

**SOAP (Simple Object Access Protocol)** is a protocol for exchanging structured information in web services.

### Key Characteristics:
- **XML-based**: Uses XML for message format
- **Protocol Independent**: Can work over HTTP, SMTP, TCP, etc.
- **Strongly Typed**: Strict schema definition
- **Built-in Security**: WS-Security standards
- **ACID Compliance**: Supports transactions

### SOAP Message Structure:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope 
    xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:web="http://www.example.com/webservice">
    
    <soap:Header>
        <!-- Optional header information -->
        <web:AuthToken>your-auth-token</web:AuthToken>
    </soap:Header>
    
    <soap:Body>
        <web:GetUser>
            <web:UserId>123</web:UserId>
        </web:GetUser>
    </soap:Body>
    
</soap:Envelope>
```

### WSDL (Web Services Description Language):
```xml
<?xml version="1.0" encoding="UTF-8"?>
<wsdl:definitions 
    xmlns:wsdl="http://schemas.xmlsoap.org/wsdl/"
    targetNamespace="http://www.example.com/webservice">
    
    <wsdl:types>
        <xsd:schema targetNamespace="http://www.example.com/webservice">
            <xsd:element name="GetUserRequest">
                <xsd:complexType>
                    <xsd:sequence>
                        <xsd:element name="UserId" type="xsd:int"/>
                    </xsd:sequence>
                </xsd:complexType>
            </xsd:element>
        </xsd:schema>
    </wsdl:types>
    
    <wsdl:message name="GetUserMessage">
        <wsdl:part name="parameters" element="tns:GetUserRequest"/>
    </wsdl:message>
    
</wsdl:definitions>
```

### Creating SOAP Service in .NET Core:
```csharp
[ServiceContract]
public interface IUserService
{
    [OperationContract]
    User GetUser(int userId);
    
    [OperationContract]
    bool CreateUser(User user);
}

public class UserService : IUserService
{
    public User GetUser(int userId)
    {
        // Implementation
        return new User { Id = userId, Name = "John Doe" };
    }
    
    public bool CreateUser(User user)
    {
        // Implementation
        return true;
    }
}

[DataContract]
public class User
{
    [DataMember]
    public int Id { get; set; }
    
    [DataMember]
    public string Name { get; set; }
}
```

### SOAP vs REST Comparison:

| Aspect | SOAP | REST |
|--------|------|------|
| Protocol | Protocol | Architectural Style |
| Data Format | XML only | JSON, XML, HTML, etc. |
| Message Size | Larger (XML overhead) | Smaller (JSON) |
| Security | Built-in WS-Security | HTTPS, OAuth |
| Caching | Not cacheable | Cacheable |
| Performance | Slower | Faster |
| Complexity | High | Low |

**[⬆ Back to Top](#table-of-contents)**

---

## 15. While upgrading .NET 3.1 to .NET 8.0, Have you faced any challenges?

### Common Challenges and Solutions:

#### 1. Breaking Changes in APIs
```csharp
// .NET 3.1 (Old way)
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<AppDbContext>(options =>
        options.UseSqlServer(connectionString));
}

// .NET 8.0 (New way)
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
```

#### 2. Startup.cs to Program.cs Migration
```csharp
// .NET 3.1 - Startup.cs
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllers();
        services.AddSwaggerGen();
    }
    
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
            app.UseSwagger();
            app.UseSwaggerUI();
        }
        
        app.UseRouting();
        app.UseAuthorization();
        app.UseEndpoints(endpoints => endpoints.MapControllers());
    }
}

// .NET 8.0 - Program.cs
var builder = WebApplication.CreateBuilder(args);

// Add services
builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// Configure pipeline
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();

app.Run();
```

#### 3. Package Updates and Compatibility
```xml
<!-- .NET 3.1 packages -->
<PackageReference Include="Microsoft.EntityFrameworkCore" Version="3.1.32" />
<PackageReference Include="Microsoft.AspNetCore.Authentication.JwtBearer" Version="3.1.32" />

<!-- .NET 8.0 updated packages -->
<PackageReference Include="Microsoft.EntityFrameworkCore" Version="8.0.0" />
<PackageReference Include="Microsoft.AspNetCore.Authentication.JwtBearer" Version="8.0.0" />
```

#### 4. DateOnly and TimeOnly Types
```csharp
// .NET 8.0 introduces new date/time types
public class Employee
{
    public DateOnly BirthDate { get; set; }  // New in .NET 6+
    public TimeOnly WorkStartTime { get; set; }  // New in .NET 6+
    
    // Old way (still supported)
    public DateTime CreatedAt { get; set; }
}
```

#### 5. Minimal APIs Enhancement
```csharp
// .NET 8.0 Enhanced Minimal APIs
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/users", async (IUserService userService) => 
{
    var users = await userService.GetAllAsync();
    return Results.Ok(users);
});

app.MapPost("/users", async (User user, IUserService userService) => 
{
    var createdUser = await userService.CreateAsync(user);
    return Results.Created($"/users/{createdUser.Id}", createdUser);
});

app.Run();
```

#### 6. Entity Framework Core Changes
```csharp
// .NET 8.0 EF Core improvements
public class AppDbContext : DbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        // New in EF Core 8: Better JSON support
        optionsBuilder.UseSqlServer(connectionString)
                     .EnableSensitiveDataLogging()
                     .LogTo(Console.WriteLine);
    }
    
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        // Enhanced JSON column support in .NET 8
        modelBuilder.Entity<User>()
            .Property(e => e.Preferences)
            .HasJsonConversion();
    }
}
```

#### 7. Performance Improvements and New Features
```csharp
// .NET 8.0 Performance features
public class WeatherService
{
    // Native AOT support improvements
    [RequiresUnreferencedCode("JSON serialization may require types not preserved")]
    public async Task<Weather> GetWeatherAsync()
    {
        // Improved HTTP client performance
        using var client = new HttpClient();
        var response = await client.GetStringAsync("api/weather");
        return JsonSerializer.Deserialize<Weather>(response);
    }
}
```

#### 8. Migration Steps:
1. **Update Target Framework**: Change `<TargetFramework>netcoreapp3.1</TargetFramework>` to `<TargetFramework>net8.0</TargetFramework>`
2. **Update Package References**: Update all NuGet packages to .NET 8 compatible versions
3. **Migrate Startup.cs**: Convert to new Program.cs minimal hosting model
4. **Test Thoroughly**: Run all unit and integration tests
5. **Update CI/CD**: Update build pipelines to use .NET 8 SDK
6. **Review Breaking Changes**: Check Microsoft's migration guide

#### 9. Common Issues and Solutions:
- **Obsolete APIs**: Replace deprecated methods with new alternatives
- **JSON Serialization**: Update to System.Text.Json if using Newtonsoft.Json
- **Authentication**: Update JWT bearer authentication setup
- **Logging**: Migrate to new logging configuration pattern

**[⬆ Back to Top](#table-of-contents)**

---

## 16. What is .NET and Angular version using in your project?

### Current Recommended Versions (2024):

#### .NET Version
```xml
<!-- Project file (.csproj) -->
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>
</Project>
```

**Latest Stable**: .NET 8.0 (LTS - Long Term Support)
- **Release Date**: November 2023
- **Support Until**: November 2026
- **Key Features**: 
  - Enhanced performance
  - Native AOT improvements
  - Better JSON support in EF Core
  - Improved minimal APIs

#### Angular Version
```json
// package.json
{
  "name": "my-angular-app",
  "version": "1.0.0",
  "dependencies": {
    "@angular/animations": "^17.0.0",
    "@angular/common": "^17.0.0",
    "@angular/compiler": "^17.0.0",
    "@angular/core": "^17.0.0",
    "@angular/forms": "^17.0.0",
    "@angular/platform-browser": "^17.0.0",
    "@angular/platform-browser-dynamic": "^17.0.0",
    "@angular/router": "^17.0.0",
    "typescript": "~5.2.0"
  }
}
```

**Latest Stable**: Angular 17
- **Release Date**: November 2023
- **Key Features**:
  - New control flow syntax (@if, @for, @switch)
  - Standalone components by default
  - Improved SSR (Server-Side Rendering)
  - New Angular CLI features

#### Sample Project Structure:

##### Backend (.NET 8.0)
```csharp
// Program.cs
var builder = WebApplication.CreateBuilder(args);

// Add services
builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

// CORS for Angular
builder.Services.AddCors(options =>
{
    options.AddPolicy("AngularApp", policy =>
    {
        policy.WithOrigins("http://localhost:4200")
              .AllowAnyMethod()
              .AllowAnyHeader();
    });
});

var app = builder.Build();

// Configure pipeline
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();
app.UseCors("AngularApp");
app.UseAuthorization();
app.MapControllers();

app.Run();
```

##### Frontend (Angular 17)
```typescript
// app.config.ts (New in Angular 17)
import { ApplicationConfig } from '@angular/core';
import { provideRouter } from '@angular/router';
import { provideHttpClient } from '@angular/common/http';

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes),
    provideHttpClient()
  ]
};

// main.ts
import { bootstrapApplication } from '@angular/platform-browser';
import { AppComponent } from './app/app.component';
import { appConfig } from './app/app.config';

bootstrapApplication(AppComponent, appConfig);
```

##### Component with New Control Flow (Angular 17)
```typescript
// user-list.component.ts
@Component({
  selector: 'app-user-list',
  standalone: true,
  template: `
    <div class="user-container">
      @if (loading) {
        <div>Loading...</div>
      } @else {
        @for (user of users; track user.id) {
          <div class="user-card">
            <h3>{{ user.name }}</h3>
            <p>{{ user.email }}</p>
          </div>
        } @empty {
          <div>No users found</div>
        }
      }
    </div>
  `
})
export class UserListComponent {
  users: User[] = [];
  loading = false;

  constructor(private userService: UserService) {}

  ngOnInit() {
    this.loadUsers();
  }

  loadUsers() {
    this.loading = true;
    this.userService.getUsers().subscribe({
      next: (users) => {
        this.users = users;
        this.loading = false;
      },
      error: (error) => {
        console.error('Error loading users:', error);
        this.loading = false;
      }
    });
  }
}
```

#### Version Compatibility Matrix:

| .NET Version | Angular Version | Node.js | TypeScript |
|--------------|-----------------|---------|------------|
| .NET 8.0 | Angular 17 | 18.x+ | 5.2+ |
| .NET 7.0 | Angular 16 | 16.x+ | 5.1+ |
| .NET 6.0 | Angular 15 | 14.x+ | 4.8+ |

#### Benefits of Using Latest Versions:

**Advantages:**
- Latest security patches
- Better performance
- Modern development features
- Long-term support (LTS versions)
- Active community support

**Migration Considerations:**
- Breaking changes between major versions
- Third-party library compatibility
- Team training requirements
- Testing and deployment updates

**[⬆ Back to Top](#table-of-contents)**

---

## Conclusion

This document covers all the essential topics for Infosys FS L1 interviews. Make sure to:

1. **Practice coding examples** - Run the provided code snippets
2. **Understand concepts** - Don't just memorize, understand the why behind each approach
3. **Prepare real examples** - Have concrete examples from your projects
4. **Stay updated** - Keep up with latest versions and features
5. **Practice explaining** - Be able to explain concepts clearly and concisely

Good luck with your interview! 🚀

**[⬆ Back to Top](#table-of-contents)**
