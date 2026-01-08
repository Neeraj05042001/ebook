# Day 2: Variables and Data Types in JavaScript

## 🏁 Chapter Overview

Welcome to Day 2 of your JavaScript Mastery journey! Today, we're diving deep into the foundation of every JavaScript program: **variables and data types**. Think of variables as the building blocks of your code—they're the containers that hold all the information your programs need to function.

### What You'll Master Today

- ✅ **Understand what variables are** and how they work in memory (Stack vs Heap)
- ✅ **Master all three variable declaration methods**: `var`, `let`, and `const` (and when to use each)
- ✅ **Identify and work with all 7 primitive data types** and 3 non-primitive types in JavaScript
- ✅ **Predict how JavaScript handles type coercion** in different operations
- ✅ **Debug common variable-related errors** that trip up beginners
- ✅ **Apply variables and data types** to solve real-world coding problems
- ✅ **Recognize memory management patterns** that affect performance

### Prerequisites Checklist

Before diving in, make sure you have:

- [ ] Completed Day 1: Introduction to JavaScript
- [ ] A code editor (VS Code recommended) installed and ready
- [ ] A browser with DevTools (Chrome or Firefox)
- [ ] Basic understanding of running JavaScript code (console or .js files)
- [ ] 60-90 minutes of focused learning time

### Why This Matters

Variables and data types aren't just academic concepts—they're the DNA of every JavaScript application you'll ever build. Here's why mastering them matters:

**For Frontend Development**: Every user interaction, every form input, every animation state is stored in variables. Understanding data types prevents bugs like adding "5" + 3 and getting "53" instead of 8.

**For Interviews**: 90% of coding interviews test your understanding of how JavaScript handles variables, scope, and type coercion. Questions about `var` vs `let`, primitive vs reference types, and type conversion are interview staples.

**For Your Career**: Writing clean, bug-free code starts with choosing the right variable declaration and understanding how JavaScript stores and manipulates data. This knowledge separates junior developers from confident professionals.

---

## ⚡ Quick Revision Summary

### Key Concepts at a Glance

| Concept | Quick Definition | Example |
|---------|-----------------|---------|
| **Variable** | Named container that stores data | `let age = 25;` |
| **Primitive Type** | Simple, immutable data stored in stack | `"hello"`, `42`, `true` |
| **Reference Type** | Complex data stored in heap | `{name: "John"}`, `[1,2,3]` |
| **Type Coercion** | Automatic type conversion by JavaScript | `"5" + 3 = "53"` |
| **Stack Memory** | Fast, fixed-size storage for primitives | Where `let x = 10` lives |
| **Heap Memory** | Dynamic storage for objects/arrays | Where `{name: "Jo"}` lives |

### Variable Declaration Syntax Reference

```javascript
// var - function-scoped, can be redeclared
var oldStyle = "avoid in modern code";

// let - block-scoped, can be reassigned
let counter = 0;
counter = 1; // ✅ Allowed

// const - block-scoped, cannot be reassigned
const PI = 3.14159;
// PI = 3; // ❌ Error!
```

### The 7 Primitive Data Types

```javascript
let num = 42;                    // Number
let str = "JavaScript";          // String
let bool = true;                 // Boolean
let nothing = null;              // Null
let notDefined;                  // Undefined
let unique = Symbol('id');       // Symbol
let huge = 9007199254740991n;   // BigInt
```

### When to Use What

| Use `const` when... | Use `let` when... | Use `var` when... |
|---------------------|-------------------|-------------------|
| Value won't change | Value will change | Never (legacy code only) |
| Configuration values | Loop counters | Maintaining old codebases |
| Function declarations | Accumulator variables | — |
| Object/Array references | Conditional assignments | — |

### Important Terminology

- **Declaration**: Creating a variable (`let x;`)
- **Initialization**: Assigning first value (`let x = 5;`)
- **Assignment**: Changing value (`x = 10;`)
- **Scope**: Where a variable can be accessed
- **Hoisting**: JavaScript's behavior of moving declarations to the top
- **Temporal Dead Zone (TDZ)**: Period where `let`/`const` exist but can't be accessed

---

## 🧩 Predict the Output

Test your understanding by predicting what each code snippet will output. Think carefully about type coercion, variable behavior, and JavaScript's quirks!

### Challenge 1: Basic Type Coercion
```javascript
console.log(5 + "5");
console.log(10 - "3");
console.log("10" - "3");
console.log(true + 1);
```

**🤔 What do you think?** Before scrolling, write down your predictions.

<details>
<summary><strong>💡 Explanation</strong></summary>

**Output:**
```
"55"
7
7
2
```

**Why:**
- `5 + "5"` → When `+` is used with a string, JavaScript converts the number to string and concatenates: `"5" + "5" = "55"`
- `10 - "3"` → The `-` operator only works with numbers, so JavaScript converts `"3"` to `3`, then calculates: `10 - 3 = 7`
- `"10" - "3"` → Both strings are converted to numbers: `10 - 3 = 7`
- `true + 1` → Boolean `true` is coerced to `1`, so: `1 + 1 = 2`

**Key Takeaway**: The `+` operator is special—it concatenates when either operand is a string. All other arithmetic operators force numeric conversion.

</details>

---

### Challenge 2: Variable Reassignment
```javascript
const person = { name: "Alice" };
person.name = "Bob";
console.log(person.name);

person = { name: "Charlie" };
```

**🤔 What happens here?**

<details>
<summary><strong>💡 Explanation</strong></summary>

**Output:**
```
"Bob"
TypeError: Assignment to constant variable
```

**Why:**
- `const` prevents **reassigning** the variable itself, but doesn't make the object immutable
- `person.name = "Bob"` modifies the object's property—this is allowed!
- `person = { name: "Charlie" }` tries to reassign the entire variable—this throws an error

**Key Takeaway**: `const` protects the reference, not the object's contents. Think of it as a locked box—you can't replace the box, but you can rearrange what's inside.

</details>

---

### Challenge 3: typeof Surprises
```javascript
console.log(typeof null);
console.log(typeof [1, 2, 3]);
console.log(typeof function(){});
console.log(typeof undefined);
```

**🤔 Predict each type**

<details>
<summary><strong>💡 Explanation</strong></summary>

**Output:**
```
"object"
"object"
"function"
"undefined"
```

**Why:**
- `typeof null` → Returns `"object"` (this is a historical JavaScript bug that was never fixed!)
- `typeof [1, 2, 3]` → Arrays are objects in JavaScript, so returns `"object"`
- `typeof function(){}` → Functions get their own type: `"function"`
- `typeof undefined` → Correctly returns `"undefined"`

