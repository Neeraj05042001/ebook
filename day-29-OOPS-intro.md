# 📘Object Oriented Programming (OOP) Explained with Real-Life Analogies

---

## 1. CHAPTER OVERVIEW

### 📹 In the Video
You learned how Object-Oriented Programming organizes code around real-world entities (objects) instead of scattered functions and variables. We explored the five core OOP principles—Abstraction, Encapsulation, Inheritance, Polymorphism, and Composition—using relatable analogies like smartphones, cars, and ATMs to make these concepts click.

### ✅ Checklist: By the end of this, you will be able to...
- [ ] Create objects using all 6 different methods (literal, constructor, class, factory, etc.)
- [ ] Implement encapsulation using private fields (`#`) to protect sensitive data
- [ ] Build class hierarchies using `extends` and `super` for inheritance
- [ ] Apply polymorphism by overriding methods in child classes
- [ ] Design systems using composition (has-a relationships) instead of deep inheritance
- [ ] Identify which OOP principle solves which real-world problem
- [ ] Debug common OOP mistakes like forgetting `new`, incorrect `this` binding, or missing `super()`

### 📚 Prerequisites
From **Module 1 to 4**, you should be comfortable with:
- Functions (regular, arrow, callbacks)
- ES6 syntax (template literals, destructuring, spread operator)
- Asynchronous concepts (promises, async/await)
- DOM manipulation basics

---

## 2. LECTURE CHEAT SHEET

### 🎯 Core OOP Vocabulary

| Term | Definition | Example |
|------|------------|---------|
| **Object** | A collection of properties (data) and methods (functions) | `const car = { brand: "Toyota", start() {...} }` |
| **Class** | A blueprint/template for creating objects | `class Car { constructor(brand) {...} }` |
| **Instance** | An actual object created from a class | `const myCar = new Car("Toyota")` |
| **Constructor** | Special method that runs when creating a new instance | `constructor(name, age) { this.name = name; }` |
| **Method** | A function that belongs to an object/class | `greet() { return "Hello"; }` |
| **Property** | Data/attribute stored in an object | `this.name`, `this.age` |
| **Private Field** | Property accessible only inside the class (ES2022+) | `#password` |
| **Static Method** | Method called on the class itself, not instances | `static multiply(a, b) {...}` |
| **Inheritance** | Child class gets properties/methods from parent | `class Dog extends Animal` |
| **Polymorphism** | Same method name, different behaviors in child classes | `makeSound()` in Dog vs Cat |

---

### 🛠️ 6 Ways to Create Objects

```javascript
// ✅ 1. Object Literal (Simple, one-time objects)
const person1 = {
  name: "Neeraj",
  greet() { console.log("Hi!"); }
};

// ✅ 2. new Object() (Rarely used, verbose)
const person2 = new Object();
person2.name = "Neeraj";

// ✅ 3. Factory Function (No 'new', returns object)
function createPerson(name) {
  return {
    name,
    greet() { console.log(`Hi, I'm ${this.name}`); }
  };
}
const person3 = createPerson("Neeraj");

// ✅ 4. Constructor Function (Old-school OOP)
function Person(name) {
  this.name = name;
  this.greet = function() { console.log(`Hi, I'm ${this.name}`); };
}
const person4 = new Person("Neeraj");

// ✅ 5. ES6 Class (Modern, recommended)
class PersonClass {
  constructor(name) {
    this.name = name;
  }
  greet() { console.log(`Hi, I'm ${this.name}`); }
}
const person5 = new PersonClass("Neeraj");

// ✅ 6. Object.create() (Manual prototype control)
const personProto = {
  greet() { console.log(`Hi, I'm ${this.name}`); }
};
const person6 = Object.create(personProto);
person6.name = "Neeraj";
```

**When to Use What?**
- **Daily use:** ES6 Classes (`class`)
- **Need private data without classes:** Factory Functions
- **One-off objects:** Object Literal
- **Legacy codebases:** Constructor Functions

---

### 🔒 Encapsulation: Public vs Private

```javascript
class BankAccount {
  #balance = 0;        // ❌ Private (cannot access outside)
  accountNumber = 123; // ✅ Public (accessible anywhere)
  
  constructor(initial) {
    this.#balance = initial;
  }
  
  // ✅ Public method (controlled access)
  deposit(amount) {
    if (amount > 0) this.#balance += amount;
  }
  
  // ❌ Private method (internal use only)
  #validateTransaction() {
    return this.#balance > 0;
  }
  
  getBalance() {
    return this.#balance; // Controlled read access
  }
}

const account = new BankAccount(1000);
account.deposit(500);
console.log(account.getBalance()); // ✅ 1500
console.log(account.#balance);     // ❌ SyntaxError: Private field
```

**Key Rules:**
- Use `#` prefix for private fields/methods
- Private members are **truly private** (not just convention like `_variable`)
- Access private data through public methods (getters/setters)

---

### 👨‍👩‍👧‍👦 Inheritance Syntax Breakdown

```javascript
class Vehicle {
  constructor(brand) {
    this.brand = brand;
    this.speed = 0;
  }
  
  accelerate(amount) {
    this.speed += amount;
    return `Speed: ${this.speed}`;
  }
}

class Car extends Vehicle {
  constructor(brand, doors) {
    super(brand);      // ⚠️ MUST call parent constructor first
    this.doors = doors; // ✅ Then set child-specific properties
  }
  
  // Override parent method
  accelerate(amount) {
    super.accelerate(amount * 2); // Call parent version
    return `${this.brand} car speeding at ${this.speed}!`;
  }
}

const myCar = new Car("Toyota", 4);
myCar.accelerate(10); // "Toyota car speeding at 20!"
```

**Critical Rules:**
1. Use `extends` to inherit from parent class
2. **Must** call `super()` before using `this` in child constructor
3. Use `super.methodName()` to call parent methods
4. Child can override parent methods (same name = replaces parent version)

---

### 🎭 The Five OOP Principles (Quick Reference)

```
┌─────────────────────────────────────────────────────────────┐
│  PRINCIPLE       │  MEANING            │  EXAMPLE            │
├─────────────────────────────────────────────────────────────┤
│  🎪 Abstraction  │  Hide complexity,   │  ATM.withdraw()     │
│                  │  show simple        │  (you don't see     │
│                  │  interface          │  bank backend)      │
├─────────────────────────────────────────────────────────────┤
│  📦 Encapsulation│  Bundle data +      │  class User {       │
│                  │  methods, restrict  │    #password        │
│                  │  access             │  }                  │
├─────────────────────────────────────────────────────────────┤
│  👨‍👩‍👧‍👦 Inheritance│  Share code         │  class Dog extends  │
│                  │  between classes    │  Animal             │
│                  │  (is-a)             │                     │
├─────────────────────────────────────────────────────────────┤
│  🎭 Polymorphism │  Same method,       │  dog.makeSound() →  │
│                  │  different          │  "Woof"             │
│                  │  behaviors          │  cat.makeSound() →  │
│                  │                     │  "Meow"             │
├─────────────────────────────────────────────────────────────┤
│  🧱 Composition  │  Build complex from │  class Car {        │
│                  │  simple objects     │    engine = new     │
│                  │  (has-a)            │    Engine()         │
│                  │                     │  }                  │
└─────────────────────────────────────────────────────────────┘
```

---

### ⚡ Static Methods vs Instance Methods

```javascript
class MathHelper {
  // ❌ Instance method (called on objects)
  double(num) {
    return num * 2;
  }
  
  // ✅ Static method (called on class itself)
  static triple(num) {
    return num * 3;
  }
}

const helper = new MathHelper();
helper.double(5);        // ✅ 10
MathHelper.triple(5);    // ✅ 15

helper.triple(5);        // ❌ TypeError
MathHelper.double(5);    // ❌ TypeError
```

**When to use Static:**
- Utility functions (Math.max, Array.from)
- Factory methods (User.create())
- Constants (Config.API_URL)

---

### 🔄 Inheritance vs Composition Decision Tree

```
Question: Do you need to share behavior?
    │
    ├─ YES → Is it an "is-a" relationship?
    │         │
    │         ├─ YES → Use INHERITANCE
    │         │        (Dog IS-A Animal)
    │         │
    │         └─ NO → Use COMPOSITION
    │                  (Car HAS-A Engine)
    │
    └─ NO → Use standalone classes
```

**Composition Example:**
```javascript
class Engine {
  start() { return "Engine started"; }
}

class Car {
  constructor() {
    this.engine = new Engine(); // ✅ Has-a relationship
  }
  
  startCar() {
    return this.engine.start();
  }
}
```

---

### 📊 Comparison: Inheritance vs Composition

| Aspect | Inheritance | Composition |
|--------|------------|-------------|
| **Relationship** | "is-a" | "has-a" |
| **Coupling** | Tight (child depends on parent) | Loose (independent parts) |
| **Flexibility** | Fixed at design time | Can change at runtime |
| **When to use** | Clear hierarchies (Animal→Dog) | Combining behaviors (Car has Engine, GPS) |
| **Code reuse** | Vertical (up the chain) | Horizontal (mix and match) |
| **Example** | `class Car extends Vehicle` | `car.engine = new Engine()` |

---

## 3. PREDICT THE OUTPUT

### 🧠 Test Your Understanding

**Question 1:**
```javascript
class Counter {
  #count = 0;
  
  increment() {
    this.#count++;
  }
  
  getCount() {
    return this.#count;
  }
}

const c1 = new Counter();
const c2 = new Counter();
c1.increment();
c1.increment();
c2.increment();

console.log(c1.getCount());
console.log(c2.getCount());
```

**🤔 Thinking Process:**
- Private field `#count` is **instance-specific** (each object has its own)
- `c1` incremented twice: 0 → 1 → 2
- `c2` incremented once: 0 → 1
- Each instance maintains separate state

**✅ Output:**
```
2
1
```

**Why:** Private fields are not shared between instances. Each object gets its own copy.

---

**Question 2:**
```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    return `${this.name} makes a sound`;
  }
}

class Dog extends Animal {
  speak() {
    return `${this.name} barks`;
  }
}

const animal = new Animal("Generic");
const dog = new Dog("Buddy");

console.log(animal.speak());
console.log(dog.speak());
```

**🤔 Thinking Process:**
- `Dog` class **overrides** the `speak()` method
- When called on `animal`: uses `Animal.speak()`
- When called on `dog`: uses `Dog.speak()` (polymorphism)

**✅ Output:**
```
Generic makes a sound
Buddy barks
```

**Why:** Method overriding. Child class method replaces parent's version.

---

**Question 3:**
```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  
  static greet() {
    return `Hello from Person class`;
  }
}

const person = new Person("John");
console.log(person.greet());
```

**🤔 Thinking Process:**
- `greet()` is a **static method** (called on class, not instance)
- `person.greet()` tries to call it on an instance
- Static methods are not inherited by instances

**✅ Output:**
```
TypeError: person.greet is not a function
```

**Why:** Static methods belong to the class itself. Correct call: `Person.greet()`

---

**Question 4:**
```javascript
class Car {
  constructor(brand) {
    this.brand = brand;
  }
}

class ElectricCar extends Car {
  constructor(brand, batteryLife) {
    this.batteryLife = batteryLife;
    super(brand);
  }
}

const tesla = new ElectricCar("Tesla", "500km");
```

**🤔 Thinking Process:**
- In child constructor, `this` is used **before** `super()`
- `super()` must be called first to initialize parent class
- Order matters: `super()` → then `this`

**✅ Output:**
```
ReferenceError: Must call super constructor in derived class before accessing 'this'
```

**Why:** Always call `super()` before using `this` in child constructors.

---

**Question 5:**
```javascript
function createUser(name) {
  return {
    name: name,
    greet: () => {
      return `Hi, I'm ${this.name}`;
    }
  };
}

