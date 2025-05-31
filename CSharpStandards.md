# C# Coding Standards Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Naming Conventions](#naming-conventions)
3. [Code Style and Formatting](#code-style-and-formatting)
4. [Error Handling](#error-handling)
5. [Best Practices](#best-practices)
6. [Testing Standards](#testing-standards)
7. [Code Review Guidelines](#code-review-guidelines)

## Introduction

This guide establishes C#-specific coding standards to ensure consistent, maintainable, and high-quality code across our projects.

## Naming Conventions

### Classes and Interfaces
- Use `PascalCase` (e.g., `CustomerRepository`, `IOrderService`)
- Use nouns or noun phrases
- Avoid abbreviations
- Use `I` prefix for interfaces

#### Examples
```csharp
// ❌ Poor
public class C {}
public interface I {}

// ✅ Good
public class Customer {}
public interface ICustomerService {}

// ❌ Poor
public class M {}
// ✅ Good
public class Manager {}

// ❌ Poor
public class P {}
// ✅ Good
public class Product {}

// ❌ Poor
public class D {}
// ✅ Good
public class Database {}

// ❌ Poor
public class U {}
// ✅ Good
public class UserRepository {}
```

### Methods
- Use `PascalCase` (e.g., `GetCustomerById()`, `ProcessOrder()`)
- Use verbs
- Be descriptive

#### Examples
```csharp
// ❌ Poor
public void P() {}
public void DoIt() {}
public void Process() {}

// ✅ Good
public void CalculateTotal() {}
public void ValidateInput() {}
public void UpdateUserProfile() {}

// ❌ Poor
public void D() { return DateTime.Now; }
public void T() { return DateTime.Now.Ticks; }

// ✅ Good
public DateTime GetCurrentDateTime() {}
public long GetCurrentTimestamp() {}

// ❌ Poor
public void X() { return 1 + 2; }
public void Y() { return 3 + 4; }

// ✅ Good
public int AddNumbers(int a, int b) { return a + b; }
public int CalculateSum(int[] numbers) { return numbers.Sum(); }
```

### Variables
- Use `camelCase` (e.g., `customerName`, `orderCount`)
- Use meaningful names
- Use plural for collections

#### Examples
```csharp
// ❌ Poor
int x = 5;
int y = 10;
int z = x + y;

// ✅ Good
int basePrice = 5;
int taxAmount = 10;
int totalPrice = basePrice + taxAmount;

// ❌ Poor
int d = DateTime.Now;
long t = DateTime.Now.Ticks;

// ✅ Good
DateTime currentDate = DateTime.Now;
long currentTimestamp = DateTime.Now.Ticks;

// ❌ Poor
int a = 1;
int b = 2;
int c = a + b;

// ✅ Good
int quantity = 1;
decimal unitPrice = 2;
decimal totalCost = quantity * unitPrice;
```

### Constants
- Use `PascalCase` (e.g., `DefaultTimeoutSeconds`, `MaxRetries`)
- Use `readonly` for non-const values

#### Examples
```csharp
// ❌ Poor
const int MAX = 100;
const int MIN = 0;

// ✅ Good
const int MAX_RETRIES = 100;
const int MIN_AGE = 0;

// ❌ Poor
const string URL = "https://api.example.com";
// ✅ Good
const string API_BASE_URL = "https://api.example.com";

// ❌ Poor
const double PI = 3.14159;
// ✅ Good
const double PI_VALUE = 3.14159;

// ❌ Poor
const int TIMEOUT = 5000;
// ✅ Good
const int REQUEST_TIMEOUT_MILLISECONDS = 5000;
```

### Properties
- Use `PascalCase` (e.g., `Name`, `Age`)

#### Examples
```csharp
// ❌ Poor
public string N { get; set; }
public int A { get; set; }

// ✅ Good
public string Name { get; set; }
public int Age { get; set; }

// ❌ Poor
public string U { get; set; }
// ✅ Good
public string Username { get; set; }

// ❌ Poor
public string E { get; set; }
// ✅ Good
public string Email { get; set; }
```

### Events
- Use `PascalCase` (e.g., `CustomerAdded`, `OrderCompleted`)

#### Examples
```csharp
// ❌ Poor
public event EventHandler E;
public event EventHandler C;

// ✅ Good
public event EventHandler CustomerAdded;
public event EventHandler OrderCompleted;

// ❌ Poor
public event EventHandler D;
// ✅ Good
public event EventHandler DataChanged;
```

### Enums
- Use `PascalCase` (e.g., `Status`, `Type`)

#### Examples
```csharp
// ❌ Poor
public enum S { A, B, C }
public enum T { X, Y, Z }

// ✅ Good
public enum Status { Active, Inactive, Pending }
public enum Type { Email, Phone, Fax }

// ❌ Poor
public enum R { P, Q, R }
// ✅ Good
public enum Role { Admin, User, Guest }
```

### Namespaces
- Use `PascalCase` (e.g., `Company.Product.Module`)

#### Examples
```csharp
// ❌ Poor
namespace C {
    // ...
}

// ✅ Good
namespace Company.Product.Module {
    // ...
}

// ❌ Poor
namespace U {
    // ...
}

// ✅ Good
namespace UserManagement {
    // ...
}
```

### File Names
- Use `PascalCase` (e.g., `CustomerService.cs`, `OrderProcessor.cs`)

#### Examples
```csharp
// ❌ Poor
// File.cs
// Class.cs
// Module.cs

// ✅ Good
// CustomerService.cs
// OrderProcessor.cs
// DataValidator.cs

// ❌ Poor
// Lib.cs
// Core.cs
// Main.cs

// ✅ Good
// AuthenticationService.cs
// OrderProcessing.cs
// DataStorage.cs
```

### Anti-Patterns to Avoid

#### Examples
```csharp
// ❌ Using vague verbs
void Process() {} // Process what?
void Handle() {} // Handle what?
void Manage() {} // Manage what?

// ❌ Using generic nouns
string data = ""; // What kind of data?
string info = ""; // What kind of info?
string value = ""; // What kind of value?

// ❌ Using abbreviations
string usr = ""; // What does usr mean?
string usrNm = ""; // What does usrNm mean?
string usrId = ""; // What does usrId mean?

// ❌ Using single-letter variables
int x = 5;
int y = 10;
int z = x + y;

// ❌ Using generic prefixes
bool is = true; // is what?
bool has = true; // has what?
bool can = true; // can what?
```

### Best Practices

1. **Be Specific**
   ```csharp
   // ❌ Poor
   void Get() {}
   void Set() {}
   
   // ✅ Good
   Customer GetCustomer() {}
   void SetCustomer(Customer customer) {}
   ```

2. **Use Domain Terms**
   ```csharp
   // ❌ Poor
   void Update() {}
   void Save() {}
   
   // ✅ Good
   void UpdateOrderStatus() {}
   void SaveCustomerProfile() {}
   ```

3. **Avoid Ambiguity**
   ```csharp
   // ❌ Poor
   void Check() {}
   void Verify() {}
   
   // ✅ Good
   void CheckUserCredentials() {}
   void VerifyPaymentStatus() {}
   ```

### Examples
```csharp
// ❌ Poor
int d = 7;
void run() {}

// ✅ Good
int daysInWeek = 7;
void CalculateMonthlySalary(Employee employee) {}
```

## Code Style and Formatting

### General Rules
- Use 4 spaces for indentation
- Use curly braces for all blocks
- Place opening brace on same line
- Use single quotes for character literals
- Use double quotes for string literals

### Examples
```csharp
// ❌ Poor
public class Customer
{
    public string Name;
    public int Age;
    
    public void SetAge(int newAge)
    {
        Age = newAge;
    }
}

// ✅ Good
public class Customer
{
    private string _name;
    private int _age;
    
    public string Name
    {
        get { return _name; }
        set { _name = value; }
    }
    
    public int Age
    {
        get { return _age; }
        private set { _age = value; }
    }
    
    public void UpdateAge(int newAge)
    {
        if (newAge < 0)
            throw new ArgumentException("Age cannot be negative");
            
        _age = newAge;
    }
}
```

### Access Modifiers
- Always specify access modifiers
- Order: `public`, `protected`, `internal`, `private`

### LINQ Examples
```csharp
// ❌ Poor
var customers = new List<Customer>();
var filtered = new List<Customer>();
foreach (var customer in customers)
{
    if (customer.Age > 18 && customer.IsActive)
        filtered.Add(customer);
}

// ✅ Good
var customers = new List<Customer>();
var filtered = customers
    .Where(c => c.Age > 18 && c.IsActive)
    .ToList();

// Grouping example
var customerGroups = customers
    .GroupBy(c => c.Country)
    .Select(g => new
    {
        Country = g.Key,
        CustomerCount = g.Count(),
        AverageAge = g.Average(c => c.Age)
    });
```

### Async/Await Examples
```csharp
// ❌ Poor
public async Task<List<Customer>> GetCustomers()
{
    var list = new List<Customer>();
    var results = await _database.QueryAsync("SELECT * FROM Customers");
    foreach (var row in results)
    {
        list.Add(new Customer
        {
            Id = row.Id,
            Name = row.Name
        });
    }
    return list;
}

// ✅ Good
public async Task<List<Customer>> GetCustomers()
{
    var results = await _database.QueryAsync("SELECT * FROM Customers");
    return results.Select(row => new Customer
    {
        Id = row.Id,
        Name = row.Name
    }).ToList();
}
```

### Pattern Matching Examples
```csharp
// ❌ Poor
public string GetCustomerType(Customer customer)
{
    if (customer is null) return "Unknown";
    if (customer.IsPremium) return "Premium";
    if (customer.IsCorporate) return "Corporate";
    return "Regular";
}

// ✅ Good
public string GetCustomerType(Customer customer)
{
    return customer switch
    {
        null => "Unknown",
        { IsPremium: true } => "Premium",
        { IsCorporate: true } => "Corporate",
        _ => "Regular"
    };
}
```

### Dependency Injection Examples
```csharp
// ❌ Poor
public class OrderService
{
    private readonly IEmailService _emailService;
    
    public OrderService()
    {
        _emailService = new EmailService();
    }
    
    public void ProcessOrder(Order order)
    {
        // ... processing
        _emailService.SendConfirmation(order.CustomerEmail);
    }
}

// ✅ Good
public interface IOrderService
{
    Task ProcessOrderAsync(Order order);
}

public class OrderService : IOrderService
{
    private readonly IEmailService _emailService;
    
    public OrderService(IEmailService emailService)
    {
        _emailService = emailService ?? throw new ArgumentNullException(nameof(emailService));
    }
    
    public async Task ProcessOrderAsync(Order order)
    {
        // ... processing
        await _emailService.SendConfirmationAsync(order.CustomerEmail);
    }
}
```

### Error Handling Examples
```csharp
// ❌ Poor
try
{
    var result = await _service.GetData();
    return result;
}
catch
{
    return null;
}

// ✅ Good
try
{
    var result = await _service.GetData();
    return result;
}
catch (ServiceException ex)
{
    _logger.LogError(ex, "Failed to get data");
    throw;
}
catch (Exception ex)
{
    _logger.LogError(ex, "Unexpected error occurred");
    throw new InvalidOperationException("Operation failed", ex);
}

### Properties
```csharp
// Auto-implemented property
public string Name { get; set; }

// Expression-bodied property
public decimal TotalAmount => items.Sum(i => i.Price * i.Quantity);
```

### Exception Handling
```csharp
try
{
    // Operation that might fail
}
catch (SpecificException ex)
{
    // Handle specific exception
}
catch (Exception ex)
{
    // Handle general exception
    throw;
}
finally
{
    // Cleanup code
}
```

## Error Handling

### Custom Exceptions
```csharp
public class ValidationException : Exception
{
    public ValidationException(string message) : base(message) { }
}

// Usage
throw new ValidationException("Invalid user input");
```

### Error Patterns
- Use specific exception types
- Don't catch and ignore exceptions
- Log errors appropriately
- Use try-catch for external operations

## Best Practices

### Performance
- Use `using` statements for disposable objects
- Avoid unnecessary allocations
- Use `async/await` for async operations
- Implement IDisposable properly

### Security
- Validate all inputs
- Use parameterized queries
- Implement proper authentication
- Handle sensitive data securely

### Code Organization
- Follow SOLID principles
- Use dependency injection
- Implement proper interfaces
- Keep classes focused

## Testing Standards

### Unit Tests
- Test one thing per test
- Use descriptive test names
- Mock dependencies
- Test edge cases

### Example
```csharp
[TestClass]
public class OrderServiceTests
{
    [TestMethod]
    public void CalculateTotal_ShouldReturnCorrectAmount()
    {
        // Arrange
        var service = new OrderService();
        
        // Act
        var result = service.CalculateTotal(items);
        
        // Assert
        Assert.AreEqual(expected, result);
    }
}
```

## Code Review Guidelines

### What to Check
- Code follows naming conventions
- Proper error handling
- Good documentation
- Performance considerations
- Security implications
- Test coverage

### Review Process
1. Check for code style violations
2. Review documentation
3. Test functionality
4. Check error handling
5. Verify security
6. Review performance
7. Check test coverage
