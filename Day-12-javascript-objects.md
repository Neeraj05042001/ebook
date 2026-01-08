# Day 12: Mastering JavaScript Objects 🏆



## Section 3: PREDICT THE OUTPUT 🔮

Test your understanding of JavaScript objects with these prediction challenges. Read each code snippet carefully, predict the output, and then check your answer.

---

### Challenge 1: Basic Object Access (Easy)

```javascript
const laptop = {
  brand: "Dell",
  price: 50000,
  "warranty-years": 2
};

console.log(laptop.brand);
console.log(laptop["warranty-years"]);
console.log(laptop.warranty-years);
```

**🤔 Think About It:**
- Which property access methods work?
- What happens with the hyphenated property name?

<details>
<summary>💡 Hint</summary>

Property names with special characters (hyphens, spaces) require bracket notation.
</details>

<details>
<summary>✅ Answer & Explanation</summary>

**Output:**
```
Dell
2
NaN (or error in strict mode)
```

**Explanation:**
1. `laptop.brand` → `"Dell"` - Standard dot notation works fine
2. `laptop["warranty-years"]` → `2` - Bracket notation correctly accesses the hyphenated property
3. `laptop.warranty-years` → Attempts to subtract `years` from `laptop.warranty` (both undefined), resulting in `NaN`

**Key Concept:** Always use bracket notation for property names containing special characters or spaces. Dot notation interprets hyphens as subtraction operators.

</details>

---

### Challenge 2: Object References (Medium)

```javascript
const user1 = { name: "Alice", age: 25 };
const user2 = { name: "Alice", age: 25 };
const user3 = user1;

console.log(user1 === user2);
console.log(user1 === user3);

user3.age = 30;
console.log(user1.age);
```

**🤔 Think About It:**
- Are objects compared by value or reference?
- What happens when we modify user3?

<details>
<summary>💡 Hint</summary>

Objects are reference types. Variables store references (memory addresses), not the actual object values.
</details>

<details>
<summary>✅ Answer & Explanation</summary>

**Output:**
```
false
true
30
```

**Explanation:**
1. `user1 === user2` → `false` - Even though both objects have identical properties, they're stored in different memory locations
2. `user1 === user3` → `true` - `user3` is assigned the reference to `user1`, so they point to the same object in memory
3. `user1.age` → `30` - Since `user3` and `user1` reference the same object, modifying through one reference affects the other

**Key Concept:** Objects are compared by reference, not by their content. Two objects with identical properties are not equal unless they reference the same memory location.

</details>

---

### Challenge 3: Dynamic Property Access (Medium)

```javascript
const product = {
  id: 101,
  name: "Laptop",
  price: 50000
};

const key1 = "name";
const key2 = "stock";

console.log(product[key1]);
console.log(product.key1);
console.log(product[key2]);
console.log(product[key2] || "Out of stock");
```

**🤔 Think About It:**
- How does bracket notation with variables work?
- What's the difference between `product[key1]` and `product.key1`?

<details>
<summary>💡 Hint</summary>

Bracket notation evaluates variables, while dot notation treats everything after the dot as a literal property name.
</details>

<details>
<summary>✅ Answer & Explanation</summary>

**Output:**
```
Laptop
undefined
undefined
Out of stock
```

