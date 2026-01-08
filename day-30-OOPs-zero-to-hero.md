# 📘 From Zero to OOP Hero with JavaScript ES6 Classes

---

## 1. CHAPTER OVERVIEW

### 📹 In the Video
You learned how ES6 classes provide a modern, clean syntax for object-oriented programming in JavaScript. We covered the complete class ecosystem—from basic syntax to advanced features like private fields, static methods, getters/setters, and inheritance—all while building practical, real-world examples.

### ✅ Checklist: By the end of this workbook, you will be able to...
- [ ] Write ES6 classes with constructors, methods, and class fields
- [ ] Implement getters and setters for computed properties and validation
- [ ] Use private (`#`) and public fields to control data access
- [ ] Create static methods and properties for class-level functionality
- [ ] Build inheritance hierarchies using `extends` and `super()`
- [ ] Apply all five OOP principles using ES6 class syntax
- [ ] Debug class-related issues in Chrome DevTools efficiently

### 📚 Prerequisites
From **Day 29 (OOP Fundamentals)**, you should be comfortable with:
- The five OOP principles (Abstraction, Encapsulation, Inheritance, Polymorphism, Composition)
- Understanding of objects, properties, and methods
- The concept of `this` keyword
- Difference between "is-a" and "has-a" relationships

---

## 2. LECTURE CHEAT SHEET

### 🎯 ES6 Class Syntax Quick Reference

```javascript
class ClassName {
  // 1. Class fields (public)
  publicField = "accessible everywhere";
  
  // 2. Private fields (ES2022+)
  #privateField = "only inside class";
  
  // 3. Static properties (class-level)
  static classProperty = "shared by all";
  
  // 4. Constructor (runs on new ClassName())
  constructor(param) {
    this.instanceProperty = param;
  }
  
  // 5. Instance methods
  instanceMethod() {
    return this.publicField;
  }
  
  // 6. Getter (computed property)
  get computedValue() {
    return this.#privateField.toUpperCase();
  }
  
  // 7. Setter (with validation)
  set value(newValue) {
    if (newValue) this.#privateField = newValue;
  }
  
  // 8. Private methods
  #privateMethod() {
    return "internal use only";
  }
  
  // 9. Static methods (called on class)
  static staticMethod() {
    return "no instance needed";
  }
}

// Creating instances
const obj = new ClassName("value");
```

---

### 📊 Class vs Constructor Function vs Object Literal

| Feature | Object Literal | Constructor Function | ES6 Class |
|---------|---------------|---------------------|-----------|
| **Syntax** | `const obj = {}` | `function Obj() {}` | `class Obj {}` |
| **Multiple instances** | ❌ No | ✅ Yes | ✅ Yes |
| **Requires `new`** | N/A | ⚠️ Optional | ✅ Required |
| **Private fields** | ❌ No | ❌ No (convention only) | ✅ Yes (`#field`) |
| **Inheritance** | ❌ Manual | ⚠️ Prototype chain | ✅ `extends` |
| **Static methods** | ❌ No | ✅ Manual | ✅ Built-in |
| **Hoisting** | N/A | ✅ Fully hoisted | ❌ Not initialized |
| **typeof** | `"object"` | `"function"` | `"function"` |
| **Best for** | Single object | Legacy code | Modern OOP |

---

### 🔑 Key Class Components Glossary

| Term | Definition | Syntax Example |
|------|------------|----------------|
| **Constructor** | Special method that initializes new instances | `constructor(name) { this.name = name; }` |
| **Class Field** | Property declared directly in class body | `age = 0;` |
| **Instance Method** | Function callable on objects | `greet() { return "Hi"; }` |
| **Static Method** | Function callable on class itself | `static create() {...}` |
| **Getter** | Method accessed like a property (reads) | `get fullName() {...}` |
| **Setter** | Method accessed like a property (writes) | `set fullName(value) {...}` |
| **Private Field** | Property accessible only inside class | `#password = "secret";` |
| **Public Field** | Property accessible everywhere | `email = "user@mail.com";` |
| **super()** | Calls parent class constructor | `super(parentArgs);` |
| **extends** | Creates inheritance relationship | `class Dog extends Animal {}` |

---

### 🏗️ Constructor Rules

```javascript
class Example {
  constructor(param) {
    // ✅ DO: Initialize properties
    this.value = param;
    
    // ✅ DO: Call private methods
    this.#initialize();
    
    // ✅ DO: Validate input
    if (!param) throw new Error("Param required");
    
    // ❌ DON'T: Return objects (breaks instanceof)
    // return { value: param }; // BAD
  }
}

// Child class constructor
class Child extends Example {
  constructor(param, extra) {
    // ✅ MUST: Call super() FIRST
    super(param);
    
    // ✅ THEN: Set child properties
    this.extra = extra;
    
    // ❌ DON'T: Use 'this' before super()
    // this.extra = extra; // ERROR
    // super(param);
  }
}
```

---

### 🔒 Private vs Public Access

```javascript
class BankAccount {
  // Public - accessible anywhere
  accountNumber = "12345";
  
  // Private - class only (truly private)
  #balance = 0;
  #pin;
  
  // Protected (convention) - still accessible but discouraged
  _transactions = [];
  
  constructor(pin) {
    this.#pin = pin;
  }
  
  // Public method - controlled access to private data
  deposit(amount, pin) {
    if (this.#validatePin(pin) && amount > 0) {
      this.#balance += amount;
      this._transactions.push({type: 'deposit', amount});
      return true;
    }
    return false;
  }
  
  // Private method - internal use
  #validatePin(pin) {
    return pin === this.#pin;
  }
  
  // Getter - read-only access
  get balance() {
    return this.#balance;
  }
}

const account = new BankAccount("1234");
console.log(account.accountNumber);     // ✅ "12345" - public
console.log(account.balance);           // ✅ 0 - via getter
console.log(account._transactions);     // ⚠️ [] - accessible but discouraged
// console.log(account.#balance);       // ❌ SyntaxError - private
// account.#validatePin("1234");        // ❌ SyntaxError - private
```

**Access Control Cheat:**
```
Public field      → Accessible: EVERYWHERE
Private field (#) → Accessible: CLASS ONLY
Protected (_)     → Accessible: EVERYWHERE (convention only)
```

---

### 🎭 Getters and Setters Patterns

```javascript
class Person {
  #firstName;
  #lastName;
  #age;
  
  constructor(firstName, lastName, age) {
    this.#firstName = firstName;
    this.#lastName = lastName;
    this.age = age; // Uses setter
  }
  
  // ✅ Computed property (read-only)
  get fullName() {
    return `${this.#firstName} ${this.#lastName}`;
  }
  
  // ✅ Writable computed property (with validation)
  set fullName(value) {
    const parts = value.split(' ');
    if (parts.length !== 2) {
      throw new Error("Full name must be 'First Last'");
    }
    this.#firstName = parts[0];
    this.#lastName = parts[1];
  }
  
  // ✅ Validation on set
  get age() {
    return this.#age;
  }
  
  set age(value) {
    if (value < 0 || value > 150) {
      throw new Error("Invalid age");
    }
    this.#age = value;
  }
  
  // ✅ Derived property (on-the-fly calculation)
  get isAdult() {
    return this.#age >= 18;
  }
}

const person = new Person("John", "Doe", 25);
console.log(person.fullName);      // "John Doe" - getter
person.fullName = "Jane Smith";    // setter (splits and validates)
console.log(person.age);           // 25
person.age = 30;                   // setter validates
console.log(person.isAdult);       // true - computed
```

**When to Use:**
- **Getter only**: Read-only computed values (`get total() { return this.price * this.qty; }`)
- **Getter + Setter**: Validated properties (`get age()` / `set age(value)`)
- **Setter only**: Write-only (rare, e.g., password setting)

---

### ⚡ Static Methods and Properties

```javascript
class User {
  static userCount = 0;           // Static property
  static MAX_USERS = 1000;        // Static constant
  
  constructor(name) {
    if (User.userCount >= User.MAX_USERS) {
      throw new Error("Max users reached");
    }
    this.name = name;
    User.userCount++;             // Access via class name
  }
  
  // Instance method (needs object)
  greet() {
    return `Hi, I'm ${this.name}`;
  }
  
  // Static method (no object needed)
  static getTotalUsers() {
    return User.userCount;
  }
  
  // Static factory method
  static createGuest() {
    return new User("Guest");
  }
  
  // Static utility
  static isValidName(name) {
    return name.length >= 2;
  }
}

// ✅ Static - called on class
console.log(User.getTotalUsers());        // 0
console.log(User.isValidName("Jo"));      // true
const guest = User.createGuest();         // Factory

// ✅ Instance - called on object
const user1 = new User("Alice");
console.log(user1.greet());               // "Hi, I'm Alice"

// ❌ Wrong usage
// user1.getTotalUsers();                 // TypeError
// User.greet();                          // TypeError
```

**Static Use Cases:**
```
✅ Utility functions    → Math.max(), Array.from()
✅ Factory methods      → User.createAdmin()
✅ Constants            → Config.API_URL
✅ Counters/Trackers   → User.userCount
✅ Validation helpers   → Email.isValid()
```

---

### 👨‍👩‍👧‍👦 Inheritance with extends and super

```javascript
// Parent class
class Vehicle {
  constructor(brand, year) {
    this.brand = brand;
    this.year = year;
  }
  
  start() {
    return `${this.brand} started`;
  }
  
  getInfo() {
    return `${this.year} ${this.brand}`;
  }
}

// Child class
class Car extends Vehicle {
  constructor(brand, year, doors) {
    // ⚠️ MUST call super() before using 'this'
    super(brand, year);           // Call parent constructor
    this.doors = doors;
  }
  
  // Override parent method
  start() {
    const parentResult = super.start(); // Call parent version
    return `${parentResult} with ${this.doors} doors`;
  }
  
  // New method (not in parent)
  honk() {
    return "Beep beep!";
  }
}

const car = new Car("Toyota", 2023, 4);
console.log(car.start());        // "Toyota started with 4 doors"
console.log(car.getInfo());      // "2023 Toyota" - inherited
console.log(car.honk());         // "Beep beep!" - own method
```

**Inheritance Checklist:**
```
✅ Use 'extends' to inherit
✅ Call super() FIRST in child constructor
✅ Use super.method() to call parent methods
✅ Override methods by redefining them
✅ Add new methods specific to child
❌ Don't use 'this' before super()
❌ Don't forget super() in child constructor
```

---

### 🔄 Method Overriding Flow

```
Parent Method Call Flow:
┌─────────────────────────────────────┐
│  child.method()                     │
│         │                           │
│         ↓                           │
│  Is method in Child?                │
│    ├─ YES → Execute Child.method() │
│    └─ NO  → Check Parent.method()  │
│                  │                  │
│                  ↓                  │
│         Execute Parent.method()    │
└─────────────────────────────────────┘

With super.method():
┌─────────────────────────────────────┐
│  child.method() {                   │
│    const result = super.method();   │
│    // Add child-specific logic      │
│  }                                  │
└─────────────────────────────────────┘
```

---

### 📋 Class Field Initialization Order

```javascript
class Example {
  field1 = this.#init();    // 1. Fields evaluated top-to-bottom
  field2 = "value";         // 2. Before constructor runs
  
  constructor(param) {      // 3. Constructor runs after fields
    this.field3 = param;    // 4. Constructor properties last
  }
  