**Key Takeaway**: `typeof` has quirks. To check if something is specifically an array, use `Array.isArray()` instead.

</details>

---

### Challenge 4: Variable Hoisting
```javascript
console.log(a);
console.log(b);
console.log(c);

var a = 10;
let b = 20;
const c = 30;
```

**🤔 What gets logged? Any errors?**

<details>
<summary><strong>💡 Explanation</strong></summary>

**Output:**
```
undefined
ReferenceError: Cannot access 'b' before initialization
(c never executes)
```

**Why:**
- `var a` is hoisted—JavaScript moves the declaration to the top but not the assignment. So `console.log(a)` sees `undefined`
- `let b` and `const c` are hoisted too, but they're in the "Temporal Dead Zone" until the line where they're declared
- Trying to access them before initialization throws a `ReferenceError`

**Key Takeaway**: Always declare variables before using them. `let` and `const` protect you from accidental early access that `var` allows.

</details>

---

### Challenge 5: Copying Primitives vs Objects
```javascript
// Primitives
let x = 10;
let y = x;
x = 20;
console.log(y);

// Objects
let obj1 = { value: 10 };
let obj2 = obj1;
obj1.value = 20;
console.log(obj2.value);
```

**🤔 Why do these behave differently?**

<details>
<summary><strong>💡 Explanation</strong></summary>

**Output:**
```
10
20
```

**Why:**
- **Primitives** (`x = 10`): When you assign `y = x`, JavaScript copies the actual value. They're independent.
- **Objects** (`obj1 = {...}`): When you assign `obj2 = obj1`, JavaScript copies the **reference** (memory address), not the object itself. Both variables point to the same object in memory.

**Visualization:**
```
Primitives:          Objects:
x: 20  y: 10        obj1 ───┐
(separate values)            ├──→ { value: 20 }
                     obj2 ───┘
                    (same object!)
```

**Key Takeaway**: Primitives are copied by value, objects by reference. To create a true copy of an object, use spread operator: `let obj2 = {...obj1};`

</details>

---

### Challenge 6: The Triple Equals Trap
```javascript
console.log(5 == "5");
console.log(5 === "5");
console.log(null == undefined);
console.log(null === undefined);
```

**🤔 Predict true or false for each**

<details>
<summary><strong>💡 Explanation</strong></summary>

**Output:**
```
true
false
true
false
```

**Why:**
- `==` (loose equality): Performs type coercion before comparison
  - `5 == "5"` → Converts `"5"` to `5`, then compares: `true`
  - `null == undefined` → Special rule: they're considered equal with `==`
  
- `===` (strict equality): No type coercion—types must match
  - `5 === "5"` → Number vs String, different types: `false`
  - `null === undefined` → Different types: `false`

**Key Takeaway**: Always use `===` and `!==` unless you have a specific reason to use loose equality. It prevents unexpected type coercion bugs.

</details>

---

### Challenge 7: String + Number Combinations
```javascript
console.log("5" + 3 + 2);
console.log(5 + 3 + "2");
console.log("5" - 3 + 2);
```

**🤔 Order matters! What's the result of each?**

<details>
<summary><strong>💡 Explanation</strong></summary>

**Output:**
```
"532"
"82"
4
```

**Why:**
- `"5" + 3 + 2` → Left to right: `"5" + 3 = "53"`, then `"53" + 2 = "532"` (string concatenation)
- `5 + 3 + "2"` → Left to right: `5 + 3 = 8`, then `8 + "2" = "82"` (number becomes string)
- `"5" - 3 + 2` → `-` forces conversion: `5 - 3 = 2`, then `2 + 2 = 4` (all numbers)

**Key Takeaway**: JavaScript evaluates left to right. Once a string enters the equation with `+`, everything after becomes concatenation. Other operators (`-`, `*`, `/`) always convert to numbers.

</details>

---

### Challenge 8: Undefined vs Null
```javascript
let a;
let b = null;

console.log(a);
console.log(b);
console.log(typeof a);
console.log(typeof b);
console.log(a == b);
console.log(a === b);
```

**🤔 What's the difference between undefined and null?**

<details>
<summary><strong>💡 Explanation</strong></summary>

**Output:**
```
undefined
null
"undefined"
"object"
true
false
```

**Why:**
- `undefined` = JavaScript's default for uninitialized variables
- `null` = Intentional absence of value (you explicitly set it)
- `typeof null` returns `"object"` (the famous JavaScript bug)
- `a == b` → `true` because `==` considers them equivalent
- `a === b` → `false` because they're different types

**Key Takeaway**: Use `null` when you want to explicitly say "this has no value." `undefined` means "this hasn't been set yet."

</details>

---

### Challenge 9: Boolean Coercion
```javascript
console.log(Boolean(""));
console.log(Boolean("0"));
console.log(Boolean(0));
console.log(Boolean([]));
console.log(Boolean({}));
```

**🤔 Which values are truthy and which are falsy?**

<details>
<summary><strong>💡 Explanation</strong></summary>

**Output:**
```
false
true
false
true
true
```

**Why:**
JavaScript has 8 falsy values: `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined`, `NaN`. Everything else is truthy!

- `Boolean("")` → Empty string is falsy: `false`
- `Boolean("0")` → Non-empty string (even "0") is truthy: `true`
- `Boolean(0)` → Zero is falsy: `false`
- `Boolean([])` → Empty array is an object, objects are truthy: `true`
- `Boolean({})` → Empty object is truthy: `true`

**Key Takeaway**: Memorize the 8 falsy values. This is crucial for conditionals and is a common interview question.

</details>

---

### Challenge 10: Template Literal Math
```javascript
let x = 5;
let y = 10;

console.log(`${x} + ${y}`);
console.log(`${x + y}`);
console.log(x + y);
console.log("x + y");
```

**🤔 What's the output of each line?**

<details>
<summary><strong>💡 Explanation</strong></summary>

**Output:**
```
"5 + 10"
"15"
15
"x + y"
```

**Why:**
- `` `${x} + ${y}` `` → Evaluates variables separately, but the `+` is just text: `"5 + 10"`
- `` `${x + y}` `` → Evaluates the expression inside `${}`: `5 + 10 = 15`, then converts to string: `"15"`
- `x + y` → No template literal, pure math: `15` (number)
- `"x + y"` → Regular string, no variable substitution: `"x + y"`

**Key Takeaway**: Template literals (backticks) evaluate expressions inside `${}`. Regular strings don't process variables at all.

</details>

---

## 🧮 Practice Problems