**Explanation:**
1. `product[key1]` → `"Laptop"` - Evaluates `key1` variable, accesses `product["name"]`
2. `product.key1` → `undefined` - Looks for a property literally named "key1" (doesn't exist)
3. `product[key2]` → `undefined` - Evaluates `key2` ("stock"), which doesn't exist in the object
4. `product[key2] || "Out of stock"` → `"Out of stock"` - Uses logical OR for fallback value

**Key Concept:** Bracket notation with variables enables dynamic property access, essential for iterating over objects or accessing properties determined at runtime.

</details>

---

### Challenge 4: Shallow Copy Behavior (Hard)

```javascript
const original = {
  name: "John",
  scores: { math: 90, science: 85 }
};

const copy1 = { ...original };
const copy2 = Object.assign({}, original);

copy1.name = "Alice";
copy1.scores.math = 95;

console.log(original.name);
console.log(original.scores.math);
console.log(copy2.scores.math);
```

**🤔 Think About It:**
- What gets copied: values or references?
- How does shallow copying affect nested objects?

<details>
<summary>💡 Hint</summary>

Shallow copies duplicate the first level but share references to nested objects.
</details>

<details>
<summary>✅ Answer & Explanation</summary>

**Output:**
```
John
95
95
```

**Explanation:**
1. `original.name` → `"John"` - First-level primitive property was safely copied; modification to `copy1.name` doesn't affect original
2. `original.scores.math` → `95` - The nested `scores` object was copied by reference. All copies share the same nested object
3. `copy2.scores.math` → `95` - Both spread operator and `Object.assign()` create shallow copies, so `copy2` also shares the nested object reference

**Key Concept:** Shallow copying creates independent first-level properties but maintains shared references to nested objects. Use `structuredClone()` or JSON methods for deep copies when needed.

</details>

---

### Challenge 5: Object Destructuring with Defaults (Medium)

```javascript
const settings = {
  theme: "dark",
  fontSize: 16
};

const { theme, fontSize, language = "English", notifications } = settings;

console.log(theme);
console.log(language);
console.log(notifications);
```

**🤔 Think About It:**
- How do default values work in destructuring?
- What happens to properties that don't exist?

<details>
<summary>💡 Hint</summary>

Default values are used only when the property is undefined or doesn't exist.
</details>

<details>
<summary>✅ Answer & Explanation</summary>

**Output:**
```
dark
English
undefined
```

**Explanation:**
1. `theme` → `"dark"` - Property exists, extracts normally
2. `language` → `"English"` - Property doesn't exist in object, default value is used
3. `notifications` → `undefined` - Property doesn't exist and no default was provided

**Key Concept:** Destructuring defaults only apply when the property is undefined or missing. They provide a clean way to handle optional properties without additional conditional logic.

</details>

---

### Challenge 6: The `in` Operator vs hasOwn (Medium)

```javascript
const parent = { inherited: true };
const child = Object.create(parent);
child.own = "value";

console.log("own" in child);
console.log("inherited" in child);
console.log(Object.hasOwn(child, "own"));
console.log(Object.hasOwn(child, "inherited"));
```

**🤔 Think About It:**
- What's the difference between `in` and `hasOwn`?
- How does prototype inheritance affect these checks?

<details>
<summary>💡 Hint</summary>

The `in` operator checks the entire prototype chain, while `hasOwn` checks only direct properties.
</details>

<details>
<summary>✅ Answer & Explanation</summary>

**Output:**
```
true
true
true
false
```

**Explanation:**
1. `"own" in child` → `true` - `in` operator finds direct property
2. `"inherited" in child` → `true` - `in` operator checks prototype chain, finds property on parent
3. `Object.hasOwn(child, "own")` → `true` - Direct property of child
4. `Object.hasOwn(child, "inherited")` → `false` - Not a direct property, exists only on prototype

**Key Concept:** Use `in` when you want to check if a property exists anywhere in the prototype chain. Use `hasOwn` when you only care about direct properties of the object itself.

</details>

---

### Challenge 7: Object.freeze() Behavior (Hard)

```javascript
const config = Object.freeze({
  apiUrl: "https://api.example.com",
  settings: {
    timeout: 5000,
    retries: 3
  }
});

config.apiUrl = "https://newapi.com";
config.newProp = "test";
config.settings.timeout = 10000;

console.log(config.apiUrl);
console.log(config.newProp);
console.log(config.settings.timeout);
```

**🤔 Think About It:**
- Does `freeze()` apply to nested objects?
- What happens to modification attempts?

<details>
<summary>💡 Hint</summary>

`Object.freeze()` is shallow - it only freezes the first level of the object.
</details>

<details>
<summary>✅ Answer & Explanation</summary>

**Output:**
```
https://api.example.com
undefined
10000
```

**Explanation:**
1. `config.apiUrl` → `"https://api.example.com"` - Frozen property cannot be modified
2. `config.newProp` → `undefined` - Cannot add new properties to frozen object
3. `config.settings.timeout` → `10000` - Nested object is NOT frozen, can be modified freely

**Key Concept:** `Object.freeze()` performs a shallow freeze. To completely freeze an object with nested structures, you need to recursively freeze each level (deep freeze).

</details>

---

### Challenge 8: Optional Chaining with Methods (Medium)

```javascript
const user = {
  name: "Alice",
  getAddress: function() {
    return this.address?.street;
  }
};

console.log(user.address?.city);
console.log(user.getAddress?.());
console.log(user.sayHello?.());
```

**🤔 Think About It:**
- How does optional chaining work with nested properties?
- Can you use it with method calls?

<details>
<summary>💡 Hint</summary>

Optional chaining short-circuits evaluation and returns undefined if any part is null/undefined.
</details>

<details>
<summary>✅ Answer & Explanation</summary>

**Output:**
```
undefined
undefined
undefined
```

**Explanation:**
1. `user.address?.city` → `undefined` - `address` doesn't exist, short-circuits without error
2. `user.getAddress?.()` → `undefined` - Method exists and executes, but returns `undefined` because `this.address` doesn't exist
3. `user.sayHello?.()` → `undefined` - Method doesn't exist; `?.()` prevents TypeError

**Key Concept:** Optional chaining (`?.`) prevents "Cannot read property of undefined" errors. Use `?.()` for optional method calls and `?.[]` for optional bracket access.

</details>

---

### Challenge 9: Destructuring with Nested Aliases (Hard)

```javascript
const company = {
  name: "TechCorp",
  location: {
    city: "San Francisco",
    country: "USA"
  }
};

const { 
  name: companyName, 
  location: { city, country: nation } 
} = company;

console.log(companyName);
console.log(city);
console.log(nation);
console.log(location);
```

**🤔 Think About It:**
- What variables get created during destructuring?
- Does destructuring with aliases affect the original variable names?

<details>
<summary>💡 Hint</summary>

Aliases create new variable names; intermediate nested names aren't automatically created.
</details>

<details>
<summary>✅ Answer & Explanation</summary>

**Output:**
```
TechCorp
San Francisco
USA
ReferenceError: location is not defined
```

**Explanation:**
1. `companyName` → `"TechCorp"` - `name` is destructured with alias `companyName`
2. `city` → `"San Francisco"` - Nested property extracted without alias
3. `nation` → `"USA"` - `country` destructured with alias `nation`
4. `location` → ReferenceError - When you destructure nested objects, only the final properties become variables, not intermediate levels

**Key Concept:** Destructuring with nested objects only creates variables for the properties you explicitly extract. Intermediate object names (like `location` here) are not automatically created as variables.

</details>

---

### Challenge 10: Object.seal() vs Object.freeze() (Hard)

```javascript
const sealed = Object.seal({ name: "Alice", age: 25 });
const frozen = Object.freeze({ name: "Bob", age: 30 });

sealed.name = "Charlie";
sealed.city = "NYC";
delete sealed.age;

frozen.name = "Dave";
frozen.city = "LA";
delete frozen.age;

console.log(sealed.name);
console.log(sealed.city);
console.log(sealed.age);
console.log(frozen.name);
console.log(frozen.city);
console.log(frozen.age);
```

**🤔 Think About It:**
- What's the difference between seal and freeze?
- Which operations are allowed on each?

<details>
<summary>💡 Hint</summary>

Sealed objects allow modifications but not additions/deletions. Frozen objects allow nothing.
</details>

<details>
<summary>✅ Answer & Explanation</summary>

**Output:**
```
Charlie
undefined
25
Bob
undefined
30
```

**Explanation:**

**Sealed Object:**
1. `sealed.name` → `"Charlie"` - ✅ Can modify existing properties
2. `sealed.city` → `undefined` - ❌ Cannot add new properties
3. `sealed.age` → `25` - ❌ Cannot delete properties

**Frozen Object:**
4. `frozen.name` → `"Bob"` - ❌ Cannot modify existing properties
5. `frozen.city` → `undefined` - ❌ Cannot add new properties
6. `frozen.age` → `30` - ❌ Cannot delete properties

**Key Concept:** 
- **Seal**: Prevents additions and deletions, allows modifications
- **Freeze**: Prevents everything (additions, deletions, AND modifications)

</details>

---

## Section 4: GUIDED PRACTICE PROBLEMS 💪

Work through these hands-on problems to solidify your understanding of JavaScript objects. Each problem builds on concepts from the chapter.

---

### Problem 1: Create a Student Profile Manager (Easy)

**📝 Task Description:**

Create a student object and implement functions to manage the student's profile including grades calculation and information updates.

**✅ Requirements Checklist:**

- [ ] Create a student object with properties: name, age, rollNumber, grades (array)
- [ ] Add a method to calculate average grade
- [ ] Add a method to add new grades
- [ ] Add a method to display student information
- [ ] Handle edge cases (empty grades array)

**🧪 Test Cases:**

```javascript
// Test Case 1: Basic student creation
// Expected: Student object with all properties

// Test Case 2: Calculate average of [85, 90, 88]
// Expected: 87.67

// Test Case 3: Add new grade 92
// Expected: grades array now contains [85, 90, 88, 92]

// Test Case 4: Calculate average with empty array
// Expected: 0 or "No grades available"
```

**✅ Solution:**

```javascript
// Create student object with methods
const student = {
  name: "Rahul Sharma",
  age: 16,
  rollNumber: "STU001",
  grades: [],
  
  // Method to add a grade
  addGrade: function(grade) {
    if (typeof grade !== 'number' || grade < 0 || grade > 100) {
      console.log("Invalid grade! Must be between 0-100");
      return;
    }
    this.grades.push(grade);
    console.log(`Grade ${grade} added successfully`);
  },
  
  // Method to calculate average
  calculateAverage: function() {
    if (this.grades.length === 0) {
      return "No grades available";
    }
    
    const sum = this.grades.reduce((total, grade) => total + grade, 0);
    return (sum / this.grades.length).toFixed(2);
  },
  
  // Method to display student info
  displayInfo: function() {
    console.log("=== Student Information ===");
    console.log(`Name: ${this.name}`);
    console.log(`Age: ${this.age}`);
    console.log(`Roll Number: ${this.rollNumber}`);
    console.log(`Grades: ${this.grades.join(', ') || 'None'}`);
    console.log(`Average: ${this.calculateAverage()}`);
    console.log("========================");
  }
};

// Test the implementation
student.addGrade(85);
student.addGrade(90);
student.addGrade(88);
student.displayInfo();

console.log("Average:", student.calculateAverage()); // 87.67

student.addGrade(92);
console.log("New Average:", student.calculateAverage()); // 88.75

// Test invalid grade
student.addGrade(150); // Invalid grade message
```

**❌ Common Mistakes:**

1. **Not validating grades:** Always validate input before adding to array
   ```javascript
   // Wrong
   addGrade: function(grade) {
     this.grades.push(grade); // No validation
   }
   
   // Right
   addGrade: function(grade) {
     if (typeof grade !== 'number' || grade < 0 || grade > 100) {
       console.log("Invalid grade!");
       return;
     }
     this.grades.push(grade);
   }
   ```

2. **Not handling empty arrays:** Division by zero causes issues
   ```javascript
   // Wrong
   return sum / this.grades.length; // Can be NaN
   
   // Right
   if (this.grades.length === 0) return "No grades available";
   ```

3. **Not using `this` keyword:** Methods need `this` to access object properties
   ```javascript
   // Wrong
   return grades.length; // ReferenceError
   
   // Right
   return this.grades.length;
   ```

**🎓 What You Learned:**

- Creating objects with multiple properties and methods
- Using `this` to reference object properties within methods
- Array manipulation within object methods
- Input validation and error handling
- Method chaining concepts

**🚀 Extension Challenges:**

1. Add a method to find the highest and lowest grades
2. Implement a grade letter system (A, B, C, D, F)
3. Add functionality to remove a specific grade
4. Create a method to compare this student with another student

---

### Problem 2: Shopping Cart System (Medium)

**📝 Task Description:**

Build a shopping cart system that can add items, remove items, calculate totals, and apply discount codes.

**✅ Requirements Checklist:**

- [ ] Create a cart object to store items
- [ ] Implement addItem() method with quantity support
- [ ] Implement removeItem() method
- [ ] Calculate total price including quantities
- [ ] Apply discount codes (percentage-based)
- [ ] Display cart contents with formatting

**🧪 Test Cases:**

```javascript
// Test Case 1: Add item "Laptop" for ₹50000, quantity 1
// Expected: Cart has 1 item, total ₹50000

// Test Case 2: Add item "Mouse" for ₹500, quantity 2
// Expected: Cart has 2 items, total ₹51000

// Test Case 3: Apply 10% discount
// Expected: Total becomes ₹45900

// Test Case 4: Remove "Mouse"
// Expected: Cart has 1 item, total ₹45000 (with discount)
```

**✅ Solution:**

```javascript
const shoppingCart = {
  items: [],
  discountPercent: 0,
  
  // Add item to cart
  addItem: function(name, price, quantity = 1) {
    // Check if item already exists
    const existingItem = this.items.find(item => item.name === name);
    
    if (existingItem) {
      existingItem.quantity += quantity;
      console.log(`Updated ${name} quantity to ${existingItem.quantity}`);
    } else {
      this.items.push({ name, price, quantity });
      console.log(`Added ${name} to cart`);
    }
  },
  
  // Remove item from cart
  removeItem: function(name) {
    const index = this.items.findIndex(item => item.name === name);
    
    if (index !== -1) {
      const removed = this.items.splice(index, 1)[0];
      console.log(`Removed ${removed.name} from cart`);
      return true;
    }
    
    console.log(`${name} not found in cart`);
    return false;
  },
  
  // Calculate subtotal (before discount)
  getSubtotal: function() {
    return this.items.reduce((total, item) => {
      return total + (item.price * item.quantity);
    }, 0);
  },
  
  // Apply discount code
  applyDiscount: function(code) {
    const discountCodes = {
      'SAVE10': 10,
      'SAVE20': 20,
      'SAVE30': 30
    };
    
    if (discountCodes[code]) {
      this.discountPercent = discountCodes[code];
      console.log(`✓ Applied ${this.discountPercent}% discount`);
    } else {
      console.log('✗ Invalid discount code');
    }
  },
  
  // Get final total (after discount)
  getTotal: function() {
    const subtotal = this.getSubtotal();
    const discount = subtotal * (this.discountPercent / 100);
    return subtotal - discount;
  },
  
  // Display cart
  displayCart: function() {
    console.log("\n========== SHOPPING CART ==========");
    
    if (this.items.length === 0) {
      console.log("Your cart is empty");
      return;
    }
    
    this.items.forEach((item, index) => {
      const itemTotal = item.price * item.quantity;
      console.log(`${index + 1}. ${item.name} - ₹${item.price} x ${item.quantity} = ₹${itemTotal}`);
    });
    
    console.log("-----------------------------------");
    console.log(`Subtotal: ₹${this.getSubtotal()}`);
    
    if (this.discountPercent > 0) {
      const discountAmount = this.getSubtotal() * (this.discountPercent / 100);
      console.log(`Discount (${this.discountPercent}%): -₹${discountAmount}`);
    }
    
    console.log(`TOTAL: ₹${this.getTotal()}`);
    console.log("===================================\n");
  }
};

// Test the shopping cart
shoppingCart.addItem("Laptop", 50000, 1);
shoppingCart.addItem("Mouse", 500, 2);
shoppingCart.addItem("Keyboard", 1500, 1);
shoppingCart.displayCart();

shoppingCart.applyDiscount("SAVE10");
shoppingCart.displayCart();

shoppingCart.removeItem("Mouse");
shoppingCart.displayCart();

// Add more of existing item
shoppingCart.addItem("Keyboard", 1500, 1);
shoppingCart.displayCart();
```

**❌ Common Mistakes:**

1. **Not checking for existing items before adding:**
   ```javascript
   // Wrong - creates duplicate entries
   addItem: function(name, price, quantity) {
     this.items.push({ name, price, quantity });
   }
   
   // Right - updates quantity if exists
   const existingItem = this.items.find(item => item.name === name);
   if (existingItem) {
     existingItem.quantity += quantity;
   }
   ```

2. **Forgetting to multiply by quantity in total calculation:**
   ```javascript
   // Wrong
   return total + item.price;
   
   // Right
   return total + (item.price * item.quantity);
   ```

3. **Mutating discount instead of calculating it:**
   ```javascript
   // Wrong - permanently changes prices
   item.price = item.price * 0.9;
   
   // Right - stores discount percentage separately
   this.discountPercent = 10;
   ```

**🎓 What You Learned:**

- Complex object state management
- Array methods (`find`, `findIndex`, `reduce`, `splice`)
- Default parameters in methods
- Conditional logic in object methods
- Formatting output for user display
- Working with multiple related methods

**🚀 Extension Challenges:**

1. Add update quantity method
2. Implement multiple discount codes (stack discounts)
3. Add tax calculation
4. Create a checkout method that clears the cart
5. Implement item limits (max quantity per item)

---

### Problem 3: Bank Account Manager with Transactions (Medium)

**📝 Task Description:**

Create a bank account system that tracks deposits, withdrawals, and maintains a transaction history.

**✅ Requirements Checklist:**

- [ ] Create account with account number, holder name, balance
- [ ] Implement deposit() method with validation
- [ ] Implement withdraw() method with overdraft protection
- [ ] Maintain transaction history array
- [ ] Add getBalance() and getTransactionHistory() methods
- [ ] Calculate total deposits and withdrawals

**🧪 Test Cases:**

```javascript
// Test Case 1: Create account with initial balance ₹10000
// Expected: Balance = ₹10000, no transactions yet

// Test Case 2: Deposit ₹5000
// Expected: Balance = ₹15000, 1 transaction recorded

// Test Case 3: Withdraw ₹3000
// Expected: Balance = ₹12000, 2 transactions recorded

// Test Case 4: Try to withdraw ₹15000 (more than balance)
// Expected: Transaction rejected, balance unchanged

// Test Case 5: Get transaction summary
// Expected: Total deposits, total withdrawals displayed
```

**✅ Solution:**

```javascript
function createBankAccount(accountNumber, holderName, initialBalance = 0) {
  return {
    accountNumber: accountNumber,
    holderName: holderName,
    balance: initialBalance,
    transactions: [],
    
    // Deposit money
    deposit: function(amount) {
      if (typeof amount !== 'number' || amount <= 0) {
        console.log("❌ Invalid deposit amount");
        return false;
      }
      
      this.balance += amount;
      this.transactions.push({
        type: 'deposit',
        amount: amount,
        date: new Date().toLocaleString(),
        balanceAfter: this.balance
      });
      
      console.log(`✓ Deposited ₹${amount}. New balance: ₹${this.balance}`);
      return true;
    },
    
    // Withdraw money
    withdraw: function(amount) {
      if (typeof amount !== 'number' || amount <= 0) {
        console.log("❌ Invalid withdrawal amount");
        return false;
      }
      
      if (amount > this.balance) {
        console.log(`❌ Insufficient funds. Available balance: ₹${this.balance}`);
        return false;
      }
      
      this.balance -= amount;
      this.transactions.push({
        type: 'withdrawal',
        amount: amount,
        date: new Date().toLocaleString(),
        balanceAfter: this.balance
      });
      
      console.log(`✓ Withdrawn ₹${amount}. New balance: ₹${this.balance}`);
      return true;
    },
    
    // Get current balance
    getBalance: function() {
      return this.balance;
    },
    
    // Get transaction history
    getTransactionHistory: function() {
      if (this.transactions.length === 0) {
        console.log("No transactions yet");
        return;
      }
      
      console.log("\n===== TRANSACTION HISTORY =====");
      this.transactions.forEach((txn, index) => {
        const symbol = txn.type === 'deposit' ? '+' : '-';
        console.log(`${index + 1}. ${txn.type.toUpperCase()}: ${symbol}₹${txn.amount}`);
        console.log(`   Date: ${txn.date}`);
        console.log(`   Balance After: ₹${txn.balanceAfter}`);
        console.log("---");
      });
      console.log("================================\n");
    },
    
    // Get summary statistics
    getSummary: function() {
      const deposits = this.transactions
        .filter(txn => txn.type === 'deposit')
        .reduce((sum, txn) => sum + txn.amount, 0);
      
      const withdrawals = this.transactions
        .filter(txn => txn.type === 'withdrawal')
        .reduce((sum, txn) => sum + txn.amount, 0);
      
      console.log("\n===== ACCOUNT SUMMARY =====");
      console.log(`Account Number: ${this.accountNumber}`);
      console.log(`Holder: ${this.holderName}`);
      console.log(`Current Balance: ₹${this.balance}`);
      console.log(`Total Deposits: ₹${deposits}`);
      console.log(`Total Withdrawals: ₹${withdrawals}`);
      console.log(`Number of Transactions: ${this.transactions.length}`);
      console.log("===========================\n");
    }
  };
}

// Test the bank account
const myAccount = createBankAccount("ACC001", "Priya Sharma", 10000);

console.log("Initial Balance:", myAccount.getBalance());

myAccount.deposit(5000);
myAccount.deposit(2000);
myAccount.withdraw(3000);
myAccount.withdraw(20000); // Should fail
myAccount.withdraw(1500);

myAccount.getTransactionHistory();
myAccount.getSummary();
```

**❌ Common Mistakes:**

1. **Not validating withdrawal amount against balance:**
   ```javascript
   // Wrong - allows negative balance
   this.balance -= amount;
   
   // Right - checks first
   if (amount > this.balance) {
     return false;
   }
   this.balance -= amount;
   ```

2. **Not recording transaction details:**
   ```javascript
   // Wrong - no audit trail
   this.balance += amount;
   
   // Right - maintains history
   this.transactions.push({
     type: 'deposit',
     amount: amount,
     date: new Date(),
     balanceAfter: this.balance
   });
   ```

3. **Mutating balance directly instead of through methods:**
   ```javascript
   // Wrong - bypasses validation
   account.balance = 50000;
   
   // Right - use methods
   account.deposit(50000);
   ```

**🎓 What You Learned:**

- Factory function pattern for creating multiple objects
- Transaction recording and audit trails
- Input validation and error handling
- Array filtering and reducing for statistics
- Working with dates in JavaScript
- Return values for success/failure feedback

**🚀 Extension Challenges:**

1. Add transfer() method to send money between accounts
2. Implement monthly interest calculation
3. Add transaction limits (max withdrawal per day)
4. Create account freezing functionality
5. Implement transaction reversal/cancellation

---

### Problem 4: Product Inventory with Nested Data (Medium)

**📝 Task Description:**

Create an inventory management system that handles products with variants (sizes, colors) and stock tracking.

**✅ Requirements Checklist:**

- [ ] Create products with nested variant information
- [ ] Track stock for each variant
- [ ] Implement method to check product availability
- [ ] Update stock levels for specific variants
- [ ]Calculate total inventory value
- [ ] Find products by criteria (price range, availability)

**🧪 Test Cases:**

```javascript
// Test Case 1: Check if "T-Shirt" in size "M" color "Blue" is available
// Expected: true/false with stock count

// Test Case 2: Update stock for "T-Shirt" size "L" color "Red"
// Expected: Stock updated successfully

// Test Case 3: Calculate total value of inventory
// Expected: Sum of (price × stock) for all variants

// Test Case 4: Find all products under ₹1000
// Expected: Array of products matching criteria
```

**✅ Solution:**

```javascript
const inventory = {
  products: [
    {
      id: 1,
      name: "T-Shirt",
      category: "Clothing",
      basePrice: 499,
      variants: [
        { size: "S", color: "Blue", stock: 20, sku: "TS-S-BLU" },
        { size: "M", color: "Blue", stock: 15, sku: "TS-M-BLU" },
        { size: "L", color: "Red", stock: 10, sku: "TS-L-RED" },
        { size: "M", color: "Black", stock: 0, sku: "TS-M-BLK" }
      ]
    },
    {
      id: 2,
      name: "Jeans",
      category: "Clothing",
      basePrice: 1299,
      variants: [
        { size: "30", color: "Blue", stock: 8, sku: "JN-30-BLU" },
        { size: "32", color: "Black", stock: 12, sku: "JN-32-BLK" }
      ]
    }
  ],
  
  // Check if specific variant is available
  checkAvailability: function(productName, size, color) {
    const product = this.products.find(p => p.name === productName);
    
    if (!product) {
      console.log(`❌ Product "${productName}" not found`);
      return null;
    }
    
    const variant = product.variants.find(
      v => v.size === size && v.color === color
    );
    
    if (!variant) {
      console.log(`❌ Variant not found`);
      return null;
    }
    
    const available = variant.stock > 0;
    console.log(
      `${productName} (${size}, ${color}): ${available ? 'Available' : 'Out of Stock'} - ${variant.stock} units`
    );
    
    return { available, stock: variant.stock, variant };
  },
  
  // Update stock for specific variant
  updateStock: function(sku, quantityChange) {
    for (let product of this.products) {
      const variant = product.variants.find(v => v.sku === sku);
      
      if (variant) {
        const oldStock = variant.stock;
        variant.stock += quantityChange;
        
        if (variant.stock < 0) {
          variant.stock = 0;
          console.log(`⚠️ Stock cannot be negative. Set to 0`);
        }
        
        console.log(
          `✓ Updated ${product.name} (${variant.size}, ${variant.color}): ${oldStock} → ${variant.stock}`
        );
        return true;
      }
    }
    
    console.log(`❌ SKU "${sku}" not found`);
    return false;
  },
  
  // Calculate total inventory value
  getTotalValue: function() {
    let total = 0;
    
    this.products.forEach(product => {
      product.variants.forEach(variant => {
        total += product.basePrice * variant.stock;
      });
    });
    
    return total;
  },
  
  // Find products by price range
  findByPriceRange: function(minPrice, maxPrice) {
    return this.products.filter(
      product => product.basePrice >= minPrice && product.basePrice <= maxPrice
    );
  },
  
  // Get low stock items (less than threshold)
  getLowStockItems: function(threshold = 5) {
    const lowStock = [];
    
    this.products.forEach(product => {
      product.variants.forEach(variant => {
        if (variant.stock < threshold) {
          lowStock.push({
            product: product.name,
            sku: variant.sku,
            size: variant.size,
            color: variant.color,
            stock: variant.stock
          });
        }
      });
    });
    
    return lowStock;
  },
  
  // Display inventory summary
  displayInventory: function() {
    console.log("\n======== INVENTORY SUMMARY ========");
    
    this.products.forEach(product => {
      console.log(`\n${product.name} (₹${product.basePrice})`);
      console.log("Variants:");
      
      product.variants.forEach(variant => {
        const status = variant.stock > 0 ? '✓' : '✗';
        console.log(
          `  ${status} ${variant.size} - ${variant.color}: ${variant.stock} units (${variant.sku})`
        );
      });
    });
    
    console.log(`\nTotal Inventory Value: ₹${this.getTotalValue()}`);
    console.log("===================================\n");
  }
};

// Test the inventory system
inventory.displayInventory();

inventory.checkAvailability("T-Shirt", "M", "Blue");
inventory.checkAvailability("T-Shirt", "M", "Black");

inventory.updateStock("TS-L-RED", -3); // Sold 3 units
inventory.updateStock("TS-M-BLK", 10); // Restocked

console.log("\nProducts under ₹1000:");
console.log(inventory.findByPriceRange(0, 1000));

console.log("\nLow stock items:");
console.log(inventory.getLowStockItems(10));

inventory.displayInventory();
```

**❌ Common Mistakes:**

1. **Not handling nested data structures properly:**
   ```javascript
   // Wrong - doesn't search nested variants
   const found = this.products.find(p => p.size === size);
   
   // Right - searches within nested arrays
   const variant = product.variants.find(v => v.size === size);
   ```

2. **Mutating original data without validation:**
   ```javascript
   // Wrong - can create negative stock
   variant.stock -= quantity;
   
   // Right - validates first
   if (variant.stock >= quantity) {
     variant.stock -= quantity;
   }
   ```

3. **Not using unique identifiers (SKU):**
   ```javascript
   // Wrong - ambiguous identification
   updateStock("T-Shirt", "M", "Blue", -5);
   
   // Right - use unique SKU
   updateStock("TS-M-BLU", -5);
   ```

**🎓 What You Learned:**

- Working with deeply nested object structures
- Array methods for searching and filtering nested data
- SKU-based inventory tracking
- Aggregating data across multiple levels
- Complex business logic implementation
- Data validation at multiple levels

**🚀 Extension Challenges:**

1. Add product search by name or category
2. Implement bulk stock updates from CSV/array
3. Add variant pricing (different prices per variant)
4. Create reorder alerts when stock is low
5. Implement product bundles (multiple products together)

---

### Problem 5: Task Manager with Priority and Status (Hard)

**📝 Task Description:**

Build a comprehensive task management system with priority levels, status tracking, filtering, and sorting capabilities.

**✅ Requirements Checklist:**

- [ ] Create tasks with title, description, priority, status, due date
- [ ] Implement methods to add, update, and delete tasks
- [ ] Filter tasks by status, priority, or due date
- [ ] Sort tasks by different criteria
- [ ] Mark tasks as complete with completion date
- [ ] Generate task statistics (pending, completed, overdue)

**🧪 Test Cases:**

```javascript
// Test Case 1: Add task with high priority
// Expected: Task added with unique ID

// Test Case 2: Filter tasks by status "pending"
// Expected: Array of pending tasks

// Test Case 3: Mark task as complete
// Expected: Status changed, completion date added

// Test Case 4: Get overdue tasks
// Expected: Array of tasks past due date

// Test Case 5: Get statistics
// Expected: Count of tasks by status and priority
```

**✅ Solution:**

```javascript
const taskManager = {
  tasks: [],
  nextId: 1,
  
  // Add new task
  addTask: function(title, description, priority = 'medium', dueDate = null) {
    const validPriorities = ['low', 'medium', 'high'];
    
    if (!validPriorities.includes(priority)) {
      console.log("❌ Invalid priority. Use: low, medium, or high");
      return null;
    }
    
    const task = {
      id: this.nextId++,
      title: title,
      description: description,
      priority: priority,
      status: 'pending',
      createdAt: new Date(),
      dueDate: dueDate ? new Date(dueDate) : null,
      completedAt: null
    };
    
    this.tasks.push(task);
    console.log(`✓ Task added: "${title}" (ID: ${task.id})`);
    return task;
  },
  
  // Update task
  updateTask: function(id, updates) {
    const task = this.tasks.find(t => t.id === id);
    
    if (!task) {
      console.log(`❌ Task ID ${id} not found`);
      return false;
    }
    
    // Validate priority if being updated
    if (updates.priority) {
      const validPriorities = ['low', 'medium', 'high'];
      if (!validPriorities.includes(updates.priority)) {
        console.log("❌ Invalid priority");
        return false;
      }
    }
    
    // Apply updates
    Object.assign(task, updates);
    console.log(`✓ Task ${id} updated`);
    return true;
  },
  
  // Delete task
  deleteTask: function(id) {
    const index = this.tasks.findIndex(t => t.id === id);
    
    if (index === -1) {
      console.log(`❌ Task ID ${id} not found`);
      return false;
    }
    
    const deleted = this.tasks.splice(index, 1)[0];
    console.log(`✓ Deleted task: "${deleted.title}"`);
    return true;
  },
  
  // Mark task as complete
  completeTask: function(id) {
    const task = this.tasks.find(t => t.id === id);
    
    if (!task) {
      console.log(`❌ Task ID ${id} not found`);
      return false;
    }
    
    task.status = 'completed';
    task.completedAt = new Date();
    console.log(`✓ Task "${task.title}" marked as complete`);
    return true;
  },
  
  // Filter tasks by status
  getTasksByStatus: function(status) {
    return this.tasks.filter(t => t.status === status);
  },
  
  // Filter tasks by priority
  getTasksByPriority: function(priority) {
    return this.tasks.filter(t => t.priority === priority);
  },
  
  // Get overdue tasks
  getOverdueTasks: function() {
    const now = new Date();
    return this.tasks.filter(t => 
      t.status === 'pending' && 
      t.dueDate && 
      t.dueDate < now
    );
  },
  
  // Sort tasks
  sortTasks: function(criteria = 'priority') {
    const priorityOrder = { high: 3, medium: 2, low: 1 };
    
    const sortedTasks = [...this.tasks];
    
    switch(criteria) {
      case 'priority':
        sortedTasks.sort((a, b) => 
          priorityOrder[b.priority] - priorityOrder[a.priority]
        );
        break;
        
      case 'dueDate':
        sortedTasks.sort((a, b) => {
          if (!a.dueDate) return 1;
          if (!b.dueDate) return -1;
          return a.dueDate - b.dueDate;
        });
        break;
        
      case 'createdAt':
        sortedTasks.sort((a, b) => b.createdAt - a.createdAt);
        break;
        
      default:
        console.log("❌ Invalid sort criteria");
        return this.tasks;
    }
    
    return sortedTasks;
  },
  
  // Get statistics
  getStatistics: function() {
    const stats = {
      total: this.tasks.length,
      pending: 0,
      completed: 0,
      overdue: 0,
      byPriority: { high: 0, medium: 0, low: 0 }
    };
    
    const now = new Date();
    
    this.tasks.forEach(task => {
      // Count by status
      if (task.status === 'pending') stats.pending++;
      if (task.status === 'completed') stats.completed++;
      
      // Count overdue
      if (task.status === 'pending' && task.dueDate && task.dueDate < now) {
        stats.overdue++;
      }
      
      // Count by priority
      stats.byPriority[task.priority]++;
    });
    
    return stats;
  },
  
  // Display all tasks
  displayTasks: function(tasks = this.tasks) {
    if (tasks.length === 0) {
      console.log("No tasks to display");
      return;
    }
    
    console.log("\n========== TASK LIST ==========");
    
    tasks.forEach(task => {
      const dueDateStr = task.dueDate 
        ? task.dueDate.toLocaleDateString() 
        : 'No due date';
      
      const statusIcon = task.status === 'completed' ? '✓' : '○';
      const priorityIcon = { high: '🔴', medium: '🟡', low: '🟢' };
      
      console.log(`\n${statusIcon} [${task.id}] ${task.title}`);
      console.log(`   ${priorityIcon[task.priority]} Priority: ${task.priority}`);
      console.log(`   Due: ${dueDateStr}`);
      console.log(`   Status: ${task.status}`);
      
      if (task.description) {
        console.log(`   Description: ${task.description}`);
      }
    });
    
    console.log("\n================================\n");
  },
  
  // Display statistics
  displayStatistics: function() {
    const stats = this.getStatistics();
    
    console.log("\n===== TASK STATISTICS =====");
    console.log(`Total Tasks: ${stats.total}`);
    console.log(`Pending: ${stats.pending}`);
    console.log(`Completed: ${stats.completed}`);
    console.log(`Overdue: ${stats.overdue}`);
    console.log("\nBy Priority:");
    console.log(`  🔴 High: ${stats.byPriority.high}`);
    console.log(`  🟡 Medium: ${stats.byPriority.medium}`);
    console.log(`  🟢 Low: ${stats.byPriority.low}`);
    console.log("===========================\n");
  }
};

// Test the task manager
taskManager.addTask(
  "Complete project documentation",
  "Write comprehensive docs for the new feature",
  "high",
  "2025-01-05"
);

taskManager.addTask(
  "Review pull requests",
  "Check and approve pending PRs",
  "medium",
  "2025-01-03"
);

taskManager.addTask(
  "Update dependencies",
  "Upgrade npm packages to latest versions",
  "low",
  "2025-01-10"
);

taskManager.addTask(
  "Fix critical bug",
  "Address security vulnerability in auth",
  "high",
  "2024-12-30" // Overdue
);

taskManager.displayTasks();

// Complete a task
taskManager.completeTask(2);

// Display by priority
console.log("\n=== HIGH PRIORITY TASKS ===");
taskManager.displayTasks(taskManager.getTasksByPriority('high'));

// Display overdue
console.log("\n=== OVERDUE TASKS ===");
taskManager.displayTasks(taskManager.getOverdueTasks());

// Display statistics
taskManager.displayStatistics();

// Sort by due date
console.log("\n=== SORTED BY DUE DATE ===");
taskManager.displayTasks(taskManager.sortTasks('dueDate'));
```

**❌ Common Mistakes:**

1. **Not generating unique IDs:**
   ```javascript
   // Wrong - can create duplicate IDs
   id: Math.random()
   
   // Right - sequential or UUID
   id: this.nextId++
   ```

2. **Not validating enum values:**
   ```javascript
   // Wrong - allows invalid priorities
   task.priority = priority;
   
   // Right - validates first
   if (!['low', 'medium', 'high'].includes(priority)) {
     return null;
   }
   ```

3. **Mutating original array when sorting:**
   ```javascript
   // Wrong - modifies original
   return this.tasks.sort(...);
   
   // Right - creates copy first
   return [...this.tasks].sort(...);
   ```

4. **Not handling null/undefined dates:**
   ```javascript
   // Wrong - crashes if dueDate is null
   return a.dueDate - b.dueDate;
   
   // Right - checks for null
   if (!a.dueDate) return 1;
   if (!b.dueDate) return -1;
   ```

**🎓 What You Learned:**

- Complex state management with multiple properties
- Filtering and sorting with multiple criteria
- Working with dates in JavaScript
- Enum validation patterns
- Statistical aggregation
- CRUD operations (Create, Read, Update, Delete)
- Non-mutating array operations
- ID generation strategies

**🚀 Extension Challenges:**

1. Add tags/categories to tasks
2. Implement task dependencies (task B depends on task A)
3. Add recurring tasks functionality
4. Implement task search by keywords
5. Add task assignment to different users
6. Create subtasks functionality
7. Export tasks to JSON
8. Add undo/redo functionality

---

## Section 7: KNOWLEDGE CHECK MCQs 📝

Test your understanding with these multiple-choice questions covering all aspects of JavaScript objects.

---

### Question 1: Basic Object Creation (Easy)

Which of the following creates a valid JavaScript object?

A) `const obj = (name: "John", age: 30);`  
B) `const obj = [name: "John", age: 30];`  
C) `const obj = {name: "John", age: 30};`  
D) `const obj = <name: "John", age: 30>;`