const user = createUser("Alice");
console.log(user.greet());
```

**🤔 Thinking Process:**
- Arrow functions don't have their own `this`
- `this` refers to the outer scope (global/window)
- `this.name` is undefined in global scope

**✅ Output:**
```
Hi, I'm undefined
```

**Why:** Arrow functions inherit `this` from parent scope. Use regular functions for methods.

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
const final = calc.add(5).multiply(3).add(10).result;
console.log(final);
```

**🤔 Thinking Process:**
- Each method returns `this` (method chaining)
- Start: result = 0
- `.add(5)` → 0 + 5 = 5
- `.multiply(3)` → 5 * 3 = 15
- `.add(10)` → 15 + 10 = 25

**✅ Output:**
```
25
```

**Why:** Returning `this` enables method chaining (fluent interface pattern).

---

**Question 7:**
```javascript
class Parent {
  constructor() {
    this.type = "parent";
  }
  
  getType() {
    return this.type;
  }
}

class Child extends Parent {
  constructor() {
    super();
    this.type = "child";
  }
}

const obj = new Child();
console.log(obj.getType());
```

**🤔 Thinking Process:**
- `super()` initializes parent (sets `type = "parent"`)
- Then child constructor overwrites `type = "child"`
- `getType()` is inherited, returns current `this.type`

**✅ Output:**
```
child
```

**Why:** Child constructor runs after parent, last assignment wins.

---

**Question 8:**
```javascript
class BankAccount {
  #balance = 1000;
  
  getBalance() {
    return this.#balance;
  }
}

const account1 = new BankAccount();
const account2 = account1;

console.log(account1.getBalance());
console.log(account2.getBalance());
console.log(account1 === account2);
```

**🤔 Thinking Process:**
- `account2` is a **reference** to the same object as `account1`
- Both variables point to the same memory location
- Comparing references with `===` returns true

**✅ Output:**
```
1000
1000
true
```

**Why:** Objects are assigned by reference, not by value.

---

**Question 9:**
```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    this.breed = breed;
  }
}

const dog = new Dog("Max", "Labrador");
console.log(dog.name);
```

**🤔 Thinking Process:**
- Child constructor doesn't call `super(name)`
- Parent's `name` property is never initialized
- Missing `super()` call

**✅ Output:**
```
ReferenceError: Must call super constructor
```

**Why:** Child constructors must call `super()` to initialize parent class.

---

**Question 10:**
```javascript
class Counter {
  static count = 0;
  
  constructor() {
    Counter.count++;
  }
  
  static getCount() {
    return Counter.count;
  }
}

new Counter();
new Counter();
new Counter();

console.log(Counter.getCount());
```

**🤔 Thinking Process:**
- `static count` is **shared across all instances**
- Each `new Counter()` increments the shared counter
- Created 3 instances: count = 0 → 1 → 2 → 3

**✅ Output:**
```
3
```

**Why:** Static properties are class-level (shared), not instance-level.

---

## 4. GUIDED PRACTICE PROBLEMS

---

### 🔥 Problem 1: Warm-up (Easy) - Library Book Tracker

**📋 Task Description:**
Create a `Book` class that tracks library books. Each book should have a title, author, ISBN, and availability status. Implement methods to borrow and return books.

**🎯 Requirements:**
1. Create a `Book` class with:
   - Properties: `title`, `author`, `isbn`, `isAvailable` (default: true)
2. Add a `borrow()` method that:
   - Marks the book as unavailable if it's available
   - Returns a success message or "Book is already borrowed"
3. Add a `returnBook()` method that:
   - Marks the book as available
   - Returns a success message
4. Add a `getInfo()` method that returns book details

**💻 Starter Code:**
```javascript
// Create your Book class here

// Test Cases
const book1 = new Book("1984", "George Orwell", "978-0-452-28423-4");
console.log(book1.getInfo());
console.log(book1.borrow());
console.log(book1.borrow()); // Try borrowing again
console.log(book1.returnBook());
console.log(book1.borrow());
```

**✅ Solution:**
```javascript
class Book {
  constructor(title, author, isbn) {
    this.title = title;
    this.author = author;
    this.isbn = isbn;
    this.isAvailable = true;
  }
  
  borrow() {
    if (this.isAvailable) {
      this.isAvailable = false;
      return `"${this.title}" has been borrowed successfully! 📚`;
    }
    return `"${this.title}" is already borrowed. Please check back later. ❌`;
  }
  
  returnBook() {
    this.isAvailable = true;
    return `"${this.title}" has been returned. Thank you! ✅`;
  }
  
  getInfo() {
    const status = this.isAvailable ? "Available ✅" : "Borrowed ❌";
    return `"${this.title}" by ${this.author} (ISBN: ${this.isbn}) - ${status}`;
  }
}

// Test Cases
const book1 = new Book("1984", "George Orwell", "978-0-452-28423-4");
console.log(book1.getInfo());
// Output: "1984" by George Orwell (ISBN: 978-0-452-28423-4) - Available ✅

console.log(book1.borrow());
// Output: "1984" has been borrowed successfully! 📚

console.log(book1.borrow());
// Output: "1984" is already borrowed. Please check back later. ❌

console.log(book1.returnBook());
// Output: "1984" has been returned. Thank you! ✅

console.log(book1.borrow());
// Output: "1984" has been borrowed successfully! 📚
```

**🚨 Common Mistakes:**
1. **Forgetting to toggle `isAvailable`:** The status must change in both methods
2. **Not validating state:** Always check if the book is available before borrowing
3. **Poor return messages:** Use descriptive messages for better UX
4. **Mutable default parameters:** Never use objects/arrays as default values in parameters

---

### 🔥 Problem 2: Logic Builder (Medium) - Bank Account with Transaction History

**📋 Task Description:**
Build a `BankAccount` class with private balance and transaction tracking. Users should be able to deposit, withdraw, and view transaction history.

**🎯 Requirements:**
1. Create a `BankAccount` class with:
   - Private field `#balance` (initialized in constructor)
   - Private field `#transactions` (array to store history)
   - Public property `accountNumber` (auto-generated)
2. Implement `deposit(amount)`:
   - Validate amount is positive
   - Update balance
   - Record transaction with timestamp
3. Implement `withdraw(amount)`:
   - Validate amount is positive and available
   - Update balance
   - Record transaction
