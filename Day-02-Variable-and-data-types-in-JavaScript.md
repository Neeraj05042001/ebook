# Day 2: Variables and Data Types in JavaScript

## Section 1: CHAPTER OVERVIEW

### 🎯 What You'll Master Today

By the end of this chapter, you'll have complete mastery over:

- **Variable declaration mechanisms** – Understanding the critical differences between `var`, `let`, and `const`, and knowing exactly when to use each one
- **JavaScript's type system** – Mastering all 7 primitive types and 3 reference types, including when and why each exists
- **Memory management fundamentals** – Understanding how JavaScript stores data in Stack vs Heap memory and why it matters for your code
- **Type coercion mechanics** – Predicting and controlling how JavaScript automatically converts between types
- **Variable scope and hoisting** – Understanding block scope, function scope, and the temporal dead zone
- **Best practices for variable naming** – Writing self-documenting code that other developers (and future you) will thank you for
- **Type checking techniques** – Using `typeof` and other methods to validate data types in your programs

---

### ✅ Prerequisites Check

Before diving in, make sure you have:

- [ ] Completed Day 1: Introduction to JavaScript
- [ ] Set up a JavaScript development environment (browser console or Node.js)
- [ ] Basic understanding of what programming is
- [ ] Familiarity with running JavaScript code in your chosen environment
- [ ] A code editor installed (VS Code, Sublime, or similar)

**Missing any of these?** No problem! You can follow along and set things up as we go. The most important prerequisite is curiosity.

---

### 💡 Why This Matters

**Career Impact:**
Variables and data types form the absolute foundation of every JavaScript program you'll ever write. In technical interviews, 40-60% of questions test your understanding of these fundamentals – not because they're difficult, but because they reveal how deeply you understand JavaScript.

Senior developers can spot a junior programmer instantly by how they declare variables. Using `var` everywhere? That signals outdated knowledge. Not understanding when to use `const`? That suggests you don't think about data immutability.

**Real-World Usage:**

Every single line of production JavaScript code uses variables and data types:
- **E-commerce:** Storing product prices (numbers), customer names (strings), cart status (booleans)
- **Social media:** Managing user profiles (objects), post lists (arrays), authentication states (booleans)
- **Banking apps:** Handling large transaction amounts (BigInt), account balances (numbers), user sessions (objects)
- **Game development:** Tracking scores (numbers), player states (objects), game loops (functions)

Understanding memory management (Stack vs Heap) becomes critical when building high-performance applications. A single memory leak from misunderstanding reference types can crash a production application serving millions of users.

**What makes this different from other languages:**
JavaScript's type coercion is both a superpower and a source of bugs. The statement `"5" + 3` equaling `"53"` has caused more production bugs than almost any other language feature. Mastering this today saves you from debugging nightmares tomorrow.

---

**🎓 Learning Philosophy for This Chapter:**

We're not just memorizing syntax – we're building mental models. You'll learn to *visualize* how JavaScript sees your code, *predict* behavior before running it, and *debug* issues by understanding root causes.

Each concept builds on the previous one. If something doesn't click immediately, that's normal. Mark it, continue forward, and return to it after seeing practical examples.

---

### 📚 How to Use This Chapter

1. **Don't skip the "Predict the Output" section** – This builds pattern recognition
2. **Actually type the code examples** – Reading isn't coding; your fingers need practice
3. **Make mistakes intentionally** – Break the code, see what happens, understand the error messages
4. **Use the debugging challenges** – They're based on real bugs from production code
5. **Take the MCQs seriously** – They're formatted like actual interview questions

---

**Ready to master variables and data types?** Let's build the foundation that every JavaScript expert stands on.

---


## Section 2: PREDICT THE OUTPUT 🔮

**Why This Section Matters:** Before writing code, sharpen your ability to think like the JavaScript engine. Predicting output builds pattern recognition – the skill that helps senior developers spot bugs instantly.

**How to Use This Section:**
1. Read the code snippet
2. Write down your prediction
3. Check the hint if stuck
4. Read the explanation
5. Run the code to verify

---

### Challenge #1: Variable Reassignment Basics
**Difficulty:** ⭐ Easy

```javascript
let score = 100;
score = 200;
const maxScore = 500;
maxScore = 600;

console.log(score);
console.log(maxScore);
```

**🤔 Think First:** What's the fundamental difference between `let` and `const`?

**💡 Hint:** `const` creates a binding that cannot be reassigned.

<details>
<summary><strong>📖 Answer & Explanation</strong></summary>

**Output:**
```
200
TypeError: Assignment to constant variable.
```

**Why:**
- `let` allows reassignment → `score` becomes 200
- `const` prevents reassignment → line 4 throws `TypeError`
- Script stops at the error, so `console.log(maxScore)` never executes
- **Key Point:** Errors stop execution. Code after an error doesn't run.

</details>

---

### Challenge #2: Type Coercion with Addition
**Difficulty:** ⭐⭐ Medium