<details>
<summary>✅ Answer</summary>

**Correct Answer: C**

**Explanation:**  
JavaScript objects are created using curly braces `{}` with key-value pairs separated by colons. Option A uses parentheses (which would be a syntax error), option B uses square brackets (which creates an array, not an object with named properties), and option D uses angle brackets (invalid syntax).

**Interview Tip:**  
If asked about object creation, mention that there are multiple ways: object literals `{}`, constructor functions with `new`, `Object.create()`, and ES6 classes. Object literals are the most common and preferred for simple objects.

**Key Concept:**  
Object literal syntax is the simplest and most direct way to create objects in JavaScript: `{key: value}`.

</details>

---

### Question 2: Property Access (Easy)

Given `const user = { "first-name": "John", age: 30 };`, which statement correctly accesses the first name?

A) `user.first-name`  
B) `user["first-name"]`  
C) `user[first-name]`  
D) `user.firstName`

<details>
<summary>✅ Answer</summary>

**Correct Answer: B**

**Explanation:**  
Property names with hyphens or special characters must be accessed using bracket notation with quotes: `user["first-name"]`. Option A would attempt to subtract `name` from `user.first`. Option C is missing quotes around the property name. Option D looks for a different property name altogether.

**Interview Tip:**  
Be prepared to explain when bracket notation is required: special characters in keys, spaces, dynamic property access with variables, or when the property name is a reserved keyword.