Now let's apply what you've learned! These problems progressively increase in difficulty and cover real-world scenarios.

---

### Problem 1: Personal Information Card 🟢 Beginner

**Task:** Create variables to store your personal information and display them in a formatted way.

**Requirements:**
- Store: first name, last name, age, city, and whether you're a student
- Use appropriate data types for each
- Display them using template literals

**Sample Output:**
```
Name: John Doe
Age: 22 years old
City: New York
Student Status: Yes
```

**Before You Code:**
Think about which data type fits each piece of information. Should "age" be a string or number? What about student status?

**Hints:**
- Use `const` for values that won't change during execution
- Template literals make formatting easier: `` `Age: ${age} years old` ``
- Boolean values can be displayed with ternary operators

<details>
<summary><strong>✅ Solution</strong></summary>

```javascript
// Declare variables with appropriate types
const firstName = "John";
const lastName = "Doe";
const age = 22;
const city = "New York";
const isStudent = true;

// Display formatted information
console.log(`Name: ${firstName} ${lastName}`);
console.log(`Age: ${age} years old`);
console.log(`City: ${city}`);
console.log(`Student Status: ${isStudent ? 'Yes' : 'No'}`);

// Alternative: Create an object
const person = {
    firstName: "John",
    lastName: "Doe",
    age: 22,
    city: "New York",
    isStudent: true
};

console.log(`
╔════════════════════════════╗
║   PERSONAL INFORMATION     ║
╠════════════════════════════╣
║ Name: ${person.firstName} ${person.lastName}
║ Age: ${person.age} years old
║ City: ${person.city}
║ Student: ${person.isStudent ? 'Yes' : 'No'}
╚════════════════════════════╝
`);
```

**Common Mistakes:**
- ❌ Using strings for age: `const age = "22"` makes math operations fail
- ❌ Not using `const` when values don't change
- ❌ Using `+` for concatenation instead of template literals (harder to read)

**What You Learned:**
- Choosing appropriate data types for different kinds of information
- Using template literals for clean string formatting
- The difference between primitive types in practical use

**Extension Challenge:** 
Add a birth year variable and calculate the age automatically. Also, add validation—what if age is negative or over 150?

</details>

---

### Problem 2: Variable Swap Challenge 🟢 Beginner

**Task:** Swap the values of two variables **without using a third variable**.

**Sample Input:**
```javascript
let a = 10;
let b = 20;
```

**Expected Output:**
```
Before swap: a = 10, b = 20
After swap: a = 20, b = 10
```

**Before You Code:**
Think about mathematical operations or ES6 features that could help you exchange values without temporary storage.

**Hints:**
- Method 1: Use arithmetic operations (addition/subtraction)
- Method 2: Use ES6 destructuring (modern approach)
- Method 3: XOR bitwise operator (advanced, only for integers)

<details>
<summary><strong>✅ Solution</strong></summary>

```javascript
// Method 1: Using Destructuring (Most modern and readable)
let a = 10;
let b = 20;

console.log(`Before swap: a = ${a}, b = ${b}`);

[a, b] = [b, a]; // Swap using array destructuring

console.log(`After swap: a = ${a}, b = ${b}`);

// Method 2: Using Arithmetic Operations
let x = 15;
let y = 25;

console.log(`Before swap: x = ${x}, y = ${y}`);

x = x + y;  // x = 15 + 25 = 40
y = x - y;  // y = 40 - 25 = 15
x = x - y;  // x = 40 - 15 = 25

console.log(`After swap: x = ${x}, y = ${y}`);

// Method 3: Using XOR (only for integers)
let m = 5;
let n = 8;

console.log(`Before swap: m = ${m}, n = ${n}`);

m = m ^ n;  // XOR operation
n = m ^ n;
m = m ^ n;

console.log(`After swap: m = ${m}, n = ${n}`);
```

**Common Mistakes:**
- ❌ Using a third variable: `let temp = a; a = b; b = temp;` (defeats the purpose)
- ❌ Arithmetic overflow with very large numbers
- ❌ Using XOR with non-integers (produces unexpected results)

**What You Learned:**
- ES6 destructuring is the cleanest solution for swapping
- Mathematical operations can manipulate values cleverly
- Understanding that variables can be reassigned based on their own values

**Extension Challenge:**
Swap three variables in a circular pattern (a→b, b→c, c→a) using only destructuring. Then try swapping elements in an array at specific indices.

</details>

---

### Problem 3: Type Checker Tool 🟡 Intermediate

**Task:** Create a function that takes any value and returns detailed type information about it.

**Requirements:**
- Identify the data type
- Check if it's primitive or reference type
- For numbers, check if it's integer or float
- For strings, show the length
- For objects/arrays, show the number of properties/elements

**Sample Input:**
```javascript
checkType(42);
checkType("Hello");
checkType([1, 2, 3]);
checkType({ name: "Alice" });
```

**Expected Output:**
```
Type: number
Category: Primitive
Number type: Integer
---
Type: string
Category: Primitive
Length: 5 characters
---
Type: object (Array)
Category: Reference
Elements: 3
---
Type: object
Category: Reference
Properties: 1
```

**Before You Code:**
How can you distinguish between arrays and objects? What about checking if a number is an integer?

**Hints:**
- Use `typeof` for basic type checking
- `Array.isArray()` helps identify arrays
- `Number.isInteger()` checks for whole numbers
- Object.keys() gives you property names

<details>
<summary><strong>✅ Solution</strong></summary>