  #init() {
    console.log("Field initialized");
    return "initialized";
  }
}

// Order of execution:
// 1. field1 = this.#init() → logs "Field initialized"
// 2. field2 = "value"
// 3. constructor runs
// 4. field3 = param
```

---

### 🎯 typeof Class and Prototype Chain

```javascript
class MyClass {
  method() {}
}

console.log(typeof MyClass);                  // "function"
console.log(MyClass.prototype);               // {constructor: class MyClass, method: f}
console.log(MyClass.prototype.constructor);   // class MyClass
console.log(MyClass === MyClass.prototype.constructor); // true

const obj = new MyClass();
console.log(obj.__proto__ === MyClass.prototype);       // true
console.log(obj.constructor === MyClass);               // true
```

**Why "function"?**
Classes are syntactic sugar over constructor functions. Under the hood:
```javascript
// This class:
class Person {
  greet() { return "hi"; }
}

// Is equivalent to:
function Person() {}
Person.prototype.greet = function() { return "hi"; };
```

---

## 3. PREDICT THE OUTPUT

### 🧠 Test Your Understanding

**Question 1:**
```javascript
class Counter {
  count = 0;
  
  increment() {
    this.count++;
  }
}

const c1 = new Counter();
const c2 = new Counter();
c1.increment();
c1.increment();
c2.increment();

console.log(c1.count);
console.log(c2.count);
```

**🤔 Thinking Process:**
- `count` is an instance field (not static)
- Each object gets its own copy
- `c1` incremented twice: 0 → 1 → 2
- `c2` incremented once: 0 → 1

**✅ Output:**
```
2
1
```

**Why:** Instance fields are separate for each object. Not shared.

---

**Question 2:**
```javascript
class Product {
  constructor(name, price) {
    this.name = name;
    this.price = price;
  }
  
  get displayPrice() {
    return `$${this.price}`;
  }
  
  set displayPrice(value) {
    this.price = parseFloat(value.replace('$', ''));
  }
}

const item = new Product("Laptop", 999);
console.log(item.displayPrice);
item.displayPrice = "$1299";
console.log(item.price);
```

**🤔 Thinking Process:**
- First `console.log`: calls getter, returns "$999"
- Assignment uses setter: removes "$", parses to 1299
- Second `console.log`: direct property access shows 1299

**✅ Output:**
```
$999
1299
```

**Why:** Getters format output, setters parse input. No parentheses needed.

---

**Question 3:**
```javascript
class Helper {
  static count = 0;
  
  constructor() {
    Helper.count++;
  }
  
  static getCount() {
    return this.count;
  }
}

new Helper();
new Helper();
console.log(Helper.getCount());
console.log(Helper.count);
```

**🤔 Thinking Process:**
- `static count` is shared across all instances
- Each `new Helper()` increments the shared counter
- First object: count = 0 → 1
- Second object: count = 1 → 2
- Both access points show same value

**✅ Output:**
```
2
2
```

**Why:** Static properties are class-level (shared), not instance-level.

---

**Question 4:**
```javascript
class Animal {
  speak() {
    return "Some sound";
  }
}

class Dog extends Animal {
  speak() {
    return super.speak() + " - Woof!";
  }
}

const dog = new Dog();
console.log(dog.speak());
```

**🤔 Thinking Process:**
- `Dog.speak()` is called
- `super.speak()` calls `Animal.speak()` → returns "Some sound"
- Then appends " - Woof!"

**✅ Output:**
```
Some sound - Woof!
```

**Why:** `super.method()` calls the parent's version, then child adds more.

---

**Question 5:**
```javascript
class User {
  #password;
  
  constructor(name, password) {
    this.name = name;
    this.#password = password;
  }
  
  checkPassword(input) {
    return input === this.#password;
  }
}

const user = new User("Alice", "secret123");
console.log(user.checkPassword("secret123"));
console.log(user.#password);
```

**🤔 Thinking Process:**
- First log: calls public method, accesses private field internally → true
- Second log: tries to access private field from outside → SyntaxError

**✅ Output:**
```
true
SyntaxError: Private field '#password' must be declared in an enclosing class
```

**Why:** Private fields (`#`) are truly private. External access throws syntax error.

---

**Question 6:**
```javascript
class Calculator {
  result = 0;
  
  add(num) {
    this.result += num;
    return this;
  }
  
  multiply(num) {
    this.result *= num;
    return this;
  }
}

const calc = new Calculator();
calc.add(5).multiply(2).add(3);
console.log(calc.result);
```

**🤔 Thinking Process:**
- Start: result = 0
- `.add(5)` → 0 + 5 = 5, returns `this`
- `.multiply(2)` → 5 * 2 = 10, returns `this`
- `.add(3)` → 10 + 3 = 13
- Methods return `this` → enables chaining

**✅ Output:**
```
13
```

**Why:** Returning `this` enables method chaining (fluent interface).

---

**Question 7:**
```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  
  greet = () => {
    return `Hi, I'm ${this.name}`;
  }
}

class Student extends Person {
  greet() {
    return super.greet() + " (Student)";
  }
}

const student = new Student("Bob");
console.log(student.greet());
```

**🤔 Thinking Process:**
- Parent's `greet` is an arrow function (instance property, not prototype method)
- Arrow functions can't be overridden via `super`
- Child's `greet()` tries to call `super.greet()` but it's not on prototype
- TypeError occurs

**✅ Output:**
```
TypeError: (intermediate value).greet is not a function
```

**Why:** Arrow function methods are instance properties, not inherited via prototype. Can't use `super` with them.

---

**Question 8:**
```javascript
class Vehicle {
  constructor(type) {
    this.type = type;
  }
}

class Car extends Vehicle {
  constructor(brand) {
    this.brand = brand;
    super("Car");
  }
}

const car = new Car("Toyota");
```

**🤔 Thinking Process:**
- Child constructor uses `this.brand` before calling `super()`
- In derived classes, `this` cannot be used before `super()`
- ReferenceError

**✅ Output:**
```
ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
```

**Why:** Always call `super()` FIRST in child constructors.

---

**Question 9:**
```javascript
class MyClass {
  field = this.initializeField();
  
  constructor() {
    console.log("Constructor");
  }
  
  initializeField() {
    console.log("Field initialized");
    return "value";
  }
}

new MyClass();
```

**🤔 Thinking Process:**
- Class fields are evaluated before constructor runs
- `field = this.initializeField()` executes first
- Then constructor runs

**✅ Output:**
```
Field initialized
Constructor
```

**Why:** Class fields initialize before the constructor executes.

---

**Question 10:**
```javascript
class Parent {
  static prop = "Parent";
  
  static showProp() {
    console.log(this.prop);
  }
}

class Child extends Parent {
  static prop = "Child";
}

Parent.showProp();
Child.showProp();
```

**🤔 Thinking Process:**
- Static properties/methods are inherited
- `this` in static methods refers to the class that called it
- `Parent.showProp()`: `this` = Parent, `this.prop` = "Parent"
- `Child.showProp()`: `this` = Child, `this.prop` = "Child"

**✅ Output:**
```
Parent
Child
```

**Why:** Static methods inherit but `this` refers to the calling class.

---
## 4. GUIDED PRACTICE PROBLEMS

---

### 🔥 Problem 1: Warm-up (Easy) - Smart Temperature Converter

**📋 Task Description:**
Create a `Temperature` class that stores temperature in Celsius and provides automatic conversion to Fahrenheit and Kelvin using getters and setters.

**🎯 Requirements:**
1. Private field `#celsius` to store temperature
2. Constructor that accepts initial Celsius value
3. Getter and setter for `celsius`
4. Getter and setter for `fahrenheit` (converts to/from Celsius)
5. Getter for `kelvin` (read-only, converts from Celsius)
6. Method `compare(otherTemp)` that returns which is warmer
7. Validate that temperature doesn't go below absolute zero (-273.15°C)

**💻 Starter Code:**
```javascript
// Create your Temperature class here

// Test Cases
const temp1 = new Temperature(25);
console.log(`${temp1.celsius}°C = ${temp1.fahrenheit}°F = ${temp1.kelvin}K`);

temp1.fahrenheit = 100;
console.log(`After setting to 100°F: ${temp1.celsius}°C`);

const temp2 = new Temperature(30);
console.log(temp1.compare(temp2));

// Should throw error
try {
  temp1.celsius = -300;
} catch (error) {
  console.log(error.message);
}
```

**✅ Solution:**
```javascript
class Temperature {
  #celsius;
  
  constructor(celsius = 0) {
    this.celsius = celsius; // Uses setter for validation
  }
  
  // Getter for celsius
  get celsius() {
    return this.#celsius;
  }
  
  // Setter for celsius with validation
  set celsius(value) {
    if (value < -273.15) {
      throw new Error("Temperature cannot be below absolute zero (-273.15°C)");
    }
    this.#celsius = value;
  }
  
  // Getter for fahrenheit (computed property)
  get fahrenheit() {
    return (this.#celsius * 9/5) + 32;
  }
  
  // Setter for fahrenheit (converts to celsius)
  set fahrenheit(value) {
    this.celsius = (value - 32) * 5/9; // Uses celsius setter for validation
  }
  
  // Getter for kelvin (read-only)
  get kelvin() {
    return this.#celsius + 273.15;
  }
  
  // Compare method
  compare(otherTemp) {
    if (this.#celsius > otherTemp.celsius) {
      return `${this.#celsius}°C is warmer than ${otherTemp.celsius}°C`;
    } else if (this.#celsius < otherTemp.celsius) {
      return `${this.#celsius}°C is cooler than ${otherTemp.celsius}°C`;
    } else {
      return "Both temperatures are equal";
    }
  }
  
  // Display method (bonus)
  display() {
    return `${this.#celsius.toFixed(2)}°C = ${this.fahrenheit.toFixed(2)}°F = ${this.kelvin.toFixed(2)}K`;
  }
}

// Test Cases
const temp1 = new Temperature(25);
console.log(temp1.display());
// Output: 25.00°C = 77.00°F = 298.15K

temp1.fahrenheit = 100;
console.log(`After setting to 100°F: ${temp1.celsius.toFixed(2)}°C`);
// Output: After setting to 100°F: 37.78°C

const temp2 = new Temperature(30);
console.log(temp1.compare(temp2));
// Output: 37.78°C is warmer than 30°C

try {
  temp1.celsius = -300;
} catch (error) {
  console.log(error.message);
  // Output: Temperature cannot be below absolute zero (-273.15°C)
}
```

**🚨 Common Mistakes:**
1. **Direct property assignment in setter:** `this.#celsius = value` without validation
2. **Not using setter in constructor:** Should call `this.celsius = celsius` to trigger validation
3. **Forgetting to convert in setter:** Fahrenheit setter must convert to Celsius
4. **Creating setter for kelvin:** Kelvin should be read-only (getter only)
5. **Not handling absolute zero:** -273.15°C is the minimum possible temperature

---

### 🔥 Problem 2: Logic Builder (Medium) - User Authentication System

**📋 Task Description:**
Build a `User` class with secure password handling, login attempts tracking, and account locking after failed attempts.

**🎯 Requirements:**
1. Private fields: `#password`, `#loginAttempts`, `#isLocked`
2. Public fields: `username`, `email`, `createdAt`
3. Constructor accepts username, email, and password
4. Method `login(password)`: 
   - Returns true if password matches
   - Increments failed attempts on wrong password
   - Locks account after 3 failed attempts