```javascript
let result1 = "5" + 3;
let result2 = "5" + 3 + 2;
let result3 = 5 + 3 + "2";

console.log(result1);
console.log(result2);
console.log(result3);
```

**🤔 Think First:** Does the order matter? When does + do math vs string concatenation?

**💡 Hint:** Operations are left-to-right. Once a string appears, everything becomes concatenation.

<details>
<summary><strong>📖 Answer & Explanation</strong></summary>

**Output:**
```
53
532
82
```

**Why:**
- `"5" + 3` → String + Number = String concatenation → `"53"`
- `"5" + 3 + 2` → `"53" + 2` → `"532"` (left-to-right, stays string)
- `5 + 3 + "2"` → `8 + "2"` → `"82"` (math first, then concatenation)

**Rule:** The `+` operator switches to concatenation mode when it encounters a string.

**Real-World Bug:**
```javascript
let total = userInput + 10; // If userInput is "5", you get "510" not 15!
let total = Number(userInput) + 10; // Always convert explicitly ✓
```

</details>

---

### Challenge #3: Type Coercion with Subtraction
**Difficulty:** ⭐⭐ Medium

```javascript
console.log("10" - 5);
console.log("10" - "5");
console.log("hello" - 5);
console.log(true - 1);
console.log(false - 1);
```

**🤔 Think First:** Unlike `+`, the `-` operator only does math. What happens when JavaScript can't convert to a number?

**💡 Hint:** Failed conversions result in `NaN`.

<details>
<summary><strong>📖 Answer & Explanation</strong></summary>

**Output:**
```
5
5
NaN
0
-1
```

**Why:**
- `-` only works with numbers, so JavaScript converts strings to numbers
- `"10" - 5` → `10 - 5` = `5`
- `"hello" - 5` → Can't convert "hello" → `NaN`
- `true - 1` → `1 - 1` = `0` (true = 1, false = 0)
- `false - 1` → `0 - 1` = `-1`

**Key Difference:** `+` concatenates strings; `-`, `*`, `/` always attempt numeric conversion.

</details>

---

### Challenge #4: Variable Hoisting
**Difficulty:** ⭐⭐⭐ Hard

```javascript
console.log(a);
console.log(b);
console.log(c);

var a = 10;
let b = 20;
const c = 30;
```

**🤔 Think First:** What happens when you use variables before declaring them?

**💡 Hint:** `var` is hoisted with `undefined`; `let`/`const` are in the "temporal dead zone."

<details>
<summary><strong>📖 Answer & Explanation</strong></summary>

**Output:**
```
undefined
ReferenceError: Cannot access 'b' before initialization
```

**Why:**
- `var a` is hoisted and initialized to `undefined` → prints `undefined`
- `let b` exists but is in the Temporal Dead Zone (TDZ) → throws `ReferenceError`
- Line 3 never executes (script stopped at error)

**What JavaScript Actually Sees:**
```javascript
var a = undefined;  // var hoisted with value
// let b, const c in TDZ - exist but inaccessible

console.log(a);  // undefined
console.log(b);  // ReferenceError - still in TDZ
```

**Best Practice:** Always declare variables at the top of their scope.

</details>

---

### Challenge #5: Reference vs Primitive Types
**Difficulty:** ⭐⭐⭐ Hard

```javascript
let num1 = 10;
let num2 = num1;
num1 = 20;

let obj1 = { value: 10 };
let obj2 = obj1;
obj1.value = 20;

console.log(num1);
console.log(num2);
console.log(obj1.value);
console.log(obj2.value);
```

**🤔 Think First:** When you copy a variable, does it copy the value or a reference?

**💡 Hint:** Primitives copy values; objects copy references (memory addresses).

<details>
<summary><strong>📖 Answer & Explanation</strong></summary>

**Output:**
```
20
10
20
20
```

**Why:**

**Primitives (Stack Memory):**
- `num2 = num1` → Copies the **value** (10)
- They're independent → changing `num1` doesn't affect `num2`

**Objects (Heap Memory):**
- `obj2 = obj1` → Copies the **reference** (memory address)
- Both point to the **same object** → changing through either variable affects both

**Visual:**
```
PRIMITIVES:          OBJECTS:
num1: 20            obj1, obj2 → Same object { value: 20 }
num2: 10            (Both point to same memory location)
(independent)       (shared reference)
```

**Safe Copying:**
```javascript
let obj2 = { ...obj1 }; // Spread operator creates new object
```

</details>

---

### Challenge #6: typeof Operator Quirks
**Difficulty:** ⭐⭐ Medium

```javascript
console.log(typeof 42);
console.log(typeof "hello");
console.log(typeof true);
console.log(typeof undefined);
console.log(typeof null);
console.log(typeof {});
console.log(typeof []);
console.log(typeof function() {});
```

**🤔 Think First:** Are there any unexpected results from `typeof`?

**💡 Hint:** One famous bug, one technically-correct-but-surprising result.

<details>
<summary><strong>📖 Answer & Explanation</strong></summary>

**Output:**
```
number
string
boolean
undefined
object
object
object
function
```