```javascript
function checkType(value) {
    // Get basic type
    let type = typeof value;
    let category = 'Primitive';
    let details = '';
    
    // Check if it's a reference type
    if (type === 'object' || type === 'function') {
        category = 'Reference';
        
        // Handle null (typeof null is 'object')
        if (value === null) {
            type = 'null';
            category = 'Primitive';
        }
        // Check if it's an array
        else if (Array.isArray(value)) {
            type = 'object (Array)';
            details = `Elements: ${value.length}`;
        }
        // Regular object
        else if (type === 'object') {
            details = `Properties: ${Object.keys(value).length}`;
        }
    }
    
    // Add specific details for primitives
    if (type === 'number') {
        details = Number.isInteger(value) ? 'Number type: Integer' : 'Number type: Float';
    } else if (type === 'string') {
        details = `Length: ${value.length} characters`;
    } else if (type === 'boolean') {
        details = `Value: ${value}`;
    }
    
    // Display results
    console.log(`Type: ${type}`);
    console.log(`Category: ${category}`);
    if (details) console.log(details);
    console.log('---');
}

// Test cases
checkType(42);                    // Integer
checkType(3.14);                  // Float
checkType("Hello");               // String
checkType(true);                  // Boolean
checkType([1, 2, 3]);            // Array
checkType({ name: "Alice" });    // Object
checkType(null);                  // Null
checkType(undefined);             // Undefined
checkType(function() {});         // Function
checkType(Symbol('id'));          // Symbol

// Bonus: Enhanced version with colors (for browser console)
function checkTypeEnhanced(value) {
    const info = {
        type: typeof value,
        category: 'Primitive',
        isPrimitive: true,
        isNull: value === null,
        isArray: Array.isArray(value),
        details: {}
    };
    
    if (info.type === 'object' || info.type === 'function') {
        info.category = 'Reference';
        info.isPrimitive = false;
        
        if (info.isNull) {
            info.category = 'Primitive';
            info.isPrimitive = true;
        } else if (info.isArray) {
            info.details.length = value.length;
            info.details.isEmpty = value.length === 0;
        } else if (info.type === 'object') {
            info.details.properties = Object.keys(value);
            info.details.propertyCount = Object.keys(value).length;
        }
    }
    
    if (info.type === 'number') {
        info.details.isInteger = Number.isInteger(value);
        info.details.isFinite = isFinite(value);
        info.details.isNaN = isNaN(value);
    } else if (info.type === 'string') {
        info.details.length = value.length;
        info.details.isEmpty = value.length === 0;
    }
    
    console.log('%cType Analysis:', 'font-weight: bold; font-size: 14px;');
    console.table(info);
    
    return info;
}
```

**Common Mistakes:**
- ❌ Forgetting that `typeof null` returns `"object"` (needs special handling)
- ❌ Not distinguishing between arrays and objects
- ❌ Using `typeof` to check if something is an array (use `Array.isArray()`)
- ❌ Not handling edge cases like `NaN` or `Infinity`

**What You Learned:**
- How to properly check data types in JavaScript
- The difference between `typeof` and specific type-checking methods
- How to extract metadata from different data types
- Building developer tools for debugging

**Extension Challenge:**
Enhance the function to handle nested objects and arrays. Show the depth of nesting and create a tree visualization. Also add checks for special numbers like `NaN`, `Infinity`, and `-0`.

</details>

---

### Problem 4: Shopping Cart Calculator 🟡 Intermediate

**Task:** Build a shopping cart system that calculates totals with discounts and tax.

**Requirements:**
- Store items with name, price, and quantity
- Calculate subtotal (price × quantity for all items)
- Apply a 10% discount if subtotal > $100
- Add 8% tax to the final amount
- Display an itemized receipt

**Sample Data:**
```javascript
// Item format: { name: string, price: number, quantity: number }
```

**Expected Output:**
```
========== RECEIPT ==========
Laptop       $999.99 x 1 = $999.99
Mouse        $25.50  x 2 = $51.00
Keyboard     $75.00  x 1 = $75.00
----------------------------
Subtotal:              $1125.99
Discount (10%):        -$112.60
Tax (8%):              $81.07
============================
TOTAL:                 $1094.46
```

**Before You Code:**
Think about the order of operations: Do you apply tax before or after discount? How will you format currency?

**Hints:**
- Use an array of objects to store items
- `.toFixed(2)` formats numbers to 2 decimal places
- Calculate in this order: Subtotal → Discount → Tax → Total
- Use template literals for formatting

<details>
<summary><strong>✅ Solution</strong></summary>

```javascript
// Shopping Cart Calculator
function calculateCart(items) {
    // Calculate subtotal
    let subtotal = 0;
    console.log('========== RECEIPT ==========');
    
    items.forEach(item => {
        const itemTotal = item.price * item.quantity;
        subtotal += itemTotal;
        
        // Format item line
        const name = item.name.padEnd(12);
        const price = `$${item.price.toFixed(2)}`;
        const qty = `x ${item.quantity}`;
        const total = `$${itemTotal.toFixed(2)}`;
        
        console.log(`${name} ${price} ${qty} = ${total}`);
    });
    
    console.log('----------------------------');
    console.log(`Subtotal:              $${subtotal.toFixed(2)}`);
    
    // Apply discount if applicable
    let discount = 0;
    if (subtotal > 100) {
        discount = subtotal * 0.10;
        console.log(`Discount (10%):        -$${discount.toFixed(2)}`);
    }
    
    // Calculate amount after discount
    const afterDiscount = subtotal - discount;
    
    // Apply tax
    const tax = afterDiscount * 0.08;
    console.log(`Tax (8%):              $${tax.toFixed(2)}`);
    
    // Calculate final total
    const total = afterDiscount + tax;
    
    console.log('============================');
    console.log(`TOTAL:                 $${total.toFixed(2)}`);
    
    return {
        subtotal,
        discount,
        tax,
        total,
        itemCount: items.length,
        totalItems: items.reduce((sum, item) => sum + item.quantity, 0)
    };
}

// Test with sample data
const cartItems = [
    { name: 'Laptop', price: 999.99, quantity: 1 },
    { name: 'Mouse', price: 25.50, quantity: 2 },
    { name: 'Keyboard', price: 75.00, quantity: 1 }
];

const receipt = calculateCart(cartItems);

console.log('\n📊 Summary:');
console.log(`Total Items: ${receipt.totalItems}`);
console.log(`You saved: $${receipt.discount.toFixed(2)}`);

// Enhanced version with discount tiers
function calculateCartAdvanced(items, customerType = 'regular') {
    const DISCOUNT_RULES = {
        regular: { threshold: 100, rate: 0.10 },
        premium: { threshold: 50, rate: 0.15 },
        vip: { threshold: 0, rate: 0.20 }
    };
    
    const TAX_RATE = 0.08;
    
    // Calculate subtotal
    const subtotal = items.reduce((sum, item) => {
        return sum + (item.price * item.quantity);
    }, 0);
    
    // Get discount rules for customer type
    const discountRule = DISCOUNT_RULES[customerType];
    let discountRate = 0;
    
    if (subtotal > discountRule.threshold) {
        discountRate = discountRule.rate;
    }
    
    const discount = subtotal * discountRate;
    const afterDiscount = subtotal - discount;
    const tax = afterDiscount * TAX_RATE;
    const total = afterDiscount + tax;
    
    // Build receipt
    console.log('\n╔════════════════════════════════╗');
    console.log('║       SHOPPING RECEIPT         ║');
    console.log('╠════════════════════════════════╣');
    console.log(`║ Customer Type: ${customerType.toUpperCase().padEnd(16)}║`);
    console.log('╠════════════════════════════════╣');
    
    items.forEach(item => {
        const line = `${item.name.padEnd(12)} $${item.price.toFixed(2).padStart(7)} x${item.quantity}`;
        console.log(`║ ${line.padEnd(30)}║`);
    });
    
    console.log('╠════════════════════════════════╣');
    console.log(`║ Subtotal:      $${subtotal.toFixed(2).padStart(15)}║`);
    
    if (discount > 0) {
        console.log```javascript
        console.log(`║ Discount (${(discountRate * 100).toFixed(0)}%): -$${discount.toFixed(2).padStart(14)}║`);
    }
    
    console.log(`║ Tax (8%):      $${tax.toFixed(2).padStart(15)}║`);
    console.log('╠════════════════════════════════╣');
    console.log(`║ TOTAL:         $${total.toFixed(2).padStart(15)}║`);
    console.log('╚════════════════════════════════╝');
    
    return {
        subtotal,
        discount,
        discountRate,
        tax,
        total,
        customerType,
        savings: discount
    };
}