5. Method `resetPassword(oldPassword, newPassword)`:
   - Validates old password
   - Changes to new password if validation passes
6. Method `unlock()`: Admin method to unlock account
7. Static method `validatePassword(password)`: Checks password strength
8. Getter `accountStatus`: Returns "Active", "Locked", or login attempts remaining

**💻 Starter Code:**
```javascript
// Create your User class here

// Test Cases
const user = new User("john_doe", "john@example.com", "SecurePass123");

console.log(user.accountStatus);
console.log(user.login("WrongPass"));  // Attempt 1
console.log(user.login("WrongPass"));  // Attempt 2
console.log(user.login("WrongPass"));  // Attempt 3
console.log(user.accountStatus);

console.log(user.login("SecurePass123")); // Should fail - locked

user.unlock();
console.log(user.login("SecurePass123")); // Should work now

console.log(User.validatePassword("weak"));
console.log(User.validatePassword("Strong123!"));
```

**✅ Solution:**
```javascript
class User {
  // Private fields
  #password;
  #loginAttempts = 0;
  #isLocked = false;
  
  // Public fields
  username;
  email;
  createdAt;
  
  // Constants
  static MAX_LOGIN_ATTEMPTS = 3;
  static MIN_PASSWORD_LENGTH = 8;
  
  constructor(username, email, password) {
    // Validate password before setting
    if (!User.validatePassword(password)) {
      throw new Error("Password does not meet security requirements");
    }
    
    this.username = username;
    this.email = email;
    this.#password = password;
    this.createdAt = new Date();
  }
  
  // Login method
  login(password) {
    // Check if account is locked
    if (this.#isLocked) {
      return {
        success: false,
        message: "Account is locked. Contact administrator."
      };
    }
    
    // Check password
    if (password === this.#password) {
      this.#loginAttempts = 0; // Reset attempts on success
      return {
        success: true,
        message: `Welcome back, ${this.username}!`
      };
    } else {
      this.#loginAttempts++;
      
      // Lock account if max attempts reached
      if (this.#loginAttempts >= User.MAX_LOGIN_ATTEMPTS) {
        this.#isLocked = true;
        return {
          success: false,
          message: "Account locked due to multiple failed login attempts."
        };
      }
      
      const attemptsLeft = User.MAX_LOGIN_ATTEMPTS - this.#loginAttempts;
      return {
        success: false,
        message: `Invalid password. ${attemptsLeft} attempt(s) remaining.`
      };
    }
  }
  
  // Reset password method
  resetPassword(oldPassword, newPassword) {
    // Verify old password
    if (oldPassword !== this.#password) {
      return {
        success: false,
        message: "Current password is incorrect."
      };
    }
    
    // Validate new password
    if (!User.validatePassword(newPassword)) {
      return {
        success: false,
        message: "New password does not meet security requirements."
      };
    }
    
    // Don't allow same password
    if (oldPassword === newPassword) {
      return {
        success: false,
        message: "New password must be different from old password."
      };
    }
    
    this.#password = newPassword;
    return {
      success: true,
      message: "Password successfully changed."
    };
  }
  
  // Unlock account (admin method)
  unlock() {
    this.#isLocked = false;
    this.#loginAttempts = 0;
    return "Account unlocked successfully.";
  }
  
  // Account status getter
  get accountStatus() {
    if (this.#isLocked) {
      return "Status: LOCKED";
    }
    
    if (this.#loginAttempts === 0) {
      return "Status: ACTIVE";
    }
    
    const attemptsLeft = User.MAX_LOGIN_ATTEMPTS - this.#loginAttempts;
    return `Status: ACTIVE (${attemptsLeft} login attempt(s) remaining)`;
  }
  
  // Static password validation
  static validatePassword(password) {
    if (password.length < User.MIN_PASSWORD_LENGTH) {
      return false;
    }
    
    // Check for at least one uppercase, one lowercase, and one number
    const hasUpperCase = /[A-Z]/.test(password);
    const hasLowerCase = /[a-z]/.test(password);
    const hasNumber = /[0-9]/.test(password);
    
    return hasUpperCase && hasLowerCase && hasNumber;
  }
  
  // Get user info (without sensitive data)
  getInfo() {
    return {
      username: this.username,
      email: this.email,
      createdAt: this.createdAt.toLocaleDateString(),
      status: this.accountStatus
    };
  }
}

// Test Cases
console.log("=== User Authentication System ===\n");

const user = new User("john_doe", "john@example.com", "SecurePass123");

console.log(user.accountStatus);
// Output: Status: ACTIVE

console.log(user.login("WrongPass"));
// Output: { success: false, message: 'Invalid password. 2 attempt(s) remaining.' }

console.log(user.login("WrongPass"));
// Output: { success: false, message: 'Invalid password. 1 attempt(s) remaining.' }

console.log(user.login("WrongPass"));
// Output: { success: false, message: 'Account locked due to multiple failed login attempts.' }

console.log(user.accountStatus);
// Output: Status: LOCKED

console.log(user.login("SecurePass123"));
// Output: { success: false, message: 'Account is locked. Contact administrator.' }

console.log(user.unlock());
// Output: Account unlocked successfully.

console.log(user.login("SecurePass123"));
// Output: { success: true, message: 'Welcome back, john_doe!' }

console.log("\n=== Password Validation ===");
console.log(User.validatePassword("weak"));        // false
console.log(User.validatePassword("Strong123!"));  // true

console.log("\n=== Password Reset ===");
console.log(user.resetPassword("WrongOld", "NewPass123"));
// Output: { success: false, message: 'Current password is incorrect.' }

console.log(user.resetPassword("SecurePass123", "NewSecure456"));
// Output: { success: true, message: 'Password successfully changed.' }

console.log(user.login("NewSecure456"));
// Output: { success: true, message: 'Welcome back, john_doe!' }
```

**🚨 Common Mistakes:**
1. **Not resetting attempts on successful login:** Should clear `#loginAttempts` when login succeeds
2. **Allowing login when locked:** Must check `#isLocked` first
3. **Not validating password in constructor:** Password requirements should be enforced at creation
4. **Exposing password in getInfo():** Never return sensitive data
5. **Not preventing same password in reset:** Old and new passwords should differ
6. **Incrementing attempts even when locked:** Only increment when account is unlocked

---

### 🔥 Problem 3: Pro Challenge (Hard) - E-Commerce Shopping Cart System

**📋 Task Description:**
Build a complete shopping cart system with `Product`, `CartItem`, `ShoppingCart`, and `Customer` classes. Implement discount codes, stock management, and order history.

**🎯 Requirements:**

**Product Class:**
- Private: `#stock`, `#price`
- Public: `id`, `name`, `category`
- Methods: `updateStock(quantity)`, `applyDiscount(percent)`
- Getters/Setters for price (validate positive)
- Static method to create product from JSON

**CartItem Class:**
- Stores product reference and quantity
- Method `getSubtotal()` calculates item total
- Validate quantity doesn't exceed stock

**ShoppingCart Class:**
- Private: `#items` (array), `#discountCode`
- Methods:
  - `addItem(product, quantity)`
  - `removeItem(productId)`
  - `updateQuantity(productId, newQuantity)`
  - `applyDiscountCode(code)` (10% off for "SAVE10", 20% for "SAVE20")
  - `getTotal()` calculates cart total with discount
  - `checkout()` processes order and updates stock
- Getter `itemCount` returns total items

**Customer Class:**
- Extends a base `User` class
- Private: `#orderHistory`
- Has a `ShoppingCart` instance
- Methods: `placeOrder()`, `getOrderHistory()`
- Track total money spent

**💻 Starter Code:**
```javascript
// Build the complete system here

// Test Scenario
const laptop = new Product("P001", "Gaming Laptop", 1200, "Electronics", 5);
const mouse = new Product("P002", "Wireless Mouse", 50, "Electronics", 10);
const keyboard = new Product("P003", "Mechanical Keyboard", 100, "Electronics", 8);

const customer = new Customer("Alice", "alice@example.com");

customer.cart.addItem(laptop, 2);
customer.cart.addItem(mouse, 1);
customer.cart.applyDiscountCode("SAVE10");

console.log(customer.cart.getTotal());
customer.placeOrder();
console.log(customer.getOrderHistory());
```

**✅ Solution:**
```javascript
// Base User class
class User {
  #password;
  
  constructor(name, email, password = "default123") {
    this.name = name;
    this.email = email;
    this.#password = password;
    this.createdAt = new Date();
  }
  
  // Protected method (convention)
  _authenticate(password) {
    return password === this.#password;
  }
}

// Product class
class Product {
  #stock;
  #price;
  
  constructor(id, name, price, category, stock) {
    this.id = id;
    this.name = name;
    this.category = category;
    this.#price = price;
    this.#stock = stock;
  }
  
  // Price getter
  get price() {
    return this.#price;
  }
  
  // Price setter with validation
  set price(value) {
    if (value < 0) {
      throw new Error("Price cannot be negative");
    }
    this.#price = value;
  }
  
  // Stock getter
  get stock() {
    return this.#stock;
  }
  
  // Check if enough stock available
  hasStock(quantity) {
    return this.#stock >= quantity;
  }
  
  // Update stock
  updateStock(quantity) {
    if (this.#stock + quantity < 0) {
      throw new Error("Insufficient stock");
    }
    this.#stock += quantity;
    return this.#stock;
  }
  
  // Apply discount
  applyDiscount(percent) {
    if (percent < 0 || percent > 100) {
      throw new Error("Discount must be between 0 and 100");
    }
    this.#price = this.#price * (1 - percent / 100);
    return this.#price;
  }
  
  // Get product info
  getInfo() {
    return {
      id: this.id,
      name: this.name,
      price: this.#price.toFixed(2),
      category: this.category,
      stock: this.#stock
    };
  }
  
  // Static factory method
  static fromJSON(json) {
    const data = JSON.parse(json);
    return new Product(data.id, data.name, data.price, data.category, data.stock);
  }
}

// CartItem class
class CartItem {
  #product;
  #quantity;
  
  constructor(product, quantity) {
    if (!product.hasStock(quantity)) {
      throw new Error(`Only ${product.stock} units of ${product.name} available`);
    }
    this.#product = product;
    this.#quantity = quantity;
  }
  
  // Getters
  get product() {
    return this.#product;
  }
  
  get quantity() {
    return this.#quantity;
  }
  
  // Update quantity with validation
  updateQuantity(newQuantity) {
    if (newQuantity <= 0) {
      throw new Error("Quantity must be positive");
    }
    if (!this.#product.hasStock(newQuantity)) {
      throw new Error(`Only ${this.#product.stock} units available`);
    }
    this.#quantity = newQuantity;
  }
  
  // Calculate subtotal
  getSubtotal() {
    return this.#product.price * this.#quantity;
  }
  
  // Get item details
  getDetails() {
    return {
      productName: this.#product.name,
      quantity: this.#quantity,
      pricePerUnit: this.#product.price.toFixed(2),
      subtotal: this.getSubtotal().toFixed(2)
    };
  }
}

// ShoppingCart class
class ShoppingCart {
  #items = [];
  #discountCode = null;
  
  // Discount codes configuration
  static DISCOUNT_CODES = {
    'SAVE10': 0.10,
    'SAVE20': 0.20,
    'WELCOME': 0.15
  };
  