4. Implement `getBalance()`: Returns current balance
5. Implement `getTransactionHistory()`: Returns last 5 transactions
6. Implement `getStatement()`: Returns formatted account statement

**💻 Starter Code:**
```javascript
// Create your BankAccount class here

// Test Cases
const account = new BankAccount(1000);
console.log(account.getBalance());

account.deposit(500);
account.deposit(200);
account.withdraw(300);
account.withdraw(2000); // Should fail
account.deposit(-100);  // Should fail

console.log(account.getStatement());
console.log(account.getTransactionHistory());
```

**✅ Solution:**
```javascript
class BankAccount {
  #balance;
  #transactions;
  
  constructor(initialBalance = 0) {
    this.#balance = initialBalance;
    this.#transactions = [];
    this.accountNumber = this.#generateAccountNumber();
    
    // Record initial deposit
    if (initialBalance > 0) {
      this.#addTransaction('deposit', initialBalance, 'Initial deposit');
    }
  }
  
  #generateAccountNumber() {
    return 'ACC' + Date.now().toString().slice(-8);
  }
  
  #addTransaction(type, amount, description) {
    this.#transactions.push({
      type,
      amount,
      description,
      balance: this.#balance,
      timestamp: new Date().toLocaleString()
    });
  }
  
  deposit(amount) {
    if (typeof amount !== 'number' || amount <= 0) {
      console.log('❌ Invalid deposit amount. Must be a positive number.');
      return false;
    }
    
    this.#balance += amount;
    this.#addTransaction('deposit', amount, 'Deposit');
    console.log(`✅ Deposited $${amount}. New balance: $${this.#balance}`);
    return true;
  }
  
  withdraw(amount) {
    if (typeof amount !== 'number' || amount <= 0) {
      console.log('❌ Invalid withdrawal amount. Must be a positive number.');
      return false;
    }
    
    if (amount > this.#balance) {
      console.log(`❌ Insufficient funds. Available balance: $${this.#balance}`);
      return false;
    }
    
    this.#balance -= amount;
    this.#addTransaction('withdraw', amount, 'Withdrawal');
    console.log(`✅ Withdrew $${amount}. Remaining balance: $${this.#balance}`);
    return true;
  }
  
  getBalance() {
    return `Current Balance: $${this.#balance}`;
  }
  
  getTransactionHistory() {
    const recentTransactions = this.#transactions.slice(-5);
    return recentTransactions.map(t => 
      `${t.timestamp} | ${t.type.toUpperCase()} | $${t.amount} | Balance: $${t.balance}`
    );
  }
  
  getStatement() {
    const statement = [
      '═══════════════════════════════════════',
      `Account Number: ${this.accountNumber}`,
      `Current Balance: $${this.#balance}`,
      `Total Transactions: ${this.#transactions.length}`,
      '═══════════════════════════════════════'
    ];
    return statement.join('\n');
  }
}

// Test Cases
const account = new BankAccount(1000);
console.log(account.getBalance());
// Output: Current Balance: $1000

account.deposit(500);
// Output: ✅ Deposited $500. New balance: $1500

account.deposit(200);
// Output: ✅ Deposited $200. New balance: $1700

account.withdraw(300);
// Output: ✅ Withdrew $300. Remaining balance: $1400

account.withdraw(2000);
// Output: ❌ Insufficient funds. Available balance: $1400

account.deposit(-100);
// Output: ❌ Invalid deposit amount. Must be a positive number.

console.log(account.getStatement());
/* Output:
═══════════════════════════════════════
Account Number: ACC12345678
Current Balance: $1400
Total Transactions: 4
═══════════════════════════════════════
*/

console.log(account.getTransactionHistory());
/* Output: Array of last 5 transactions with timestamps */
```

**🚨 Common Mistakes:**
1. **Not validating input:** Always check if amount is a positive number
2. **Forgetting to update balance:** Both deposit and withdraw must modify `#balance`
3. **Not recording failed transactions:** Only successful operations should be logged
4. **Exposing private fields:** Never return `#transactions` directly (return a copy or formatted version)
5. **Missing `#` for private fields:** Private fields MUST have the `#` prefix
6. **Incorrect timestamp handling:** Use `Date` objects consistently

---

### 🔥 Problem 3: Pro Challenge (Hard) - E-Learning Platform

**📋 Task Description:**
Build a mini e-learning platform with `Course`, `Student`, and `Instructor` classes. Implement inheritance, composition, and all OOP principles.

**🎯 Requirements:**
1. Create a `Person` base class with:
   - Properties: `id`, `name`, `email`
   - Method: `getProfile()`
2. Create `Student` class extending `Person`:
   - Additional property: `enrolledCourses` (array)
   - Private property: `#grades` (object mapping courseId to grade)
   - Methods: `enrollInCourse(course)`, `submitAssignment(courseId, grade)`, `getGPA()`
3. Create `Instructor` class extending `Person`:
   - Additional property: `coursesTaught` (array)
   - Method: `createCourse(title, duration)`, `assignGrade(student, courseId, grade)`
4. Create `Course` class with:
   - Properties: `id`, `title`, `instructor`, `duration`, `students`, `maxCapacity`
   - Methods: `addStudent(student)`, `removeStudent(studentId)`, `isFull()`, `getEnrollmentRate()`
5. Implement proper encapsulation, validation, and error handling

**💻 Starter Code:**
```javascript
// Create your classes here

// Test Scenario
const instructor1 = new Instructor("I001", "Dr. Smith", "smith@university.edu");
const course1 = instructor1.createCourse("JavaScript Mastery", "12 weeks");

const student1 = new Student("S001", "Alice", "alice@email.com");
const student2 = new Student("S002", "Bob", "bob@email.com");

student1.enrollInCourse(course1);
student2.enrollInCourse(course1);

instructor1.assignGrade(student1, course1.id, 95);
instructor1.assignGrade(student2, course1.id, 87);

console.log(student1.getGPA());
console.log(course1.getEnrollmentRate());
```

**✅ Solution:**
```javascript
// Base Person class
class Person {
  constructor(id, name, email) {
    this.id = id;
    this.name = name;
    this.email = email;
    this.createdAt = new Date();
  }
  
  getProfile() {
    return {
      id: this.id,
      name: this.name,
      email: this.email,
      role: this.constructor.name
    };
  }
}

// Course class (Composition - used by Student and Instructor)
class Course {
  static courseCounter = 1000;
  
  constructor(title, instructor, duration, maxCapacity = 30) {
    this.id = `CRS${++Course.courseCounter}`;
    this.title = title;
    this.instructor = instructor;
    this.duration = duration;
    this.maxCapacity = maxCapacity;
    this.students = [];
    this.createdAt = new Date();
  }
  
  addStudent(student) {
    if (this.isFull()) {
      return `❌ Course "${this.title}" is full (${this.maxCapacity} students)`;
    }
    
    if (this.students.find(s => s.id === student.id)) {
      return `❌ ${student.name} is already enrolled in "${this.title}"`;
    }
    
    this.students.push(student);
    return `✅ ${student.name} enrolled in "${this.title}"`;
  }
  
  removeStudent(studentId) {
    const index = this.students.findIndex(s => s.id === studentId);
    if (index === -1) {
      return `❌ Student not found in this course`;
    }
    
    const removed = this.students.splice(index, 1)[0];
    return `✅ ${removed.name} removed from "${this.title}"`;
  }
  
  isFull() {
    return this.students.length >= this.maxCapacity;
  }
  
  getEnrollmentRate() {
    const rate = (this.students.length / this.maxCapacity) * 100;
    return `${this.title}: ${this.students.length}/${this.maxCapacity} students (${rate.toFixed(1)}% full)`;
  }
  
  getCourseInfo() {
    return {
      id: this.id,
      title: this.title,
      instructor: this.instructor.name,
      duration: this.duration,
      enrolled: this.students.length,
      capacity: this.maxCapacity
    };
  }
}

// Student class (Inheritance from Person)
class Student extends Person {
  #grades;
  
  constructor(id, name, email) {
    super(id, name, email);
    this.enrolledCourses = [];
    this.#grades = {};
  }
  
  enrollInCourse(course) {
    if (this.enrolledCourses.find(c => c.id === course.id)) {
      console.log(`❌ Already enrolled in "${course.title}"`);
      return false;
    }
    
    const result = course.addStudent(this);
    console.log(result);
    
    if (result.startsWith('✅')) {
      this.enrolledCourses.push(course);
      return true;
    }
    return false;
  }
  
  unenrollFromCourse(courseId) {
    const course = this.enrolledCourses.find(c => c.id === courseId);
    if (!course) {
      console.log(`❌ Not enrolled in this course`);
      return false;
    }
    
    course.removeStudent(this.id);
    this.enrolledCourses = this.enrolledCourses.filter(c => c.id !== courseId);
    delete this.#grades[courseId];
    console.log(`✅ Unenrolled from "${course.title}"`);
    return true;
  }
  
  submitAssignment(courseId, grade) {
    if (!this.enrolledCourses.find(c => c.id === courseId)) {
      return `❌ Not enrolled in this course`;
    }
    
    if (grade < 0 || grade > 100) {
      return `❌ Invalid grade. Must be between 0-100`;
    }
    
    this.#grades[courseId] = grade;
    return `✅ Assignment submitted. Grade: ${grade}`;
  }
  
  getGPA() {
    const grades = Object.values(this.#grades);
    if (grades.length === 0) return 'No grades yet';
    
    const average = grades.reduce((sum, grade) => sum + grade, 0) / grades.length;
    const gpa = (average / 100) * 4; // Convert to 4.0 scale
    
    return {
      averageScore: average.toFixed(2),
      gpa: gpa.toFixed(2),
      coursesCompleted: grades.length
    };
  }
  
  getTranscript() {
    return this.enrolledCourses.map(course => ({
      course: course.title,
      grade: this.#grades[course.id] || 'Pending',
      instructor: course.instructor.name
    }));
  }
}

// Instructor class (Inheritance from Person)
class Instructor extends Person {
  constructor(id, name, email) {
    super(id, name, email);
    this.coursesTaught = [];
  }
  
  createCourse(title, duration, maxCapacity = 30) {
    const course = new Course(title, this, duration, maxCapacity);
    this.coursesTaught.push(course);
    console.log(`✅ Course "${title}" created by ${this.name}`);
    return course;
  }
  
  assignGrade(student, courseId, grade) {
    const course = this.coursesTaught.find(c => c.id === courseId);
    
    if (!course) {
      console.log(`❌ You don't teach this course`);
      return false;
    }
    
    if (!course.students.find(s => s.id === student.id)) {
      console.log(`❌ ${student.name} is not enrolled in this course`);
      return false;
    }
    
    const result = student.submitAssignment(courseId, grade);
    console.log(`${this.name} assigned grade to ${student.name}: ${result}`);
    return true;
  }
  
  getTeachingLoad() {
    const totalStudents = this.coursesTaught.reduce(
      (sum, course) => sum + course.students.length, 0
    );
    
    return {
      instructor: this.name,
      coursesTeaching: this.coursesTaught.length,
      totalStudents,
      courses: this.coursesTaught.map(c => c.title)
    };
  }
}