// Test with different customer types
const testCart = [
    { name: 'Laptop', price: 999.99, quantity: 1 },
    { name: 'Mouse', price: 25.50, quantity: 2 },
    { name: 'Keyboard', price: 75.00, quantity: 1 }
];

console.log('\n--- Regular Customer ---');
calculateCartAdvanced(testCart, 'regular');

console.log('\n--- Premium Customer ---');
calculateCartAdvanced(testCart, 'premium');

console.log('\n--- VIP Customer ---');
calculateCartAdvanced(testCart, 'vip');
```

**Common Mistakes:**
- ❌ Applying tax before discount (should be: subtotal → discount → tax)
- ❌ Not using `.toFixed(2)` causing floating-point precision errors
- ❌ String concatenation instead of proper number addition
- ❌ Not considering the case when discount doesn't apply

**What You Learned:**
- Working with arrays of objects (real-world data structure)
- Order of operations in calculations
- String formatting and padding for alignment
- Conditional logic based on thresholds
- Reducing arrays to single values

**Extension Challenge:**
Add a "coupon code" system where users can enter codes like "SAVE20" for additional discounts. Handle invalid codes gracefully. Also, add a "free shipping" threshold at $150.

</details>

---

### Problem 5: Temperature Converter 🟡 Intermediate

**Task:** Create a temperature converter that handles Celsius, Fahrenheit, and Kelvin conversions with validation.

**Requirements:**
- Convert between all three temperature scales
- Validate that temperatures are physically possible (Kelvin ≥ 0)
- Display results with appropriate precision
- Show which scale makes most sense for the temperature

**Sample Input/Output:**
```javascript
convertTemperature(100, 'C', 'F')  // → "212.00°F (Water boiling point)"
convertTemperature(-300, 'C', 'K') // → "Invalid: Temperature below absolute zero"
```

**Before You Code:**
What's the relationship between these scales? Kelvin can't be negative (absolute zero). Should you convert directly between each pair, or use an intermediate scale?

**Hints:**
- Formulas: C = K - 273.15, F = C × 9/5 + 32
- Convert everything to Celsius first, then to target scale
- Absolute zero is -273.15°C or 0 K
- Add context for common temperatures (freezing, boiling, etc.)

<details>
<summary><strong>✅ Solution</strong></summary>

```javascript
function convertTemperature(value, fromScale, toScale) {
    // Normalize scale inputs (accept 'C', 'c', 'Celsius', etc.)
    fromScale = fromScale.toUpperCase().charAt(0);
    toScale = toScale.toUpperCase().charAt(0);
    
    // Define absolute zero in different scales
    const ABSOLUTE_ZERO = {
        'C': -273.15,
        'F': -459.67,
        'K': 0
    };
    
    // Validate input temperature
    if (value < ABSOLUTE_ZERO[fromScale]) {
        return `❌ Invalid: Temperature below absolute zero (${ABSOLUTE_ZERO[fromScale]}°${fromScale})`;
    }
    
    // Same scale conversion
    if (fromScale === toScale) {
        return formatResult(value, toScale, value);
    }
    
    // Step 1: Convert to Celsius (intermediate)
    let celsius;
    switch (fromScale) {
        case 'C':
            celsius = value;
            break;
        case 'F':
            celsius = (value - 32) * 5 / 9;
            break;
        case 'K':
            celsius = value - 273.15;
            break;
        default:
            return '❌ Invalid scale. Use C, F, or K';
    }
    
    // Step 2: Convert from Celsius to target scale
    let result;
    switch (toScale) {
        case 'C':
            result = celsius;
            break;
        case 'F':
            result = celsius * 9 / 5 + 32;
            break;
        case 'K':
            result = celsius + 273.15;
            break;
        default:
            return '❌ Invalid scale. Use C, F, or K';
    }
    
    return formatResult(value, fromScale, result, toScale);
}

function formatResult(originalValue, originalScale, convertedValue, targetScale) {
    const symbol = targetScale === 'K' ? 'K' : `°${targetScale}`;
    const formatted = `${convertedValue.toFixed(2)}${symbol}`;
    
    // Add contextual information
    const context = getTemperatureContext(convertedValue, targetScale);
    
    return `${originalValue}°${originalScale} = ${formatted}${context}`;
}

function getTemperatureContext(temp, scale) {
    // Convert to Celsius for comparison
    let celsius;
    if (scale === 'C') {
        celsius = temp;
    } else if (scale === 'F') {
        celsius = (temp - 32) * 5 / 9;
    } else if (scale === 'K') {
        celsius = temp - 273.15;
    }
    
    const contexts = [
        { temp: -273.15, text: " (Absolute zero - coldest possible)" },
        { temp: -89.2, text: " (Coldest recorded on Earth)" },
        { temp: -40, text: " (C and F scales meet)" },
        { temp: 0, text: " (Water freezing point)" },
        { temp: 20, text: " (Room temperature)" },
        { temp: 37, text: " (Human body temperature)" },
        { temp: 100, text: " (Water boiling point)" },
        { temp: 1000, text: " (Extremely hot)" }
    ];
    
    // Find closest context
    for (let i = contexts.length - 1; i >= 0; i--) {
        if (celsius >= contexts[i].temp) {
            return contexts[i].text;
        }
    }
    
    return "";
}