**Key Concept:**  
Use bracket notation `obj["property"]` for properties with special characters; use dot notation `obj.property` for standard property names.

</details>

---

### Question 3: Object References (Medium)

What will this code output?

```javascript
const a = { x: 1 };
const b = a;
const c = { x: 1 };

console.log(a === b);
console.log(a === c);
```

A) `true`, `true`  
B) `true`, `false`  
C) `false`, `true`  
D) `false`, `false`

<details>
<summary>✅ Answer</summary>

**Correct Answer: B**

**Explanation:**  
`a === b` is `true` because `b` is assigned the reference to the same object as `a` - they point to the same memory location. `a === c` is `false` because `c` is a completely new object with the same content but stored in a different memory location. Objects are compared by reference, not by their content.

**Interview Tip:**  
This is a very common interview question! Emphasize that JavaScript objects are reference types, and assignment copies the reference, not the object itself. Contrast this with primitive types (numbers, strings, booleans) which are compared by value.

**Key Concept:**  
Objects are compared by reference (memory location), not by value (content). Two objects with identical properties are not equal unless they reference the same object.

</details>

---

### Question 4: Object Destructuring (Medium)

What will be logged?

```javascript
const { name, age = 25, city } = { name: "Alice", city: "NYC" };
console.log(age);
```