**The Surprises:**

1. **`typeof null` → `"object"`** ❌
   - This is a **bug** from 1995 that can't be fixed (would break millions of websites)
   - Workaround: `value === null`

2. **`typeof []` → `"object"`** ✓
   - Technically correct (arrays ARE objects in JavaScript)
   - To check for arrays: `Array.isArray(value)`

**Correct Type Checking:**
```javascript
// Null check
if (value === null) { }

// Array check
if (Array.isArray(value)) { }

// Plain object check
if (typeof value === 'object' && value !== null && !Array.isArray(value)) { }
```

</details>

---

### Challenge #7: Const with Objects
**Difficulty:** ⭐⭐⭐ Hard

```javascript
const person = {
  name: "Alice",
  age: 25
};

person.age = 26;
person.city = "New York";

console.log(person.age);
console.log(person.city);

person = { name: "Bob" };
console.log(person.name);
```

**🤔 Think First:** Can you modify properties of a `const` object? Can you replace the object entirely?

**💡 Hint:** `const` prevents reassignment, not mutation.

<details>
<summary><strong>📖 Answer & Explanation</strong></summary>

**Output:**
```
26
New York
TypeError: Assignment to constant variable.
```

**Why:**

✓ **Allowed (Mutation):**
```javascript
person.age = 26;         // Modify property
person.city = "New York"; // Add property
delete person.age;        // Delete property
```

❌ **Not Allowed (Reassignment):**
```javascript
person = { name: "Bob" }; // TypeError - can't reassign
```

**The Rule:** `const` locks the **reference**, not the **contents**.

**Mental Model:**
```
const person → 0x001 ← This pointer is locked
               ↓
        { name: "Alice",
          age: 26 ← Can change ✓
        }
```

**True Immutability:**
```javascript
const person = Object.freeze({ name: "Alice" });
person.name = "Bob"; // Silently fails (throws in strict mode)
```

</details>

---

### Challenge #8: Undefined vs Null
**Difficulty:** ⭐⭐ Medium

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

**🤔 Think First:** What's the difference between these two "empty" values?

**💡 Hint:** `undefined` = JavaScript's default; `null` = programmer's choice.

<details>
<summary><strong>📖 Answer & Explanation</strong></summary>

**Output:**
```
undefined
null
undefined
object
true
false
```

**Why:**

- `undefined` → JavaScript's automatic "no value yet"
- `null` → Programmer's explicit "intentionally empty"
- `typeof null` → `"object"` (the famous bug)
- `a == b` → `true` (loose equality considers them equal)
- `a === b` → `false` (strict equality - different types)

**When Each Appears:**

**Undefined:**
```javascript
let x;              // Uninitialized
function f() {}     // No return value
obj.missing         // Missing property
```

**Null:**
```javascript
let user = null;    // Explicitly "no user"
user = fetchUser(); // Intentional placeholder
```

**Best Practices:**
```javascript
// Check for both
if (value == null) { } // Catches both undefined and null

// Modern way
let name = username ?? "Guest"; // Nullish coalescing
```

</details>

---

### Challenge #9: Template Literals
**Difficulty:** ⭐⭐ Medium

```javascript
let name = "Alice";
let age = 25;

let message1 = "Name: " + name + ", Age: " + age;
let message2 = `Name: ${name}, Age: ${age}`;
let message3 = `Next year: ${age + 1}`;
let message4 = `Status: ${age >= 18 ? "Adult" : "Minor"}`;

console.log(message2);
console.log(message3);
console.log(message4);
```

**🤔 Think First:** What can go inside `${}`? What type is the result?

**💡 Hint:** Any JavaScript expression works. Result is always a string.

<details>
<summary><strong>📖 Answer & Explanation</strong></summary>

**Output:**
```
Name: Alice, Age: 25
Next year: 26
Status: Adult
```

**Why:**
- Template literals use backticks `` ` `` and `${}` for expressions
- `${age + 1}` → Evaluates to `26`, converts to string
- `${age >= 18 ? "Adult" : "Minor"}` → Ternary operator evaluates to `"Adult"`
- Final result is **always a string**, regardless of expression type

**What Works Inside `${}`:**
```javascript
`Math: ${5 + 5}`                    // "Math: 10"
`Function: ${Math.random()}`         // "Function: 0.742..."
`Method: ${"hi".toUpperCase()}`      // "Method: HI"
`Property: ${user.name}`             // "Property: Alice"
```

**Real-World Usage:**
```javascript
const url = `https://api.com/users/${userId}`;
const html = `<div class="${className}">${content}</div>`;
```

</details>

---

### Challenge #10: The typeof NaN
**Difficulty:** ⭐⭐⭐ Hard

```javascript
let result = 0 / 0;