  // Add item to cart
  addItem(product, quantity = 1) {
    // Check if product already in cart
    const existingItem = this.#items.find(item => item.product.id === product.id);
    
    if (existingItem) {
      const newQuantity = existingItem.quantity + quantity;
      existingItem.updateQuantity(newQuantity);
      return `Updated ${product.name} quantity to ${newQuantity}`;
    } else {
      const cartItem = new CartItem(product, quantity);
      this.#items.push(cartItem);
      return `Added ${quantity} x ${product.name} to cart`;
    }
  }
  
  // Remove item from cart
  removeItem(productId) {
    const index = this.#items.findIndex(item => item.product.id === productId);
    if (index === -1) {
      throw new Error("Product not found in cart");
    }
    const removed = this.#items.splice(index, 1)[0];
    return `Removed ${removed.product.name} from cart`;
  }
  
  // Update item quantity
  updateQuantity(productId, newQuantity) {
    const item = this.#items.find(item => item.product.id === productId);
    if (!item) {
      throw new Error("Product not found in cart");
    }
    
    if (newQuantity === 0) {
      return this.removeItem(productId);
    }
    
    item.updateQuantity(newQuantity);
    return `Updated quantity to ${newQuantity}`;
  }
  
  // Apply discount code
  applyDiscountCode(code) {
    const upperCode = code.toUpperCase();
    if (!ShoppingCart.DISCOUNT_CODES[upperCode]) {
      throw new Error("Invalid discount code");
    }
    this.#discountCode = upperCode;
    const percent = ShoppingCart.DISCOUNT_CODES[upperCode] * 100;
    return `Discount code applied: ${percent}% off`;
  }
  
  // Remove discount code
  removeDiscountCode() {
    this.#discountCode = null;
    return "Discount code removed";
  }
  
  // Calculate subtotal (before discount)
  getSubtotal() {
    return this.#items.reduce((total, item) => total + item.getSubtotal(), 0);
  }
  
  // Calculate discount amount
  getDiscountAmount() {
    if (!this.#discountCode) return 0;
    return this.getSubtotal() * ShoppingCart.DISCOUNT_CODES[this.#discountCode];
  }
  
  // Calculate total (after discount)
  getTotal() {
    return this.getSubtotal() - this.getDiscountAmount();
  }
  
  // Get item count
  get itemCount() {
    return this.#items.reduce((count, item) => count + item.quantity, 0);
  }
  
  // Get cart summary
  getSummary() {
    const subtotal = this.getSubtotal();
    const discount = this.getDiscountAmount();
    const total = this.getTotal();
    
    return {
      items: this.#items.map(item => item.getDetails()),
      itemCount: this.itemCount,
      subtotal: subtotal.toFixed(2),
      discount: discount.toFixed(2),
      discountCode: this.#discountCode,
      total: total.toFixed(2)
    };
  }
  
  // Checkout process
  checkout() {
    if (this.#items.length === 0) {
      throw new Error("Cart is empty");
    }
    
    // Create order snapshot
    const order = {
      items: this.#items.map(item => ({
        productId: item.product.id,
        productName: item.product.name,
        quantity: item.quantity,
        price: item.product.price,
        subtotal: item.getSubtotal()
      })),
      subtotal: this.getSubtotal(),
      discount: this.getDiscountAmount(),
      total: this.getTotal(),
      discountCode: this.#discountCode,
      date: new Date()
    };
    
    // Update product stock
    this.#items.forEach(item => {
      item.product.updateStock(-item.quantity);
    });
    
    // Clear cart
    this.#items = [];
    this.#discountCode = null;
    
    return order;
  }
  
  // Check if cart is empty
  isEmpty() {
    return this.#items.length === 0;
  }
  
  // Clear cart
  clear() {
    this.#items = [];
    this.#discountCode = null;
    return "Cart cleared";
  }
}

// Customer class (extends User)
class Customer extends User {
  #orderHistory = [];
  #totalSpent = 0;
  
  constructor(name, email) {
    super(name, email);
    this.cart = new ShoppingCart();
    this.address = null;
  }
  
  // Place order
  placeOrder() {
    if (this.cart.isEmpty()) {
      throw new Error("Cannot place order with empty cart");
    }
    
    const order = this.cart.checkout();
    this.#orderHistory.push(order);
    this.#totalSpent += order.total;
    
    return {
      success: true,
      message: "Order placed successfully!",
      orderId: this.#orderHistory.length,
      total: order.total.toFixed(2)
    };
  }
  
  // Get order history
  getOrderHistory() {
    return this.#orderHistory.map((order, index) => ({
      orderId: index + 1,
      date: order.date.toLocaleDateString(),
      items: order.items.length,
      total: order.total.toFixed(2),
      discountCode: order.discountCode || 'None'
    }));
  }
  
  // Get specific order details
  getOrderDetails(orderId) {
    const order = this.#orderHistory[orderId - 1];
    if (!order) {
      throw new Error("Order not found");
    }
    return order;
  }
  
  // Get total spent
  get totalSpent() {
    return this.#totalSpent;
  }
  
  // Get customer summary
  getSummary() {
    return {
      name: this.name,
      email: this.email,
      totalOrders: this.#orderHistory.length,
      totalSpent: this.#totalSpent.toFixed(2),
      averageOrderValue: this.#orderHistory.length > 0 
        ? (this.#totalSpent / this.#orderHistory.length).toFixed(2)
        : '0.00'
    };
  }
}

// ==================== Test Scenario ====================

console.log("=== E-Commerce Shopping Cart System ===\n");

// Create products
const laptop = new Product("P001", "Gaming Laptop", 1200, "Electronics", 5);
const mouse = new Product("P002", "Wireless Mouse", 50, "Electronics", 10);
const keyboard = new Product("P003", "Mechanical Keyboard", 100, "Electronics", 8);

console.log("Products created:");
console.log(laptop.getInfo());
console.log(mouse.getInfo());
console.log(keyboard.getInfo());

// Create customer
const customer = new Customer("Alice Johnson", "alice@example.com");
console.log("\nCustomer created:", customer.name);

// Add items to cart
console.log("\n=== Adding Items to Cart ===");
console.log(customer.cart.addItem(laptop, 2));
console.log(customer.cart.addItem(mouse, 1));
console.log(customer.cart.addItem(keyboard, 1));

// Show cart summary
console.log("\n=== Cart Summary ===");
console.log(customer.cart.getSummary());

// Apply discount code
console.log("\n=== Applying Discount ===");
console.log(customer.cart.applyDiscountCode("SAVE10"));
console.log("Total after discount:", customer.cart.getTotal().toFixed(2));

// Place order
console.log("\n=== Placing Order ===");
console.log(customer.placeOrder());

// Check updated stock
console.log("\n=== Updated Stock ===");
console.log(laptop.getInfo());
console.log(mouse.getInfo());

// Add more items and place another order
console.log("\n=== Second Order ===");
customer.cart.addItem(mouse, 2);
customer.cart.addItem(keyboard, 1);
customer.cart.applyDiscountCode("SAVE20");
console.log(customer.placeOrder());

// View order history
console.log("\n=== Order History ===");
console.log(customer.getOrderHistory());

// Customer summary
console.log("\n=== Customer Summary ===");
console.log(customer.getSummary());

// Test error handling
console.log("\n=== Error Handling Tests ===");
try {
  customer.cart.addItem(laptop, 10); // Not enough stock
} catch (error) {
  console.log("Error caught:", error.message);
}

try {
  customer.cart.applyDiscountCode("INVALID");
} catch (error) {
  console.log("Error caught:", error.message);
}
```

**Expected Output:**
```
=== E-Commerce Shopping Cart System ===

Products created:
{ id: 'P001', name: 'Gaming Laptop', price: '1200.00', category: 'Electronics', stock: 5 }
{ id: 'P002', name: 'Wireless Mouse', price: '50.00', category: 'Electronics', stock: 10 }
{ id: 'P003', name: 'Mechanical Keyboard', price: '100.00', category: 'Electronics', stock: 8 }

Customer created: Alice Johnson

=== Adding Items to Cart ===
Added 2 x Gaming Laptop to cart
Added 1 x Wireless Mouse to cart
Added 1 x Mechanical Keyboard to cart

=== Cart Summary ===
{
  items: [
    { productName: 'Gaming Laptop', quantity: 2, pricePerUnit: '1200.00', subtotal: '2400.00' },
    { productName: 'Wireless Mouse', quantity: 1, pricePerUnit: '50.00', subtotal: '50.00' },
    { productName: 'Mechanical Keyboard', quantity: 1, pricePerUnit: '100.00', subtotal: '100.00' }
  ],
  itemCount: 4,
  subtotal: '2550.00',
  discount: '0.00',
  discountCode: null,
  total: '2550.00'
}

=== Applying Discount ===
Discount code applied: 10% off
Total after discount: 2295.00

=== Placing Order ===
{ success: true, message: 'Order placed successfully!', orderId: 1, total: '2295.00' }

=== Updated Stock ===
{ id: 'P001', name: 'Gaming Laptop', price: '1200.00', category: 'Electronics', stock: 3 }
{ id: 'P002', name: 'Wireless Mouse', price: '50.00', category: 'Electronics', stock: 9 }

...
```

**🚨 Common Mistakes:**
1. **Not validating stock before adding:** Check `hasStock()` in CartItem constructor
2. **Forgetting to update stock on checkout:** Must decrement product stock
3. **Modifying cart during checkout:** Create order snapshot before clearing
4. **Not handling existing items:** Check if product already in cart before adding new CartItem
5. **Direct access to private fields:** Use getters/setters consistently
6. **Not clearing discount after checkout:** Reset `#discountCode` after order
7. **Circular references:** CartItem stores product reference, be careful with serialization
8. **Not validating discount codes:** Check if code exists before applying

---

## 5. DEBUG CHALLENGES

**Instructions:** Each code snippet has 1-3 bugs. Find them, explain why they're wrong, and provide the fix.

---

### 🐛 Challenge 1: Constructor Confusion

```javascript
class Rectangle {
  constructor(width, height) {
    width = width;
    height = height;
  }
  
  getArea() {
    return this.width * this.height;
  }
}

const rect = new Rectangle(5, 10);
console.log(rect.getArea());
```

**🔍 What's the bug?**

<details>
<summary>Click to see the answer</summary>

**🐛 Bug:** Missing `this` keyword in constructor assignments

**Why it's wrong:** 
- `width = width` just assigns the parameter to itself (no-op)
- Properties must be assigned to `this.width` and `this.height`
- Without `this`, the properties are never created on the instance
- `rect.width` and `rect.height` are `undefined`
- `undefined * undefined` = `NaN`

**✅ Fixed Code:**
```javascript
class Rectangle {
  constructor(width, height) {
    this.width = width;    // ✅ Use this.width
    this.height = height;  // ✅ Use this.height
  }
  
  getArea() {
    return this.width * this.height;
  }
}

const rect = new Rectangle(5, 10);
console.log(rect.getArea()); // 50
```

**Output:** `50`

**Common mistake:** Forgetting that constructor parameters need to be assigned to `this.propertyName` to become instance properties.

</details>

---

### 🐛 Challenge 2: Getter/Setter Mix-up