// ==================== Test Scenario ====================

const instructor1 = new Instructor("I001", "Dr. Smith", "smith@university.edu");
const instructor2 = new Instructor("I002", "Prof. Johnson", "johnson@university.edu");

// Create courses
const course1 = instructor1.createCourse("JavaScript Mastery", "12 weeks");
const course2 = instructor1.createCourse("React Advanced", "8 weeks");
const course3 = instructor2.createCourse("Node.js Backend", "10 weeks", 5);

// Create students
const student1 = new Student("S001", "Alice", "alice@email.com");
const student2 = new Student("S002", "Bob", "bob@email.com");
const student3 = new Student("S003", "Charlie", "charlie@email.com");

// Enroll students
student1.enrollInCourse(course1);
student1.enrollInCourse(course2);
student2.enrollInCourse(course1);
student2.enrollInCourse(course3);
student3.enrollInCourse(course1);

// Assign grades
instructor1.assignGrade(student1, course1.id, 95);
instructor1.assignGrade(student2, course1.id, 87);
instructor1.assignGrade(student3, course1.id, 92);
instructor1.assignGrade(student1, course2.id, 88);

// Check results
console.log('\n📊 Student 1 GPA:', student1.getGPA());
console.log('📊 Student 2 GPA:', student2.getGPA());

console.log('\n📈 Course Enrollment:');
console.log(course1.getEnrollmentRate());
console.log(course2.getEnrollmentRate());
console.log(course3.getEnrollmentRate());

console.log('\n👨‍🏫 Instructor Teaching Load:');
console.log(instructor1.getTeachingLoad());

console.log('\n📋 Student 1 Transcript:');
console.log(student1.getTranscript());
```

**Expected Output:**
```
✅ Course "JavaScript Mastery" created by Dr. Smith
✅ Course "React Advanced" created by Dr. Smith
✅ Course "Node.js Backend" created by Prof. Johnson
✅ Alice enrolled in "JavaScript Mastery"
✅ Alice enrolled in "React Advanced"
✅ Bob enrolled in "JavaScript Mastery"
✅ Bob enrolled in "Node.js Backend"
✅ Charlie enrolled in "JavaScript Mastery"
Dr. Smith assigned grade to Alice: ✅ Assignment submitted. Grade: 95
Dr. Smith assigned grade to Bob: ✅ Assignment submitted. Grade: 87
Dr. Smith assigned grade to Charlie: ✅ Assignment submitted. Grade: 92
Dr. Smith assigned grade to Alice: ✅ Assignment submitted. Grade: 88

📊 Student 1 GPA: { averageScore: '91.50', gpa: '3.66', coursesCompleted: 2 }
📊 Student 2 GPA: { averageScore: '87.00', gpa: '3.48', coursesCompleted: 1 }

📈 Course Enrollment:
JavaScript Mastery: 3/30 students (10.0% full)
React Advanced: 1/30 students (3.3% full)
Node.js Backend: 1/5 students (20.0% full)

👨‍🏫 Instructor Teaching Load:
{
  instructor: 'Dr. Smith',
  coursesTeaching: 2,
  totalStudents: 4,
  courses: [ 'JavaScript Mastery', 'React Advanced' ]
}

📋 Student 1 Transcript:
[
  { course: 'JavaScript Mastery', grade: 95, instructor: 'Dr. Smith' },
  { course: 'React Advanced', grade: 88, instructor: 'Dr. Smith' }
]
```

**🚨 Common Mistakes:**
1. **Circular references:** Student adds course, course adds student - handle bidirectional relationships carefully
2. **Not calling `super()`:** Always call parent constructor in child classes
3. **Direct array manipulation:** Use methods like `push()`, `splice()` instead of direct assignment
4. **Not validating relationships:** Check if student is enrolled before assigning grades
5. **Forgetting encapsulation:** Grades should be private (`#grades`)
6. **Poor error messages:** Always provide descriptive feedback
7. **Not handling edge cases:** What if course is full? Student already enrolled?
8. **Memory leaks:** When removing students, also remove their grades

---

## 5. DEBUG CHALLENGES

**Instructions:** Each code snippet has 1-3 bugs. Find them, explain why they're wrong, and provide the fix.

---

### 🐛 Challenge 1: The Missing Super

```javascript
class Vehicle {
  constructor(brand, year) {
    this.brand = brand;
    this.year = year;
  }
  
  getAge() {
    return 2024 - this.year;
  }
}

class Car extends Vehicle {
  constructor(brand, year, doors) {
    this.doors = doors;
    super(brand, year);
  }
}

const myCar = new Car("Toyota", 2020, 4);
console.log(myCar.brand);
```

**🔍 What's the bug?**

<details>
<summary>Click to see the answer</summary>

**🐛 Bug:** `super()` is called AFTER using `this.doors`

**Why it's wrong:** In derived classes, you must call `super()` before accessing `this`. The parent constructor needs to run first to properly initialize the object.

**✅ Fixed Code:**
```javascript
class Car extends Vehicle {
  constructor(brand, year, doors) {
    super(brand, year);  // ✅ Call super() FIRST
    this.doors = doors;
  }
}
```

**Rule:** Always call `super()` as the **first statement** in a child class constructor.

</details>

---

### 🐛 Challenge 2: Private Field Access

```javascript
class User {
  #password;
  
  constructor(username, password) {
    this.username = username;
    this.#password = password;
  }
  
  checkPassword(inputPassword) {
    return inputPassword === this.password;
  }
}

const user = new User("john", "secret123");
console.log(user.checkPassword("secret123"));
```

**🔍 What's the bug?**

<details>
<summary>Click to see the answer</summary>

**🐛 Bug:** Inside `checkPassword()`, it accesses `this.password` instead of `this.#password`

**Why it's wrong:** Private fields MUST be accessed with the `#` prefix everywhere, including inside the class. `this.password` is `undefined` because there's no public `password` property.

**✅ Fixed Code:**
```javascript
checkPassword(inputPassword) {
  return inputPassword === this.#password;  // ✅ Use #password
}
```

**Output:** `true`

**Common mistake:** Forgetting that `#` is part of the field name, not just a declaration syntax.

</details>

---

### 🐛 Challenge 3: Static Method Confusion

```javascript
class MathHelper {
  constructor(value) {
    this.value = value;
  }
  
  static double(num) {
    return num * 2;
  }
  
  triple() {
    return this.value * 3;
  }
}

const helper = new MathHelper(5);
console.log(helper.double(10));
console.log(MathHelper.triple());
```

**🔍 What are the bugs?**

<details>
<summary>Click to see the answer</summary>

**🐛 Bug 1:** Trying to call static method `double()` on an instance
**🐛 Bug 2:** Trying to call instance method `triple()` on the class

**Why it's wrong:**
- Static methods belong to the **class** and are called with `ClassName.method()`
- Instance methods belong to **objects** and are called with `objectName.method()`

**✅ Fixed Code:**
```javascript
const helper = new MathHelper(5);
console.log(MathHelper.double(10));  // ✅ Static method on class
console.log(helper.triple());         // ✅ Instance method on object
```