console.log(result);
console.log(typeof result);
console.log(result === NaN);
console.log(result === result);
console.log(isNaN(result));
console.log(Number.isNaN(result));
```

**🤔 Think First:** What type is NaN? How do you check for it?

**💡 Hint:** NaN is the only value not equal to itself.

<details>
<summary><strong>📖 Answer & Explanation</strong></summary>

**Output:**
```
NaN
number
false
false
true
true
```

**Why:**

1. `0 / 0` → Mathematical impossibility = `NaN`
2. `typeof NaN` → `"number"` (yes, really! It's a special numeric value)
3. `NaN === NaN` → `false` (NaN is never equal to anything, even itself)
4. `result === result` → `false` (only value in JS with this behavior)
5. `isNaN(result)` → `true` (but has issues - see below)
6. `Number.isNaN(result)` → `true` (the correct way)

**The Problem with isNaN():**
```javascript
isNaN("hello")     // true ⚠️ (converts to number first - misleading!)
Number.isNaN("hello") // false ✓ (only true for actual NaN)
```

**How to Check for NaN:**
```javascript
// ✓ Best way (ES6)
Number.isNaN(value)

// ✓ Self-comparison trick
value !== value

// ⚠️ Works but has issues
isNaN(value)

// ❌ Never works
value === NaN
```

**NaN Sources:**
```javascript
0 / 0              // NaN
"hello" * 5        // NaN
Math.sqrt(-1)      // NaN
parseInt("abc")    // NaN
```

**Interview Answer:** "NaN means 'Not a Number' but ironically has type 'number'. It's the only value not equal to itself. Use `Number.isNaN()` to check for it, not the older `isNaN()` which has type coercion issues."

</details>

---

## 🎯 Section Summary

**Master These Patterns:**
- `+` with strings = concatenation; other operators = numeric conversion
- `var` hoists with `undefined`; `let`/`const` have Temporal Dead Zone
- Primitives copy values; objects copy references
- `const` prevents reassignment, not mutation
- `typeof null` = `"object"` (famous bug)
- NaN !== NaN (use `Number.isNaN()`)



---


## Section 3: GUIDED PRACTICE PROBLEMS 💪

**Purpose:** Apply what you learned about variables and data types through focused practice. Every problem uses ONLY concepts from Day 1 (Introduction) and Day 2 (Variables & Data Types).

**How to Approach:**
1. Read the problem completely
2. Think before coding
3. Write your solution
4. Compare with provided solution
5. Learn from common mistakes

---

### Problem #1: Variable Declaration Practice


#### 📝 Task Description
Practice declaring variables using `var`, `let`, and `const`. Understand which can be reassigned and which cannot.

#### 🧪 Test Cases
```javascript
// Expected behavior:
var score = 100;
score = 150;        // ✓ Works (var allows reassignment)

let age = 25;
age = 26;           // ✓ Works (let allows reassignment)

const PI = 3.14;
PI = 3.14159;       // ❌ Error: Assignment to constant variable
```

#### 🤔 Before You Code
1. Which keyword allows reassignment: `var`, `let`, or `const`?
2. What happens when you try to reassign a `const` variable?
3. When should you use `const` vs `let`?


#### ✅ Solution

```javascript
// Using var (reassignable)
var score = 100;
console.log("Initial score:", score); // 100
score = 150;
console.log("Updated score:", score); // 150

// Using let (reassignable)
let age = 25;
console.log("Initial age:", age); // 25
age = 26;
console.log("Updated age:", age); // 26

// Using const (NOT reassignable)
const PI = 3.14159;
console.log("PI value:", PI); // 3.14159

// This will cause an error:
// PI = 3.14; // TypeError: Assignment to constant variable

// Practical example
const birthYear = 2000;
let currentYear = 2024;
let calculatedAge = currentYear - birthYear;

console.log("Age:", calculatedAge); // 24

currentYear = 2025; // ✓ Can change
calculatedAge = currentYear - birthYear;
console.log("Age next year:", calculatedAge); // 25

// birthYear = 2001; // ❌ Would cause error
```

#### ❌ Common Mistakes

```javascript
// ❌ Using const for values that need to change
const counter = 0;
counter = 1; // Error!

// ❌ Declaring same variable twice
let name = "Alice";
let name = "Bob"; // Error!

// ❌ Not initializing const
const x; // Error! const needs a value immediately
```

---

### Problem #2: Working with Primitive Types


#### 📝 Task Description
Create variables for each primitive data type and use `typeof` to check their types.

#### 🧪 Test Cases
```javascript
let age = 25;
typeof age               // "number"

let name = "Alice";
typeof name              // "string"

let isStudent = true;
typeof isStudent         // "boolean"

let result;
typeof result            // "undefined"

let data = null;
typeof data              // "object" (JavaScript bug)
```

#### 🤔 Before You Code
1. What are the 5 main primitive types you learned today?
2. How do you create an `undefined` variable?
3. Why does `typeof null` return "object"?

#### 💡 Hints
<details>
<summary>Hint 1</summary>

For undefined, just declare without assigning: `let variableName;`
</details>

<details>
<summary>Hint 2</summary>

Use `console.log(typeof variableName)` to check types.
</details>

#### ✅ Solution

```javascript
// Number type
let age = 25;
let price = 99.99;
let negative = -15;