```javascript
class Person {
  #age;
  
  constructor(name, age) {
    this.name = name;
    this.#age = age;
  }
  
  get age() {
    return this.#age;
  }
  
  set age() {
    if (value >= 0) {
      this.#age = value;
    }
  }
}

const person = new Person("John", 25);
person.age = 30;
console.log(person.age);
```

**🔍 What's the bug?**

<details>
<summary>Click to see the answer</summary>

**🐛 Bug:** Setter is missing the parameter `value`

**Why it's wrong:**
- Setter syntax requires a parameter: `set age(value)`
- Without parameter, `value` is `undefined` inside the setter
- The condition `value >= 0` is always `false` when value is undefined
- Age never gets updated

**✅ Fixed Code:**
```javascript
class Person {
  #age;
  
  constructor(name, age) {
    this.name = name;
    this.#age = age;
  }
  
  get age() {
    return this.#age;
  }
  
  set age(value) {  // ✅ Added parameter
    if (value >= 0) {
      this.#age = value;
    }
  }
}

const person = new Person("John", 25);
person.age = 30;
console.log(person.age); // 30
```

**Output:** `30`

**Rule:** Setters MUST have exactly one parameter.

</details>

---

### 🐛 Challenge 3: Static Method Error

```javascript
class MathHelper {
  value = 10;
  
  static double(num) {
    return num * 2;
  }
  
  triple() {
    return this.value * 3;
  }
}

const helper = new MathHelper();
console.log(helper.double(5));
console.log(MathHelper.triple());
```

**🔍 What are the bugs?**

<details>
<summary>Click to see the answer</summary>

**🐛 Bug 1:** Calling static method on instance (`helper.double()`)  
**🐛 Bug 2:** Calling instance method on class (`MathHelper.triple()`)

**Why it's wrong:**
- **Static methods** belong to the class → Call with `ClassName.method()`
- **Instance methods** belong to objects → Call with `objectName.method()`
- `helper.double()` → `TypeError: helper.double is not a function`
- `MathHelper.triple()` → `TypeError: MathHelper.triple is not a function`

**✅ Fixed Code:**
```javascript
class MathHelper {
  value = 10;
  
  static double(num) {
    return num * 2;
  }
  
  triple() {
    return this.value * 3;
  }
}

const helper = new MathHelper();
console.log(MathHelper.double(5));  // ✅ Static on class
console.log(helper.triple());        // ✅ Instance on object
```

**Output:**
```
10
30
```

**Memory aid:**
- **Static** = **S**hared, on **C**lass
- **Instance** = **I**ndividual, on **O**bject

</details>

---

### 🐛 Challenge 4: Inheritance Super Issue

```javascript
class Animal {
  constructor(name, species) {
    this.name = name;
    this.species = species;
  }
  
  describe() {
    return `${this.name} is a ${this.species}`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    this.breed = breed;
    super(name, "Dog");
  }
  
  describe() {
    return super.describe() + ` (${this.breed})`;
  }
}

const dog = new Dog("Max", "Labrador");
console.log(dog.describe());
```

**🔍 What's the bug?**

<details>
<summary>Click to see the answer</summary>

**🐛 Bug:** Using `this` before calling `super()` in child constructor

**Why it's wrong:**
- In derived classes, you **cannot** access `this` before calling `super()`
- `this.breed = breed` happens before `super(name, "Dog")`
- JavaScript throws: `ReferenceError: Must call super constructor before accessing 'this'`
- Parent must initialize the object first, then child can add to it

**✅ Fixed Code:**
```javascript
class Animal {
  constructor(name, species) {
    this.name = name;
    this.species = species;
  }
  
  describe() {
    return `${this.name} is a ${this.species}`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name, "Dog");    // ✅ Call super() FIRST
    this.breed = breed;    // ✅ Then use this
  }
  
  describe() {
    return super.describe() + ` (${this.breed})`;
  }
}

const dog = new Dog("Max", "Labrador");
console.log(dog.describe());
// Output: Max is a Dog (Labrador)
```

**Golden Rule:** In child constructors:
1. **First:** Call `super()`
2. **Then:** Use `this`

</details>

---

### 🐛 Challenge 5: Private Field Access

```javascript
class BankAccount {
  #balance = 1000;
  
  constructor(accountNumber) {
    this.accountNumber = accountNumber;
  }
  
  deposit(amount) {
    this.balance += amount;
    return this.balance;
  }
  
  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount("12345");
console.log(account.deposit(500));
console.log(account.getBalance());
```

**🔍 What's the bug?**

<details>
<summary>Click to see the answer</summary>

**🐛 Bug:** Accessing private field without `#` in `deposit()` method

**Why it's wrong:**
- Private field is declared as `#balance`
- In `deposit()`, code uses `this.balance` (no `#`)
- `this.balance` is a different (non-existent public) property
- Creates new public property `balance` = `undefined + 500` = `NaN`
- Private `#balance` remains 1000 (unchanged)

**✅ Fixed Code:**
```javascript
class BankAccount {
  #balance = 1000;
  
  constructor(accountNumber) {
    this.accountNumber = accountNumber;
  }
  
  deposit(amount) {
    this.#balance += amount;  // ✅ Use #balance
    return this.#balance;     // ✅ Use #balance
  }
  
  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount("12345");
console.log(account.deposit(500));   // 1500
console.log(account.getBalance());   // 1500
```

**Output:**
```
1500
1500
```

**Critical reminder:** Private field name **includes** the `#`. It's not a prefix that you add/remove—it's part of the identifier.

</details>

---

### 🐛 Challenge 6: Class Field Initialization Order

```javascript
class Configuration {
  apiUrl = this.getBaseUrl();
  environment = "production";
  
  getBaseUrl() {
    return this.environment === "production" 
      ? "https://api.example.com"
      : "https://dev.api.example.com";
  }
}

const config = new Configuration();
console.log(config.apiUrl);
```

**🔍 What's the bug?**

<details>
<summary>Click to see the answer</summary>

**🐛 Bug:** Field initialization order problem

**Why it's wrong:**
- Class fields are initialized **top-to-bottom** before the constructor
- `apiUrl = this.getBaseUrl()` runs **first**
- At that moment, `this.environment` is still `undefined` (not initialized yet)
- `environment = "production"` runs **after** `apiUrl`
- So `getBaseUrl()` checks `undefined === "production"` → `false`
- Returns dev URL instead of production URL

**✅ Fixed Code - Option 1 (Reorder fields):**
```javascript
class Configuration {
  environment = "production";      // ✅ Initialize first
  apiUrl = this.getBaseUrl();      // ✅ Then use it
  
  getBaseUrl() {
    return this.environment === "production" 
      ? "https://api.example.com"
      : "https://dev.api.example.com";
  }
}

const config = new Configuration();
console.log(config.apiUrl); // https://api.example.com
```

**✅ Fixed Code - Option 2 (Use constructor):**
```javascript
class Configuration {
  environment = "production";
  
  constructor() {
    this.apiUrl = this.getBaseUrl();  // ✅ Set in constructor
  }
  
  getBaseUrl() {
    return this.environment === "production" 
      ? "https://api.example.com"
      : "https://dev.api.example.com";
  }
}
```

**✅ Fixed Code - Option 3 (Use getter):**
```javascript
class Configuration {
  environment = "production";
  
  get apiUrl() {  // ✅ Computed property (no initialization order issue)
    return this.getBaseUrl();
  }
  
  getBaseUrl() {
    return this.environment === "production" 
      ? "https://api.example.com"
      : "https://dev.api.example.com";
  }
}
```

**Lesson:** Be mindful of field initialization order. Fields that depend on other fields should either:
1. Be declared after dependencies
2. Be initialized in constructor
3. Be getters (computed properties)

</details>

---

### 🐛 Challenge 7: Method Chaining Broken

```javascript
class QueryBuilder {
  #query = "";
  
  select(fields) {
    this.#query += `SELECT ${fields} `;
  }
  
  from(table) {
    this.#query += `FROM ${table} `;
    return this;
  }
  
  where(condition) {
    this.#query += `WHERE ${condition}`;
    return this;
  }
  
  build() {
    return this.#query;
  }
}

const query = new QueryBuilder()
  .select("*")
  .from("users")
  .where("age > 18")
  .build();

console.log(query);
```

**🔍 What's the bug?**

<details>
<summary>Click to see the answer</summary>

**🐛 Bug:** `select()` method doesn't return `this`, breaking the chain

**Why it's wrong:**
- Method chaining requires every method to return `this`
- `select("*")` returns `undefined`
- Trying to call `.from()` on `undefined` → `TypeError: Cannot read property 'from' of undefined`
- Even though `from()` and `where()` return `this`, the chain is already broken

**✅ Fixed Code:**
```javascript
class QueryBuilder {
  #query = "";
  
  select(fields) {
    this.#query += `SELECT ${fields} `;
    return this;  // ✅ Return this for chaining
  }
  
  from(table) {
    this.#query += `FROM ${table} `;
    return this;
  }
  
  where(condition) {
    this.#query += `WHERE ${condition}`;
    return this;
  }
  
  build() {
    return this.#query.trim();  // Optional: trim extra spaces
  }
}

const query = new QueryBuilder()
  .select("*")
  .from("users")
  .where("age > 18")
  .build();

console.log(query);
// Output: SELECT * FROM users WHERE age > 18
```

**Method Chaining Checklist:**
- ✅ Every chainable method returns `this`
- ✅ Terminal method (like `build()`) returns final value
- ✅ All or nothing—one missing `return this` breaks the chain

</details>

---

### 🐛 Challenge 8: Getter Without Backing Field

```javascript
class Circle {
  constructor(radius) {
    this.radius = radius;
  }
  
  get diameter() {
    return this.radius * 2;
  }
  
  set diameter(value) {
    this.diameter = value / 2;
  }
}

const circle = new Circle(5);
console.log(circle.diameter);
circle.diameter = 20;
console.log(circle.radius);
```

**🔍 What's the bug?**

<details>
<summary>Click to see the answer</summary>

**🐛 Bug:** Infinite recursion in setter

**Why it's wrong:**
- Setter does: `this.diameter = value / 2`
- This calls the setter again (recursive)
- Setter calls itself infinitely → `RangeError: Maximum call stack size exceeded`
- Should update the underlying `radius` property, not `diameter`

**✅ Fixed Code:**
```javascript
class Circle {
  constructor(radius) {
    this.radius = radius;
  }
  
  get diameter() {
    return this.radius * 2;
  }
  
  set diameter(value) {
    this.radius = value / 2;  // ✅ Update underlying radius
  }
  
  get area() {
    return Math.PI * this.radius ** 2;
  }
}

const circle = new Circle(5);
console.log(circle.diameter);  // 10
circle.diameter = 20;
console.log(circle.radius);    // 10
console.log(circle.diameter);  // 20
console.log(circle.area.toFixed(2)); // 314.16
```

**Output:**
```
10
10
20
314.16
```

**Pattern for computed properties:**
```javascript
// ✅ Correct pattern:
get computedProperty() {
  return this.backingField * 2;  // Read from backing field
}

set computedProperty(value) {
  this.backingField = value / 2;  // Write to backing field
}

// ❌ Wrong pattern (infinite recursion):
set computedProperty(value) {
  this.computedProperty = value;  // Calls itself!
}
```

</details>

---

## 6. COMMON PITFALLS & ANTI-PATTERNS

### ❌ Anti-Pattern 1: Forgetting `new` Keyword