A) `undefined`  
B) `25`  
C) `null`  
D) `Error`

<details>
<summary>✅ Answer</summary>

**Correct Answer: B**

**Explanation:**  
The object being destructured doesn't have an `age` property, so the default value of `25` is used. Default values in destructuring only apply when the property is `undefined` or doesn't exist. This is different from the nullish coalescing operator which also applies to `null`.

**Interview Tip:**  
Explain that destructuring with defaults is extremely useful for function parameters, especially when creating functions with optional configuration objects. It makes code more readable than repeatedly checking `if (config.option === undefined)`.

**Key Concept:**  
Destructuring allows you to extract properties and assign default values in one concise statement: `const { prop = defaultValue } = obj;`

</details>

---

### Question 5: Shallow vs Deep Copy (Hard)

What will this code output?

```javascript
const original = { a: 1, b: { c: 2 } };
const copy = { ...original };
copy.b.c = 3;
console.log(original.b.c);
```

A) `1`  
B) `2`  
C) `3`  
D) `undefined`

<details>
<summary>✅ Answer</summary>

**Correct Answer: C**

**Explanation:**  
The spread operator creates a shallow copy, meaning it only copies the first level of properties. The nested object `b` is copied by reference, so both `original.b` and `copy.b` point to the same nested object. When `copy.b.c` is modified, it affects the shared nested object, so `original.b.c` also becomes `3`.

**Interview Tip:**  
This is a critical concept for React developers and anyone working with immutable data patterns. Be ready to explain the difference between shallow and deep copies, and when you'd need a deep copy (use `structuredClone()` or `JSON.parse(JSON.stringify())` with limitations).

**Key Concept:**  
Shallow copies (`...`, `Object.assign()`) duplicate first-level properties but share references to nested objects. Deep copies require special methods.

</details>

---

### Question 6: Object Methods (Medium)

What will this code output?

```javascript
const calc = {
  value: 10,
  add: function(n) {
    this.value += n;
    return this;
  }
};

calc.add(5).add(3);
console.log(calc.value);
```

A) `10`  
B) `15`  
C) `18`  
D) `Error`

<details>
<summary>✅ Answer</summary>

**Correct Answer: C**

**Explanation:**  
The `add` method returns `this` (the object itself), enabling method chaining. First call: `calc.add(5)` makes `value = 15` and returns `calc`. Second call: `.add(3)` on the returned object makes `value = 18`. This pattern is called "fluent interface" or "method chaining."

**Interview Tip:**  
Method chaining is widely used in popular libraries like jQuery, Lodash, and D3.js. Mention that returning `this` from methods allows for cleaner, more readable code when multiple operations need to be performed on the same object.

**Key Concept:**  
Returning `this` from object methods enables method chaining: `obj.method1().method2().method3()`.

</details>

---

### Question 7: Object.freeze() (Medium)

What happens with this code?

```javascript
const obj = Object.freeze({ x: 1, y: { z: 2 } });
obj.x = 10;
obj.y.z = 20;
console.log(obj.x, obj.y.z);
```

A) `10`, `20`  
B) `1`, `2`  
C) `1`, `20`  
D) `Error`

<details>
<summary>✅ Answer</summary>

**Correct Answer: C**

**Explanation:**  
`Object.freeze()` performs a shallow freeze - it prevents modification of first-level properties (`obj.x` remains `1`), but nested objects are not frozen and can still be modified (`obj.y.z` becomes `20`). To freeze nested objects, you'd need to recursively freeze each level.

**Interview Tip:**  
This is important for understanding immutability in JavaScript. Explain that `Object.freeze()` is shallow, and for truly immutable nested structures, you either need a deep freeze function or use immutable data libraries like Immer or Immutable.js.

**Key Concept:**  
`Object.freeze()` is shallow - it only freezes direct properties, not nested objects.

</details>

---

### Question 8: Dynamic Property Names (Medium)

What will be the output?

```javascript
const key = "status";
const obj = {
  name: "Task",
  [key]: "pending"
};
console.log(obj.status);
```

A) `undefined`  
B) `"pending"`  
C) `"status"`  
D) `Error`

<details>
<summary>✅ Answer</summary>

**Correct Answer: B**

**Explanation:**  
Square brackets in object literals create computed property names. The variable `key` is evaluated at runtime, so `[key]` becomes `["status"]`, creating a property named `status` with value `"pending"`. This ES6 feature allows dynamic property creation during object initialization.