// Comprehensive converter with multiple conversions
function temperatureConverter(value, fromScale) {
    console.log(`\n🌡️  Temperature Conversion Tool`);
    console.log(`━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━`);
    console.log(`Original: ${value}°${fromScale.toUpperCase()}`);
    console.log(`━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━`);
    
    const scales = ['C', 'F', 'K'];
    
    scales.forEach(scale => {
        if (scale !== fromScale.toUpperCase()) {
            const result = convertTemperature(value, fromScale, scale);
            console.log(`→ ${result}`);
        }
    });
    
    console.log(`━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━\n`);
}

// Test cases
console.log(convertTemperature(0, 'C', 'F'));
console.log(convertTemperature(100, 'C', 'F'));
console.log(convertTemperature(32, 'F', 'C'));
console.log(convertTemperature(273.15, 'K', 'C'));
console.log(convertTemperature(-300, 'C', 'K'));
console.log(convertTemperature(-40, 'C', 'F'));

// Comprehensive conversions
temperatureConverter(25, 'C');
temperatureConverter(98.6, 'F');
temperatureConverter(300, 'K');

// Batch conversion table
function createConversionTable(temperatures, fromScale) {
    console.log(`\n📊 Conversion Table: ${fromScale} to all scales`);
    console.log('┌──────────┬──────────┬──────────┬──────────┐');
    console.log(`│ Original │ Celsius  │ Fahren.  │  Kelvin  │`);
    console.log('├──────────┼──────────┼──────────┼──────────┤');
    
    temperatures.forEach(temp => {
        const c = convertTemperature(temp, fromScale, 'C').split('=')[1].split('°')[0].trim();
        const f = convertTemperature(temp, fromScale, 'F').split('=')[1].split('°')[0].trim();
        const k = convertTemperature(temp, fromScale, 'K').split('=')[1].split('K')[0].trim();
        
        console.log(`│ ${String(temp).padStart(8)} │ ${c.padStart(8)} │ ${f.padStart(8)} │ ${k.padStart(8)} │`);
    });
    
    console.log('└──────────┴──────────┴──────────┴──────────┘\n');
}

createConversionTable([0, 25, 37, 100], 'C');
```

**Common Mistakes:**
- ❌ Converting directly between F and K without going through Celsius
- ❌ Not validating for absolute zero
- ❌ Using incorrect conversion formulas (F = C × 9/5 + 32, not C × 1.8 + 32)
- ❌ Not handling case sensitivity in scale inputs
- ❌ Rounding too early in multi-step conversions

**What You Learned:**
- Multi-step conversions using intermediate values
- Input validation for physical constraints
- Switch statements for multiple conditions
- Contextual information display
- String formatting and interpolation

**Extension Challenge:**
Add more temperature scales (Rankine, Réaumur). Create a function that suggests which scale is most appropriate based on the temperature range (e.g., Kelvin for very cold, Celsius for everyday, etc.). Add color-coded output for different temperature ranges.

</details>

---

### Problem 6: Data Type Transformation Pipeline 🔴 Advanced

**Task:** Create a system that transforms data through a series of type conversions and validates each step.

**Requirements:**
- Accept any input type
- Convert through a specified pipeline (e.g., number → string → array → object)
- Log each transformation step
- Handle conversion errors gracefully
- Return the transformation history

**Sample Pipeline:**
```javascript
Input: 42
Step 1: Number → String → "42"
Step 2: String → Array → ["4", "2"]
Step 3: Array → Object → { 0: "4", 1: "2" }
```

**Before You Code:**
How do you convert between fundamentally different types? What happens when a conversion is impossible? Should you stop or skip that step?

**Hints:**
- Use a chain of transformation functions
- Store intermediate results in an array
- Try-catch for error handling
- Document why each conversion works or fails

<details>
<summary><strong>✅ Solution</strong></summary>

```javascript
class DataTransformer {
    constructor() {
        this.transformations = {
            // Number conversions
            'number->string': (val) => String(val),
            'number->boolean': (val) => Boolean(val),
            'number->array': (val) => [val],
            'number->object': (val) => ({ value: val }),
            
            // String conversions
            'string->number': (val) => {
                const num = Number(val);
                if (isNaN(num)) throw new Error('Cannot convert to number');
                return num;
            },
            'string->boolean': (val) => val.length > 0,
            'string->array': (val) => val.split(''),
            'string->object': (val) => ({ value: val, length: val.length }),
            
            // Boolean conversions
            'boolean->number': (val) => val ? 1 : 0,
            'boolean->string': (val) => String(val),
            'boolean->array': (val) => [val],
            'boolean->object': (val) => ({ value: val }),
            
            // Array conversions
            'array->string': (val) => val.join(''),
            'array->number': (val) => {
                if (val.length !== 1) throw new Error('Array must have one element');
                return Number(val[0]);
            },
            'array->boolean': (val) => val.length > 0,
            'array->object': (val) => {
                return val.reduce((obj, item, index) => {
                    obj[index] = item;
                    return obj;
                }, {});
            },
            
            // Object conversions
            'object->string': (val) => JSON.stringify(val),
            'object->array': (val) => Object.values(val),
            'object->boolean': (val) => Object.keys(val).length > 0,
            'object->number': (val) => {
                const keys = Object.keys(val);
                if (keys.length !== 1) throw new Error('Object must have one property');
                return Number(val[keys[0]]);
            }
        };
        
        this.history = [];
    }
    
    getType(value) {
        if (Array.isArray(value)) return 'array';
        if (value === null) return 'null';
        return typeof value;
    }
    
    transform(value, targetType) {
        const sourceType = this.getType(value);
        const key = `${sourceType}->${targetType}`;
        
        if (sourceType === targetType) {
            return { success: true, value, message: 'No conversion needed' };
        }
        
        if (!this.transformations[key]) {
            return {
                success: false,
                value,
                message: `No transformation available from ${sourceType} to ${targetType}`
            };
        }
        
        try {
            const transformed = this.transformations[key](value);
            return {
                success: true,
                value: transformed,
                message: `Converted ${sourceType} to ${targetType}`
            };
        } catch (error) {
            return {
                success: false,
                value,
                message: `Conversion failed: ${error.message}`
            };
        }
    }
    