console.log("age:", age, "- type:", typeof age);
console.log("price:", price, "- type:", typeof price);

// String type
let firstName = "Alice";
let city = 'New York';
let greeting = `Hello`;

console.log("firstName:", firstName, "- type:", typeof firstName);
console.log("city:", city, "- type:", typeof city);

// Boolean type
let isStudent = true;
let hasLicense = false;

console.log("isStudent:", isStudent, "- type:", typeof isStudent);
console.log("hasLicense:", hasLicense, "- type:", typeof hasLicense);

// Undefined type
let result;
let nothing;

console.log("result:", result, "- type:", typeof result);
console.log("nothing:", nothing, "- type:", typeof nothing);

// Null type
let data = null;
let empty = null;

console.log("data:", data, "- type:", typeof data); // "object" - known bug
console.log("empty:", empty, "- type:", typeof empty);

// All types together
console.log("\n=== Type Summary ===");
console.log("25 is", typeof 25);
console.log("'hello' is", typeof "hello");
console.log("true is", typeof true);
console.log("undefined is", typeof undefined);
console.log("null is", typeof null);
```

#### ❌ Common Mistakes

```javascript
// ❌ Confusing strings and numbers
let age = "25"; // This is a STRING, not a number!
let realAge = 25; // This is a NUMBER

// ❌ Manually setting undefined
let x = undefined; // Don't do this, just: let x;

// ❌ Thinking typeof null returns "null"
if (typeof data === "null") { } // Never works! Use: if (data === null)
```

---

### Problem #3: Type Coercion Exploration


#### 📝 Task Description
Experiment with type coercion by performing operations between different data types. Understand when JavaScript converts types automatically.

#### 🧪 Test Cases
```javascript
"5" + 3              // "53" (string)
"10" - 5             // 5 (number)
"10" * "2"           // 20 (number)
true + 1             // 2 (number)
false + 10           // 10 (number)
"hello" - 5          // NaN (number)
```

#### 🤔 Before You Code
1. What happens when you use `+` with a string and number?
2. What happens when you use `-`, `*`, `/` with strings?
3. What numeric values do `true` and `false` convert to?


#### ✅ Solution

```javascript
// Addition with strings (concatenation)
console.log("=== Addition (+ operator) ===");
console.log("5" + 3);          // "53" - becomes string
console.log(3 + "5");          // "35" - becomes string
console.log("5" + 3 + 2);      // "532" - all become strings
console.log(5 + 3 + "2");      // "82" - math first, then string

// Subtraction, Multiplication, Division (converts to numbers)
console.log("\n=== Other Math Operators ===");
console.log("10" - 5);         // 5 - string becomes number
console.log("10" - "5");       // 5 - both strings become numbers
console.log("10" * "2");       // 20 - strings become numbers
console.log("20" / "4");       // 5 - strings become numbers

// Booleans in math operations
console.log("\n=== Booleans as Numbers ===");
console.log(true + 1);         // 2 (true = 1)
console.log(false + 1);        // 1 (false = 0)
console.log(true + true);      // 2 (1 + 1)
console.log(true - false);     // 1 (1 - 0)
console.log(true * 5);         // 5 (1 * 5)

// Invalid conversions result in NaN
console.log("\n=== Invalid Operations (NaN) ===");
console.log("hello" - 5);      // NaN - can't convert "hello"
console.log("abc" * 10);       // NaN
console.log(typeof NaN);       // "number" - NaN is type number!

// Checking types after operations
console.log("\n=== Result Types ===");
console.log(typeof ("5" + 3));    // "string"
console.log(typeof ("10" - 5));   // "number"
console.log(typeof (true + 1));   // "number"
```

#### ❌ Common Mistakes

```javascript
// ❌ Expecting "5" + 3 to be 8
let result = "5" + 3; // "53", not 8

// ❌ Not understanding left-to-right evaluation
let x = 5 + 3 + "2";  // "82" not "532" (math happens first)

// ❌ Thinking NaN is type "NaN"
typeof NaN; // It's "number", not "NaN"!
```

---

### Problem #4: Null vs Undefined


#### 📝 Task Description
Explore the differences between `null` and `undefined`, and learn when JavaScript uses each.

#### 🧪 Test Cases
```javascript
let a;              // undefined (not assigned)
let b = null;       // null (explicitly empty)

a == b              // true (loose equality)
a === b             // false (strict equality)
typeof a            // "undefined"
typeof b            // "object"
```

#### 🤔 Before You Code
1. When does JavaScript automatically give you `undefined`?
2. When should you use `null` instead of `undefined`?
3. What's the difference between `==` and `===`?



#### ✅ Solution

```javascript
// Creating undefined and null
let undefinedVar;              // Just declared, automatically undefined
let nullVar = null;            // Explicitly set to null

console.log("=== Values ===");
console.log("undefinedVar:", undefinedVar);  // undefined
console.log("nullVar:", nullVar);            // null