**Output:**
```
20
15
```

**Memory trick:**
- **Static** = **S**hared across all, called on **C**lass
- **Instance** = **I**ndividual, called on **O**bjects

</details>

---

### 🐛 Challenge 4: Arrow Function Context

```javascript
class Counter {
  constructor() {
    this.count = 0;
  }
  
  increment = () => {
    this.count++;
  }
  
  reset() {
    this.count = 0;
  }
  
  getCount = () => {
    return this.count;
  }
}

class ExtendedCounter extends Counter {
  increment() {
    super.increment();
    console.log(`Count is now ${this.count}`);
  }
}

const counter = new ExtendedCounter();
counter.increment();
```

**🔍 What's the bug?**

<details>
<summary>Click to see the answer</summary>

**🐛 Bug:** Arrow functions cannot be overridden because they're instance properties, not prototype methods

**Why it's wrong:**
- Arrow functions in class fields are assigned to each instance
- `super.increment()` tries to call the parent's prototype method, but `increment` is an instance property
- Arrow functions don't exist on the prototype chain, so inheritance doesn't work

**✅ Fixed Code:**
```javascript
class Counter {
  constructor() {
    this.count = 0;
  }
  
  increment() {  // ✅ Regular method (on prototype)
    this.count++;
  }
  
  reset() {
    this.count = 0;
  }
  
  getCount() {  // ✅ Regular method
    return this.count;
  }
}

class ExtendedCounter extends Counter {
  increment() {
    super.increment();  // ✅ Now this works
    console.log(`Count is now ${this.count}`);
  }
}

const counter = new ExtendedCounter();
counter.increment();  // Output: Count is now 1
```

**Rule:** Use regular methods for inheritance, arrow functions for callbacks that need to preserve `this`.

</details>

---

### 🐛 Challenge 5: Encapsulation Violation

```javascript
class BankAccount {
  constructor(balance) {
    this.balance = balance;
    this.transactions = [];
  }
  
  deposit(amount) {
    this.balance += amount;
    this.transactions.push({ type: 'deposit', amount });
  }
  
  getTransactions() {
    return this.transactions;
  }
}

const account = new BankAccount(1000);
account.deposit(500);

const transactions = account.getTransactions();
transactions.push({ type: 'withdraw', amount: 999999 });

console.log(account.getTransactions().length);
```

**🔍 What's the bug?**

<details>
<summary>Click to see the answer</summary>

**🐛 Bug:** Returning the actual `transactions` array allows external code to modify internal state

**Why it's wrong:**
- Arrays and objects are passed by reference
- Returning `this.transactions` gives direct access to internal data
- External code can manipulate the array without validation
- Violates encapsulation principle

**✅ Fixed Code:**
```javascript
class BankAccount {
  #balance;
  #transactions;
  
  constructor(balance) {
    this.#balance = balance;
    this.#transactions = [];
  }
  
  deposit(amount) {
    if (amount <= 0) return false;
    this.#balance += amount;
    this.#transactions.push({ 
      type: 'deposit', 
      amount,
      timestamp: new Date()
    });
    return true;
  }
  
  getTransactions() {
    // ✅ Return a COPY, not the original
    return this.#transactions.map(t => ({ ...t }));
    // Or: return [...this.#transactions];
  }
  
  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount(1000);
account.deposit(500);

const transactions = account.getTransactions();
transactions.push({ type: 'withdraw', amount: 999999 });  // ✅ Doesn't affect internal state

console.log(account.getTransactions().length);  // 1 (not 2)
```

**Best Practices:**
1. Make fields private with `#`
2. Return copies of arrays/objects, not originals
3. Validate all inputs in public methods
4. Never expose internal state directly

</details>

---

### 🐛 Challenge 6: Constructor Function vs Class

```javascript
function Animal(name) {
  this.name = name;
  
  this.speak = function() {
    return `${this.name} makes a sound`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }
  
  speak() {
    return `${this.name} barks`;
  }
}

const dog = new Dog("Max", "Labrador");
console.log(dog.speak());
```

**🔍 What's the bug?**

<details>
<summary>Click to see the answer</summary>

**🐛 Bug:** Cannot use `extends` with constructor functions (ES5 style)

**Why it's wrong:**
- `extends` is ES6 class syntax
- Constructor functions use a different inheritance pattern (prototype chain)
- Mixing the two syntaxes causes errors

**✅ Fixed Code - Option 1 (Convert to classes):**
```javascript
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
    return `${this.name} barks`;
  }
}

const dog = new Dog("Max", "Labrador");
console.log(dog.speak());  // Max barks
```

**✅ Fixed Code - Option 2 (Use constructor functions):**
```javascript
function Animal(name) {
  this.name = name;
}

Animal.prototype.speak = function() {
  return `${this.name} makes a sound`;
};

function Dog(name, breed) {
  Animal.call(this, name);  // Call parent constructor
  this.breed = breed;
}

// Set up inheritance
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.speak = function() {
  return `${this.name} barks`;
};

const dog = new Dog("Max", "Labrador");
console.log(dog.speak());  // Max barks
```

**Recommendation:** Use ES6 classes for modern code. Constructor functions are legacy.

</details>

---

### 🐛 Challenge 7: Method Chaining Broken

```javascript
class Calculator {
  constructor() {
    this.result = 0;
  }
  
  add(num) {
    this.result += num;
  }
  
  multiply(num) {
    this.result *= num;
    return this;
  }
  
  subtract(num) {
    this.result -= num;
    return this;
  }
}

const calc = new Calculator();
const final = calc.add(10).multiply(2).subtract(5).result;
console.log(final);
```

**🔍 What's the bug?**

<details>
<summary>Click to see the answer</summary>

**🐛 Bug:** The `add()` method doesn't return `this`, breaking the chain

**Why it's wrong:**
- Method chaining requires every method to return `this`
- `calc.add(10)` returns `undefined`
- Trying to call `.multiply()` on `undefined` causes: `TypeError: Cannot read property 'multiply' of undefined`

**✅ Fixed Code:**
```javascript
class Calculator {
  constructor() {
    this.result = 0;
  }
  
  add(num) {
    this.result += num;
    return this;  // ✅ Return this for chaining
  }
  
  multiply(num) {
    this.result *= num;
    return this;
  }
  
  subtract(num) {
    this.result -= num;
    return this;
  }
  
  getValue() {
    return this.result;
  }
}

const calc = new Calculator();
const final = calc.add(10).multiply(2).subtract(5).getValue();
console.log(final);  // 15
```

**Calculation:** 0 + 10 = 10 → 10 × 2 = 20 → 20 - 5 = 15

**Pattern:** This is called **Fluent Interface** - makes code more readable.

</details>

---

### 🐛 Challenge 8: Composition Mistake

```javascript
class Engine {
  constructor(horsepower) {
    this.horsepower = horsepower;
  }
  
  start() {
    return `Engine with ${this.horsepower} HP started`;
  }
}

class Car {
  constructor(brand, engineHP) {
    this.brand = brand;
    this.engine = engineHP;
  }
  
  startCar() {
    return this.engine.start();
  }
}

const myCar = new Car("Toyota", 200);
console.log(myCar.startCar());
```

**🔍 What's the bug?**

<details>
<summary>Click to see the answer</summary>

**🐛 Bug:** Storing primitive value (200) instead of Engine object

**Why it's wrong:**
- Composition means "has-a" relationship with objects
- `this.engine` should be an `Engine` instance, not a number
- Trying to call `.start()` on a number causes: `TypeError: this.engine.start is not a function`

**✅ Fixed Code:**
```javascript
class Engine {
  constructor(horsepower) {
    this.horsepower = horsepower;
  }
  
  start() {
    return `Engine with ${this.horsepower} HP started`;
  }
}

class Car {
  constructor(brand, engineHP) {
    this.brand = brand;
    this.engine = new Engine(engineHP);  // ✅ Create Engine object
  }
  
  startCar() {
    return this.engine.start();
  }
  
  getCarInfo() {
    return `${this.brand} with ${this.engine.horsepower} HP`;
  }
}

const myCar = new Car("Toyota", 200);
console.log(myCar.startCar());  // Engine with 200 HP started
console.log(myCar.getCarInfo()); // Toyota with 200 HP
```

**Alternative approach (Dependency Injection):**
```javascript
class Car {
  constructor(brand, engine) {
    this.brand = brand;
    this.engine = engine;  // Pass in pre-created engine
  }
  
  startCar() {
    return this.engine.start();
  }
}

const v8Engine = new Engine(400);
const sportsCar = new Car("Ferrari", v8Engine);
```

**Composition Checklist:**
- ✅ Store object instances, not primitives
- ✅ Use `new` to create component objects
- ✅ Access component methods through the stored object

</details>

---

## 6. COMMON PITFALLS & ANTI-PATTERNS

### ❌ Anti-Pattern 1: Creating Methods Inside Constructor