```javascript
// ❌ BAD - Forgot 'new'
class User {
  constructor(name) {
    this.name = name;
  }
}

const user = User("John");  // TypeError: Class constructor cannot be invoked without 'new'
```

```javascript
// ✅ GOOD - Use 'new'
const user = new User("John");
```

**Why it matters:** ES6 classes **require** `new`. Constructor functions were more forgiving (but caused bugs).

---

### ❌ Anti-Pattern 2: Mixing Arrow Functions as Methods

```javascript
// ❌ BAD - Arrow function as class method
class Counter {
  count = 0;
  
  increment = () => {  // Instance property, not prototype method
    this.count++;
  }
}

class ExtendedCounter extends Counter {
  increment() {  // Can't override arrow function
    super.increment();  // Error!
    console.log("Count:", this.count);
  }
}
```

```javascript
// ✅ GOOD - Regular method for inheritance
class Counter {
  count = 0;
  
  increment() {  // Prototype method
    this.count++;
  }
}

class ExtendedCounter extends Counter {
  increment() {
    super.increment();  // Works!
    console.log("Count:", this.count);
  }
}
```

**When to use arrow functions in classes:**
- ✅ Event handlers that need fixed `this` context
- ✅ Callbacks passed to other functions
- ❌ Methods you might want to override
- ❌ Methods called via `super`

---

### ❌ Anti-Pattern 3: Not Validating in Setters

```javascript
// ❌ BAD - No validation
class Person {
  set age(value) {
    this._age = value;  // Accepts any value!
  }
}

const person = new Person();
person.age = -50;        // Nonsensical
person.age = "twenty";   // Wrong type
person.age = 999;        // Unrealistic
```

```javascript
// ✅ GOOD - Validate in setter
class Person {
  #age;
  
  get age() {
    return this.#age;
  }
  
  set age(value) {
    if (typeof value !== 'number') {
      throw new TypeError('Age must be a number');
    }
    if (value < 0 || value > 150) {
      throw new RangeError('Age must be between 0 and 150');
    }
    this.#age = value;
  }
}
```

**Setter best practices:**
- ✅ Validate type
- ✅ Validate range
- ✅ Throw descriptive errors
- ✅ Document constraints

---

### ❌ Anti-Pattern 4: Exposing Private Data via Getters

```javascript
// ❌ BAD - Returns reference to private array
class TodoList {
  #todos = [];
  
  getTodos() {
    return this.#todos;  // Exposes internal array!
  }
}

const list = new TodoList();
const todos = list.getTodos();
todos.push("Hack the system");  // Modifies internal state!
```

```javascript
// ✅ GOOD - Return a copy
class TodoList {
  #todos = [];
  
  getTodos() {
    return [...this.#todos];  // Return copy
  }
  
  // Or use getter
  get todos() {
    return this.#todos.map(todo => ({...todo}));  // Deep copy
  }
}
```

**Protection strategies:**
```javascript
// For arrays:
return [...this.#array];                    // Shallow copy
return this.#array.map(item => ({...item})); // Deep copy

// For objects:
return {...this.#object};                   // Shallow copy
return JSON.parse(JSON.stringify(this.#object)); // Deep copy (limited)

// For primitives:
return this.#primitive;  // Safe (passed by value)
```

---

### ❌ Anti-Pattern 5: Overusing Static Methods

```javascript
// ❌ BAD - Everything is static (not OOP)
class UserService {
  static users = [];
  
  static addUser(user) {
    this.users.push(user);
  }
  
  static getUser(id) {
    return this.users.find(u => u.id === id);
  }
}

// This is just a namespace, not OOP
```

```javascript
// ✅ GOOD - Use instance methods for stateful operations
class UserService {
  #users = [];
  
  addUser(user) {
    this.#users.push(user);
    return user;
  }
  
  getUser(id) {
    return this.#users.find(u => u.id === id);
  }
  
  // Static for utilities
  static validateEmail(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  }
}

// Multiple independent instances
const adminService = new UserService();
const guestService = new UserService();
```

**When to use static:**
- ✅ Utility functions (no state needed)
- ✅ Factory methods
- ✅ Constants
- ❌ Operations that need instance state
- ❌ Most business logic

---

### ❌ Anti-Pattern 6: Deep Inheritance Hierarchies

```javascript
// ❌ BAD - Too many levels
class LivingBeing {}
class Animal extends LivingBeing {}
class Mammal extends Animal {}
class Carnivore extends Mammal {}
class Feline extends Carnivore {}
class DomesticCat extends Feline {}  // 6 levels deep!

// Changes to LivingBeing affect DomesticCat
// Hard to understand, maintain, test
```

```javascript
// ✅ GOOD - Prefer composition
class Animal {
  constructor(name) {
    this.name = name;
    this.diet = null;      // Composition
    this.habitat = null;   // Composition
  }
}

class Diet {
  constructor(type) {
    this.type = type; // "carnivore", "herbivore", etc.
  }
}

class Habitat {
  constructor(type) {
    this.type = type; // "domestic", "wild", etc.
  }
}

const cat = new Animal("Fluffy");
cat.diet = new Diet("carnivore");
cat.habitat = new Habitat("domestic");
```

**Rule of thumb:** 
- Max 2-3 levels of inheritance
- Beyond that, use composition
- "Favor composition over inheritance"

---

### ❌ Anti-Pattern 7: Constructor Does Too Much

```javascript
// ❌ BAD - Constructor has side effects
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
    
    // Side effects in constructor:
    this.sendWelcomeEmail();      // ❌ Network call
    this.logToDatabase();         // ❌ I/O operation
    this.updateAnalytics();       // ❌ External dependency
    console.log("User created");  // ❌ Logging
  }
}

// Hard to test, unpredictable, slow
```

```javascript
// ✅ GOOD - Constructor only initializes
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
    this.createdAt = new Date();
  }
  
  // Separate method for post-creation actions
  async initialize() {
    await this.sendWelcomeEmail();
    await this.logToDatabase();
    this.updateAnalytics();
  }
  
  // Or use static factory
  static async create(name, email) {
    const user = new User(name, email);
    await user.initialize();
    return user;
  }
}

// Usage:
const user = await User.create("John", "john@example.com");
```

**Constructor best practices:**
- ✅ Initialize properties
- ✅ Validate inputs
- ✅ Call `super()` if needed
- ❌ No async operations
- ❌ No network calls
- ❌ No file I/O
- ❌ Minimal logging

---

## 7. KNOWLEDGE CHECK MCQs

**Question 1:**
What is the correct way to declare a private field in an ES6 class?

A) `private field = value;`  
B) `_field = value;`  
C) `#field = value;` ✅  
D) `const field = value;`

**Explanation:** Private fields use the `#` prefix. `_field` is just a naming convention (not truly private). `private` keyword doesn't exist in JavaScript classes.

---

**Question 2:**
What will this code output?

```javascript
class Counter {
  static count = 0;
  constructor() {
    Counter.count++;
  }
}
new Counter();
new Counter();
console.log(Counter.count);
```

A) `0`  
B) `1`  
C) `2` ✅  
D) `undefined`

**Explanation:** Static properties are shared across all instances. Each `new Counter()` increments the shared `count`.

---

**Question 3:**
Which statement about getters is TRUE?

A) Getters must have parameters  
B) Getters are called with parentheses like `obj.getter()`  
C) Getters are accessed like properties without parentheses ✅  
D) Getters cannot return computed values

**Explanation:** Getters are accessed like properties: `obj.propertyName`, not `obj.propertyName()`. They can return computed values.

---

**Question 4:**
In a child class constructor, when must `super()` be called?

A) At the end of the constructor  
B) Before using `this` ✅  
C) After initializing all properties  
D) It's optional

**Explanation:** `super()` must be called **before** accessing `this` in derived class constructors. This initializes the parent class.

---

**Question 5:**
What does `typeof MyClass` return?

A) `"class"`  
B) `"object"`  
C) `"function"` ✅  
D) `"constructor"`

**Explanation:** Classes are syntactic sugar over constructor functions. Under the hood, they're functions.

---

**Question 6:**
Which is the correct syntax for a setter?

A) `setter age(value) { this._age = value; }`  
B) `set age(value) { this._age = value; }` ✅  
C) `set age { this._age = value; }`  
D) `age(value) { this._age = value; }`

**Explanation:** Setters use the `set` keyword followed by property name and exactly one parameter.

---

**Question 7:**
What happens if you forget `return this` in a method designed for chaining?

A) The code throws an error  
B) The chain breaks and subsequent calls fail ✅  
C) JavaScript automatically returns `this`  
D) The method returns `undefined` but chaining still works

**Explanation:** Without `return this`, the method returns `undefined`. Trying to call the next method on `undefined` causes `TypeError`.

---

**Question 8:**
Which is TRUE about static methods?

A) They can access instance properties  
B) They are called on class instances  
C) They cannot access `this`  
D) They are called on the class itself ✅

**Explanation:** Static methods belong to the class, not instances. Called with `ClassName.method()`. `this` refers to the class, not an instance.

---

**Question 9:**
What's the difference between these two?

```javascript
class A {
  field = "value";
}

class B {
  constructor() {
    this.field = "value";
  }
}
```

A) A is invalid syntax  
B) B is invalid syntax  
C) Both create the same result ✅  
D) A creates a static property

**Explanation:** Class fields (A) and constructor assignment (B) both create instance properties. Class fields are modern syntax (ES2022), initialized before constructor runs.

---

**Question 10:**
Can private fields be accessed by child classes?

A) Yes, that's the point of private fields  
B) Yes, if the child uses `super`  
C) No, private fields are class-only ✅  
D) Only with getters/setters

**Explanation:** Private fields (`#field`) are **truly private**—accessible only within the class that declares them. Child classes cannot access them directly.

---

**Question 11:**
What's wrong with this code?

```javascript
class Circle {
  get radius() {
    return this.radius;
  }
}
```

A) Nothing, it's correct  
B) Infinite recursion—getter calls itself ✅  
C) Missing setter  
D) Should use `#radius`

**Explanation:** The getter `return this.radius` calls itself infinitely. Should return a backing field like `this._radius` or `this.#radius`.

---

**Question 12:**
Which inheritance keyword is used in ES6 classes?

A) `inherits`  
B) `extends` ✅  
C) `implements`  
D) `derives`

**Explanation:** ES6 uses `extends` for inheritance: `class Child extends Parent {}`.

---

**Question 13:**
What's the purpose of class fields?

A) To create static properties  
B) To initialize instance properties without constructor ✅  
C) To make properties private  
D) To create computed properties

**Explanation:** Class fields provide a shorthand for initializing instance properties directly in the class body, without needing the constructor.

---

**Question 14:**
When are class fields initialized?

A) When the class is defined  
B) Before the constructor runs ✅  
C) After the constructor runs  
D) When first accessed

**Explanation:** Class fields are evaluated and assigned **before** the constructor executes, in top-to-bottom order.

---

**Question 15:**
What does `super.method()` do?

A) Calls the current class's method  
B) Calls the parent class's method ✅  
C) Calls a static method  
D) Creates a new instance

**Explanation:** `super.method()` calls the parent (superclass) version of the method, allowing you to extend rather than completely replace parent functionality.

---

## 8. REAL INTERVIEW QUESTIONS

### 🎤 Question 1: Explain how ES6 classes handle inheritance differently from prototype-based inheritance