// Checking types
console.log("\n=== Types ===");
console.log("typeof undefinedVar:", typeof undefinedVar);  // "undefined"
console.log("typeof nullVar:", typeof nullVar);            // "object" (bug!)

// Comparing with == (loose equality)
console.log("\n=== Loose Equality (==) ===");
console.log("undefinedVar == nullVar:", undefinedVar == nullVar);      // true
console.log("undefinedVar == undefined:", undefinedVar == undefined);  // true
console.log("nullVar == null:", nullVar == null);                      // true

// Comparing with === (strict equality)
console.log("\n=== Strict Equality (===) ===");
console.log("undefinedVar === nullVar:", undefinedVar === nullVar);    // false
console.log("undefinedVar === undefined:", undefinedVar === undefined);// true
console.log("nullVar === null:", nullVar === null);                    // true

// Real-world scenarios
console.log("\n=== Real Scenarios ===");

// Scenario 1: Uninitialized variable
let userName;
console.log("userName before assignment:", userName);  // undefined

// Scenario 2: Explicitly clearing a value
let selectedItem = "Apple";
console.log("Selected:", selectedItem);
selectedItem = null;  // Clear the selection
console.log("After clearing:", selectedItem);  // null

// Scenario 3: Checking for "no value" (both null and undefined)
let value1;
let value2 = null;

if (value1 == null) {
  console.log("value1 has no value");  // Runs (undefined == null)
}

if (value2 == null) {
  console.log("value2 has no value");  // Runs (null == null)
}
```

#### ❌ Common Mistakes

```javascript
// ❌ Manually assigning undefined
let x = undefined; // Don't do this! Just: let x;

// ❌ Using typeof to check for null
if (typeof data === "null") { } // Never works! Use: if (data === null)

// ❌ Confusing == and ===
null == undefined  // true (loose)
null === undefined // false (strict)
```

---

### Problem #5: Variable Naming and Best Practices


#### 📝 Task Description
Practice proper variable naming conventions and understand what makes a good variable name in JavaScript.

#### 🧪 Test Cases
```javascript
// ✓ Valid names
let firstName = "Alice";
let age_2024 = 25;
let $price = 100;
let _temp = 50;

// ❌ Invalid names
let 2024age = 25;        // Can't start with number
let first-name = "Bob";  // Can't use hyphens
let let = 10;            // Can't use reserved keywords
```

#### 🤔 Before You Code
1. What characters can variable names contain?
2. Can variable names start with numbers?
3. What are some reserved keywords you can't use?


#### ✅ Solution

```javascript
// ✓ Good variable names (descriptive and clear)
let firstName = "Alice";
let lastName = "Johnson";
let userAge = 25;
let isLoggedIn = true;
let totalPrice = 99.99;
let itemCount = 5;

console.log("User:", firstName, lastName);
console.log("Age:", userAge);
console.log("Logged in:", isLoggedIn);

// ✓ Valid but less common patterns
let $price = 100;           // $ is allowed
let _tempValue = 50;        // _ is allowed
let user2Name = "Bob";      // Numbers allowed (but not at start)
let CONSTANT_VALUE = 100;   // ALL_CAPS for constants

// ❌ Invalid variable names (these will cause errors)
// let 2users = 10;         // Error: Can't start with number
// let user-name = "Test";  // Error: Can't use hyphens
// let let = 5;             // Error: 'let' is a reserved keyword
// let const = 10;          // Error: 'const' is a reserved keyword
// let var = 15;            // Error: 'var' is a reserved keyword

// Naming conventions comparison
console.log("\n=== Naming Styles ===");

// ✓ camelCase (standard for JavaScript)
let userFirstName = "Alice";
let totalAmount = 500;

// ✓ snake_case (valid but not JavaScript convention)
let user_first_name = "Bob";
let total_amount = 600;

// ✓ PascalCase (usually for classes, but technically valid)
let UserFirstName = "Charlie";

// Choose descriptive names
console.log("\n=== Bad vs Good Names ===");

// ❌ Poor naming (unclear)
let x = 25;
let y = "Alice";
let z = true;

// ✓ Good naming (clear purpose)
let studentAge = 25;
let studentName = "Alice";
let isEnrolled = true;

console.log("Student:", studentName, "Age:", studentAge, "Enrolled:", isEnrolled);

// Real-world example
let productName = "Laptop";
let productPrice = 999.99;
let inStock = true;
let quantity = 10;

console.log("\n=== Product Info ===");
console.log("Product:", productName);
console.log("Price: $" + productPrice);
console.log("In Stock:", inStock);
console.log("Quantity:", quantity);
```

#### ❌ Common Mistakes

```javascript
// ❌ Starting with a number
let 1stPlace = "Gold"; // Error!

// ❌ Using hyphens
let first-name = "Alice"; // Error! Use firstName or first_name

// ❌ Using reserved keywords
let new = 10;    // Error!
let function = 5; // Error!