```javascript
// ❌ BAD - Creates new function for each instance
class User {
  constructor(name) {
    this.name = name;
    this.greet = function() {  // ❌ New function per object
      return `Hi, I'm ${this.name}`;
    };
  }
}
```

```javascript
// ✅ GOOD - Shared method on prototype
class User {
  constructor(name) {
    this.name = name;
  }
  
  greet() {  // ✅ One function shared by all instances
    return `Hi, I'm ${this.name}`;
  }
}
```

**Why it matters:** Creating 1000 users? Bad approach = 1000 functions. Good approach = 1 function.

---

### ❌ Anti-Pattern 2: Returning Objects from Constructor

```javascript
// ❌ BAD - Breaks instanceof and inheritance
function User(name) {
  this.name = name;
  return { name: name };  // ❌ Don't return objects
}
```

```javascript
// ✅ GOOD - Constructor automatically returns this
function User(name) {
  this.name = name;
  // No return statement needed
}
```

---

### ❌ Anti-Pattern 3: Deep Inheritance Chains

```javascript
// ❌ BAD - Too many levels
class LivingThing {}
class Animal extends LivingThing {}
class Mammal extends Animal {}
class Carnivore extends Mammal {}
class Feline extends Carnivore {}
class Cat extends Feline {}  // 6 levels deep!
```

```javascript
// ✅ GOOD - Prefer composition
class Animal {
  constructor() {
    this.movement = new Movement();  // Composition
    this.diet = new Diet();
  }
}
```

**Rule of thumb:** Max 2-3 levels of inheritance. Beyond that, use composition.

---

### ❌ Anti-Pattern 4: Modifying Prototypes of Built-in Objects

```javascript
// ❌ BAD - Never modify built-in prototypes
Array.prototype.last = function() {
  return this[this.length - 1];
};
```

```javascript
// ✅ GOOD - Create utility functions
function getLastElement(arr) {
  return arr[arr.length - 1];
}
```

**Why:** Breaks other libraries, causes naming conflicts, unpredictable behavior.

---

### ❌ Anti-Pattern 5: Not Validating Constructor Arguments

```javascript
// ❌ BAD - No validation
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
}

const rect = new Rectangle(-5, "hello");  // Invalid data stored
```

```javascript
// ✅ GOOD - Validate inputs
class Rectangle {
  constructor(width, height) {
    if (typeof width !== 'number' || width <= 0) {
      throw new Error('Width must be a positive number');
    }
    if (typeof height !== 'number' || height <= 0) {
      throw new Error('Height must be a positive number');
    }
    this.width = width;
    this.height = height;
  }
}
```

---

### ❌ Anti-Pattern 6: Exposing Internal State

```javascript
// ❌ BAD - Direct property access
class BankAccount {
  constructor() {
    this.balance = 1000;  // ❌ Anyone can modify
  }
}

const account = new BankAccount();
account.balance = 9999999;  // No validation!
```

```javascript
// ✅ GOOD - Private fields with controlled access
class BankAccount {
  #balance;
  
  constructor(initial) {
    this.#balance = initial;
  }
  
  deposit(amount) {
    if (amount > 0) {
      this.#balance += amount;
      return true;
    }
    return false;
  }
  
  getBalance() {
    return this.#balance;
  }
}
```

---

## 7. KNOWLEDGE CHECK MCQs

**Question 1:**
What will be the output?
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

**Question 2:**
Which statement is TRUE about private fields?

A) Private fields can be accessed from child classes  
B) Private fields use the `_` prefix convention  
C) Private fields use the `#` prefix and are truly private ✅  
D) Private fields are just a naming convention

**Explanation:** `#` creates truly private fields (not just convention). They cannot be accessed outside the class, even by child classes.

---

**Question 3:**
What's wrong with this code?
```javascript
class Dog extends Animal {
  constructor(name) {
    this.name = name;
    super();
  }
}
```

A) Nothing, it's correct  
B) `super()` must be called before using `this` ✅  
C) `super()` is not needed  
D) Should use `super.constructor()`

**Explanation:** In derived classes, `super()` MUST be called before accessing `this`.

---

**Question 4:**
Which creates a "has-a" relationship?

A) `class Car extends Vehicle`  
B) `class Car { engine = new Engine() }` ✅  
C) `class Car implements Vehicle`  
D) `Car.prototype = Vehicle.prototype`

**Explanation:** Composition (has-a) means containing instances of other classes. Inheritance is "is-a".

---

**Question 5:**
What will this output?
```javascript
class Helper {
  static greet() {
    return "Hello";
  }
}

const h = new Helper();
console.log(h.greet());
```

A) `"Hello"`  
B) `undefined`  
C) `TypeError: h.greet is not a function` ✅  
D) `"Hello from instance"`

**Explanation:** Static methods belong to the class, not instances. Must call `Helper.greet()`.

---

**Question 6:**
What is polymorphism in OOP?

A) Hiding complex implementation details  
B) Bundling data and methods together  
C) Same method name, different behaviors in subclasses ✅  
D) Creating objects from classes

**Explanation:** Polymorphism means "many forms" - child classes can override parent methods to provide different implementations.

---

**Question 7:**
What's the correct way to call a parent method from a child class?

A) `parent.method()`  
B) `this.parent.method()`  
C) `super.method()` ✅  
D) `base.method()`

**Explanation:** Use `super.methodName()` to access parent class methods from child classes.

---

**Question 8:**
Which is NOT a principle of OOP?

A) Abstraction  
B) Encapsulation  
C) Compilation ✅  
D) Polymorphism

**Explanation:** The five OOP principles are: Abstraction, Encapsulation, Inheritance, Polymorphism, and Composition. Compilation is not an OOP principle.

---

**Question 9:**
What happens if you forget `new` when creating an instance?

```javascript
class User {
  constructor(name) {
    this.name = name;
  }
}

const user = User("John");
```

A) Works fine, creates an object  
B) `TypeError: Class constructor cannot be invoked without 'new'` ✅  
C) Returns `undefined`  
D) Creates a global variable

**Explanation:** ES6 classes MUST be called with `new`. Constructor functions (old style) would cause different issues.

---

**Question 10:**
What's the purpose of encapsulation?

A) To make code run faster  
B) To protect data and control access ✅  
C) To create multiple objects  
D) To inherit from parent classes

**Explanation:** Encapsulation bundles data with methods and restricts direct access, protecting internal state.

---

**Question 11:**
What will this output?

```javascript
class Animal {
  makeSound() {
    return "Some sound";
  }
}

class Dog extends Animal {
  makeSound() {
    return super.makeSound() + " - Woof!";
  }
}

const dog = new Dog();
console.log(dog.makeSound());
```

A) `"Woof!"`  
B) `"Some sound"`  
C) `"Some sound - Woof!"` ✅  
D) `Error`

**Explanation:** `super.makeSound()` calls the parent method, then we append " - Woof!".

---

**Question 12:**
Which is the best way to create multiple similar objects?

A) Object literal for each one  
B) Using a class with constructor ✅  
C) Copying and pasting code  
D) Using global variables

**Explanation:** Classes provide a blueprint for creating multiple objects with the same structure and behavior.

---

**Question 13:**
What's wrong with returning arrays/objects directly from getters?

```javascript
class Store {
  #items = [1, 2, 3];
  
  getItems() {
    return this.#items;
  }
}
```

A) Nothing, it's correct  
B) It exposes internal state and allows external modification ✅  
C) Arrays cannot be returned  
D) Should use `this.items` instead

**Explanation:** Returning the actual array allows external code to modify it. Return a copy: `return [...this.#items]`.

---

**Question 14:**
What's the difference between `class` and `constructor function`?

A) No difference, just syntax  
B) Classes must use `new`, constructor functions are optional  
C) Classes are syntactic sugar over constructor functions ✅  
D) Constructor functions are deprecated

**Explanation:** ES6 classes are syntactic sugar - they compile to constructor functions with prototypes under the hood, but with cleaner syntax and stricter rules.

---

**Question 15:**
When should you use composition over inheritance?

A) Always, inheritance is bad  
B) When you need a "has-a" relationship ✅  
C) When you need a "is-a" relationship  
D) Never, always use inheritance

**Explanation:** Use composition for "has-a" (Car has Engine), inheritance for "is-a" (Dog is Animal). Composition is more flexible.

---

## 8. REAL INTERVIEW QUESTIONS

### 🎤 Question 1: Explain the difference between Classical Inheritance and Prototypal Inheritance in JavaScript

**❌ Junior Answer:**
"JavaScript uses classes to create objects and they inherit from each other using `extends`."

**✅ Senior Answer:**
"JavaScript uses **prototypal inheritance**, not classical inheritance like Java or C++. Here's the key difference:

**Classical Inheritance (Java/C++):**
- Classes are blueprints, objects are instances
- Inheritance is class-to-class
- Fixed at compile time

**Prototypal Inheritance (JavaScript):**
- Objects inherit directly from other objects via the prototype chain
- Every object has an internal `[[Prototype]]` link
- ES6 `class` syntax is syntactic sugar over prototype-based inheritance

```javascript
// Under the hood, this class:
class Animal {
  speak() { return "sound"; }
}

// Becomes this:
function Animal() {}
Animal.prototype.speak = function() { return "sound"; };

// When you create an instance:
const dog = new Animal();
// dog.__proto__ === Animal.prototype (prototype chain)
```