**❌ Junior Answer:**
"ES6 classes use the `extends` keyword to inherit from parent classes, and you use `super()` to call the parent constructor. It's cleaner than the old way with prototypes."

**✅ Senior Answer:**
"ES6 classes are syntactic sugar over JavaScript's prototype-based inheritance, but they provide important improvements:

**Under the Hood:**
```javascript
// ES6 Class
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    return `${this.name} makes a sound`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }
  speak() {
    return `${super.speak()} - Woof!`;
  }
}

// Compiles roughly to:
function Animal(name) {
  this.name = name;
}
Animal.prototype.speak = function() {
  return this.name + ' makes a sound';
};

function Dog(name, breed) {
  Animal.call(this, name);  // super()
  this.breed = breed;
}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;
```

**Key Differences:**

1. **Syntax & Readability**: Classes are more intuitive and less error-prone than manually setting up prototype chains.

2. **Strict Mode**: Class bodies always run in strict mode—no accidental globals.

3. **Hoisting**: Classes are not initialized until evaluated (temporal dead zone), unlike function constructors which are fully hoisted:
```javascript
const dog = new Dog(); // ReferenceError with class
class Dog {}

const cat = new Cat(); // Works with function
function Cat() {}
```

4. **Constructor Enforcement**: Classes must be called with `new`, whereas functions can be called without it (causing bugs):
```javascript
class MyClass {}
MyClass(); // TypeError

function MyFunction() {}
MyFunction(); // Works but creates bugs (this = window)
```

5. **Super Keyword**: `super()` and `super.method()` provide clean parent access. With prototypes, you'd need `ParentClass.prototype.method.call(this)`.

6. **Static Inheritance**: Static methods are properly inherited with `extends`:
```javascript
class Parent {
  static method() { return 'parent'; }
}
class Child extends Parent {}
Child.method(); // 'parent' - inherited

// With prototypes, static methods don't inherit automatically
```

7. **Property Descriptors**: Class methods are non-enumerable by default, matching built-in behavior:
```javascript
class MyClass {
  method() {}
}
Object.keys(new MyClass()); // [] - method not enumerable

function MyFunction() {}
MyFunction.prototype.method = function() {};
Object.keys(new MyFunction()); // [] but method is enumerable on prototype
```

The mental model remains prototype-based, but classes provide safety rails and cleaner syntax that prevent common pitfalls."

**Follow-up they might ask:** "Can you show the prototype chain of an instance?"

```javascript
class Animal {}
class Dog extends Animal {}
const dog = new Dog();

console.log(dog.__proto__ === Dog.prototype);              // true
console.log(Dog.prototype.__proto__ === Animal.prototype); // true
console.log(Animal.prototype.__proto__ === Object.prototype); // true
console.log(Object.prototype.__proto__);                   // null

// The chain: dog → Dog.prototype → Animal.prototype → Object.prototype → null
```

---

### 🎤 Question 2: How do private fields (#) differ from the underscore convention (_)?

**❌ Junior Answer:**
"Private fields with # are truly private and can't be accessed from outside the class, while underscore is just a convention that tells developers not to use it, but they still can."

**✅ Senior Answer:**
"The difference is fundamental—one is a language feature, the other is a social contract:

**1. True Privacy vs Convention:**

```javascript
// Convention-based (old way)
class BankAccount {
  constructor(balance) {
    this._balance = balance;  // "Please don't touch"
  }
}

const account = new BankAccount(1000);
console.log(account._balance);  // 1000 - accessible!
account._balance = 9999999;     // Can modify directly
// ⚠️ No actual protection

// True private fields (ES2022)
class SecureBankAccount {
  #balance;
  
  constructor(balance) {
    this.#balance = balance;
  }
  
  getBalance() {
    return this.#balance;
  }
}

const secure = new SecureBankAccount(1000);
console.log(secure.#balance);  // SyntaxError - truly inaccessible
// ✅ Language-enforced privacy
```

**2. Name Collision:**

```javascript
// Convention - prone to conflicts
class Parent {
  _data = "parent";
}

class Child extends Parent {
  _data = "child";  // Overwrites parent's _data
}

// Private fields - no conflicts
class SecureParent {
  #data = "parent";
}

class SecureChild extends SecureParent {
  #data = "child";  // Separate field, doesn't conflict
}
```

**3. Performance:**

Private fields are optimized by JavaScript engines:
- Engine can optimize access patterns
- No runtime property lookup
- Fields are fixed at class definition time

```javascript
// Underscore: standard property lookup
account._balance  // Dynamic property access

// Private field: optimized internal slot
account.#balance  // Direct internal slot access (faster)
```

**4. Debugging:**

```javascript
class Debug {
  #private = "secret";
  _protected = "visible";
}

const obj = new Debug();
console.log(obj);  
// Logs: Debug { _protected: "visible" }
// #private doesn't show in console.log

// In DevTools debugger, private fields appear under [[PrivateProperties]]
```

**5. Reflection and Introspection:**

```javascript
const account = new SecureBankAccount(1000);

// Underscore properties are visible
Object.keys(account);              // ['_balance']
Object.getOwnPropertyNames(account); // ['_balance']
JSON.stringify(account);           // {"_balance": 1000}

// Private fields are invisible
Object.keys(secureAccount);        // []
Object.getOwnPropertyNames(secureAccount); // []
JSON.stringify(secureAccount);     // {}
```

**When to use each:**

**Use `#private`:**
- Truly sensitive data (passwords, tokens)
- Internal implementation details
- When you need guaranteed encapsulation
- Modern codebases (Node 14.6+, modern browsers)

**Use `_protected`:**
- When you need child classes to access
- Legacy code compatibility
- Signal 'internal API' to developers
- When you want properties in JSON serialization

**Example combining both:**

```javascript
class User {
  #password;        // Truly private (security)
  _sessionToken;    // Protected (child classes need it)
  email;            // Public
  
  constructor(email, password) {
    this.email = email;
    this.#password = this.#hash(password);
    this._sessionToken = null;
  }
  
  #hash(password) {
    // Private utility method
    return /* hash implementation */;
  }
  
  authenticate(password) {
    return this.#hash(password) === this.#password;
  }
}

class AdminUser extends User {
  extendSession() {
    // Can access _sessionToken (protected)
    this._sessionToken = /* new token */;
    
    // Cannot access #password (private to User)
    // this.#password  // SyntaxError
  }
}
```

In summary: underscore is documentation, hash is enforcement. Always prefer `#` for true privacy in modern JavaScript."

---

### 🎤 Question 3: Explain getters and setters in classes. When and why would you use them?

**❌ Junior Answer:**
"Getters and setters let you get and set values in a class. You use `get` for reading and `set` for writing. They make your code look cleaner."

**✅ Senior Answer:**
"Getters and setters provide controlled access to properties, enabling validation, computation, and encapsulation. They're fundamental to proper OOP design:

**1. Validation & Data Integrity:**

```javascript
class User {
  #age;
  
  get age() {
    return this.#age;
  }
  
  set age(value) {
    // Type validation
    if (typeof value !== 'number') {
      throw new TypeError('Age must be a number');
    }
    
    // Range validation
    if (value < 0 || value > 150) {
      throw new RangeError('Invalid age range');
    }
    
    // Business rules
    if (value < 18) {
      this.#isMinor = true;
    }
    
    this.#age = Math.floor(value);  // Ensure integer
  }
}

// Usage looks like property access
const user = new User();
user.age = 25;      // Uses setter (validated)
console.log(user.age); // Uses getter (25)
// user.age = -5;   // Throws RangeError
```

**2. Computed Properties:**

```javascript
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
  
  // Computed on-the-fly (no storage)
  get area() {
    return this.width * this.height;
  }
  
  get perimeter() {
    return 2 * (this.width + this.height);
  }
  
  // Derived property you can set
  set area(value) {
    // Keep aspect ratio, adjust width
    const ratio = this.width / this.height;
    this.height = Math.sqrt(value / ratio);
    this.width = value / this.height;
  }
}

const rect = new Rectangle(10, 5);
console.log(rect.area);      // 50 (computed)
rect.area = 100;             // Adjusts dimensions
console.log(rect.width);     // ~14.14
console.log(rect.height);    // ~7.07
```

**3. Backward Compatibility:**

```javascript
// Version 1: Direct property
class Product {
  constructor(price) {
    this.price = price;
  }
}

// Version 2: Add currency conversion (backward compatible)
class Product {
  #priceInCents;
  
  constructor(price) {
    this.price = price;  // Uses setter
  }
  
  // Existing code still works
  get price() {
    return this.#priceInCents / 100;
  }
  
  set price(value) {
    this.#priceInCents = Math.round(value * 100);  // Store as cents
  }
  
  // New functionality
  get priceInCents() {
    return this.#priceInCents;
  }
}

// Old code still works
const product = new Product(19.99);
console.log(product.price);  // 19.99
```

**4. Lazy Initialization:**

```javascript
class DataManager {
  #data;
  #cache;
  
  get processedData() {
    // Compute only when first accessed
    if (!this.#cache) {
      console.log('Computing...');
      this.#cache = this.#expensiveOperation(this.#data);
    }
    return this.#cache;
  }
  
  set data(value) {
    this.#data = value;
    this.#cache = null;  // Invalidate cache
  }
  
  #expensiveOperation(data) {
    // Heavy computation
    return data.map(/* complex transformation */);
  }
}
```

**5. Side Effects & Notifications:**

```javascript
class ObservableProperty {
  #value;
  #listeners = [];
  
  get value() {
    return this.#value;
  }
  
  set value(newValue) {
    const oldValue = this.#value;
    this.#value = newValue;
    
    // Notify observers
    this.#listeners.forEach(listener => {
      listener(newValue, oldValue);
    });
  }
  
  subscribe(callback) {
    this.#listeners.push(callback);
  }
}

const prop = new ObservableProperty();
prop.subscribe((newVal, oldVal) => {
  console.log(`Changed from ${oldVal} to ${newVal}`);
});
prop.value = 10;  // Logs: Changed from undefined to 10
```

**Performance Considerations:**

```javascript
class Performance {
  #cached;
  
  // ✅ Good: Simple getter (fast)
  get simple() {
    return this.#cached;
  }
  
  // ⚠️ Warning: Expensive getter (avoid)
  get expensive() {
    return this.#data.map(x => x * 2).filter(x => x > 10).reduce((a, b) => a + b);
  }
  
  // ✅ Better: Cache expensive computations
  get optimized() {
    if (!this.#cached) {
      this.#cached = /* expensive operation */;
    }
    return this.#cached;
  }
}
```

**Best Practices:**

1. **Use getters for:**
   - Computed properties (area, fullName)
   - Formatted data (price in different currencies)
   - Derived state (isAdult from age)
   - Read-only properties

2. **Use setters for:**
   - Validation (age, email)
   - Data transformation (trimming, parsing)
   - Maintaining invariants (keeping related properties in sync)
   - Change notifications

3. **Avoid:**
   - Heavy computation in getters (cache instead)
   - Side effects in getters (getters should be read-only operations)
   - Setters that don't relate to the property name
   - Using getters/setters for everything (overengineering)

**Pattern: Getter-only for read-only:**

```javascript
class Config {
  #settings;
  
  constructor(settings) {
    this.#settings = Object.freeze(settings);
  }
  
  // Getter only = read-only
  get apiUrl() {
    return this.#settings.apiUrl;
  }
  
  get timeout() {
    return this.#settings.timeout;
  }
  
  // No setters = immutable config
}
```