**Interview Tip:**  
Computed property names are extremely useful when building objects dynamically, especially when the property names come from user input, API responses, or configuration. This is cleaner than creating an empty object and then using bracket notation to add properties.

**Key Concept:**  
Computed property names `[expression]` in object literals allow dynamic property creation: `{ [variable]: value }`.

</details>

---

### Question 9: Optional Chaining (Medium)

What will this code output?

```javascript
const user = { profile: { name: "Alice" } };
console.log(user.profile?.name);
console.log(user.settings?.theme);
console.log(user.getEmail?.());
```

A) `"Alice"`, `undefined`, `Error`  
B) `"Alice"`, `undefined`, `undefined`  
C) `undefined`, `undefined`, `undefined`  
D) `Error` on first line

<details>
<summary>✅ Answer</summary>

**Correct Answer: B**

**Explanation:**  
1. `user.profile?.name` → `"Alice"` - `profile` exists, so it accesses `name`
2. `user.settings?.theme` → `undefined` - `settings` doesn't exist, short-circuits to `undefined`
3. `user.getEmail?.()` → `undefined` - `getEmail` doesn't exist, optional method call returns `undefined` instead of throwing error

**Interview Tip:**  
Optional chaining (`?.`) was introduced in ES2020 and is a game-changer for handling potentially null/undefined values. Emphasize that it's not just for properties but also works with methods (`?.()`) and array indexing (`?.[0]`). It prevents the common "Cannot read property of undefined" error.

**Key Concept:**  
Optional chaining (`?.`) safely accesses nested properties and returns `undefined` instead of throwing errors if any part of the chain is null/undefined.

</details>

---

### Question 10: Object.keys() vs Object.entries() (Medium)

What will this code output?

```javascript
const user = { name: "John", age: 30 };
console.log(Object.keys(user).length);
console.log(Object.entries(user).length);
console.log(Object.entries(user)[0]);
```

A) `2`, `2`, `"name"`  
B) `2`, `2`, `["name", "John"]`  
C) `2`, `4`, `["name", "John"]`  
D) `undefined`, `undefined`, `undefined`

<details>
<summary>✅ Answer</summary>

**Correct Answer: B**

**Explanation:**  
- `Object.keys(user)` returns `["name", "age"]` with length `2`
- `Object.entries(user)` returns `[["name", "John"], ["age", 30]]` with length `2` (two entries)
- `Object.entries(user)[0]` is the first entry: `["name", "John"]`

**Interview Tip:**  
These methods are essential for iterating over objects. Explain the trio: `Object.keys()` for property names, `Object.values()` for values only, and `Object.entries()` for both as `[key, value]` pairs. `Object.entries()` is particularly useful with destructuring in loops.

**Key Concept:**  
`Object.entries()` returns an array of `[key, value]` pairs, perfect for iterating objects with `for...of` loops or array methods.

</details>

---

### Question 11: Object Assignment (Easy)

Which creates a new object by merging properties?

A) `Object.combine(obj1, obj2)`  
B) `Object.assign({}, obj1, obj2)`  
C) `Object.merge(obj1, obj2)`  
D) `Object.join(obj1, obj2)`

<details>
<summary>✅ Answer</summary>

**Correct Answer: B**

**Explanation:**  
`Object.assign(target, ...sources)` copies properties from source objects to a target object. Using an empty object `{}` as the target creates a new object without modifying the originals. The other methods don't exist in JavaScript.

**Interview Tip:**  
Mention that the spread operator `{...obj1, ...obj2}` is now more commonly used than `Object.assign()` for merging objects as it's more concise and readable. However, `Object.assign()` is still useful when you want to merge into an existing object or need its return value.

**Key Concept:**  
`Object.assign({},obj1, obj2)` or `{...obj1, ...obj2}` merges objects into a new object (shallow merge).

</details>

---

### Question 12: Nested Object Access (Hard)

What's the safest way to access deeply nested properties?

```javascript
const data = { user: { profile: { email: "test@example.com" } } };
```

A) `data.user.profile.email || "No email"`  
B) `data?.user?.profile?.email ?? "No email"`  
C) `data && data.user && data.user.profile && data.user.profile.email`  
D) Both B and C

<details>
<summary>✅ Answer</summary>

**Correct Answer: D**

**Explanation:**  
Both B and C safely access nested properties without errors. Option B uses modern optional chaining (`?.`) and nullish coalescing (`??`), which is more concise. Option C uses the traditional approach of checking each level. Option A could fail if `user` or `profile` don't exist.

**Interview Tip:**  
While both work, strongly advocate for optional chaining in modern codebases. It's more readable, less error-prone, and supported in all modern browsers and Node.js versions. Mention that for legacy browser support, transpilers like Babel can convert it.

**Key Concept:**  
Optional chaining (`?.`) is the modern, preferred way to safely access nested properties: `obj?.prop1?.prop2?.prop3`.

</details>

---

### Question 13: Constructor Functions (Medium)

What does the `new` keyword do?

```javascript
function Person(name) {
  this.name = name;
}
const john = new Person("John");
```

A) Creates a new function  
B) Creates a new object, sets `this` to it, and returns it  
C) Makes a copy of the Person function  
D) Binds `this` to the global object

<details>
<summary>✅ Answer</summary>

**Correct Answer: B**

**Explanation:**  
The `new` keyword: (1) Creates a new empty object, (2) Sets `this` inside the constructor to that new object, (3) Links the object's prototype to the constructor's prototype, (4) Returns the new object (unless the constructor explicitly returns a different object).

**Interview Tip:**  
Be ready to explain the four steps of what `new` does. Also mention that ES6 classes are syntactic sugar over constructor functions - they work the same way internally. Forgetting `new` is a common mistake that causes `this` to point to the global object (or undefined in strict mode).

**Key Concept:**  
`new` creates an instance: it creates a new object, sets `this` to it, executes the constructor, and returns the object.

</details>

---

### Question 14: Object.seal() (Medium)

What's true about `Object.seal()`?

A) Prevents all modifications to the object  
B) Prevents adding/deleting properties but allows modifications  
C) Only prevents adding new properties  
D) Makes the object immutable at all levels

<details>
<summary>✅ Answer</summary>

**Correct Answer: B**

**Explanation:**  
`Object.seal()` prevents adding new properties and deleting existing properties, but allows modifying existing property values. It's a middle ground between a regular object and `Object.freeze()`.

**Interview Tip:**  
Explain the progression: regular object (anything allowed) → sealed object (can modify but not add/delete) → frozen object (nothing allowed). Mention that both seal and freeze are shallow operations.

**Key Concept:**  
`Object.seal()` allows modifications but prevents structural changes (adding/deleting properties).

</details>

---

### Question 15: Factory Functions vs Constructors (Medium)

What's a key difference between factory functions and constructor functions?

A) Factory functions are faster  
B) Factory functions don't require the `new` keyword  
C) Constructor functions can't have methods  
D) Factory functions can't create objects

<details>
<summary>✅ Answer</summary>

**Correct Answer: B**

**Explanation:**  
Factory functions are regular functions that return new objects and don't require `new`. Constructor functions must be called with `new` to work properly. Both can create objects with methods, but factory functions offer more flexibility (private variables, conditional logic) while constructor functions are more memory-efficient with shared methods on the prototype.

**Interview Tip:**  
Discuss the trade-offs: Constructor functions with prototypes are more memory-efficient for many instances. Factory functions provide better encapsulation and don't have the "forgetting new" problem. ES6 classes are essentially a cleaner syntax for constructor functions.

**Key Concept:**  
Factory functions return objects directly without `new`; constructor functions require `new` and use `this`.

</details>

---

### Question 16: Object Iteration (Medium)

Which loop is best for iterating over an object's own properties?

A) `for...in` (always safe)  
B) `for...of` with the object directly  
C) `for...in` with `hasOwnProperty` check  
D) Regular `for` loop

<details>
<summary>✅ Answer</summary>

**Correct Answer: C**

**Explanation:**  
`for...in` iterates over all enumerable properties including inherited ones from the prototype chain. Using `hasOwnProperty()` (or modern `Object.hasOwn()`) ensures you only process the object's own properties. `for...of` doesn't work directly on objects (only on iterables like arrays).

**Interview Tip:**  
Mention that modern alternatives are often better: `Object.keys()`, `Object.values()`, or `Object.entries()` combined with array methods. These automatically give you only own properties and are more functional in style.

**Key Concept:**  
`for...in` iterates all enumerable properties (including inherited); use `hasOwnProperty` check or use `Object.keys()` for own properties only.

</details>

---

### Question 17: Nullish Coalescing in Objects (Hard)

What will this output?

```javascript
const user = { name: "Alice", age: 0, email: "" };
console.log(user.age || 18);
console.log(user.age ?? 18);
console.log(user.email || "No email");
console.log(user.email ?? "No email");
```

A) `18`, `0`, `"No email"`, `""`  
B) `0`, `0`, `""`, `""`  
C) `18`, `18`, `"No email"`, `"No email"`  
D) `0`, `18`, `""`, `"No email"`

<details>
<summary>✅ Answer</summary>

**Correct Answer: A**

**Explanation:**  
- `||` treats `0` and `""` as falsy, so returns the default value
- `??` only treats `null` and `undefined` as "nullish", so returns the actual value even if it's `0` or `""`
- First line: `0 || 18` → `18` (0 is falsy)
- Second line: `0 ?? 18` → `0` (0 is not null/undefined)
- Third line: `"" || "No email"` → `"No email"` ("" is falsy)
- Fourth line: `"" ?? "No email"` → `""` ("" is not null/undefined)

**Interview Tip:**  
This is a critical distinction! Explain that `??` is safer for default values when `0`, `false`, or `""` are valid values. Use `||` for general "give me something truthy" and `??` for "give me a default only if null/undefined."

**Key Concept:**  
`??` (nullish coalescing) only replaces `null`/`undefined`; `||` replaces any falsy value (`0`, `false`, `""`, `null`, `undefined`).