JavaScript's approach is more flexible - objects can inherit from any object at runtime, not just at design time. The prototype chain allows property lookup: if `dog.speak()` isn't found on `dog`, JavaScript checks `dog.__proto__` (Animal.prototype), then `Animal.prototype.__proto__` (Object.prototype), and so on."

**Follow-up they might ask:** "How would you implement inheritance without using `class` or `extends`?"

```javascript
// Using Object.create
const animal = {
  speak() { return "sound"; }
};

const dog = Object.create(animal);
dog.bark = function() { return "woof"; };

console.log(dog.speak()); // Inherited from animal
console.log(dog.bark());  // Own method
```

---

### 🎤 Question 2: How do you implement private methods and properties in JavaScript?

**❌ Junior Answer:**
"Use an underscore prefix like `_privateMethod` to indicate it's private."

**✅ Senior Answer:**
"There are several approaches with different levels of privacy:

**1. ES2022 Private Fields (True Privacy):**
```javascript
class BankAccount {
  #balance = 0;  // Truly private
  #pin;
  
  constructor(initial, pin) {
    this.#balance = initial;
    this.#pin = pin;
  }
  
  #validatePin(inputPin) {  // Private method
    return inputPin === this.#pin;
  }
  
  withdraw(amount, pin) {
    if (!this.#validatePin(pin)) return false;
    if (amount <= this.#balance) {
      this.#balance -= amount;
      return true;
    }
    return false;
  }
}

const account = new BankAccount(1000, "1234");
// account.#balance  // SyntaxError - truly private
```

**2. Closures (Pre-ES2022):**
```javascript
function createBankAccount(initial) {
  let balance = initial;  // Private via closure
  
  return {
    deposit(amount) {
      balance += amount;
    },
    getBalance() {
      return balance;
    }
  };
}
```

**3. WeakMaps (Advanced):**
```javascript
const privateData = new WeakMap();

class BankAccount {
  constructor(initial) {
    privateData.set(this, { balance: initial });
  }
  
  getBalance() {
    return privateData.get(this).balance;
  }
}
```

**4. Convention (No Real Privacy):**
```javascript
class User {
  _password = "secret";  // Just a convention, still accessible
}
```

I prefer **#private fields** for modern JavaScript - they provide true encapsulation, are performant, and have native support. Closures are great for module patterns, but classes with # fields are the standard for OOP in ES2022+."

---

### 🎤 Question 3: Explain method overriding and `super` keyword with a real-world example

**❌ Junior Answer:**
"Method overriding is when you change a method in a child class. `super` calls the parent method."

**✅ Senior Answer:**
"Method overriding allows child classes to provide specialized implementations of parent methods while maintaining polymorphism. Let me demonstrate with a real-world payment processing example:

```javascript
class PaymentProcessor {
  constructor(amount) {
    this.amount = amount;
    this.transactionFee = 0;
  }
  
  calculateTotal() {
    return this.amount + this.transactionFee;
  }
  
  process() {
    console.log(`Processing $${this.calculateTotal()}`);
    return this.performTransaction();
  }
  
  performTransaction() {
    return { success: true, amount: this.amount };
  }
}

class CreditCardProcessor extends PaymentProcessor {
  constructor(amount, cardType) {
    super(amount);
    this.cardType = cardType;
    this.transactionFee = amount * 0.029; // 2.9% fee
  }
  
  // Override: Add credit card specific logic
  performTransaction() {
    const result = super.performTransaction(); // Call parent
    return {
      ...result,
      cardType: this.cardType,
      fee: this.transactionFee,
      message: "Credit card charged successfully"
    };
  }
}

class PayPalProcessor extends PaymentProcessor {
  constructor(amount, email) {
    super(amount);
    this.email = email;
    this.transactionFee = amount * 0.034 + 0.30; // 3.4% + $0.30
  }
  
  // Override with completely different implementation
  performTransaction() {
    return {
      success: true,
      amount: this.amount,
      processor: "PayPal",
      email: this.email,
      fee: this.transactionFee,
      message: "PayPal payment completed"
    };
  }
}

// Polymorphism in action
const processors = [
  new CreditCardProcessor(100, "Visa"),
  new PayPalProcessor(100, "user@example.com")
];

processors.forEach(p => {
  console.log(p.process());
});
```

**Key points about `super`:**

1. **In constructors:** `super(args)` must be called before using `this`
2. **In methods:** `super.methodName()` calls the parent's version
3. **Property access:** `super.property` accesses parent properties
4. **Cannot skip levels:** `super` only accesses immediate parent, not grandparent

**When to override vs extend:**
- **Override completely:** When child needs entirely different logic (PayPal example)
- **Extend with super:** When you want parent logic + additions (CreditCard example)
- **Don't override:** When parent implementation is sufficient

This pattern enables the **Open/Closed Principle** - classes are open for extension but closed for modification."

---

### 🎤 Question 4: When would you use Composition over Inheritance? Provide a code example.

**❌ Junior Answer:**
"Use composition when you want to combine things together instead of inheriting."

**✅ Senior Answer:**
"Composition is preferred when you need flexible 'has-a' relationships rather than rigid 'is-a' hierarchies. The principle is **'Favor composition over inheritance'** because:

1. **Inheritance issues:**
   - Creates tight coupling
   - Fragile base class problem (changing parent breaks children)
   - Deep hierarchies become unmaintainable
   - Forces single inheritance (JavaScript limitation)

2. **Composition benefits:**
   - Loose coupling
   - Easy to change behaviors at runtime
   - Better code reuse
   - More testable

**Real-world example - Game Characters:**

```javascript
// ❌ BAD: Inheritance approach
class Character {}
class Warrior extends Character {}
class Mage extends Character {}
class WarriorMage extends ??? // Can't extend both!

// ✅ GOOD: Composition approach
class Character {
  constructor(name) {
    this.name = name;
    this.abilities = [];
  }
  
  addAbility(ability) {
    this.abilities.push(ability);
    return this; // Method chaining
  }
  
  useAbility(abilityName) {
    const ability = this.abilities.find(a => a.name === abilityName);
    return ability ? ability.execute(this) : "Ability not found";
  }
}

// Separate ability classes
class MeleeAttack {
  constructor() {
    this.name = "Melee Attack";
    this.damage = 50;
  }
  
  execute(character) {
    return `${character.name} swings sword for ${this.damage} damage!`;
  }
}

class Fireball {
  constructor() {
    this.name = "Fireball";
    this.damage = 80;
    this.manaCost = 30;
  }
  
  execute(character) {
    return `${character.name} casts Fireball for ${this.damage} damage!`;
  }
}

class Heal {
  constructor() {
    this.name = "Heal";
    this.healAmount = 40;
  }
  
  execute(character) {
    return `${character.name} heals for ${this.healAmount} HP!`;
  }
}

// Create flexible characters by composing abilities
const warrior = new Character("Conan")
  .addAbility(new MeleeAttack());

const mage = new Character("Gandalf")
  .addAbility(new Fireball())
  .addAbility(new Heal());

const battleMage = new Character("Elara")
  .addAbility(new MeleeAttack())
  .addAbility(new Fireball())
  .addAbility(new Heal());

console.log(warrior.useAbility("Melee Attack"));
console.log(battleMage.useAbility("Fireball"));
console.log(battleMage.useAbility("Melee Attack"));
```

**Another example - Car customization:**

```javascript
// Components
class Engine {
  constructor(type, hp) {
    this.type = type;
    this.horsepower = hp;
  }
  start() { return `${this.type} engine started (${this.horsepower} HP)`; }
}

class Transmission {
  constructor(type) {
    this.type = type; // "Automatic" or "Manual"
  }
  shift() { return `${this.type} transmission shifting`; }
}

class GPS {
  navigate(destination) {
    return `Navigating to ${destination}`;
  }
}

// Car uses composition
class Car {
  constructor(brand, model) {
    this.brand = brand;
    this.model = model;
    this.components = {};
  }
  
  addComponent(name, component) {
    this.components[name] = component;
    return this;
  }
  
  start() {
    return this.components.engine?.start() || "No engine installed";
  }
  
  navigate(destination) {
    return this.components.gps?.navigate(destination) || "No GPS installed";
  }
}

// Flexible configuration
const basicCar = new Car("Toyota", "Corolla")
  .addComponent("engine", new Engine("4-cylinder", 150))
  .addComponent("transmission", new Transmission("Automatic"));

const luxuryCar = new Car("BMW", "X5")
  .addComponent("engine", new Engine("V8", 400))
  .addComponent("transmission", new Transmission("Automatic"))
  .addComponent("gps", new GPS());

console.log(luxuryCar.start());
console.log(luxuryCar.navigate("New York"));
```

**Decision tree:**
- Use **Inheritance** when: True 'is-a' relationship (Dog IS-A Animal)
- Use **Composition** when: 'has-a' or 'uses-a' (Car HAS-A Engine)

In practice, I use composition for 80% of cases - it's more maintainable and flexible."

---