    pipeline(initialValue, steps) {
        console.log('\n🔄 Data Transformation Pipeline');
        console.log('═'.repeat(50));
        
        let currentValue = initialValue;
        let currentType = this.getType(initialValue);
        
        console.log(`📥 Input: ${JSON.stringify(initialValue)} (${currentType})`);
        console.log('─'.repeat(50));
        
        this.history = [{
            step: 0,
            type: currentType,
            value: initialValue,
            success: true,
            message: 'Initial value'
        }];
        
        steps.forEach((targetType, index) => {
            const result = this.transform(currentValue, targetType);
            
            const stepNumber = index + 1;
            const status = result.success ? '✅' : '❌';
            
            console.log(`\n${status} Step ${stepNumber}: ${currentType} → ${targetType}`);
            console.log(`   Message: ${result.message}`);
            
            if (result.success) {
                console.log(`   Result: ${JSON.stringify(result.value)}`);
                currentValue = result.value;
                currentType = this.getType(currentValue);
            } else {
                console.log(`   Keeping: ${JSON.stringify(currentValue)}`);
            }
            
            this.history.push({
                step: stepNumber,
                sourceType: currentType,
                targetType,
                value: result.value,
                success: result.success,
                message: result.message
            });
        });
        
        console.log('\n' + '═'.repeat(50));
        console.log(`📤 Final Output: ${JSON.stringify(currentValue)} (${currentType})`);
        console.log('═'.repeat(50) + '\n');
        
        return {
            finalValue: currentValue,
            finalType: currentType,
            history: this.history,
            successRate: this.calculateSuccessRate()
        };
    }
    
    calculateSuccessRate() {
        const successful = this.history.filter(h => h.success).length - 1; // Exclude initial
        const total = this.history.length - 1;
        return total > 0 ? ((successful / total) * 100).toFixed(1) + '%' : 'N/A';
    }
    
    printSummary() {
        console.log('\n📊 Transformation Summary');
        console.log('┌────────┬─────────────┬─────────────┬─────────────┐');
        console.log('│  Step  │   Source    │   Target    │   Status    │');
        console.log('├────────┼─────────────┼─────────────┼─────────────┤');
        
        this.history.slice(1).forEach(h => {
            const step = `  ${h.step}`.padEnd(6);
            const source = h.sourceType.padEnd(11);
            const target = h.targetType.padEnd(11);
            const status = (h.success ? '✅ Success' : '❌ Failed').padEnd(11);
            console.log(`│ ${step} │ ${source} │ ${target} │ ${status} │`);
        });
        
        console.log('└────────┴─────────────┴─────────────┴─────────────┘');
        console.log(`\nSuccess Rate: ${this.calculateSuccessRate()}\n`);
    }
}

// Example usage
const transformer = new DataTransformer();

// Pipeline 1: Number transformations
console.log('\n🔹 Pipeline 1: Number Journey');
let result = transformer.pipeline(42, ['string', 'array', 'object', 'string']);
transformer.printSummary();

// Pipeline 2: String transformations
console.log('\n🔹 Pipeline 2: String Journey');
result = transformer.pipeline("123", ['number', 'boolean', 'string', 'array']);
transformer.printSummary();

// Pipeline 3: Complex journey with failures
console.log('\n🔹 Pipeline 3: Complex Journey (with failures)');
result = transformer.pipeline([1, 2, 3], ['string', 'number', 'array', 'object']);
transformer.printSummary();

// Pipeline 4: Boolean transformations
console.log('\n🔹 Pipeline 4: Boolean Journey');
result = transformer.pipeline(true, ['number', 'string', 'array', 'object']);
transformer.printSummary();

// Advanced: Chain multiple pipelines
function chainPipelines(transformations) {
    const transformer = new DataTransformer();
    let currentValue = transformations[0].input;
    
    console.log('\n🔗 Chained Pipelines');
    console.log('═'.repeat(60));
    
    transformations.forEach((t, index) => {
        console.log(`\n▶️  Pipeline ${index + 1}: ${t.name}`);
        const result = transformer.pipeline(currentValue, t.steps);
        currentValue = result.finalValue;
    });
    
    return currentValue;
}

const chainedResult = chainPipelines([
    {
        name: 'Numberify',
        input: "42",
        steps: ['number', 'boolean', 'number']
    },
    {
        name: 'Objectify',
        input: undefined, // Will use result from previous
        steps: ['string', 'array', 'object']
    }
]);

console.log(`\n🎯 Final chained result: ${JSON.stringify(chainedResult)}`);
```

**Common Mistakes:**
- ❌ Not handling conversion failures gracefully
- ❌ Assuming all conversions are possible
- ❌ Not documenting the transformation logic
- ❌ Modifying the original value instead of creating new ones
- ❌ Not validating intermediate results

**What You Learned:**
- Building complex data processing pipelines
- Error handling in transformation chains
- Object-oriented programming patterns
- Dynamic function selection based on types
- Logging and debugging complex workflows

**Extension Challenge:**
Add support for custom transformation functions. Allow users to register their own `type->type` converters. Create a visual representation of the transformation graph. Add rollback capability if any step fails.

</details>

---

### Problem 7: Memory Usage Analyzer 🔴 Advanced

**Task:** Create a tool that analyzes how different data structures use memory and compares primitive vs reference types.

**Requirements:**
- Compare memory behavior of primitives vs objects
- Demonstrate shallow vs deep copying
- Show how variable reassignment affects memory
- Visualize the stack and heap

**Sample Scenario:**
```javascript
// Show the difference between:
let x = 10; let y = x;  // Primitive
let obj1 = {val: 10}; let obj2 = obj1;  // Reference
```

**Before You Code:**
How can you demonstrate that primitives are copied by value and objects by reference? What happens when you mutate each?

**Hints:**
- Create test scenarios that modify copied values
- Use object comparison to show reference sharing
- Document expected vs actual behavior
- Create visual representations of memory

<details>
<summary><strong>✅ Solution</strong></summary>

```javascript
class MemoryAnalyzer {
    constructor() {
        this.stackMemory = [];
        this.heapMemory = [];
        this.operationLog = [];
    }
    
    // Simulate variable creation
    createVariable(name, value, scope = 'global') {
        const type = this.getType(value);
        const isPrimitive = this.isPrimitive(value);
        
        const variable = {
            name,
            value,
            type,
            isPrimitive,
            scope,
            memoryLocation: isPrimitive ? 'stack' : 'heap',
            heapReference: isPrimitive ? null : this.allocateHeap(value),
            createdAt: new Date().toISOString()
        };
        
        if (isPrimitive) {
            this.stackMemory.push(variable);
        } else {
            this.stackMemory.push({
                ...variable,
                value: `ref:${variable.heapReference}`
            });
        }
        
        this.log(`Created ${name} = ${JSON.stringify(value)} (${type}, ${variable.memoryLocation})`);
        
        return variable;
    }
    
    allocateHeap(value) {
        const id = `heap_${this.heapMemory.length}`;
        this.heapMemory.push({
            id,
            value: JSON.parse(JSON.stringify(value)), // Deep clone
            references: 1
        });
        return id;
    }
    