</details>

---

### Question 18: Object Property Shorthand (Easy)

What does this syntax create?

```javascript
const name = "Alice";
const age = 30;
const user = { name, age };
```

A) Syntax error  
B) `{ name: name, age: age }`  
C) `{ "name", "age" }`  
D) An array

<details>
<summary>✅ Answer</summary>

**Correct Answer: B**

**Explanation:**  
ES6 property shorthand allows omitting the value when the variable name matches the property name. `{ name, age }` is equivalent to `{ name: name, age: age }`. This is syntactic sugar that makes code more concise.

**Interview Tip:**  
This is ubiquitous in modern JavaScript, especially in React components and Node.js. Mention that it works in destructuring too: `const { name, age } = user`. It's part of making JavaScript more concise and readable.

**Key Concept:**  
Property shorthand `{ name }` is equivalent to `{ name: name }` when the variable name matches the property name.

</details>

---

### Question 19: `this` in Object Methods (Hard)

What will this code output?

```javascript
const obj = {
  value: 42,
  getValue: function() {
    return this.value;
  }
};

const getValue = obj.getValue;
console.log(getValue());
```

A) `42`  
B) `undefined`  
C) `Error`  
D) `null`

<details>
<summary>✅ Answer</summary>

**Correct Answer: B**

**Explanation:**  
When the method is extracted and called as `getValue()`, it loses its context. `this` no longer refers to `obj` but to the global object (or `undefined` in strict mode). Since `this.value` doesn't exist in that context, it returns `undefined`. To fix this, use arrow functions, `.bind()`, or call it as `obj.getValue()`.

**Interview Tip:**  
This is a fundamental concept about `this` binding. Explain the four binding rules: implicit (method call), explicit (`.call()`, `.apply()`, `.bind()`), new binding, and default binding. This question tests understanding of how `this` can be "lost" when a method is passed around.

**Key Concept:**  
`this` in methods depends on how the function is called. Extracted methods lose their context: `const fn = obj.method; fn();` makes `this` undefined/global.

</details>

---

### Question 20: structuredClone() (Hard)

What's the advantage of `structuredClone()` over `JSON.parse(JSON.stringify())`?

A) It's faster in all cases  
B) It handles dates, Maps, Sets, and circular references  
C) It works in older browsers  
D) It creates shallow copies

<details>
<summary>✅ Answer</summary>

**Correct Answer: B**

**Explanation:**  
`structuredClone()` is a modern API that can deep clone complex objects including Dates, Maps, Sets, TypedArrays, and even handles circular references. `JSON.parse(JSON.stringify())` fails on these: Dates become strings, Maps/Sets become empty objects, functions are removed, and circular references throw errors.

**Interview Tip:**  
Mention that `structuredClone()` is relatively new (2022) and while it's supported in modern environments, you might need a polyfill for older browsers. For maximum compatibility, lodash's `_.cloneDeep()` is still a solid choice. Explain the limitations of the JSON method and when each approach is appropriate.

**Key Concept:**  
`structuredClone()` is the modern way to deep clone objects, handling complex types that `JSON.parse(JSON.stringify())` can't.

</details>

---

## Section 8: REAL INTERVIEW QUESTIONS 🎯

Practice with actual questions asked in technical interviews, complete with strategies and model answers.

---

### Question 1: Object Comparison Function (Entry Level)

**⏱ Time Expected:** 10-15 minutes  
**🎯 What They're Testing:** Understanding of object references, property iteration, and comparison logic

**Question:**  
Write a function `areObjectsEqual(obj1, obj2)` that checks if two objects are equal (have the same properties with the same values). Handle nested objects.

**💡 What Interviewers Want to See:**
- Understanding that `===` doesn't work for object comparison
- Systematic approach to comparing properties
- Handling edge cases (null, different types, nested objects)
- Clean, readable code

**✅ Strong Answer Template:**

```javascript
function areObjectsEqual(obj1, obj2) {
  // Handle edge cases
  if (obj1 === obj2) return true;
  
  if (obj1 == null || obj2 == null) return false;
  
  if (typeof obj1 !== 'object' || typeof obj2 !== 'object') {
    return obj1 === obj2;
  }
  
  // Check if they have the same number of properties
  const keys1 = Object.keys(obj1);
  const keys2 = Object.keys(obj2);
  
  if (keys1.length !== keys2.length) return false;
  
  // Check each property
  for (let key of keys1) {
    if (!keys2.includes(key)) return false;
    
    // Recursively compare values
    if (!areObjectsEqual(obj1[key], obj2[key])) {
      return false;
    }
  }
  
  return true;
}

// Test cases
console.log(areObjectsEqual({a: 1}, {a: 1})); // true
console.log(areObjectsEqual({a: 1}, {a: 2})); // false
console.log(areObjectsEqual({a: {b: 2}}, {a: {b: 2}})); // true
console.log(areObjectsEqual({a: 1, b: 2}, {a: 1})); // false
```

**📊 Talk Through Your Approach:**
"First, I handle edge cases: if both references are the same, they're equal. If either is null, they're not equal unless both are null. Then I compare the number of properties - if they differ, the objects can't be equal. Finally, I recursively compare each property value, which handles nested objects."

**🔄 Follow-up Questions to Expect:**
1. **"How would you handle circular references?"**
   - "I'd use a WeakMap to track visited objects and detect cycles."

2. **"What about array properties?"**
   - "I'd add a check: if both values are arrays, compare them element by element."

3. **"How would you make this work for deep objects with 100+ nested levels?"**
   - "I'd consider an iterative approach using a stack to avoid stack overflow from deep recursion."