// ❌ Unclear single-letter names (except in specific cases like loops)
let a = "user@email.com"; // What is 'a'? Use: let userEmail
```

---

### Problem #6: Working with const and Objects


#### 📝 Task Description
Understand that `const` prevents reassignment but allows modification of object properties.

#### 🧪 Test Cases
```javascript
const person = { name: "Alice", age: 25 };
person.age = 26;              // ✓ Works (modifying property)
person.city = "NYC";          // ✓ Works (adding property)
person = { name: "Bob" };     // ❌ Error (reassignment)
```

#### 🤔 Before You Code
1. Can you change properties of a `const` object?
2. Can you add new properties to a `const` object?
3. Can you reassign a `const` object to a completely new object?


#### ✅ Solution

```javascript
// Creating a const object
const person = {
  name: "Alice",
  age: 25
};

console.log("Initial person:", person);

// ✓ Modifying properties (ALLOWED)
person.age = 26;
console.log("After age change:", person);

// ✓ Adding new properties (ALLOWED)
person.city = "New York";
console.log("After adding city:", person);

// ✓ Changing multiple properties (ALLOWED)
person.name = "Alice Johnson";
person.age = 27;
console.log("After multiple changes:", person);

// ❌ Reassigning the entire object (NOT ALLOWED)
// person = { name: "Bob", age: 30 }; // TypeError!

// Same behavior with arrays
const colors = ["red", "blue"];
console.log("\nInitial colors:", colors);

// ✓ Modifying array elements (ALLOWED)
colors[0] = "green";
console.log("After change:", colors);

// ✓ Adding to array (ALLOWED)
colors[2] = "yellow";
console.log("After adding:", colors);

// ❌ Reassigning array (NOT ALLOWED)
// colors = ["purple", "orange"]; // TypeError!

// Comparing const with primitives vs objects
console.log("\n=== const with Primitives ===");
const num = 10;
console.log("Number:", num);
// num = 20; // Error - can't reassign

const text = "Hello";
console.log("Text:", text);
// text = "Hi"; // Error - can't reassign

console.log("\n=== const with Objects ===");
const user = { username: "alice123" };
user.username = "alice456"; // ✓ Works - modifying property
console.log("Modified user:", user);
```

#### ❌ Common Mistakes

```javascript
// ❌ Thinking const makes objects immutable
const obj = { x: 10 };
obj.x = 20; // This works! const doesn't freeze the object

// ❌ Trying to reassign const object
const data = { value: 1 };
data = { value: 2 }; // Error!

// ✓ Correct: modify properties instead
data.value = 2; // Works fine
```

---

### Problem #7: Type Checking with typeof


#### 📝 Task Description
Practice using the `typeof` operator to check variable types and understand its quirks.

#### 🧪 Test Cases
```javascript
typeof 42                 // "number"
typeof "hello"            // "string"
typeof true               // "boolean"
typeof undefined          // "undefined"
typeof null               // "object" (quirk!)
typeof {}                 // "object"
typeof []                 // "object" (arrays are objects)
```

#### 🤔 Before You Code
1. What does `typeof` return for each primitive type?
2. Why does `typeof null` return "object"?
3. What does `typeof` return for objects and arrays?


#### ✅ Solution

```javascript
// Checking primitive types
console.log("=== Primitive Types ===");
console.log("typeof 42:", typeof 42);
console.log("typeof 3.14:", typeof 3.14);
console.log("typeof -100:", typeof -100);

console.log("typeof 'hello':", typeof "hello");
console.log('typeof "world":', typeof "world");
console.log("typeof `template`:", typeof `template`);

console.log("typeof true:", typeof true);
console.log("typeof false:", typeof false);

let notAssigned;
console.log("typeof notAssigned:", typeof notAssigned);

let empty = null;
console.log("typeof null:", typeof empty); // "object" - the famous bug!

// Checking with variables
console.log("\n=== Checking Variables ===");
let age = 25;
let name = "Alice";
let isStudent = true;
let score;
let data = null;

console.log("age is", typeof age);
console.log("name is", typeof name);
console.log("isStudent is", typeof isStudent);
console.log("score is", typeof score);
console.log("data is", typeof data);

// Checking reference types
console.log("\n=== Reference Types ===");
let person = { name: "Bob" };
let numbers = [1, 2, 3];

console.log("typeof person:", typeof person);    // "object"
console.log("typeof numbers:", typeof numbers);   // "object" (arrays are objects!)

// Using typeof in conditions
console.log("\n=== Using typeof in Logic ===");
let value = 42;

if (typeof value === "number") {
  console.log("value is a number");
}

if (typeof value === "string") {
  console.log("value is a string");
} else {
  console.log("value is NOT a string");
}

// Checking before operations
let userInput = "25";
console.log("\nuserInput:", userInput, "- Type:", typeof userInput);

if (typeof userInput === "string") {
  console.log("userInput is a string, not ready for math");
}
```

#### ❌ Common Mistakes

```javascript
// ❌ Expecting typeof null to be "null"
if (typeof null === "null") { } // Never true! Use: if (value === null)

// ❌ Not using quotes around type names
if (typeof x === number) { } // Error! Use: typeof x === "number"