Getters and setters create a boundary between internal representation and external interface, allowing you to change implementation without breaking dependent code—a cornerstone of maintainable OOP design."

---

### 🎤 Question 4: What are static methods and properties? Provide real-world use cases.

**❌ Junior Answer:**
"Static methods are methods you can call without creating an instance of the class. You call them directly on the class name like `MyClass.staticMethod()`. They're useful for utility functions."

**✅ Senior Answer:**
"Static members belong to the class itself rather than instances. They're shared across all instances and provide class-level functionality. Let me demonstrate with practical patterns:

**1. Factory Methods (Creational Pattern):**

```javascript
class User {
  constructor(name, email, role) {
    this.name = name;
    this.email = email;
    this.role = role;
    this.createdAt = new Date();
  }
  
  // Static factory methods
  static createAdmin(name, email) {
    return new User(name, email, 'admin');
  }
  
  static createGuest() {
    return new User('Guest', 'guest@example.com', 'guest');
  }
  
  static fromJSON(json) {
    const data = JSON.parse(json);
    return new User(data.name, data.email, data.role);
  }
  
  static fromDatabase(row) {
    const user = new User(row.name, row.email, row.role);
    user.id = row.id;
    user.createdAt = new Date(row.created_at);
    return user;
  }
}

// Usage - clearer intent
const admin = User.createAdmin('Alice', 'alice@example.com');
const guest = User.createGuest();
const restored = User.fromJSON('{"name":"Bob",...}');
```

**2. Validation & Utility Functions:**

```javascript
class Email {
  constructor(address) {
    if (!Email.isValid(address)) {
      throw new Error('Invalid email');
    }
    this.address = address;
  }
  
  // Static validators
  static isValid(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  }
  
  static normalize(email) {
    return email.toLowerCase().trim();
  }
  
  static extractDomain(email) {
    return email.split('@')[1];
  }
}

// Used without instances
if (Email.isValid(input)) {
  const domain = Email.extractDomain(input);
  const email = new Email(Email.normalize(input));
}
```

**3. Configuration & Constants:**

```javascript
class DatabaseConnection {
  // Static configuration
  static DEFAULT_TIMEOUT = 5000;
  static MAX_RETRIES = 3;
  static ALLOWED_HOSTS = ['localhost', 'db.example.com'];
  
  // Static connection pool
  static #pool = [];
  static #maxConnections = 10;
  
  constructor(host) {
    if (!DatabaseConnection.ALLOWED_HOSTS.includes(host)) {
      throw new Error('Host not allowed');
    }
    this.host = host;
    this.connected = false;
  }
  
  static getConnection(host) {
    // Return from pool if available
    const existing = this.#pool.find(conn => 
      conn.host === host && !conn.inUse
    );
    
    if (existing) {
      existing.inUse = true;
      return existing;
    }
    
    // Create new if pool not full
    if (this.#pool.length < this.#maxConnections) {
      const conn = new DatabaseConnection(host);
      conn.inUse = true;
      this.#pool.push(conn);
      return conn;
    }
    
    throw new Error('Connection pool exhausted');
  }
  
  static releaseConnection(connection) {
    connection.inUse = false;
  }
}
```

**4. Counters & Tracking:**

```javascript
class Transaction {
  static #transactionCounter = 0;
  static #totalVolume = 0;
  static #transactions = [];
  
  constructor(amount, description) {
    this.id = ++Transaction.#transactionCounter;
    this.amount = amount;
    this.description = description;
    this.timestamp = new Date();
    
    // Update class-level stats
    Transaction.#totalVolume += amount;
    Transaction.#transactions.push(this);
  }
  
  static getTotalVolume() {
    return Transaction.#totalVolume;
  }
  
  static getTransactionCount() {
    return Transaction.#transactionCounter;
  }
  
  static getAverageAmount() {
    return Transaction.#totalVolume / Transaction.#transactionCounter;
  }
  
  static getTransactionsByRange(startDate, endDate) {
    return Transaction.#transactions.filter(t => 
      t.timestamp >= startDate && t.timestamp <= endDate
    );
  }
}

// Usage
const t1 = new Transaction(100, 'Purchase');
const t2 = new Transaction(50, 'Refund');
const t3 = new Transaction(200, 'Sale');

console.log(Transaction.getTotalVolume());      // 350
console.log(Transaction.getTransactionCount()); // 3
console.log(Transaction.getAverageAmount());    // 116.67
```

**5. Singleton Pattern:**

```javascript
class AppConfig {
  static #instance = null;
  
  #settings;
  
  constructor() {
    if (AppConfig.#instance) {
      throw new Error('Use AppConfig.getInstance()');
    }
    this.#settings = {};
  }
  
  static getInstance() {
    if (!AppConfig.#instance) {
      AppConfig.#instance = new AppConfig();
    }
    return AppConfig.#instance;
  }
  
  static reset() {
    AppConfig.#instance = null;
  }
  
  set(key, value) {
    this.#settings[key] = value;
  }
  
  get(key) {
    return this.#settings[key];
  }
}

// Always returns same instance
const config1 = AppConfig.getInstance();
const config2 = AppConfig.getInstance();
console.log(config1 === config2);  // true
```

**6. Static Inheritance:**

```javascript
class Shape {
  static TYPE = 'GENERIC';
  
  static describe() {
    return `This is a ${this.TYPE}`;  // 'this' = calling class
  }
}

class Circle extends Shape {
  static TYPE = 'CIRCLE';
}

class Rectangle extends Shape {
  static TYPE = 'RECTANGLE';
}

console.log(Shape.describe());     // "This is a GENERIC"
console.log(Circle.describe());    // "This is a CIRCLE"
console.log(Rectangle.describe()); // "This is a RECTANGLE"

// Static methods inherit and 'this' refers to the calling class
```

**When NOT to use static:**

```javascript
// ❌ BAD: Should be instance method
class ShoppingCart {
  static items = [];  // Shared by ALL carts!
  
  static addItem(item) {
    this.items.push(item);  // All users share same cart
  }
}

// ✅ GOOD: Instance method
class ShoppingCart {
  #items = [];  // Each cart has its own items
  
  addItem(item) {
    this.#items.push(item);
  }
}
```

**Key Principles:**

1. **Static = Class-level**: No access to instance data (no `this.instanceProperty`)
2. **Use for operations that don't need instance state**: Utilities, factories, constants
3. **Static methods can call other static methods**: `this.otherStaticMethod()`
4. **Static properties are shared**: Changes affect all code using the class
5. **Inherited by child classes**: But `this` refers to the calling class

**Real-World Example - ID Generator:**

```javascript
class Model {
  static #lastId = 0;
  static #registry = new Map();
  
  constructor() {
    this.id = ++Model.#lastId;
    Model.#registry.set(this.id, this);
  }
  
  static find(id) {
    return Model.#registry.get(id);
  }
  
  static findAll() {
    return Array.from(Model.#registry.values());
  }
  
  static count() {
    return Model.#registry.size;
  }
  
  static clear() {
    Model.#lastId = 0;
    Model.#registry.clear();
  }
}

class User extends Model {
  constructor(name) {
    super();
    this.name = name;
  }
}

const user1 = new User('Alice');  // id: 1
const user2 = new User('Bob');    // id: 2

console.log(Model.find(1).name);  // 'Alice'
console.log(Model.count());       // 2
```

Static members are essential for implementing design patterns, managing shared state, and providing class-level utilities—they're a fundamental tool in the OOP toolkit."

---

## 10. CHAPTER SUMMARY & NEXT STEPS

### 🎯 Quick Recap

**You've mastered ES6 Classes:**

✅ **Basic Syntax:**
- Class declaration with `class` keyword
- Constructor for initialization
- Instance methods and properties
- Creating instances with `new`

✅ **Advanced Features:**
- Private fields (`#field`) for true encapsulation
- Getters/setters for computed properties and validation
- Static methods/properties for class-level functionality
- Class fields for clean property initialization

✅ **Inheritance:**
- `extends` keyword for creating hierarchies
- `super()` to call parent constructors
- Method overriding and `super.method()` calls
- Understanding the prototype chain

✅ **OOP Principles Applied:**
- **Encapsulation**: Private fields protect data
- **Abstraction**: Public interface hides complexity
- **Inheritance**: Code reuse through extends
- **Polymorphism**: Method overriding for different behaviors
- **Composition**: Building complex objects from simpler ones

✅ **Debugging Skills:**
- Inspecting private fields in DevTools
- Setting breakpoints on getters/setters
- Tracing inheritance call stacks
- Debugging `this` context issues

---

### 🚨 Key Takeaways (Print This!)

```
┌─────────────────────────────────────────────────────┐
│  ES6 CLASS GOLDEN RULES                             │
├─────────────────────────────────────────────────────┤
│  1. Always call super() BEFORE using this in child  │
│  2. Private fields (#) are truly private           │
│  3. Static methods: Class.method(), not obj.method()│
│  4. Getters: no params, accessed like properties   │
│  5. Setters: exactly 1 param, validate input       │
│  6. Return this for method chaining                │
│  7. Classes require 'new' keyword (enforced)        │
│  8. typeof MyClass === "function" (sugar over funcs)│
└─────────────────────────────────────────────────────┘
```

---

### 🔗 Connection to Next Topic

**Today** you mastered **ES6 Classes**—the modern, clean syntax for OOP in JavaScript.

**Tomorrow (Day 31)** you'll dive deep into **JavaScript Prototypes**—the underlying mechanism that makes classes work. You'll learn:
- What prototypes actually are and how they work
- The prototype chain in detail
- `__proto__` vs `prototype` vs `Object.getPrototypeOf()`
- How classes compile to prototypes under the hood
- Advanced prototype patterns
- Why understanding prototypes makes you a better JavaScript developer

**The Bridge:** Classes are the *what*, prototypes are the *how*. Understanding prototypes will give you X-ray vision into JavaScript's object system and help you debug complex inheritance issues, optimize performance, and truly understand what's happening when you use `extends` and `super`.

---

### ✅ Self-Assessment Checklist

Before moving to Day 31, you should be able to:

- [ ] Write a class with constructor, public/private fields, and methods
- [ ] Implement getters and setters with validation
- [ ] Create static methods and understand when to use them
- [ ] Build inheritance hierarchies with extends and super
- [ ] Debug private fields and inheritance in DevTools
- [ ] Explain why typeof MyClass === "function"
- [ ] Implement method chaining by returning `this`
- [ ] Apply all five OOP principles using classes

**If you checked all boxes:** You're ready for Day 31! 🚀

**If you missed some:** Review the guided problems and debug challenges for those concepts.


---

### 🎉 Congratulations on Completing Day 30! 🎉

**You've unlocked:**
- 🏆 ES6 Class Master
- 🔐 Private Field Expert
- ⚡ Static Method Specialist
- 👨‍👩‍👧‍👦 Inheritance Pro
- 🐛 Class Debugging Detective

**Next:** Day 31 - Master JavaScript Prototypes and Object Patterns

---

<div align="center">

*"Classes are syntactic sugar, but prototypes are the secret sauce. Tomorrow, we taste the sauce!"* 🍝

**Ready for Day 31?** 

The journey from OOP Hero to Prototype Master begins now! 🚀

</div>