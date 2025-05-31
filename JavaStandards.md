# Java Coding Standards Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Naming Conventions](#naming-conventions)
3. [Code Style and Formatting](#code-style-and-formatting)
4. [Error Handling](#error-handling)
5. [Best Practices](#best-practices)
6. [Testing Standards](#testing-standards)
7. [Code Review Guidelines](#code-review-guidelines)

## Introduction

This guide establishes Java-specific coding standards to ensure consistent, maintainable, and high-quality code across our projects.

## Naming Conventions

### Classes and Interfaces

- Use `PascalCase` (e.g., `CustomerService`, `OrderRepository`)
- Use nouns or noun phrases
- Avoid abbreviations
- Use `I` prefix for interfaces (optional)

#### Examples
```java
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

- Use `camelCase` (e.g., `calculateTax()`, `processOrder()`)
- Use verbs
- Be descriptive
- Follow JavaBean conventions

#### Examples
```java
// ❌ Poor
public void p() {}
public void doIt() {}
public void process() {}

// ✅ Good
public void calculateTotal() {}
public void validateInput() {}
public void updateUserProfile() {}

// ❌ Poor
public void d() { return new Date(); }
public void t() { return new Date().getTime(); }

// ✅ Good
public Date getCurrentDate() {}
public long getCurrentTimestamp() {}

// ❌ Poor
public void x() { return 1 + 2; }
public void y() { return 3 + 4; }

// ✅ Good
public int addNumbers(int a, int b) { return a + b; }
public int calculateSum(int[] numbers) { return Arrays.stream(numbers).sum(); }
```

### Variables

- Use `camelCase` (e.g., `customerName`, `orderCount`)
- Use meaningful names
- Use plural for collections

#### Examples
```java
// ❌ Poor
int x = 5;
int y = 10;
int z = x + y;

// ✅ Good
int basePrice = 5;
int taxAmount = 10;
int totalPrice = basePrice + taxAmount;

// ❌ Poor
int d = new Date();
long t = new Date().getTime();

// ✅ Good
Date currentDate = new Date();
long currentTimestamp = new Date().getTime();

// ❌ Poor
int a = 1;
int b = 2;
int c = a + b;

// ✅ Good
int quantity = 1;
BigDecimal unitPrice = BigDecimal.valueOf(2);
BigDecimal totalCost = quantity.multiply(unitPrice);
```

### Constants

- Use `UPPER_SNAKE_CASE` (e.g., `MAX_RETRIES`, `DEFAULT_TIMEOUT`)
- Use `private static final` for constants

#### Examples
```java
// ❌ Poor
private static final int MAX = 100;
private static final int MIN = 0;
void run() {}

// ✅ Good
int daysInWeek = 7;
void calculateMonthlySalary(Employee employee) {}
```

## Code Style and Formatting

### General Rules
- Use 4 spaces for indentation
- Use curly braces for all blocks
- Place opening brace on same line
- Use single quotes for character literals
- Use double quotes for string literals

### Examples
```java
// ❌ Poor
public class Customer {
    String name;
    int age;
    
    void setName(String name) {
        this.name = name;
    }
    
    void setAge(int age) {
        this.age = age;
    }
}

// ✅ Good
public class Customer {
    private String name;
    private int age;
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("Name cannot be null or empty");
        }
        this.name = name;
    }
    
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        if (age < 0) {
            throw new IllegalArgumentException("Age cannot be negative");
        }
        this.age = age;
    }
}
```

### Access Modifiers
- Always specify access modifiers
- Order: `public`, `protected`, `internal`, `private`

### Stream API Examples
```java
// ❌ Poor
List<String> names = new ArrayList<>();
for (Customer customer : customers) {
    if (customer.isActive() && customer.getAge() > 18) {
        names.add(customer.getName());
    }
}

// ✅ Good
List<String> names = customers.stream()
    .filter(Customer::isActive)
    .filter(c -> c.getAge() > 18)
    .map(Customer::getName)
    .collect(Collectors.toList());

// Grouping example
Map<String, List<Customer>> customersByCountry = customers.stream()
    .collect(Collectors.groupingBy(Customer::getCountry));

// Complex grouping
Map<String, DoubleSummaryStatistics> statsByCountry = customers.stream()
    .collect(Collectors.groupingBy(
        Customer::getCountry,
        Collectors.summarizingDouble(Customer::getAge)
    ));
```

### Optional Examples
```java
// ❌ Poor
public String getCustomerName(long customerId) {
    Customer customer = customerRepository.findById(customerId);
    if (customer == null) {
        return "Unknown";
    }
    return customer.getName();
}

// ✅ Good
public Optional<String> getCustomerName(long customerId) {
    return Optional.ofNullable(customerRepository.findById(customerId))
        .map(Customer::getName);
}

// Using Optional in a method chain
public String getCustomerName(long customerId) {
    return Optional.ofNullable(customerRepository.findById(customerId))
        .map(Customer::getName)
        .orElse("Unknown");
}
```

### Exception Handling Examples
```java
// ❌ Poor
public void processOrder(Order order) {
    try {
        orderService.process(order);
    } catch (Exception e) {
        System.out.println("Error: " + e.getMessage());
    }
}

// ✅ Good
public void processOrder(Order order) {
    try {
        orderService.process(order);
    } catch (OrderProcessingException e) {
        logger.error("Failed to process order: " + e.getMessage(), e);
        throw e;
    } catch (Exception e) {
        logger.error("Unexpected error processing order", e);
        throw new RuntimeException("Failed to process order", e);
    }
}
```

### Builder Pattern Example
```java
// ❌ Poor
public class Order {
    private long id;
    private String customerName;
    private List<Item> items;
    private LocalDate orderDate;
    
    // Too many constructors
    public Order(long id, String customerName) { ... }
    public Order(long id, String customerName, List<Item> items) { ... }
    public Order(long id, String customerName, List<Item> items, LocalDate date) { ... }
}

// ✅ Good
public class Order {
    private final long id;
    private final String customerName;
    private final List<Item> items;
    private final LocalDate orderDate;
    
    private Order(Builder builder) {
        this.id = builder.id;
        this.customerName = builder.customerName;
        this.items = builder.items;
        this.orderDate = builder.orderDate;
    }
    
    public static class Builder {
        private final long id;
        private final String customerName;
        private List<Item> items = new ArrayList<>();
        private LocalDate orderDate;
        
        public Builder(long id, String customerName) {
            this.id = id;
            this.customerName = customerName;
        }
        
        public Builder addItem(Item item) {
            items.add(item);
            return this;
        }
        
        public Builder setOrderDate(LocalDate date) {
            this.orderDate = date;
            return this;
        }
        
        public Order build() {
            return new Order(this);
        }
    }
}

// Usage
Order order = new Order.Builder(123, "John Doe")
    .addItem(new Item("Book", 29.99))
    .setOrderDate(LocalDate.now())
    .build();
```

### Lambda Expressions Examples
```java
// ❌ Poor
button.addActionListener(new ActionListener() {
    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("Button clicked!");
    }
});

// ✅ Good
button.addActionListener(e -> System.out.println("Button clicked!"));

// Complex lambda with multiple statements
Runnable task = () -> {
    try {
        Thread.sleep(1000);
        System.out.println("Task completed");
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
    }
};
``

### Properties
```java
// Getter/Setter
private String name;

public String getName() {
    return name;
}

public void setName(String name) {
    this.name = name;
}

// Builder pattern
public static class Builder {
    private String name;
    
    public Builder name(String name) {
        this.name = name;
        return this;
    }
    
    public Customer build() {
        return new Customer(this);
    }
}
```

### Exception Handling
```java
try {
    // Operation that might fail
} catch (SpecificException ex) {
    // Handle specific exception
    logger.error("Error occurred: " + ex.getMessage());
} catch (Exception ex) {
    // Handle general exception
    throw new RuntimeException("Unexpected error", ex);
}
```

## Error Handling

### Custom Exceptions
```java
public class ValidationException extends RuntimeException {
    public ValidationException(String message) {
        super(message);
    }
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
- Use `StringBuilder` for string concatenation
- Avoid unnecessary object creation
- Use `final` for constants and immutable classes
- Implement proper equals() and hashCode()

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
```java
@Test
public void shouldCalculateCorrectTotal() {
    // Arrange
    OrderService service = new OrderService();
    
    // Act
    BigDecimal result = service.calculateTotal(items);
    
    // Assert
    assertEquals(expected, result);
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
