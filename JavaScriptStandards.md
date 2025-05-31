# JavaScript Coding Standards Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Naming Conventions](#naming-conventions)
3. [Comments and Documentation](#comments-and-documentation)
4. [Error Handling](#error-handling)
5. [Code Style and Formatting](#code-style-and-formatting)
6. [Best Practices](#best-practices)
7. [Testing Standards](#testing-standards)
8. [Code Review Guidelines](#code-review-guidelines)

## Introduction

This guide establishes JavaScript-specific coding standards to ensure consistent, maintainable, and high-quality code across our projects.

## Naming Conventions

Naming conventions are **essential** for writing code that is **readable**, **maintainable**, and **collaborative**. Consistent naming helps developers quickly understand what the code is doing and how different parts of a system interact.

---
- Prefix with `CONST_` if needed
- Example: `const API_KEY = 'your-key';`

### Examples
```javascript
// ❌ Poor
let x = 5;
function process() {}

// ✅ Good
let userCount = 5;
function processPaymentTransaction(paymentDetails) {}
```

## Comments and Documentation

### JSDoc Documentation
```javascript
/**
 * Calculates the total price including tax
 * @param {Object} item - The item details
 * @param {number} item.price - The item price
 * @param {number} item.quantity - The quantity
 * @param {number} taxRate - The tax rate (0-1)
 * @returns {number} The total price including tax
 */
function calculateTotal(item, taxRate) {
    // Implementation
}
```

### Code Comments
- Use comments to explain "why", not "what"
- Keep comments updated with code changes
- Use consistent comment style

## Error Handling

### Error Objects
```javascript
// Custom error class
class ValidationError extends Error {
    constructor(message, code) {
        super(message);
        this.code = code;
        this.name = 'ValidationError';
    }
}

// Usage
throw new ValidationError('Invalid email format', 'INVALID_EMAIL');
```

### Error Handling Patterns
```javascript
// Promise-based error handling
try {
    const result = await someAsyncOperation();
} catch (error) {
    // Handle error
}

// Error-first callbacks
function processFile(filePath, callback) {
    fs.readFile(filePath, (err, data) => {
        if (err) {
            callback(err);
            return;
        }
        // Process data
    });
}
```

## Code Style and Formatting

### ES6+ Features
- Use `const` and `let` instead of `var`
- Use arrow functions for callbacks
- Use template literals
- Use destructuring
- Use spread/rest operators

### Examples
```javascript
// ❌ Poor
var items = [];
for (var i = 0; i < 10; i++) {
    items.push({
        id: i,
        name: 'Item ' + i
    });
}

// ✅ Good
const items = Array.from({length: 10}, (_, i) => ({
    id: i,
    name: `Item ${i}`
}));

// ❌ Poor
function calculateTotal(items) {
    let total = 0;
    for (var i = 0; i < items.length; i++) {
        total += items[i].price * items[i].quantity;
    }
    return total;
}

// ✅ Good
const calculateTotal = (items) => 
    items.reduce((total, item) => total + (item.price * item.quantity), 0);

// ❌ Poor
const user = {
    id: 1,
    firstName: 'John',
    lastName: 'Doe'
};

const fullName = user.firstName + ' ' + user.lastName;

// ✅ Good
const { id, firstName, lastName } = user;
const fullName = `${firstName} ${lastName}`;
```

### Formatting Rules
- 2 spaces for indentation
- Single quotes for strings
- Semicolons at the end of statements
- Curly braces on the same line

### Async/Await Examples
```javascript
// ❌ Poor
function getUserData(userId) {
    return new Promise((resolve, reject) => {
        // ... async operation
        resolve(data);
    });
}

// ✅ Good
const getUserData = async (userId) => {
    try {
        const response = await fetch(`/api/users/${userId}`);
        if (!response.ok) {
            throw new Error('Failed to fetch user data');
        }
        return await response.json();
    } catch (error) {
        console.error('Error fetching user data:', error);
        throw error;
    }
};

// ❌ Poor
const numbers = [1, 2, 3];
const doubled = numbers.map(function(x) { return x * 2; });

// ✅ Good
const numbers = [1, 2, 3];
const doubled = numbers.map(x => x * 2);
```

### Object Destructuring Examples
```javascript
// ❌ Poor
const user = {
    id: 1,
    name: 'John',
    email: 'john@example.com',
    address: {
        street: '123 Main St',
        city: 'New York'
    }
};

function getUserInfo(user) {
    return `${user.name} (${user.email}) lives in ${user.address.city}`;
}

// ✅ Good
const user = {
    id: 1,
    name: 'John',
    email: 'john@example.com',
    address: {
        street: '123 Main St',
        city: 'New York'
    }
};

const getUserInfo = ({ name, email, address: { city } }) => 
    `${name} (${email}) lives in ${city}`;

// Nested destructuring
const { address: { street, city } } = user;
```

### Spread/Rest Examples
```javascript
// ❌ Poor
const numbers1 = [1, 2, 3];
const numbers2 = [4, 5, 6];
const allNumbers = numbers1.concat(numbers2);

const sum = function() {
    let total = 0;
    for (let i = 0; i < arguments.length; i++) {
        total += arguments[i];
    }
    return total;
};

// ✅ Good
const numbers1 = [1, 2, 3];
const numbers2 = [4, 5, 6];
const allNumbers = [...numbers1, ...numbers2];

const sum = (...numbers) => numbers.reduce((total, num) => total + num, 0);

// Object spread
const baseConfig = {
    theme: 'dark',
    language: 'en'
};

const userConfig = {
    ...baseConfig,
    theme: 'light'
}; // { theme: 'light', language: 'en' }

## Best Practices

### Performance
- Use `const` for immutables
- Avoid global variables
- Use `Object.freeze()` for constants
- Use `Set` and `Map` for collections

### Security
- Sanitize user inputs
- Use HTTPS
- Avoid eval()
- Use strict mode

### Code Organization
- Use modules
- Keep files small and focused
- Follow single responsibility principle
- Use async/await for async operations

## Testing Standards

### Unit Tests
- Test one thing per test
- Use descriptive test names
- Mock external dependencies
- Test edge cases

### Example
```javascript
describe('PaymentService', () => {
    it('should calculate correct total', () => {
        // Test implementation
    });
});
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