## 9. DEVTOOLS & DEBUGGING

### 🔍 Debugging OOP in Chrome DevTools

#### 1. Inspecting Object Structure

**Console Commands:**
```javascript
class User {
  #password;
  constructor(name, password) {
    this.name = name;
    this.#password = password;
  }
}

const user = new User("John", "secret123");

// In Console:
console.dir(user);  // Shows object structure with prototype chain
console.log(user);  // Shows properties only

// Check prototype
console.log(user.__proto__);
console.log(Object.getPrototypeOf(user));

// Check if property exists
console.log(user.hasOwnProperty('name'));  // true
console.log('name' in user);  // true
```

**What you'll see:**
```
User {name: "John", #password: "secret123"}
  name: "John"
  #password: "secret123"
  [[Prototype]]: Object
    constructor: class User
    [[Prototype]]: Object
```

---

#### 2. Debugging Private Fields

Private fields (`#field`) won't show up in regular console.log:

```javascript
class Account {
  #balance = 1000;
  
  getBalance() {
    debugger;  // Set breakpoint here
    return this.#balance;
  }
}

const acc = new Account();
acc.getBalance();
```

**In DevTools:**
1. Open **Sources** tab
2. When `debugger` hits, look at **Scope** panel
3. Expand `this` → you'll see private fields listed as `[[PrivateProperties]]`

---

#### 3. Tracing Inheritance Chain

```javascript
class Animal {
  speak() { return "sound"; }
}

class Dog extends Animal {
  bark() { return "woof"; }
}

const dog = new Dog();

// Console commands:
console.log(dog instanceof Dog);     // true
console.log(dog instanceof Animal);  // true
console.log(dog instanceof Object);  // true

// Manual prototype chain traversal
let current = dog;
while (current) {
  console.log(current.constructor.name);
  current = Object.getPrototypeOf(current);
}
// Output: Dog → Animal → Object → null
```

---

#### 4. Setting Breakpoints on Method Calls

**In Sources Tab:**
1. Find your class method
2. Click line number to set breakpoint
3. When method executes, inspect:
   - **this** context (what object called it)
   - **super** calls (check Call Stack)
   - Local variables vs instance properties

**Call Stack Example:**
```
bark (Dog.js:10)
  └─ speak (Animal.js:5)
      └─ (anonymous) (main.js:20)
```

---

#### 5. Monitoring Object Changes

```javascript
class Counter {
  constructor() {
    this.count = 0;
  }
  increment() {
    this.count++;
  }
}

const counter = new Counter();

// Watch for property changes (not in private fields)
Object.defineProperty(counter, 'count', {
  get() { return this._count; },
  set(value) {
    console.trace('count changed to:', value);
    this._count = value;
  }
});
```

---

#### 6. Common Debugging Scenarios

**Scenario 1: "Why is `this` undefined?"**
```javascript
class Button {
  constructor() {
    this.clicks = 0;
  }
  
  handleClick() {
    console.log(this);  // Set breakpoint here
    this.clicks++;
  }
}

const btn = new Button();
document.querySelector('#btn').addEventListener('click', btn.handleClick);
// Problem: 'this' is the button element, not Button instance
```

**Solution in DevTools:**
1. Set breakpoint in `handleClick`
2. Check **this** in Scope panel
3. See it's the DOM element, not Button instance
4. Fix: Use arrow function or `.bind()`

---

**Scenario 2: "Method not found error"**
```javascript
class Parent {
  greet() { return "hi"; }
}

class Child extends Parent {}

const child = new Child();
child.greet();  // Works

// But if you do:
const greet = child.greet;
greet();  // TypeError
```

**Debug in Console:**
```javascript
console.log(child.greet);           // [Function: greet]
console.log(typeof child.greet);    // "function"
console.log(child.hasOwnProperty('greet'));  // false (inherited)

const greet = child.greet;
console.log(greet.toString());  // See the function code
```

---

#### 7. DevTools Tips for OOP

**📌 Useful Console Methods:**
```javascript
// Check constructor
console.log(obj.constructor.name);

// List all properties (including inherited)
console.log(Object.getOwnPropertyNames(obj));

// List only own properties
console.log(Object.keys(obj));

// Check if method exists
console.log(typeof obj.method === 'function');

// Deep clone for comparison
const clone = JSON.parse(JSON.stringify(obj));
```

**📌 Sources Panel:**
- **Blackbox Script**: Right-click → Blackbox to skip library code
- **Watch Expressions**: Add `this.propertyName` to monitor values
- **Call Stack**: Trace method calls from child to parent

**📌 Memory Profiling:**
```javascript
// Check for memory leaks in class instances
class BigObject {
  constructor() {
    this.data = new Array(1000000);
  }
}

// Take heap snapshot before and after creating objects
const objects = [];
for (let i = 0; i < 100; i++) {
  objects.push(new BigObject());
}
```

---

## 10. CHAPTER SUMMARY & NEXT STEPS

### 🎯 Quick Recap

**You've learned:**

✅ **Objects & Classes:**
- 6 ways to create objects (literal, class, factory, constructor, Object.create, new Object)
- ES6 class syntax with constructors and methods
- Static vs instance methods

✅ **The Five OOP Principles:**
- **Abstraction** - Hide complexity, expose simple interfaces
- **Encapsulation** - Bundle data + methods, protect with private fields (`#`)
- **Inheritance** - Share code between classes using `extends` and `super`
- **Polymorphism** - Same method, different behaviors in child classes
- **Composition** - Build complex objects from simpler ones (has-a relationship)

✅ **Advanced Concepts:**
- Method chaining (return `this`)
- Method overriding vs extending
- Private fields and methods
- When to use inheritance vs composition

✅ **Debugging Skills:**
- Inspecting prototype chains
- Finding private fields in DevTools
- Tracing method calls
- Common `this` context issues

---

### 🚨 Key Takeaways (Print This!)

```
┌─────────────────────────────────────────────────────┐
│  GOLDEN RULES OF OOP                                │
├─────────────────────────────────────────────────────┤
│  1. Always call super() BEFORE using this           │
│  2. Use # for private fields (truly private)        │
│  3. Return this for method chaining                 │
│  4. Prefer composition over deep inheritance        │
│  5. Validate constructor inputs                     │
│  6. Return copies of arrays/objects, not originals  │
│  7. Static methods: Class.method() not obj.method() │
│  8. Don't modify built-in prototypes                │
└─────────────────────────────────────────────────────┘
```

---

### 🔗 Connection to Next Topic

**Today** you learned the **fundamentals of OOP** - the concepts and principles.

**Tomorrow (Day 30)** you'll become an **OOP Hero** by:
- Mastering advanced ES6 class features (getters, setters, computed properties)
- Building complex real-world systems (Todo App, Shopping Cart, Game Engine)
- Learning design patterns (Singleton, Factory, Observer)
- Understanding when NOT to use OOP
- Performance optimization techniques
- Preparing for senior-level OOP interviews

**The Bridge:** Think of today as learning the alphabet. Tomorrow, you'll write novels.

---

### ✅ Self-Assessment Checklist

Before moving to Day 30, you should be able to:

- [ ] Create a class with constructor, methods, and private fields
- [ ] Implement inheritance with `extends` and call `super()` correctly
- [ ] Explain all 5 OOP principles with examples
- [ ] Choose between inheritance and composition
- [ ] Debug OOP code using Chrome DevTools
- [ ] Identify and fix common OOP mistakes
- [ ] Build a multi-class system (like the E-Learning platform)

**If you checked all boxes:** You're ready for Day 30! 🚀

**If you missed some:** Review the relevant sections and practice the guided problems again.

---

### 📚 Additional Practice (Optional)

**Mini-Projects to solidify your learning:**

1. **Library Management System** (2-3 hours)
   - Book, Member, Librarian classes
   - Borrowing/returning functionality
   - Due dates and late fees

2. **Restaurant Order System** (2-3 hours)
   - Menu, Order, Customer, Chef classes
   - Order processing pipeline
   - Bill calculation with taxes/tips

3. **Social Media Platform** (3-4 hours)
   - User, Post, Comment, Like classes
   - Follow/unfollow functionality
   - Timeline/feed generation

---

### 🎉 Ready for Day 30?

**Tomorrow's Preview:**
```javascript
class UltraClass {
  // Getters & Setters
  get fullName() { return `${this.first} ${this.last}`; }
  set fullName(name) { /*...*/ }
  
  // Static factories
  static createFromJSON(json) { /*...*/ }
  
  // Private computed properties
  #calculateScore() { /*...*/ }
  
  // And much more! 🔥
}
```

---

<div align="center">

## 🎊 Congratulations on Completing Day 29! 🎊

**You've unlocked:**
- 🏆 OOP Fundamentals Master
- 🔐 Encapsulation Expert
- 🧬 Inheritance & Composition Pro
- 🐛 Debugging Detective


**Next:** Day 30 - From Zero to OOP Hero with JavaScript ES6 Classes

---

*"Good code is not just about solving problems—it's about solving them elegantly, maintainably, and scalably. OOP gives you the tools. Now go build something amazing!"* 🚀

</div>