// ❌ Thinking typeof tells you if it's an array
typeof [] === "object" // true, but doesn't tell you it's an array specifically
```

---

### Problem #8: String vs Number Operations


#### 📝 Task Description
Understand how JavaScript handles operations differently when values are strings vs numbers, and practice predicting results.

#### 🧪 Test Cases
```javascript
5 + 5                // 10 (number + number)
"5" + "5"            // "55" (string + string)
"5" + 5              // "55" (string + number)
5 + "5"              // "55" (number + string)

10 - 5               // 5 (number - number)
"10" - "5"           // 5 (string - string → converts to numbers)
"10" - 5             // 5 (string - number → converts to number)
```

#### 🤔 Before You Code
1. When does the `+` operator do addition vs concatenation?
2. Do `-`, `*`, `/` operators ever concatenate strings?
3. What happens if you do math with strings that contain non-numeric text?


#### ✅ Solution

```javascript
// Addition with numbers
console.log("=== Number Addition ===");
console.log("5 + 5 =", 5 + 5);              // 10
console.log("10 + 20 =", 10 + 20);          // 30
console.log("100 + 50 =", 100 + 50);        // 150

// String concatenation
console.log("\n=== String Concatenation ===");
console.log('"5" + "5" =', "5" + "5");      // "55"
console.log('"10" + "20" =', "10" + "20");  // "1020"
console.log('"Hello" + "World" =', "Hello" + "World"); // "HelloWorld"
console.log('"Hello" + " " + "World" =', "Hello" + " " + "World"); // "Hello World"

// Mixed: String + Number (becomes concatenation)
console.log("\n=== String + Number ===");
console.log('"5" + 5 =', "5" + 5);          // "55"
console.log('5 + "5" =', 5 + "5");          // "55"
console.log('"Age: " + 25 =', "Age: " + 25); // "Age: 25"
console.log('100 + " dollars" =', 100 + " dollars"); // "100 dollars"

// Order matters with multiple operations
console.log("\n=== Order of Operations ===");
console.log('5 + 5 + "5" =', 5 + 5 + "5");        // "105" (10 + "5")
console.log('"5" + 5 + 5 =', "5" + 5 + 5);        // "555" ("5" + 5 + 5)
console.log('5 + "5" + 5 =', 5 + "5" + 5);        // "555" ("55" + 5)

// Subtraction always converts to numbers
console.log("\n=== Subtraction (Always Numbers) ===");
console.log("10 - 5 =", 10 - 5);            // 5
console.log('"10" - 5 =', "10" - 5);        // 5 (converts "10" to 10)
console.log('10 - "5" =', 10 - "5");        // 5 (converts "5" to 5)
console.log('"10" - "5" =', "10" - "5");    // 5 (converts both to numbers)

// Multiplication and Division
console.log("\n=== Multiplication and Division ===");
console.log('"10" * "2" =', "10" * "2");    // 20 (converts to numbers)
console.log('"20" / "4" =', "20" / "4");    // 5 (converts to numbers)
console.log('"5" * 3 =', "5" * 3);          // 15
console.log('100 / "2" =', 100 / "2");      // 50

// Invalid conversions
console.log("\n=== Invalid Conversions (NaN) ===");
console.log('"hello" - 5 =', "hello" - 5);           // NaN
console.log('"abc" * 10 =', "abc" * 10);             // NaN
console.log('"test" / 2 =', "test" / 2);             // NaN
console.log('typeof NaN =', typeof NaN);              // "number"

// Practical examples
console.log("\n=== Real-World Scenarios ===");

// Scenario 1: Building a message
let userName = "Alice";
let userAge = 25;
let message = "User: " + userName + ", Age: " + userAge;
console.log(message); // "User: Alice, Age: 25"

// Scenario 2: Calculating with string inputs (common bug source)
let price = "50";
let quantity = 2;
let wrong = price + quantity;     // "502" - Oops! Concatenation
console.log("Wrong total:", wrong);

// To fix: convert string to number first
let priceNum = 50;  // Should be a number from start
let correct = priceNum * quantity;
console.log("Correct total:", correct); // 100

// Scenario 3: Mixing text and numbers
let score = 95;
let resultMessage = "Your score: " + score + " points";
console.log(resultMessage); // "Your score: 95 points"
```

#### ❌ Common Mistakes

```javascript
// ❌ Expecting "5" + 3 to equal 8
"5" + 3  // "53", not 8

// ❌ Forgetting left-to-right evaluation
5 + 5 + "5"  // "105", not "555"

// ❌ Doing math with string numbers without converting
let price = "100";  // This is a STRING
let total = price * 2;  // Works but brittle - better to ensure it's a number
```

---

## 🎯Summary

**Key Insights:**
- Use `const` by default, `let` when you need reassignment
- The `+` operator is special - it concatenates strings
- Other math operators (`-`, `*`, `/`) always convert to numbers
- `typeof null` returns "object" (known bug)
- `undefined` appears automatically, `null` is explicitly set