**🚫 Red Flags to Avoid:**
- Using `JSON.stringify()` for comparison (doesn't work reliably due to property order)
- Not handling null/undefined
- Not considering nested objects
- Using `==` instead of `===` for primitives

**💼 Pro Tips:**
- Mention that libraries like Lodash have `_.isEqual()` for production use
- Show awareness of performance: "For frequent comparisons, we might cache results"
- Discuss the difference between shallow and deep equality

---

### Question 2: Implement a Simple Cache (Mid Level)

**⏱ Time Expected:** 15-20 minutes  
**🎯 What They're Testing:** Object manipulation, closure concepts, and practical problem-solving

**Question:**  
Implement a cache system with `set(key, value)`, `get(key)`, `has(key)`, and `clear()` methods. Add a size limit that removes the oldest entries when exceeded.

**💡 What Interviewers Want to See:**
- Clean object-oriented design
- Understanding of data structures (objects vs Maps)
- Handling of edge cases
- Consideration of memory management

**✅ Strong Answer Template:**

```javascript
function createCache(maxSize = 100) {
  const cache = {};
  const insertionOrder = [];
  
  return {
    set: function(key, value) {
      // If key exists, update it
      if (key in cache) {
        cache[key] = value;
        return true;
      }
      
      // If cache is full, remove oldest entry
      if (insertionOrder.length >= maxSize) {
        const oldestKey = insertionOrder.shift();
        delete cache[oldestKey];
      }
      
      // Add new entry
      cache[key] = value;
      insertionOrder.push(key);
      return true;
    },
    
    get: function(key) {
      return cache[key];
    },
    
    has: function(key) {
      return key in cache;
    },
    
    delete: function(key) {
      if (key in cache) {
        delete cache[key];
        const index = insertionOrder.indexOf(key);
        if (index > -1) {
          insertionOrder.splice(index, 1);
        }
        return true;
      }
      return false;
    },
    
    clear: function() {
      Object.keys(cache).forEach(key => delete cache[key]);
      insertionOrder.length = 0;
    },
    
    size: function() {
      return insertionOrder.length;
    },
    
    keys: function() {
      return [...insertionOrder];
    }
  };
}

// Test the cache
const cache = createCache(3);
cache.set('a', 1);
cache.set('b', 2);
cache.set('c', 3);
console.log(cache.size()); // 3

cache.set('d', 4); // Should remove 'a'
console.log(cache.has('a')); // false
console.log(cache.has('d')); // true

console.log(cache.get('b')); // 2
cache.clear();
console.log(cache.size()); // 0
```

**📊 Talk Through Your Approach:**
"I'm using a factory function that returns an object with cache methods. I use a plain object for O(1) key-value storage and an array to track insertion order for the LRU (Least Recently Used) eviction policy. When the cache reaches max size, I remove the oldest entry before adding a new one."

**🔄 Follow-up Questions to Expect:**
1. **"How would you make this an LRU cache where accessing an item makes it 'recent'?"**
   ```javascript
   get: function(key) {
     if (key in cache) {
       // Move to end (most recent)
       const index = insertionOrder.indexOf(key);
       insertionOrder.splice(index, 1);
       insertionOrder.push(key);
       return cache[key];
     }
     return undefined;
   }
   ```

2. **"What if we wanted TTL (Time To Live) for entries?"**
   - "I'd store objects with `{ value, timestamp }` and check expiration on get()."

3. **"Could you use a Map instead?"**
   - "Yes! Maps maintain insertion order natively, which would simplify the code."

**🚫 Red Flags to Avoid:**
- Not considering performance (inefficient deletion from arrays)
- Memory leaks (not properly cleaning up)
- Not handling edge cases (negative max size, invalid keys)

**💼 Pro Tips:**
- Mention real-world use cases: API response caching, memoization
- Discuss trade-offs between Map and plain objects
- Consider mentioning WeakMap for garbage collection benefits

---

### Question 3: Flatten a Nested Object (Mid Level)

**⏱ Time Expected:** 15-20 minutes  
**🎯 What They're Testing:** Recursion, object traversal, string manipulation

**Question:**  
Write a function that flattens a nested object. For example:
```javascript
const input = {
  a: 1,
  b: {
    c: 2,
    d: {
      e: 3
    }
  }
};
// Output: { 'a': 1, 'b.c': 2, 'b.d.e': 3 }
```

**💡 What Interviewers Want to See:**
- Recursive thinking
- String building for nested keys
- Handling different data types
- Clean, maintainable code

**✅ Strong Answer Template:**

```javascript
function flattenObject(obj, prefix = '', result = {}) {
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      const newKey = prefix ? `${prefix}.${key}` : key;
      
      if (typeof obj[key] === 'object' && obj[key] !== null && !Array.isArray(obj[key])) {
        // Recursively flatten nested object
        flattenObject(obj[key], newKey, result);
      } else {
        // Add primitive value or array
        result[newKey] = obj[key];
      }
    }
  }
  
  return result;
}

// Test cases
const nested = {
  a: 1,
  b: {
    c: 2,
    d: {
      e: 3,
      f: 4
    }
  },
  g: [1, 2, 3],
  h: null
};

console.log(flattenObject(nested));
// { a: 1, 'b.c': 2, 'b.d.e': 3, 'b.d.f': 4, g: [1,2,3], h: null }

// Alternative: Handle arrays differently
function flattenWithArrays(obj, prefix = '', result = {}) {
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      const newKey = prefix ? `${prefix}.${key}` : key;
      const value = obj[key];
      
      if (Array.isArray(value)) {
        value.forEach((item, index) => {
          const arrayKey = `${newKey}[${index}]`;
          if (typeof item === 'object' && item !== null) {
            flattenWithArrays(item, arrayKey, result);
          } else {
            result[arrayKey] = item;
          }
        });
      } else if (typeof value === 'object' && value !== null) {
        flattenWithArrays(value, newKey, result);
      } else {
        result[newKey] = value;
      }
    }
  }
  
  return result;
}
```

**📊 Talk Through Your Approach:**
"I use recursion to traverse the object tree. For each property, I build a new key by concatenating the parent path with a dot. If the value is an object, I recurse deeper. If it's a primitive, I add it to the result object. I handle arrays separately - treating them as leaf values unless the interviewer wants them flattened too."

**🔄 Follow-up Questions to Expect:**
1. **"How would you unflatten this back to nested?"**
   ```javascript
   function unflattenObject(flat) {
     const result = {};
     for (let key in flat) {
       const keys = key.split('.');
       keys.reduce((acc, k, i) => {
         if (i === keys.length - 1) {
           acc[k] = flat[key];
         } else {
           acc[k] = acc[k] || {};
         }
         return acc[k];
       }, result);
     }
     return result;
   }
   ```

2. **"What if there are circular references?"**
   - "I'd use a WeakSet to track visited objects and skip them."

3. **"Can you make this work iteratively instead of recursively?"**
   - "Yes, I'd use a stack with `[object, prefix]` pairs."

**🚫 Red Flags to Avoid:**
- Not handling null (typeof null === 'object')
- Not using hasOwnProperty (could get prototype properties)
- Creating excessive intermediate objects
- Not considering array handling

**💼 Pro Tips:**
- Mention libraries like flat/unflatten npm packages
- Discuss use cases: API data transformation, form handling
- Consider performance for very deep objects

---

### Question 4: Group Objects by Property (Mid Level)

**⏱ Time Expected:** 10-15 minutes  
**🎯 What They're Testing:** Array/object manipulation, functional programming concepts

**Question:**  
Write a function `groupBy(array, key)` that groups an array of objects by a specified property.

```javascript
const users = [
  { name: "Alice", age: 25, city: "NYC" },
  { name: "Bob", age: 30, city: "LA" },
  { name: "Charlie", age: 25, city: "NYC" }
];
groupBy(users, 'age');
// { 25: [{name: "Alice", ...}, {name: "Charlie", ...}], 30: [{name: "Bob", ...}] }
```

**💡 What Interviewers Want to See:**
- Efficient use of reduce or loop
- Handling of missing keys
- Clean, functional approach

**✅ Strong Answer Template:**

```javascript
function groupBy(array, key) {
  return array.reduce((result, item) => {
    // Get the grouping key value
    const groupKey = item[key];
    
    // Initialize array if doesn't exist
    if (!result[groupKey]) {
      result[groupKey] = [];
    }
    
    // Add item to group
    result[groupKey].push(item);
    
    return result;
  }, {});
}

// Alternative: handle nested keys
function groupByNested(array, keyPath) {
  return array.reduce((result, item) => {
    // Support nested keys like 'address.city'
    const keys = keyPath.split('.');
    const groupKey = keys.reduce((obj, k) => obj?.[k], item);
    
    if (!result[groupKey]) {
      result[groupKey] = [];
    }
    
    result[groupKey].push(item);
    return result;
  }, {});
}

// Alternative: handle function as key
function groupByFunction(array, keyOrFn) {
  return array.reduce((result, item) => {
    const groupKey = typeof keyOrFn === 'function' 
      ? keyOrFn(item) 
      : item[keyOrFn];
    
    result[groupKey] = result[groupKey] || [];
    result[groupKey].push(item);
    return result;
  }, {});
}

// Tests
const users = [
  { name: "Alice", age: 25, city: "NYC" },
  { name: "Bob", age: 30, city: "LA" },
  { name: "Charlie", age: 25, city: "NYC" }
];

console.log(groupBy(users, 'age'));
console.log(groupBy(users, 'city'));

// With function
console.log(groupByFunction(users, user => user.age >= 30 ? 'senior' : 'junior'));
```

**📊 Talk Through Your Approach:**
"I use the reduce method to accumulate results into an object. For each item, I extract the grouping key value, initialize an array for that key if it doesn't exist, and push the item into that group. The reduce pattern is perfect here because we're transforming an array into a grouped object."

**🔄 Follow-up Questions to Expect:**
1. **"What if the key doesn't exist on some objects?"**
   ```javascript
   const groupKey = item[key] ?? 'undefined';
   ```

2. **"How would you count items instead of collecting them?"**
   ```javascript
   result[groupKey] = (result[groupKey] || 0) + 1;
   ```

3. **"Can this work with multiple keys?"**
   ```javascript
   function groupByMultiple(array, keys) {
     return array.reduce((result, item) => {
       const groupKey = keys.map(k => item[k]).join('-');
       result[groupKey] = result[groupKey] || [];
       result[groupKey].push(item);
       return result;
     }, {});
   }
   ```

**🚫 Red Flags to Avoid:**
- Using inefficient nested loops
- Not initializing the array before pushing
- Mutating the original array
- Not considering undefined/null values

**💼 Pro Tips:**
- Mention Lodash's `_.groupBy()` for production
- Discuss Map vs Object for grouping (Map allows non-string keys)
- Consider using Object.groupBy() (new ES2024 feature)

---

### Question 5: Deep Merge Objects (Senior Level)

**⏱ Time Expected:** 20-25 minutes  
**🎯 What They're Testing:** Complex recursion, edge case handling, design decisions

**Question:**  
Write a function `deepMerge(target, source)` that deeply merges two objects. Arrays should be concatenated, and nested objects should be recursively merged.

**💡 What Interviewers Want to See:**
- Handling of edge cases (null, arrays, different types)
- Recursive strategy
- Consideration of mutation vs immutability
- Performance awareness

**✅ Strong Answer Template:**

```javascript
function deepMerge(target, source) {
  // Handle null/undefined
  if (source == null || target == null) {
    return target ?? source;
  }
  
  // If types don't match, source overwrites target
  if (typeof target !== typeof source) {
    return source;
  }
  
  // Handle arrays
  if (Array.isArray(target) && Array.isArray(source)) {
    return [...target, ...source];
  }
  
  // Handle non-object types
  if (typeof target !== 'object') {
    return source;
  }
  
  // Deep merge objects
  const result = { ...target };
  
  for (let key in source) {
    if (source.hasOwnProperty(key)) {
      if (typeof source[key] === 'object' && source[key] !== null && !Array.isArray(source[key])) {
        // Recursively merge nested objects
        result[key] = deepMerge(target[key], source[key]);
      } else {
        // Direct assignment for primitives and arrays
        result[key] = source[key];
      }
    }
  }
  
  return result;
}

// Test cases
const obj1 = {
  a: 1,
  b: { c: 2, d: 3 },
  e: [1, 2],
  f: "hello"
};

const obj2 = {
  b: { c: 4, e: 5 },
  e: [3, 4],
  f: "world",
  g: 6
};

console.log(deepMerge(obj1, obj2));
// {
//   a: 1,
//   b: { c: 4, d: 3, e: 5 },
//   e: [1, 2, 3, 4],
//   f: "world",
//   g: 6
// }

// Immutable version (doesn't modify original)
function deepMergeImmutable(...objects) {
  if (objects.length === 0) return {};
  if (objects.length === 1) return objects[0];
  
  return objects.reduce((acc, obj) => {
    return deepMerge(acc, obj);
  });
}
```

**📊 Talk Through Your Approach:**
"I start by handling edge cases - null values and type mismatches. For arrays, I concatenate them. For objects, I create a new result object to avoid mutating the original. Then I iterate through the source object's properties. If a property is a nested object, I recursively merge it. Otherwise, I directly assign the value. This ensures deep merging while maintaining immutability."

**🔄 Follow-up Questions to Expect:**
1. **"How would you handle circular references?"**
   ```javascript
   function deepMergeWithCircular(target, source, seen = new WeakMap()) {
     if (seen.has(source)) return seen.get(source);
     
     const result = { ...target };
     seen.set(source, result);
     
     // ... rest of merge logic
     
     return result;
   }
   ```

2. **"What if you wanted to merge arrays by index instead of concatenating?"**
   ```javascript
   if (Array.isArray(target) && Array.isArray(source)) {
     const maxLen = Math.max(target.length, source.length);
     return Array.from({ length: maxLen }, (_, i) => 
       deepMerge(target[i], source[i])
     );
   }
   ```

3. **"How would you make array merging strategy configurable?"**
   - "Add an options parameter: `{ arrayMerge: 'concat' | 'replace' | 'merge' }`"

**🚫 Red Flags to Avoid:**
- Mutating the original objects
- Not handling null (typeof null === 'object')
- Shallow copying nested structures
- Not considering performance with large objects
- Not handling arrays at all

**💼 Pro Tips:**