    getType(value) {
        if (Array.isArray(value)) return 'array';
        if (value === null) return 'null';
        return typeof value;
    }
    
    isPrimitive(value) {
        const type = typeof value;
        return value === null || (type !== 'object' && type !== 'function');
    }
    
    copyVariable(sourceName, targetName) {
        const source = this.stackMemory.find(v => v.name === sourceName);
        
        if (!source) {
            this.log(`❌ Variable ${sourceName} not found`);
            return null;
        }
        
        if (source.isPrimitive) {
            // Primitive: Copy value
            this.createVariable(targetName, source.value);
            this.log(`✅ Copied primitive: ${targetName} = ${sourceName} (independent copy)`);
        } else {
            // Reference: Copy reference
            const heapObj = this.heapMemory.find(h => h.id === source.heapReference);
            if (heapObj) {
                heapObj.references++;
            }
            
            this.stackMemory.push({
                ...source,
                name: targetName
            });
            
            this.log(`✅ Copied reference: ${targetName} = ${sourceName} (same object in heap)`);
        }
    }
    
    modifyVariable(name, newValue) {
        const variable = this.stackMemory.find(v => v.name === name);
        
        if (!variable) {
            this.log(`❌ Variable ${name} not found`);
            return;
        }
        
        if (variable.isPrimitive) {
            variable.value = newValue;
            this.log(`Modified ${name} = ${newValue} (only this variable affected)`);
        } else {
            const heapObj = this.heapMemory.find(h => h.id === variable.heapReference);
            if (heapObj) {
                heapObj.value = newValue;
                this.log(`Modified ${name} (all references to heap_${heapObj.id} affected)`);
            }
        }
    }
    
    log(message) {
        this.operationLog.push({
            timestamp: Date.now(),
            message
        });
    }
    
    visualize() {
        console.log('\n' + '═'.repeat(70));
        console.log('                     MEMORY VISUALIZATION');
        console.log('═'.repeat(70));
        
        // Stack Memory
        console.log('\n📚 STACK MEMORY (Primitives & References)');
        console.log('┌─────────────┬─────────────────────┬──────────────┬────────────┐');
        console.log('│  Variable   │       Value         │     Type     │  Location  │');
        console.log('├─────────────┼─────────────────────┼──────────────┼────────────┤');
        
        this.stackMemory.forEach(v => {
            const name = v.name.padEnd(11);
            const value = String(v.value).substring(0, 18).padEnd(19);
            const type = v.type.padEnd(12);
            const location = v.memoryLocation.padEnd(10);
            console.log(`│ ${name} │ ${value} │ ${type} │ ${location} │`);
        });
        
        console.log('└─────────────┴─────────────────────┴──────────────┴────────────┘');
        
        // Heap Memory
        if (this.heapMemory.length > 0) {
            console.log('\n🗄️  HEAP MEMORY (Objects & Arrays)');
            console.log('┌──────────────┬────────────────────────────────┬────────────┐');
            console.log('│  Heap ID     │           Value                │   Refs     │');
            console.log('├──────────────┼────────────────────────────────┼────────────┤');
            
            this.heapMemory.forEach(h => {
                const id = h.id.padEnd(12);
                const value = JSON.stringify(h.value).substring(0, 28).padEnd(30);
                const refs = String(h.references).padEnd(10);
                console.log(`│ ${id} │ ${value} │ ${refs} │`);
            });
            
            console.log('└──────────────┴────────────────────────────────┴────────────┘');
        }
        
        // Operation Log
        console.log('\n📝 OPERATION LOG');
        console.log('─'.repeat(70));
        this.operationLog.forEach((log, index) => {
            console.log(`${index + 1}. ${log.message}`);
        });
        console.log('═'.repeat(70) + '\n');
    }
    
    demonstratePrimitiveVsReference() {
        console.log('\n🔬 DEMONSTRATION: Primitive vs Reference Types\n');
        
        console.log('▶️  SCENARIO 1: Copying Primitives');
        console.log('─'.repeat(50));
        this.createVariable('x', 10);
        this.copyVariable('x', 'y');
        this.visualize();
        
        console.log('\n▶️  Modifying x to 20...');
        this.modifyVariable('x', 20);
        this.visualize();
        
        console.log('💡 Result: y is still 10 (independent copy)');
        
        // Reset for next demo
        this.stackMemory = [];
        this.heapMemory = [];
        this.operationLog = [];
        
        console.log('\n\n▶️  SCENARIO 2: Copying Objects');
        console.log('─'.repeat(50));
        this.createVariable('obj1', { value: 10 });
        this.copyVariable('obj1', 'obj2');
        this.visualize();
        
        console.log('\n▶️  Modifying obj1.value to 20...');
        this.modifyVariable('obj1', { value: 20 });
        this.visualize();
        
        console.log('💡 Result: obj2.value is also 20 (shared reference)');
    }
    
    demonstrateShallowVsDeep() {
        this.stackMemory = [];
        this.heapMemory = [];
        this.operationLog = [];
        
        console.log('\n\n🔬 DEMONSTRATION: Shallow vs Deep Copy\n');
        
        console.log('▶️  Creating nested object...');
        const original = {
            name: 'Alice',
            address: {
                city: 'New York',
                zip: 10001
            }
        };
        
        this.createVariable('original', original);
        
        console.log('\n▶️  Shallow copy (spread operator)...');
        // Simulating: const shallow = {...original}
        this.createVariable('shallow', { ...original });
        
        console.log('\n▶️  Deep copy (JSON method)...');
        // Simulating: const deep = JSON.parse(JSON.stringify(original))
        this.createVariable('deep', JSON.parse(JSON.stringify(original)));
        
        this.visualize();
        
        console.log('\n💡 Analysis:');
        console.log('  • Shallow copy: Nested objects still share references');
        console.log('  • Deep copy: Completely independent, all levels copied');
    }
}

// Run demonstrations
const analyzer = new MemoryAnalyzer();

analyzer.demonstratePrimitiveVsReference();
analyzer.demonstrateShallowVsDeep();

// Interactive comparison
function compareMemoryBehavior() {
    console.log('\n' + '='.repeat(70));
    console.log('           INTERACTIVE MEMORY BEHAVIOR COMPARISON');
    console.log('='.repeat(70));
    
    // Test 1: Primitive copying
    console.log('\n📊 Test 1: Primitive Variable Behavior');
    console.log('─'.repeat(50));
    
    let a = 5;
    let b = a;
    console.log(`Initial: a = ${a}, b = ${b}`);
    
    a = 10;
    console.log(`After a = 10: a = ${a}, b = ${b}`);