# Day 32: Master JavaScript Modules


## 1. CHAPTER OVERVIEW

### 📹 In the Video
You learned that JavaScript modules are reusable pieces of code that can be exported from one file and imported into another, enabling better code organization, encapsulation, and avoiding global scope pollution—with support for both static imports/exports and dynamic `import()` for lazy loading.

### ✅ Learning Checklist
By the end of this workbook, you will be able to:
- [ ] Create modules using named exports, default exports, and mixed exports
- [ ] Import modules using various syntaxes including aliases and namespaces
- [ ] Implement dynamic imports for conditional and lazy loading
- [ ] Organize real-world projects with proper module structure
- [ ] Write tree-shakable code for optimal bundle size
- [ ] Debug module-related errors using browser DevTools
- [ ] Handle multiple module imports using Promise APIs

### 📚 Prerequisites
From **Day 31 (Prototypes & Object Patterns)**, you should know:
- How to create and use constructor functions and classes
- Understanding of `this` binding and context
- Object creation patterns and inheritance
- How to organize code into reusable components

**Note:** Modules build on your OOP knowledge—you'll be exporting classes, functions, and objects across files!

---

## 2. LECTURE CHEAT SHEET

### 🔑 Key Terminology Glossary

| Term | Definition |
|------|------------|
| **Module** | Self-contained, reusable piece of code in a separate file with its own scope |
| **Export** | Makes functions, objects, or values available to other modules |
| **Import** | Brings in functionality from other modules into the current file |
| **Named Export** | Export with a specific name; imported with curly braces `{ name }` |
| **Default Export** | One main export per module; imported without curly braces |
| **Namespace Import** | Import all exports under a single object using `* as Name` |
| **Dynamic Import** | Runtime loading of modules using `import()` function (returns Promise) |
| **Tree Shaking** | Build-time optimization that removes unused exports from bundle |

---

### 📊 Module System Comparison

```
┌─────────────────┬────────────────────┬────────────────────┐
│ Feature         │ ES6 Modules        │ CommonJS (Node)    │
├─────────────────┼────────────────────┼────────────────────┤
│ Syntax          │ import/export      │ require/exports    │
│ Loading         │ Asynchronous       │ Synchronous        │
│ Tree Shaking    │ ✅ Yes             │ ❌ No              │
│ Browser Native  │ ✅ Yes (type=module)│ ❌ No             │
│ Static Analysis │ ✅ Yes             │ ❌ Limited         │
│ Top-level await │ ✅ Yes             │ ❌ No              │
└─────────────────┴────────────────────┴────────────────────┘
```

**This workbook focuses on ES6 Modules** (modern standard for browsers and modern Node.js).

---

### 🎯 Export Patterns Quick Reference

```javascript
// ========================================
// NAMED EXPORTS (Multiple per module)
// ========================================

// Method 1: Export during declaration
export const PI = 3.14159;
export function square(x) {
  return x * x;
}
export class Calculator {
  add(a, b) { return a + b; }
}

// Method 2: Export after declaration
const PI = 3.14159;
function square(x) {
  return x * x;
}
class Calculator {
  add(a, b) { return a + b; }
}
export { PI, square, Calculator };

// Method 3: Export with alias
export { square as sq, Calculator as Calc };

// ========================================
// DEFAULT EXPORT (One per module)
// ========================================

// Method 1: Named then default
class Calculator {
  add(a, b) { return a + b; }
}
export default Calculator;

// Method 2: Anonymous default
export default class {
  add(a, b) { return a + b; }
}

// Method 3: Function default
export default function(x) {
  return x * x;
}

// ========================================
// MIXED EXPORTS (Default + Named)
// ========================================

export default class Calculator {
  add(a, b) { return a + b; }
}
export const PI = 3.14159;
export function square(x) { return x * x; }
```

---

### 📥 Import Patterns Quick Reference

```javascript
// ========================================
// NAMED IMPORTS
// ========================================

// Basic named import
import { PI, square } from './math.js';

// Multiple named imports
import { PI, square, Calculator } from './math.js';

// Import with alias
import { square as sq } from './math.js';

// Multiple imports with aliases
import { 
  square as sq, 
  Calculator as Calc 
} from './math.js';

// ========================================
// DEFAULT IMPORTS
// ========================================

// Import default (can use any name)
import Calculator from './calculator.js';
import Calc from './calculator.js';  // Same thing
import MyCalc from './calculator.js'; // Also same

// ========================================
// MIXED IMPORTS (Default + Named)
// ========================================

// Import default and named together
import Calculator, { PI, square } from './math.js';

// With aliases
import Calc, { square as sq } from './math.js';

// ========================================
// NAMESPACE IMPORTS
// ========================================

// Import all exports as object
import * as MathUtils from './math.js';
// Use: MathUtils.PI, MathUtils.square()

// ========================================
// SIDE-EFFECT IMPORTS
// ========================================

// Import for side effects only (no exports needed)
import './polyfills.js';
import './init-app.js';

// ========================================
// DYNAMIC IMPORTS
// ========================================

// Async/await syntax
const module = await import('./math.js');
module.square(5);

// Promise syntax
import('./math.js').then(module => {
  module.square(5);
});

// Conditional import
if (condition) {
  const module = await import('./math.js');
}
```

---

### 🔄 Re-Export Patterns (Combined Exports)

```javascript
// ========================================
// index.js - Module Hub Pattern
// ========================================

// Re-export specific named exports
export { add, subtract } from './math.js';
export { User, Admin } from './users.js';

// Re-export with alias
export { add as sum } from './math.js';
export { User as Customer } from './users.js';

// Re-export all named exports
export * from './math.js';
export * from './users.js';

// Re-export all as namespace
export * as MathUtils from './math.js';
export * as UserUtils from './users.js';

// Re-export default with new name
export { default as Calculator } from './calculator.js';

// ========================================
// Usage in main.js
// ========================================

// Clean single import point
import { add, subtract, User, Admin } from './index.js';
```

---

### 📋 Export vs Import: Matching Patterns

```javascript
// ========================================
// PATTERN 1: Named Export/Import
// ========================================

// math.js
export const PI = 3.14;
export function square(x) { return x * x; }

// main.js
import { PI, square } from './math.js';

// ========================================
// PATTERN 2: Default Export/Import
// ========================================

// calculator.js
export default class Calculator { }

// main.js
import Calculator from './calculator.js';
import Calc from './calculator.js';  // Can rename

// ========================================
// PATTERN 3: Mixed Export/Import
// ========================================

// shapes.js
export default class Circle { }
export const PI = 3.14;

// main.js
import Circle, { PI } from './shapes.js';

// ========================================
// PATTERN 4: Namespace Import
// ========================================

// math.js
export const PI = 3.14;
export function square(x) { return x * x; }

// main.js
import * as Math from './math.js';
Math.PI;       // 3.14
Math.square(5); // 25
```

---

### ⚡ Dynamic Import Flow

```
┌─────────────────────────────────────────────────────┐
│ Static Import (Top of file)                         │
│ ───────────────────────────────────                 │
│ • Parsed at load time                               │
│ • Cannot be conditional                             │
│ • All modules loaded upfront                        │
│ • Synchronous binding                               │
│                                                      │
│ import { fn } from './module.js';                   │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│ Dynamic Import (Anywhere in code)                   │
│ ──────────────────────────────────────              │
│ • Loaded at runtime                                 │
│ • Can be conditional                                │
│ • Lazy loading (only when needed)                   │
│ • Returns Promise                                   │
│                                                      │
│ const module = await import('./module.js');         │
│                                                      │
│ Use Cases:                                          │
│ • Code splitting                                    │
│ • Conditional loading                               │
│ • User interaction triggered                        │
│ • Route-based loading                               │
└─────────────────────────────────────────────────────┘
```

---

### 🌳 Tree Shaking Rules

**✅ Tree-Shakable (Good)**
```javascript
// utils.js - Named exports
export function usedFunction() { }
export function unusedFunction() { }  // Won't be in bundle

// main.js - Specific imports
import { usedFunction } from './utils.js';
// Bundle only includes usedFunction
```

**❌ Not Tree-Shakable (Bad)**
```javascript
// utils.js - Object export
export default {
  usedFunction() { },
  unusedFunction() { }  // WILL be in bundle!
};

// main.js
import utils from './utils.js';
utils.usedFunction();
// Bundle includes entire object
```

**Best Practices for Tree Shaking:**
1. ✅ Use named exports for utilities
2. ✅ Import specific functions: `import { fn } from 'module'`
3. ✅ Avoid side effects in modules
4. ✅ Use ES6 modules (not CommonJS)
5. ❌ Avoid `import *` unless necessary
6. ❌ Avoid default exports for utility libraries

---

### 📁 Module File Organization Patterns

```
┌─────────────────────────────────────────────────────┐
│ PATTERN 1: Flat Structure (Small Projects)          │
├─────────────────────────────────────────────────────┤
│ project/                                            │
│ ├── index.html                                      │
│ ├── main.js                                         │
│ ├── math.js                                         │
│ ├── utils.js                                        │
│ └── api.js                                          │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│ PATTERN 2: Feature-Based (Medium Projects)          │
├─────────────────────────────────────────────────────┤
│ project/                                            │
│ ├── index.html                                      │
│ ├── src/                                            │
│ │   ├── main.js                                     │
│ │   ├── users/                                      │
│ │   │   ├── index.js    (re-exports)               │
│ │   │   ├── user.js                                │
│ │   │   └── auth.js                                │
│ │   ├── posts/                                      │
│ │   │   ├── index.js                               │
│ │   │   ├── post.js                                │
│ │   │   └── comments.js                            │
│ │   └── utils/                                      │
│ │       ├── index.js                               │
│ │       ├── math.js                                │
│ │       └── string.js                              │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│ PATTERN 3: Layered (Large Projects)                 │
├─────────────────────────────────────────────────────┤
│ project/                                            │
│ ├── index.html                                      │
│ ├── src/                                            │
│ │   ├── main.js                                     │
│ │   ├── components/    (UI components)             │
│ │   ├── services/      (API, business logic)       │
│ │   ├── models/        (Data structures)           │
│ │   ├── utils/         (Helper functions)          │
│ │   └── config/        (Configuration)             │
└─────────────────────────────────────────────────────┘
```

---

### 🚨 Critical Rules

```javascript
// ❌ WRONG - Import must be at top level
if (condition) {
  import { fn } from './module.js';  // SyntaxError!
}

// ✅ CORRECT - Use dynamic import for conditional
if (condition) {
  const { fn } = await import('./module.js');
}

// ❌ WRONG - Destructuring in import
import math from './math.js';
const { square } = math;  // Unnecessary

// ✅ CORRECT - Destructure during import
import { square } from './math.js';

// ❌ WRONG - File extension matters in browsers
import { fn } from './module';  // May fail in browser

// ✅ CORRECT - Always include .js extension
import { fn } from './module.js';

// ❌ WRONG - Exporting after import (ambiguous)
export { fn } from './module.js';
export function fn() { }  // Name collision!

// ✅ CORRECT - Use aliases
export { fn as importedFn } from './module.js';
export function fn() { }
```

---

### 📦 HTML Setup for Modules

```html
<!DOCTYPE html>
<html>
<head>
  <title>My App</title>
</head>
<body>
  <!-- ✅ CORRECT - type="module" required -->
  <script type="module" src="./main.js"></script>
  
  <!-- Features of module scripts:
       • Deferred by default (like defer attribute)
       • Strict mode by default
       • Module scope (not global)
       • Can use import/export
       • CORS required for cross-origin
  -->
  
  <!-- ❌ WRONG - Without type="module" -->
  <script src="./main.js"></script>
  <!-- import/export won't work -->
</body>
</html>
```

---

### 🔄 Module Execution Order

```javascript
// ========================================
// Execution Order Example
// ========================================

// a.js
console.log('A: Start');
import { b } from './b.js';
console.log('A: End');
export const a = 'A';

// b.js
console.log('B: Start');
import { c } from './c.js';
console.log('B: End');
export const b = 'B';

// c.js
console.log('C: Start');
console.log('C: End');
export const c = 'C';

// main.js
console.log('Main: Start');
import { a } from './a.js';
console.log('Main: End');

// OUTPUT:
// C: Start
// C: End
// B: Start
// B: End
// A: Start
// A: End
// Main: Start
// Main: End
```

**Key Point:** Modules are executed depth-first, but each module runs only once (cached).

---

## 3. PREDICT THE OUTPUT

### Challenge 1: Basic Named Export/Import
```javascript
// math.js
export const PI = 3.14;
export function square(x) {
  return x * x;
}

// main.js
import { PI, square } from './math.js';
console.log(PI);
console.log(square(5));
```

**🤔 Thinking Process:**
- `PI` is exported as named export
- `square` is exported as named export
- Both imported correctly with curly braces
- Direct access to both

**✅ Output:**
```
3.14
25
```

**💡 Why:** Named exports require curly braces and exact names in import statement.

---

### Challenge 2: Default Export Renaming
```javascript
// calculator.js
export default class Calculator {
  add(a, b) {
    return a + b;
  }
}

// main.js
import Calc from './calculator.js';
import MyCalculator from './calculator.js';

const c1 = new Calc();
const c2 = new MyCalculator();

console.log(c1.add(2, 3));
console.log(c2.add(4, 5));
console.log(Calc === MyCalculator);
```

**🤔 Thinking Process:**
- Default exports can be renamed during import
- Both `Calc` and `MyCalculator` reference the same class
- Both instances work identically
- Both names point to same constructor

**✅ Output:**
```
5
9
true
```

**💡 Why:** Default imports can use any name—they all reference the same exported value.

---

### Challenge 3: Mixed Export/Import
```javascript
// shapes.js
export default class Circle {
  constructor(radius) {
    this.radius = radius;
  }
}
export const PI = 3.14159;
export function area(radius) {
  return PI * radius * radius;
}

// main.js
import Circle, { PI, area } from './shapes.js';
console.log(new Circle(5).radius);
console.log(PI);
console.log(area(5));
```

**🤔 Thinking Process:**
- Default export (Circle) imported without curly braces
- Named exports (PI, area) imported with curly braces
- Both can be imported in single statement
- Order: default first, then named in curly braces

**✅ Output:**
```
5
3.14159
78.53975
```

**💡 Why:** You can mix default and named imports in one statement: `import Default, { named } from '...'`

---

### Challenge 4: Namespace Import
```javascript
// utils.js
export const name = 'Utils';
export function upper(str) {
  return str.toUpperCase();
}
export function lower(str) {
  return str.toLowerCase();
}

// main.js
import * as Utils from './utils.js';
console.log(Utils.name);
console.log(Utils.upper('hello'));
console.log(Utils.lower);
```

**🤔 Thinking Process:**
- `* as Utils` imports all named exports under `Utils` object
- Access via dot notation: `Utils.name`, `Utils.upper()`
- `Utils.lower` is the function itself (not called)

**✅ Output:**
```
Utils
HELLO
[Function: lower]
```

**💡 Why:** Namespace imports create an object containing all named exports.

---

### Challenge 5: Import Alias
```javascript
// math.js
export function calculate() {
  return 'Math calculation';
}

// physics.js
export function calculate() {
  return 'Physics calculation';
}

// main.js
import { calculate as mathCalc } from './math.js';
import { calculate as physicsCalc } from './physics.js';

console.log(mathCalc());
console.log(physicsCalc());
```

**🤔 Thinking Process:**
- Both modules export function with same name
- Aliases avoid naming conflict
- Each alias references different function
- Clear separation despite same original name

**✅ Output:**
```
Math calculation
Physics calculation
```

**💡 Why:** Aliases (using `as`) resolve naming conflicts when importing from multiple modules.

---

### Challenge 6: Export Alias
```javascript
// utils.js
function verLongFunctionName() {
  return 'Hello';
}
export { verLongFunctionName as greet };

// main.js
import { greet } from './utils.js';
console.log(greet());
```

**🤔 Thinking Process:**
- Function defined with long name internally
- Exported with shorter alias `greet`
- Import uses the exported name (alias)
- Original name not accessible from outside

**✅ Output:**
```
Hello
```

**💡 Why:** Export aliases provide cleaner API—internal naming doesn't affect external interface.

---

### Challenge 7: Re-export Tracing
```javascript
// math.js
export function add(a, b) {
  return a + b;
}

// index.js
export { add } from './math.js';
export function multiply(a, b) {
  return a * b;
}

// main.js
import { add, multiply } from './index.js';
console.log(add(2, 3));
console.log(multiply(2, 3));
```

**🤔 Thinking Process:**
- `add` defined in math.js
- `index.js` re-exports `add` from math.js
- `index.js` also exports its own `multiply`
- Both accessible from single import in main.js
- Clean API through index.js hub

**✅ Output:**
```
5
6
```

**💡 Why:** Re-exports create a single entry point (index.js) for multiple modules.

---

### Challenge 8: Default + Named Re-export
```javascript
// calculator.js
export default class Calculator {
  add(a, b) { return a + b; }
}
export const VERSION = '1.0';

// index.js
export { default as Calculator } from './calculator.js';
export { VERSION } from './calculator.js';

// main.js
import { Calculator, VERSION } from './index.js';
console.log(new Calculator().add(1, 2));
console.log(VERSION);
```

**🤔 Thinking Process:**
- Default export needs special syntax: `{ default as NewName }`
- Named export re-exported normally
- Both accessible as named imports from index.js
- Default becomes named export in re-export

**✅ Output:**
```
3
1.0
```

**💡 Why:** When re-exporting default, use `{ default as Name }` to give it a name.

---

### Challenge 9: Re-export All
```javascript
// math.js
export function add(a, b) { return a + b; }
export function sub(a, b) { return a - b; }

// string.js
export function upper(s) { return s.toUpperCase(); }
export function lower(s) { return s.toLowerCase(); }

// index.js
export * from './math.js';
export * from './string.js';

// main.js
import { add, upper } from './index.js';
console.log(add(1, 2));
console.log(upper('hello'));
```

**🤔 Thinking Process:**
- `export *` re-exports all named exports
- Combines math.js and string.js exports
- Single import point for all utilities
- Clean module aggregation

**✅ Output:**
```
3
HELLO
```

**💡 Why:** `export * from './module'` re-exports all named exports from that module.

---

### Challenge 10: Dynamic Import Basic
```javascript
// math.js
export function square(x) {
  return x * x;
}

// main.js
async function loadMath() {
  const math = await import('./math.js');
  console.log(math.square(5));
}

loadMath();
console.log('Loading...');
```

**🤔 Thinking Process:**
- `import()` returns a Promise
- `await` waits for module to load
- Module object has all exports
- "Loading..." logs first (async)
- Then module loads and executes

**✅ Output:**
```
Loading...
25
```

**💡 Why:** Dynamic import is asynchronous—main code continues while module loads.

---

### Challenge 11: Dynamic Import with Destructuring
```javascript
// utils.js
export const name = 'Utils';
export function greet() {
  return 'Hello';
}

// main.js
(async () => {
  const { name, greet } = await import('./utils.js');
  console.log(name);
  console.log(greet());
})();
```

**🤔 Thinking Process:**
- Dynamic import returns module object
- Destructure specific exports immediately
- IIFE with async for top-level await simulation
- Cleaner than accessing via `module.name`

**✅ Output:**
```
Utils
Hello
```

**💡 Why:** You can destructure the module object returned by dynamic import.

---

### Challenge 12: Conditional Dynamic Import
```javascript
// heavy-feature.js
export function expensiveOperation() {
  return 'Expensive result';
}

// main.js
async function loadFeature(condition) {
  if (condition) {
    const module = await import('./heavy-feature.js');
    return module.expensiveOperation();
  }
  return 'Feature not loaded';
}

console.log(await loadFeature(false));
console.log(await loadFeature(true));
```

**🤔 Thinking Process:**
- Module only loaded when condition is true
- First call: condition false, module not loaded
- Second call: condition true, module loads
- Lazy loading saves bandwidth

**✅ Output:**
```
Feature not loaded
Expensive result
```

**💡 Why:** Dynamic imports enable conditional loading—load only when needed.

---

### Challenge 13: Promise.all with Imports
```javascript
// math.js
export const PI = 3.14;

// string.js
export function upper(s) { return s.toUpperCase(); }

// main.js
async function loadAll() {
  const [math, string] = await Promise.all([
    import('./math.js'),
    import('./string.js')
  ]);
  
  console.log(math.PI);
  console.log(string.upper('hello'));
}

loadAll();
```

**🤔 Thinking Process:**
- Load multiple modules in parallel
- `Promise.all` waits for all to load
- Destructure array of module objects
- More efficient than sequential loading

**✅ Output:**
```
3.14
HELLO
```

**💡 Why:** `Promise.all` loads multiple modules concurrently for better performance.

---

### Challenge 14: Module Scope
```javascript
// counter.js
let count = 0;
export function increment() {
  count++;
  return count;
}
export function getCount() {
  return count;
}

// main.js
import { increment, getCount } from './counter.js';
console.log(increment());
console.log(increment());
console.log(getCount());
```

**🤔 Thinking Process:**
- `count` is private to counter.js module
- Exported functions can access `count`
- State persists between function calls
- Module scope provides encapsulation

**✅ Output:**
```
1
2
2
```

**💡 Why:** Modules have their own scope—variables not exported remain private.

---

### Challenge 15: Import Side Effects
```javascript
// logger.js
console.log('Logger initialized');
export function log(msg) {
  console.log(msg);
}

// main.js
import './logger.js';  // Side-effect import
import { log } from './logger.js';

log('Hello');
```

**🤔 Thinking Process:**
- First import runs the module (logs "Logger initialized")
- Module is cached—runs only once
- Second import reuses cached module
- "Logger initialized" appears only once

**✅ Output:**
```
Logger initialized
Hello
```

**💡 Why:** Modules execute once on first import—subsequent imports use cached version.

---

## 4. GUIDED PRACTICE PROBLEMS

### 🔥 Problem 1: The Warm-Up - Utility Library

**📝 Task Description:**
Create a simple math utility library split across multiple files. Build a module structure with basic operations (add, subtract, multiply, divide) in separate files, then combine them through an index.js hub. Finally, use these utilities in a main.js file.

**🎯 Requirements:**
1. Create `add.js`, `subtract.js`, `multiply.js`, `divide.js` - each with one function
2. Create `index.js` that re-exports all operations
3. Create `main.js` that imports from index.js and performs calculations
4. Use named exports throughout
5. Test all four operations

**🚀 Starter Code:**

```javascript
// add.js
// TODO: Export add function

// subtract.js
// TODO: Export subtract function

// multiply.js
// TODO: Export multiply function

// divide.js
// TODO: Export divide function with zero check

// index.js
// TODO: Re-export all functions

// main.js
// TODO: Import and test all operations
```

**✅ Solution:**

```javascript
// ========================================
// add.js
// ========================================
export function add(a, b) {
  return a + b;
}

// ========================================
// subtract.js
// ========================================
export function subtract(a, b) {
  return a - b;
}

// ========================================
// multiply.js
// ========================================
export function multiply(a, b) {
  return a * b;
}

// ========================================
// divide.js
// ========================================
export function divide(a, b) {
  if (b === 0) {
    throw new Error('Division by zero');
  }
  return a / b;
}

// ========================================
// index.js - Module Hub
// ========================================
export { add } from './add.js';
export { subtract } from './subtract.js';
export { multiply } from './multiply.js';
export { divide } from './divide.js';

// Alternative: Export all at once
// export * from './add.js';
// export * from './subtract.js';
// export * from './multiply.js';
// export * from './divide.js';

// ========================================
// main.js
// ========================================
import { add, subtract, multiply, divide } from './index.js';

console.log('=== Math Operations ===');
console.log('10 + 5 =', add(10, 5));          // 15
console.log('10 - 5 =', subtract(10, 5));     // 5
console.log('10 * 5 =', multiply(10, 5));     // 50
console.log('10 / 5 =', divide(10, 5));       // 2

// Test error handling
try {
  console.log('10 / 0 =', divide(10, 0));
} catch (error) {
  console.error('Error:', error.message);    // Division by zero
}
```

**⚠️ Common Mistakes:**

```javascript
// ❌ WRONG - Forgot to export
function add(a, b) {
  return a + b;
}
// Other files can't import this!

// ❌ WRONG - Wrong import syntax
import add from './add.js';  // No default export!

// ❌ WRONG - Missing file extension
import { add } from './add';  // May fail in browser

// ❌ WRONG - Circular dependency in index.js
// index.js importing from main.js that imports from index.js

// ✅ CORRECT - Always export and use proper syntax
export function add(a, b) {
  return a + b;
}
```

---

### 🔥 Problem 2: The Logic Builder - Blog Application

**📝 Task Description:**
Build a modular blog application with separate modules for posts, users, and comments. Implement proper encapsulation with private state and public methods. Create a feature-based module structure that demonstrates real-worlorganization.

**🎯 Requirements:**
1. **post.js**: `Post` class with `create`, `update`, `delete`, `getAll` methods
2. **user.js**: `User` class with `create`, `authenticate`, `getProfile` methods
3. **comment.js**: `Comment` class linked to posts
4. **index.js**: Combined exports hub
5. **main.js**: Demonstrate complete workflow
6. Use private module state (arrays to store data)
7. Mix default and named exports appropriately

**🚀 Starter Code:**

```javascript
// post.js
// TODO: Implement Post management

// user.js
// TODO: Implement User management

// comment.js
// TODO: Implement Comment management

// index.js
// TODO: Create module hub

// main.js
// TODO: Build blog workflow
```

**✅ Solution:**

```javascript
// ========================================
// post.js
// ========================================
const posts = [];  // Private state
let nextId = 1;

export class Post {
  constructor(title, content, authorId) {
    this.id = nextId++;
    this.title = title;
    this.content = content;
    this.authorId = authorId;
    this.createdAt = new Date();
  }
}

export function createPost(title, content, authorId) {
  const post = new Post(title, content, authorId);
  posts.push(post);
  return post;
}

export function updatePost(id, updates) {
  const post = posts.find(p => p.id === id);
  if (!post) {
    throw new Error(`Post ${id} not found`);
  }
  Object.assign(post, updates);
  return post;
}

export function deletePost(id) {
  const index = posts.findIndex(p => p.id === id);
  if (index === -1) {
    throw new Error(`Post ${id} not found`);
  }
  return posts.splice(index, 1)[0];
}

export function getAllPosts() {
  return [...posts];  // Return copy
}

export function getPostById(id) {
  return posts.find(p => p.id === id);
}

// ========================================
// user.js
// ========================================
const users = [];  // Private state
let nextUserId = 1;

export default class User {
  constructor(username, email, password) {
    this.id = nextUserId++;
    this.username = username;
    this.email = email;
    this.password = password;  // In reality, hash this!
    this.createdAt = new Date();
  }
}

export function createUser(username, email, password) {
  // Check if user exists
  if (users.find(u => u.email === email)) {
    throw new Error('User already exists');
  }
  
  const user = new User(username, email, password);
  users.push(user);
  return user;
}

export function authenticate(email, password) {
  const user = users.find(u => u.email === email && u.password === password);
  if (!user) {
    throw new Error('Invalid credentials');
  }
  return user;
}

export function getUser ById(id) {
  return users.find(u => u.id === id);
}

export function getAllUsers() {
  return users.map(({ password, ...user }) => user);  // Remove passwords
}

// ========================================
// comment.js
// ========================================
const comments = [];  // Private state
let nextCommentId = 1;

export class Comment {
  constructor(postId, authorId, content) {
    this.id = nextCommentId++;
    this.postId = postId;
    this.authorId = authorId;
    this.content = content;
    this.createdAt = new Date();
  }
}

export function addComment(postId, authorId, content) {
  const comment = new Comment(postId, authorId, content);
  comments.push(comment);
  return comment;
}

export function getCommentsByPost(postId) {
  return comments.filter(c => c.postId === postId);
}

export function deleteComment(id) {
  const index = comments.findIndex(c => c.id === id);
  if (index === -1) {
    throw new Error(`Comment ${id} not found`);
  }
  return comments.splice(index, 1)[0];
}

// ========================================
// index.js - Module Hub
// ========================================
export { 
  createPost, 
  updatePost, 
  deletePost, 
  getAllPosts, 
  getPostById,
  Post 
} from './post.js';

export { 
  default as User,  // Re-export default as named
  createUser, 
  authenticate, 
  getUserById, 
  getAllUsers 
} from './user.js';

export {
  addComment,
  getCommentsByPost,
  deleteComment,
  Comment
} from './comment.js';

// ========================================
// main.js - Blog Application
// ========================================
import {
  // Post functions
  createPost,
  updatePost,
  getAllPosts,
  getPostById,
  
  // User functions
  User,
  createUser,
  authenticate,
  getUserById,
  
  // Comment functions
  addComment,
  getCommentsByPost
} from './index.js';

console.log('=== Blog Application Demo ===\n');

// 1. Create users
console.log('--- Creating Users ---');
const user1 = createUser('john_doe', 'john@example.com', 'password123');
const user2 = createUser('jane_smith', 'jane@example.com', 'pass456');
console.log(`Created: ${user1.username}, ${user2.username}`);

// 2. Authenticate user
console.log('\n--- Authentication ---');
try {
  const authUser = authenticate('john@example.com', 'password123');
  console.log(`✅ ${authUser.username} authenticated`);
} catch (error) {
  console.error('❌', error.message);
}

// 3. Create posts
console.log('\n--- Creating Posts ---');
const post1 = createPost(
  'Introduction to JavaScript Modules',
  'Modules help organize code...',
  user1.id
);
const post2 = createPost(
  'Advanced Module Patterns',
  'Dynamic imports are powerful...',
  user2.id
);
console.log(`Created posts: "${post1.title}", "${post2.title}"`);

// 4. Update post
console.log('\n--- Updating Post ---');
updatePost(post1.id, {
  content: 'Modules help organize code and improve maintainability!'
});
console.log(`Updated: "${getPostById(post1.id).content}"`);

// 5. Add comments
console.log('\n--- Adding Comments ---');
addComment(post1.id, user2.id, 'Great article!');
addComment(post1.id, user1.id, 'Thanks for reading!');
addComment(post2.id, user1.id, 'Very informative.');

const post1Comments = getCommentsByPost(post1.id);
console.log(`Post 1 has ${post1Comments.length} comments`);

// 6. Display all posts with comments
console.log('\n--- All Posts ---');
getAllPosts().forEach(post => {
  const author = getUserById(post.authorId);
  const comments = getCommentsByPost(post.id);
  
  console.log(`\n📝 ${post.title}`);
  console.log(`   By: ${author.username}`);
  console.log(`   Content: ${post.content.substring(0, 50)}...`);
  console.log(`   Comments: ${comments.length}`);
  
  comments.forEach((comment, idx) => {
    const commentAuthor = getUserById(comment.authorId);
    console.log(`   ${idx + 1}. ${commentAuthor.username}: ${comment.content}`);
  });
});
```

**⚠️ Common Mistakes:**

```javascript
// ❌ WRONG - Exposing private state directly
export const posts = [];  // External code can mutate!

// ✅ CORRECT - Return copies
export function getAllPosts() {
  return [...posts];
}

// ❌ WRONG - No error handling
export function getPostById(id) {
  return posts.find(p => p.id === id);  // Might return undefined
}

// ✅ CORRECT - Validate and throw errors
export function getPostById(id) {
  const post = posts.find(p => p.id === id);
  if (!post) {
    throw new Error(`Post ${id} not found`);
  }
  return post;
}

// ❌ WRONG - Circular imports
// post.js imports from user.js
// user.js imports from post.js
// Creates initialization errors!

// ✅ CORRECT - One-way dependencies
// user.js doesn't import from post.js
// post.js can import from user.js
```

---


### Problem 3: The Pro Challenge - Dynamic Theme Loader

**📝 Task Description:**
Build a theme system that dynamically loads CSS theme modules based on user selection. The themes should be loaded on-demand (not all at once) to optimize performance.

**🎯 Learning Objectives:**
- Use dynamic imports (`import()`)
- Handle Promise-based loading
- Implement error handling
- Create a real-world lazy-loading pattern

**📦 Starter Code:**

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>Theme Loader</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      transition: all 0.3s ease;
    }
    button {
      padding: 10px 20px;
      margin: 5px;
      cursor: pointer;
      border: none;
      border-radius: 5px;
    }
  </style>
</head>
<body>
  <h1>Dynamic Theme Loader</h1>
  <div>
    <button id="light">Light Theme</button>
    <button id="dark">Dark Theme</button>
    <button id="ocean">Ocean Theme</button>
  </div>
  <p id="status">Current theme: None</p>
  <script type="module" src="main.js"></script>
</body>
</html>
```

```javascript
// theme-light.js
// TODO: Export theme object

// theme-dark.js
// TODO: Export theme object

// theme-ocean.js
// TODO: Export theme object

// theme-loader.js
// TODO: Create loadTheme function

// main.js
// TODO: Handle button clicks and load themes
```

**📋 Step-by-Step Requirements:**
1. Create 3 theme modules (light, dark, ocean), each exporting a default theme object with:
   - `name` property
   - `colors` object (background, text, button properties)
   - `apply()` method to set CSS styles
2. In `theme-loader.js`, create an async `loadTheme(themeName)` function that:
   - Dynamically imports the correct theme module
   - Returns the theme object
   - Handles errors if theme doesn't exist
3. In `main.js`:
   - Import the theme loader
   - Add click handlers to buttons
   - Load and apply themes dynamically
   - Show loading status

---

**✅ THE SOLUTION:**

```javascript
// theme-light.js
export default {
  name: 'Light',
  colors: {
    background: '#ffffff',
    text: '#333333',
    buttonBg: '#007bff',
    buttonText: '#ffffff'
  },
  apply() {
    document.body.style.backgroundColor = this.colors.background;
    document.body.style.color = this.colors.text;
    document.querySelectorAll('button').forEach(btn => {
      btn.style.backgroundColor = this.colors.buttonBg;
      btn.style.color = this.colors.buttonText;
    });
  }
};

// theme-dark.js
export default {
  name: 'Dark',
  colors: {
    background: '#1a1a1a',
    text: '#ffffff',
    buttonBg: '#bb86fc',
    buttonText: '#000000'
  },
  apply() {
    document.body.style.backgroundColor = this.colors.background;
    document.body.style.color = this.colors.text;
    document.querySelectorAll('button').forEach(btn => {
      btn.style.backgroundColor = this.colors.buttonBg;
      btn.style.color = this.colors.buttonText;
    });
  }
};

// theme-ocean.js
export default {
  name: 'Ocean',
  colors: {
    background: '#0a4d68',
    text: '#e0f4ff',
    buttonBg: '#088395',
    buttonText: '#ffffff'
  },
  apply() {
    document.body.style.backgroundColor = this.colors.background;
    document.body.style.color = this.colors.text;
    document.querySelectorAll('button').forEach(btn => {
      btn.style.backgroundColor = this.colors.buttonBg;
      btn.style.color = this.colors.buttonText;
    });
  }
};

// theme-loader.js
export async function loadTheme(themeName) {
  const statusEl = document.getElementById('status');
  
  try {
    statusEl.textContent = `Loading ${themeName} theme...`;
    
    // Dynamic import with template literal
    const themeModule = await import(`./theme-${themeName}.js`);
    const theme = themeModule.default;
    
    theme.apply();
    statusEl.textContent = `Current theme: ${theme.name}`;
    
    console.log(`✅ ${theme.name} theme loaded successfully`);
    return theme;
    
  } catch (error) {
    statusEl.textContent = `❌ Failed to load ${themeName} theme`;
    console.error('Theme loading error:', error);
    
    // Fallback to light theme
    const fallback = await import('./theme-light.js');
    fallback.default.apply();
    statusEl.textContent = 'Current theme: Light (fallback)';
    
    return null;
  }
}

// main.js
import { loadTheme } from './theme-loader.js';

// Track currently loaded theme to avoid re-loading
let currentTheme = null;

// Add event listeners to buttons
document.getElementById('light').addEventListener('click', async () => {
  if (currentTheme !== 'light') {
    currentTheme = 'light';
    await loadTheme('light');
  }
});

document.getElementById('dark').addEventListener('click', async () => {
  if (currentTheme !== 'dark') {
    currentTheme = 'dark';
    await loadTheme('dark');
  }
});

document.getElementById('ocean').addEventListener('click', async () => {
  if (currentTheme !== 'ocean') {
    currentTheme = 'ocean';
    await loadTheme('ocean');
  }
});

// Load default theme on page load
loadTheme('light').then(() => {
  currentTheme = 'light';
});
```

**🚨 Common Mistakes:**
- ❌ Not using `async/await` or `.then()` - dynamic imports return Promises
- ❌ Hardcoding file paths instead of using template literals - defeats the purpose of dynamic loading
- ❌ Not handling errors - themes might fail to load
- ❌ Re-loading same theme repeatedly - wastes resources
- ❌ Forgetting to access `.default` on the imported module object
- ❌ Not providing fallback theme - leaves user with broken UI

**💡 Pro Tips:**
- Cache loaded themes to avoid re-importing
- Add loading spinners for better UX
- Consider using `Promise.all()` to preload multiple themes in background
- Use `try...catch` for graceful degradation

---

### Problem 4: Feature-Based Blog Module

**📝 Task Description:**
Build a modular blog system with separate concerns: posts management, comments management, and user authentication. Structure it following feature-based architecture.

**🎯 Learning Objectives:**
- Organize code by feature (not by type)
- Create index files as public APIs
- Use mixed exports (default + named)
- Handle inter-module dependencies

**📦 File Structure:**
```
blog-app/
├── features/
│   ├── posts/
│   │   ├── index.js
│   │   ├── post-model.js
│   │   └── post-service.js
│   ├── comments/
│   │   ├── index.js
│   │   └── comment-service.js
│   └── auth/
│       ├── index.js
│       └── auth-service.js
└── main.js
```

**📋 Step-by-Step Requirements:**
1. **Posts Feature:**
   - `post-model.js`: Post class with id, title, content, author, createdAt
   - `post-service.js`: createPost, getPostById, getAllPosts, deletePost
   - `index.js`: Re-export public API

2. **Comments Feature:**
   - `comment-service.js`: addComment (to a post), getComments (for a post)
   - `index.js`: Re-export

3. **Auth Feature:**
   - `auth-service.js`: login, logout, getCurrentUser, isAuthenticated
   - `index.js`: Re-export

4. **Main.js:**
   - Import from feature index files (not individual modules)
   - Simulate a blog workflow: login → create post → add comment → display

---

**✅ THE SOLUTION:**

```javascript
// features/auth/auth-service.js
let currentUser = null;

export function login(username) {
  if (!username || username.trim() === '') {
    throw new Error('Username is required');
  }
  currentUser = {
    username,
    loginTime: new Date().toISOString()
  };
  console.log(`✅ User "${username}" logged in`);
  return currentUser;
}

export function logout() {
  const user = currentUser;
  currentUser = null;
  console.log(`✅ User "${user?.username}" logged out`);
  return user;
}

export function getCurrentUser() {
  return currentUser;
}

export function isAuthenticated() {
  return currentUser !== null;
}

// features/auth/index.js
export { login, logout, getCurrentUser, isAuthenticated } from './auth-service.js';

// features/posts/post-model.js
export default class Post {
  constructor(title, content, author) {
    this.id = Date.now() + Math.random(); // Simple unique ID
    this.title = title;
    this.content = content;
    this.author = author;
    this.createdAt = new Date().toISOString();
    this.comments = [];
  }

  addComment(comment) {
    this.comments.push(comment);
  }

  getCommentsCount() {
    return this.comments.length;
  }
}

// features/posts/post-service.js
import Post from './post-model.js';
import { getCurrentUser, isAuthenticated } from '../auth/index.js';

const posts = [];

export function createPost(title, content) {
  if (!isAuthenticated()) {
    throw new Error('You must be logged in to create a post');
  }
  
  const author = getCurrentUser().username;
  const post = new Post(title, content, author);
  posts.push(post);
  
  console.log(`✅ Post created: "${title}" by ${author}`);
  return post;
}

export function getPostById(id) {
  return posts.find(post => post.id === id) || null;
}

export function getAllPosts() {
  return [...posts]; // Return copy
}

export function deletePost(id) {
  if (!isAuthenticated()) {
    throw new Error('You must be logged in to delete a post');
  }
  
  const index = posts.findIndex(post => post.id === id);
  if (index !== -1) {
    const deleted = posts.splice(index, 1)[0];
    console.log(`✅ Post deleted: "${deleted.title}"`);
    return deleted;
  }
  return null;
}

// features/posts/index.js
export { default as Post } from './post-model.js';
export { createPost, getPostById, getAllPosts, deletePost } from './post-service.js';

// features/comments/comment-service.js
import { getPostById } from '../posts/index.js';
import { getCurrentUser, isAuthenticated } from '../auth/index.js';

export function addComment(postId, text) {
  if (!isAuthenticated()) {
    throw new Error('You must be logged in to comment');
  }
  
  const post = getPostById(postId);
  if (!post) {
    throw new Error('Post not found');
  }
  
  const comment = {
    id: Date.now() + Math.random(),
    author: getCurrentUser().username,
    text,
    createdAt: new Date().toISOString()
  };
  
  post.addComment(comment);
  console.log(`✅ Comment added to "${post.title}" by ${comment.author}`);
  return comment;
}

export function getComments(postId) {
  const post = getPostById(postId);
  return post ? post.comments : [];
}

// features/comments/index.js
export { addComment, getComments } from './comment-service.js';

// main.js
import { login, logout, getCurrentUser } from './features/auth/index.js';
import { createPost, getAllPosts } from './features/posts/index.js';
import { addComment, getComments } from './features/comments/index.js';

console.log('=== Blog Application Demo ===\n');

try {
  // 1. User logs in
  login('john_doe');
  console.log('Current user:', getCurrentUser());
  console.log('');

  // 2. Create posts
  const post1 = createPost(
    'Learning JavaScript Modules',
    'Today I learned about import and export...'
  );
  const post2 = createPost(
    'Async JavaScript',
    'Promises and async/await are amazing!'
  );
  console.log('');

  // 3. Add comments
  addComment(post1.id, 'Great post! Very helpful.');
  addComment(post1.id, 'Thanks for sharing!');
  addComment(post2.id, 'I love async/await too!');
  console.log('');

  // 4. Display all posts with comments
  console.log('=== All Blog Posts ===');
  getAllPosts().forEach(post => {
    console.log(`\n📝 ${post.title}`);
    console.log(`   By: ${post.author}`);
    console.log(`   Content: ${post.content}`);
    console.log(`   Comments (${post.getCommentsCount()}):`);
    
    getComments(post.id).forEach(comment => {
      console.log(`   - ${comment.author}: ${comment.text}`);
    });
  });

  // 5. Logout
  console.log('');
  logout();

} catch (error) {
  console.error('❌ Error:', error.message);
}
```

**🚨 Common Mistakes:**
- ❌ Importing from internal modules directly instead of using index.js
- ❌ Not checking authentication before operations
- ❌ Circular dependency issues (auth importing from posts, posts importing from auth)
- ❌ Mutating returned arrays/objects without protection
- ❌ Not handling "not found" scenarios

**💡 Architecture Benefits:**
- ✅ Each feature is self-contained
- ✅ Index files act as public APIs (hide internal structure)
- ✅ Easy to test individual features
- ✅ Can move entire feature folders around
- ✅ Clear dependency relationships

---

### Problem 5: Advanced - Module-Based Plugin System

**📝 Task Description:**
Create a plugin system where plugins can be dynamically loaded and registered. The system should support plugin lifecycle hooks (init, destroy) and provide a shared API to plugins.

**🎯 Learning Objectives:**
- Advanced dynamic imports
- Plugin architecture pattern
- Shared module state
- Error boundaries for plugins

**📋 Step-by-Step Requirements:**
1. Create `plugin-manager.js` that:
   - Maintains a registry of loaded plugins
   - Provides `registerPlugin(name, module)` function
   - Provides `loadPlugin(pluginName)` async function
   - Provides `getPlugin(name)` function
   - Provides `executeAll(hookName, ...args)` to run hooks

2. Create 3 sample plugins:
   - `logger-plugin.js`: Logs actions
   - `analytics-plugin.js`: Tracks events
   - `validator-plugin.js`: Validates data

3. Each plugin must export:
   - `name` property
   - `init()` function
   - `destroy()` function
   - Custom methods

4. In `main.js`:
   - Load plugins dynamically based on config
   - Initialize all plugins
   - Use plugin methods
   - Clean up on exit

---

**✅ THE SOLUTION:**

```javascript
// plugin-manager.js
const plugins = new Map();

export function registerPlugin(pluginModule) {
  if (!pluginModule.name) {
    throw new Error('Plugin must have a name property');
  }
  
  if (plugins.has(pluginModule.name)) {
    console.warn(`⚠️  Plugin "${pluginModule.name}" is already registered`);
    return false;
  }
  
  plugins.set(pluginModule.name, pluginModule);
  console.log(`✅ Plugin "${pluginModule.name}" registered`);
  return true;
}

export async function loadPlugin(pluginName) {
  try {
    const pluginModule = await import(`./plugins/${pluginName}.js`);
    const plugin = pluginModule.default;
    registerPlugin(plugin);
    return plugin;
  } catch (error) {
    console.error(`❌ Failed to load plugin "${pluginName}":`, error.message);
    return null;
  }
}

export function getPlugin(name) {
  return plugins.get(name) || null;
}

export function getAllPlugins() {
  return Array.from(plugins.values());
}

export async function initializeAll() {
  console.log('\n🚀 Initializing all plugins...');
  const results = await Promise.allSettled(
    getAllPlugins().map(plugin => {
      if (typeof plugin.init === 'function') {
        return plugin.init();
      }
      return Promise.resolve();
    })
  );
  
  results.forEach((result, index) => {
    const plugin = getAllPlugins()[index];
    if (result.status === 'fulfilled') {
      console.log(`  ✅ ${plugin.name} initialized`);
    } else {
      console.error(`  ❌ ${plugin.name} failed:`, result.reason);
    }
  });
}

export async function destroyAll() {
  console.log('\n🧹 Destroying all plugins...');
  for (const plugin of getAllPlugins()) {
    if (typeof plugin.destroy === 'function') {
      await plugin.destroy();
      console.log(`  ✅ ${plugin.name} destroyed`);
    }
  }
  plugins.clear();
}

export async function executeHook(hookName, ...args) {
  const results = [];
  for (const plugin of getAllPlugins()) {
    if (typeof plugin[hookName] === 'function') {
      try {
        const result = await plugin[hookName](...args);
        results.push({ plugin: plugin.name, result });
      } catch (error) {
        console.error(`❌ Hook "${hookName}" failed in ${plugin.name}:`, error);
      }
    }
  }
  return results;
}

// plugins/logger-plugin.js
const logs = [];

export default {
  name: 'logger',
  
  init() {
    console.log('Logger plugin starting...');
    this.log('info', 'Logger initialized');
  },
  
  destroy() {
    console.log(`Logger plugin shutting down (captured ${logs.length} logs)`);
    logs.length = 0; // Clear logs
  },
  
  log(level, message) {
    const entry = {
      level,
      message,
      timestamp: new Date().toISOString()
    };
    logs.push(entry);
    console.log(`[${level.toUpperCase()}] ${message}`);
  },
  
  getLogs() {
    return [...logs];
  },
  
  getLogCount() {
    return logs.length;
  }
};

// plugins/analytics-plugin.js
const events = [];

export default {
  name: 'analytics',
  
  init() {
    console.log('Analytics plugin starting...');
    this.track('plugin_initialized', { plugin: 'analytics' });
  },
  
  destroy() {
    console.log(`Analytics plugin shutting down (tracked ${events.length} events)`);
    this.sendBatch(); // Send remaining events
    events.length = 0;
  },
  
  track(eventName, properties = {}) {
    const event = {
      name: eventName,
      properties,
      timestamp: Date.now()
    };
    events.push(event);
    console.log(`📊 Event tracked: ${eventName}`);
  },
  
  sendBatch() {
    if (events.length === 0) return;
    console.log(`📤 Sending ${events.length} events to analytics server...`);
    // Simulate API call
    console.log('Events:', events);
  },
  
  getEventCount() {
    return events.length;
  }
};

// plugins/validator-plugin.js
const validationRules = new Map();

export default {
  name: 'validator',
  
  init() {
    console.log('Validator plugin starting...');
    // Register default rules
    this.addRule('email', (value) => {
      const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      return regex.test(value);
    });
    this.addRule('minLength', (value, min) => {
      return value.length >= min;
    });
    this.addRule('required', (value) => {
      return value !== null && value !== undefined && value !== '';
    });
  },
  
  destroy() {
    console.log('Validator plugin shutting down');
    validationRules.clear();
  },
  
  addRule(name, validatorFn) {
    validationRules.set(name, validatorFn);
  },
  
  validate(ruleName, value, ...args) {
    const rule = validationRules.get(ruleName);
    if (!rule) {
      throw new Error(`Validation rule "${ruleName}" not found`);
    }
    return rule(value, ...args);
  },
  
  validateObject(obj, schema) {
    const errors = [];
    for (const [field, rules] of Object.entries(schema)) {
      for (const [ruleName, ruleValue] of Object.entries(rules)) {
        const isValid = this.validate(ruleName, obj[field], ruleValue);
        if (!isValid) {
          errors.push(`${field} failed ${ruleName} validation`);
        }
      }
    }
    return {
      isValid: errors.length === 0,
      errors
    };
  }
};

// main.js
import {
  loadPlugin,
  getPlugin,
  initializeAll,
  destroyAll,
  executeHook
} from './plugin-manager.js';

async function runPluginSystem() {
  console.log('=== Plugin System Demo ===\n');
  
  try {
    // 1. Load plugins dynamically
    console.log('📦 Loading plugins...');
    const pluginsToLoad = ['logger-plugin', 'analytics-plugin', 'validator-plugin'];
    
    await Promise.all(pluginsToLoad.map(name => loadPlugin(name)));
    
    // 2. Initialize all plugins
    await initializeAll();
    
    console.log('\n--- Using Plugins ---\n');
    
    // 3. Use logger plugin
    const logger = getPlugin('logger');
    logger.log('info', 'Application started');
    logger.log('warning', 'Low memory detected');
    logger.log('error', 'Failed to connect to database');
    console.log(`Total logs: ${logger.getLogCount()}`);
    
    // 4. Use analytics plugin
    const analytics = getPlugin('analytics');
    analytics.track('page_view', { page: '/home' });
    analytics.track('button_click', { button: 'submit' });
    analytics.track('form_submit', { form: 'contact' });
    console.log(`Total events: ${analytics.getEventCount()}\n`);
    
    // 5. Use validator plugin
    const validator = getPlugin('validator');
    
    const userData = {
      email: 'test@example.com',
      password: 'secret123',
      username: 'john'
    };
    
    const schema = {
      email: { required: true, email: true },
      password: { required: true, minLength: 8 },
      username: { required: true, minLength: 3 }
    };
    
    const validation = validator.validateObject(userData, schema);
    console.log('Validation result:', validation);
    
    // 6. Test with invalid data
    const invalidData = {
      email: 'invalid-email',
      password: '123',
      username: ''
    };
    
    const invalidValidation = validator.validateObject(invalidData, schema);
    console.log('Invalid data validation:', invalidValidation);
    
    // 7. Execute custom hook across all plugins
    console.log('\n--- Custom Hook Execution ---');
    await executeHook('beforeAppClose');
    
    // 8. Cleanup
    await destroyAll();
    
  } catch (error) {
    console.error('❌ Plugin system error:', error);
  }
}

// Run the system
runPluginSystem();
```

**🚨 Common Mistakes:**
- ❌ Not using `Promise.allSettled()` for plugin initialization (one failure breaks all)
- ❌ Not providing error boundaries for plugin execution
- ❌ Forgetting to clean up plugin state in `destroy()`
- ❌ Not validating plugin structure before registration
- ❌ Hardcoding plugin names instead of using dynamic imports

**💡 Real-World Applications:**
- CMS plugin systems (WordPress-style)
- Browser extension architectures
- VS Code extension system
- Game mod loaders
- Micro-frontend architectures

---

## 5. DEBUG CHALLENGES

### 🐛 Challenge 1: Missing File Extension
```javascript
// math.js
export function add(a, b) {
  return a + b;
}

// main.js
import { add } from './math'; // ❌ BUG HERE
console.log(add(5, 3));
```

**❓ What's Wrong?**
<details>
<summary>Click to see the bug</summary>

**🐛 The Bug:** Missing `.js` file extension in the import statement.

**✅ Fixed Code:**
```javascript
import { add } from './math.js'; // ✅ Must include extension
```

**📚 Explanation:** ES6 modules require explicit file extensions. This is different from Node.js CommonJS where extensions are optional. Browsers need the full path to fetch the module file.
</details>

---

### 🐛 Challenge 2: Wrong Import Syntax
```javascript
// calculator.js
export default class Calculator {
  add(a, b) { return a + b; }
}

// main.js
import { Calculator } from './calculator.js'; // ❌ BUG HERE
const calc = new Calculator();
console.log(calc.add(2, 3));
```

**❓ What's Wrong?**
<details>
<summary>Click to see the bug</summary>

**🐛 The Bug:** Using named import syntax `{ Calculator }` for a default export.

**✅ Fixed Code:**
```javascript
import Calculator from './calculator.js'; // ✅ No braces for default export
```

**📚 Explanation:** Default exports are imported WITHOUT curly braces. Named exports require braces. This is one of the most common module mistakes.
</details>

---

### 🐛 Challenge 3: Circular Dependency
```javascript
// a.js
import { b } from './b.js';
export const a = 'A';
console.log('In a.js, b is:', b);

// b.js
import { a } from './a.js';
export const b = 'B';
console.log('In b.js, a is:', a); // ❌ BUG: a is undefined

// main.js
import './a.js';
```

**❓ What's Wrong?**
<details>
<summary>Click to see the bug</summary>

**🐛 The Bug:** Circular dependency causes `a` to be `undefined` when `b.js` tries to access it.

**✅ Fixed Code - Approach 1 (Restructure):**
```javascript
// shared.js
export const a = 'A';
export const b = 'B';

// a.js
import { b } from './shared.js';
console.log('Using b:', b);

// b.js
import { a } from './shared.js';
console.log('Using a:', a);
```

**✅ Fixed Code - Approach 2 (Lazy Access):**
```javascript
// a.js
import { getB } from './b.js';
export const a = 'A';
console.log('In a.js, b is:', getB()); // Call function instead

// b.js
import { a } from './a.js';
export const b = 'B';
export function getB() { return b; }
console.log('In b.js, a is:', a);
```

**📚 Explanation:** Circular dependencies occur when two modules import each other. The best fix is to extract shared code into a third module. If unavoidable, use functions to lazy-load values.
</details>

---

### 🐛 Challenge 4: Namespace vs Named Import
```javascript
// utils.js
export function greet() { return 'Hello'; }
export function farewell() { return 'Goodbye'; }

// main.js
import * as Utils from './utils.js';
console.log(greet()); // ❌ BUG HERE
console.log(Utils.greet()); // This would work
```

**❓ What's Wrong?**
<details>
<summary>Click to see the bug</summary>

**🐛 The Bug:** Using namespace import (`import * as Utils`) but trying to access functions directly without the namespace.

**✅ Fixed Code:**
```javascript
// Option 1: Use the namespace
import * as Utils from './utils.js';
console.log(Utils.greet());
console.log(Utils.farewell());

// Option 2: Use named imports instead
import { greet, farewell } from './utils.js';
console.log(greet());
console.log(farewell());
```

**📚 Explanation:** When you use `import * as Name`, all exports are grouped under that `Name` object. You must access them as `Name.export`. If you want direct access, use named imports with braces.
</details>

---

### 🐛 Challenge 5: Dynamic Import Not Awaited
```javascript
// data.js
export const users = ['Alice', 'Bob', 'Charlie'];

// main.js
function loadData() {
  const module = import('./data.js'); // ❌ BUG HERE
  console.log(module.users); // undefined
}
loadData();
```

**❓ What's Wrong?**
<details>
<summary>Click to see the bug</summary>

**🐛 The Bug:** Dynamic `import()` returns a Promise, but the code doesn't wait for it to resolve.

**✅ Fixed Code - Option 1 (async/await):**
```javascript
async function loadData() {
  const module = await import('./data.js'); // ✅ Wait for Promise
  console.log(module.users); // ['Alice', 'Bob', 'Charlie']
}
loadData();
```

**✅ Fixed Code - Option 2 (.then()):**
```javascript
function loadData() {
  import('./data.js').then(module => {
    console.log(module.users); // ['Alice', 'Bob', 'Charlie']
  });
}
loadData();
```

**📚 Explanation:** `import()` is asynchronous and returns a Promise. You MUST use `await` or `.then()` to access the loaded module. This is different from static imports which are synchronous.
</details>

---

## 6. COMMON PITFALLS & ANTI-PATTERNS

### ❌ Anti-Pattern 1: Importing Entire Library When You Need One Function

**Don't Do This:**
```javascript
import * as _ from 'lodash';

const result = _.chunk([1, 2, 3, 4], 2);
```

**✅ Do This:**
```javascript
import { chunk } from 'lodash';

const result = chunk([1, 2, 3, 4], 2);
```

**Why:** Importing everything prevents tree shaking. Your bundle includes the ENTIRE library even though you only use one function. This drastically increases bundle size.

---

### ❌ Anti-Pattern 2: Exporting Objects Instead of Individual Functions

**Don't Do This:**
```javascript
// utils.js
export default {
  add(a, b) { return a + b; },
  subtract(a, b) { return a - b; },
  multiply(a, b) { return a * b; }
};

// main.js
import utils from './utils.js';
utils.add(2, 3); // Can't tree-shake unused methods
```

**✅ Do This:**
```javascript
// utils.js
export function add(a, b) { return a + b; }
export function subtract(a, b) { return a - b; }
export function multiply(a, b) { return a * b; }

// main.js
import { add } from './utils.js'; // Only add() in bundle
```

**Why:** Default object exports are NOT tree-shakable. Bundlers can't determine which methods are used. Named exports allow dead code elimination.

---

### ❌ Anti-Pattern 3: Deep Import Paths

**Don't Do This:**
```javascript
import { Button } from '../../../components/ui/buttons/Button.js';
import { Modal } from '../../../components/ui/modals/Modal.js';
```

**✅ Do This:**
```javascript
// components/ui/index.js (barrel export)
export { Button } from './buttons/Button.js';
export { Modal } from './modals/Modal.js';

// main.js
import { Button, Modal } from './components/ui/index.js';
```

**Why:** Deep paths are fragile and hard to refactor. Index files create clean public APIs and hide internal structure.

---

### ❌ Anti-Pattern 4: Side Effects in Module Initialization

**Don't Do This:**
```javascript
// config.js
export const config = { theme: 'dark' };

// Immediately mutates DOM on import! ❌
document.body.style.backgroundColor = config.theme === 'dark' ? '#000' : '#fff';
```

**✅ Do This:**
```javascript
// config.js
export const config = { theme: 'dark' };

export function applyTheme() {
  document.body.style.backgroundColor = config.theme === 'dark' ? '#000' : '#fff';
}

// main.js
import { config, applyTheme } from './config.js';
applyTheme(); // Explicit, controlled timing
```

**Why:** Side effects during import make code unpredictable and hard to test. Keep modules pure; export functions that consumers can call explicitly.

---

### ❌ Anti-Pattern 5: Not Handling Dynamic Import Failures

**Don't Do This:**
```javascript
async function loadFeature(name) {
  const module = await import(`./features/${name}.js`);
  return module.default; // ❌ No error handling
}
```

**✅ Do This:**
```javascript
async function loadFeature(name) {
  try {
    const module = await import(`./features/${name}.js`);
    return module.default;
  } catch (error) {
    console.error(`Failed to load feature "${name}":`, error);
    // Return fallback or default feature
    return await import('./features/default.js').then(m => m.default);
  }
}
```

**Why:** Dynamic imports can fail (network errors, missing files, syntax errors). Always wrap them in try-catch and provide fallbacks.

---

### ❌ Anti-Pattern 6: Mixing CommonJS and ES Modules

**Don't Do This:**
```javascript
// module.js
export const value = 42;

// main.js
const { value } = require('./module.js'); // ❌ Mixing syntax
```

**✅ Do This:**
```javascript
// Use ES modules consistently
import { value } from './module.js';

// OR use CommonJS consistently (Node.js)
const { value } = require('./module.js');
```

**Why:** Mixing module systems causes compatibility issues. Choose one system and stick with it throughout your project.

---

## 7. KNOWLEDGE CHECK MCQs

**Q1:** Which statement is TRUE about ES6 modules?
- A) They are loaded synchronously
- B) They must use `.mjs` extension
- C) They are hoisted and executed before other code
- D) They require `type="module"` in script tags

<details>
<summary>Answer</summary>

**✅ D) They require `type="module"` in script tags**

**Explanation:** When using ES6 modules in HTML, you MUST add `type="module"` to the script tag. Modules are actually loaded asynchronously (not synchronously), `.js` extension works fine (`.mjs` is optional), and while imports are hoisted, module execution follows import order.
</details>

---

**Q2:** What's the output?
```javascript
// math.js
export default function add(a, b) { return a + b; }
export const PI = 3.14;

// main.js
import add, { PI } from './math.js';
console.log(typeof add, typeof PI);
```
- A) `undefined undefined`
- B) `function number`
- C) `object number`
- D) `function function`

<details>
<summary>Answer</summary>

**✅ B) `function number`**

**Explanation:** `add` is a function (default export), and `PI` is a number (named export). The syntax correctly mixes default and named imports.
</details>

---

**Q3:** Which import syntax is CORRECT for a default export?
- A) `import { Calculator } from './calc.js'`
- B) `import Calculator from './calc.js'`
- C) `import * as Calculator from './calc.js'`
- D) `import default Calculator from './calc.js'`

<details>
<summary>Answer</summary>

**✅ B) `import Calculator from './calc.js'`**

**Explanation:** Default exports are imported WITHOUT curly braces. Option A is for named exports, C is for namespace imports, and D uses invalid syntax.
</details>

---

**Q4:** What does tree shaking do?
- A) Sorts imports alphabetically
- B) Removes unused exports from bundles
- C) Compresses JavaScript files
- D) Validates module syntax

<details>
<summary>Answer</summary>

**✅ B) Removes unused exports from bundles**

**Explanation:** Tree shaking is a build optimization that eliminates dead code by removing exports that are never imported, reducing bundle size.
</details>

---

**Q5:** Which is TRUE about dynamic imports?
- A) They return the module directly
- B) They block code execution
- C) They return a Promise
- D) They can only be used in Node.js

<details>
<summary>Answer</summary>

**✅ C) They return a Promise**

**Explanation:** `import()` is asynchronous and returns a Promise that resolves to the module object. You must use `await` or `.then()` to access it.
</details>

---

**Q6:** What's the purpose of `export * from './module.js'`?
- A) Export default from another module
- B) Re-export all named exports from another module
- C) Import all exports as a namespace
- D) Delete all exports

<details>
<summary>Answer</summary>

**✅ B) Re-export all named exports from another module**

**Explanation:** `export * from` re-exports all NAMED exports from the specified module, creating a hub pattern. It does NOT re-export the default export.
</details>

---

**Q7:** What's wrong with this code?
```javascript
import { users } from './data.js';
users = []; // Clear the array
```
- A) Nothing, it works fine
- B) Imported bindings are read-only
- C) Missing semicolon
- D) Wrong import syntax

<details>
<summary>Answer</summary>

**✅ B) Imported bindings are read-only**

**Explanation:** Imported values are immutable bindings. You cannot reassign them. While you CAN mutate the array's contents (`users.push()`, `users.length = 0`), you cannot reassign the binding itself.
</details>

---

**Q8:** What does `import * as Utils from './utils.js'` do?
- A) Imports only the default export
- B) Creates a namespace object with all exports
- C) Imports all files in the directory
- D) Imports nothing

<details>
<summary>Answer</summary>

**✅ B) Creates a namespace object with all exports**

**Explanation:** `import * as Name` creates an object where all named exports become properties. Access them as `Name.exportName`. The default export (if any) becomes `Name.default`.
</details>

---

**Q9:** When should you use dynamic imports?
- A) For all imports to improve performance
- B) For conditional/lazy loading of modules
- C) Only in Node.js applications
- D) To import CSS files

<details>
<summary>Answer</summary>

**✅ B) For conditional/lazy loading of modules**

**Explanation:** Dynamic imports are perfect for code splitting and loading modules on-demand (e.g., admin panels, heavy libraries, theme switchers). They shouldn't replace all static imports as they add async complexity.
</details>

---

**Q10:** Which file extension is REQUIRED in ES6 module imports?
- A) No extension needed
- B) `.js` must be included
- C) `.mjs` must be used
- D) Any extension works

<details>
<summary>Answer</summary>

**✅ B) `.js` must be included**

**Explanation:** ES6 modules require explicit file extensions in import paths. While `.mjs` can be used for ES modules, `.js` is the standard and MUST be included (unlike CommonJS where it's optional).
</details>

---

**Q11:** What's the maximum number of default exports per module?
- A) Unlimited
- B) One
- C) Two
- D) Three

<details>
<summary>Answer</summary>

**✅ B) One**

**Explanation:** A module can have only ONE default export (but unlimited named exports). This is a fundamental rule of ES6 modules.
</details>

---

**Q12:** What happens if you import a non-existent module?
- A) Returns `undefined`
- B) Returns an empty object
- C) Throws an error
- D) Creates the module automatically

<details>
<summary>Answer</summary>

**✅ C) Throws an error**

**Explanation:** Importing a non-existent module throws a module resolution error. This applies to both static and dynamic imports (dynamic imports reject the Promise).
</details>

---

**Q13:** Which is the BEST practice for utility functions?
- A) Export as default object with methods
- B) Export each function individually as named export
- C) Use namespace imports everywhere
- D) Put all functions in global scope

<details>
<summary>Answer</summary>

**✅ B) Export each function individually as named export**

**Explanation:** Named exports enable tree shaking, allowing bundlers to remove unused functions. This results in smaller bundle sizes compared to default object exports.
</details>

---

**Q14:** Can you use `await` with static imports?
```javascript
const module = await import { add } from './math.js';
```
- A) Yes, it's required
- B) No, static imports don't return Promises
- C) Only in async functions
- D) Only in Node.js

<details>
<summary>Answer</summary>

**✅ B) No, static imports don't return Promises**

**Explanation:** Static `import` statements are not function calls and don't return Promises. They're declarations that are processed before code runs. Only dynamic `import()` returns a Promise.
</details>

---

**Q15:** What's the purpose of creating `index.js` files?
- A) Required by JavaScript specification
- B) Create a public API / barrel export
- C) Improve performance
- D) Enable tree shaking

<details>
<summary>Answer</summary>

**✅ B) Create a public API / barrel export**

**Explanation:** Index files (barrel exports) act as a public API for a module/feature, re-exporting selected items. This hides internal structure, simplifies imports, and makes refactoring easier.
</details>

---

## 8. REAL INTERVIEW QUESTIONS

### 🎤 Question 1: Explain the difference between named and default exports. When would you use each?

**👶 Junior Answer:**
"Named exports use curly braces when importing, and you can have multiple per file. Default exports don't use curly braces and you can only have one per file. I'd use named exports for utility functions and default for main classes."

**🎯 Senior Answer:**
"Named exports create explicit bindings that enable tree shaking—bundlers can statically analyze which exports are used and eliminate dead code. They're ideal for utility libraries where consumers might need only specific functions. Default exports are best for modules with a single primary export, like a React component or configuration object.

The key trade-off is tree-shakability versus convenience. Named exports require exact names (unless aliased), which catches typos at compile time. Default exports offer flexibility in naming but prevent effective tree shaking when used with object literals containing methods.

I follow this pattern: named exports for utilities/services with multiple functions, default exports for classes, components, or when there's a clear 'main' export. I avoid default object exports like `export default { fn1, fn2 }` because they bundle everything together, negating tree shaking benefits."

---

### 🎤 Question 2: How do circular dependencies occur and how do you handle them?

**👶 Junior Answer:**
"Circular dependencies happen when module A imports from module B, and module B imports from module A. This creates a loop. To fix it, I'd move the shared code to a third file that both can import from."

**🎯 Senior Answer:**
"Circular dependencies occur when modules form an import cycle: A → B → A. JavaScript handles this by returning partially-initialized modules—when B tries to import from A while A is still loading, B receives A's exports in their current (incomplete) state, often resulting in `undefined` values.

There are several strategies:

**1. Restructure (Best):** Extract shared dependencies into a third module, breaking the cycle.

**2. Lazy initialization:** Use functions to defer access:
```javascript
// a.js
import { getB } from './b.js';
export const a = 'A';
export function useB() { return getB(); } // Deferred access
```

**3. Dependency injection:** Pass dependencies as parameters rather than importing them.

**4. Interface segregation:** Split modules by concern—often circular deps indicate modules are too tightly coupled.

I use ESLint's `import/no-cycle` rule to catch these during development. The root cause is usually a violation of the acyclic dependencies principle, signaling a need to rethink module boundaries."

---

### 🎤 Question 3: Explain how dynamic imports work and their performance implications.

**👶 Junior Answer:**
"Dynamic imports use `import()` as a function and return a Promise. They load modules at runtime instead of at the start. This is good for performance because you only load code when you need it."

**🎯 Senior Answer:**
"Dynamic imports leverage the `import()` function which returns a Promise resolving to the module object. Unlike static imports that are parsed and loaded during the module resolution phase before code execution, dynamic imports are runtime operations triggered by code flow.

**Performance implications:**

**Benefits:**
- **Code splitting:** Webpack/Rollup automatically create separate chunks, reducing initial bundle size
- **Lazy loading:** Heavy features load on-demand (e.g., admin panels, chart libraries)
- **Conditional loading:** Different modules for mobile/desktop or feature flags
- **Route-based splitting:** Load page components only when navigated to

**Trade-offs:**
- **Latency:** Network round-trip for each chunk (mitigated with prefetching: `<link rel="prefetch">`)
- **Waterfall risk:** Sequential dynamic imports create loading chains
- **Cache complexity:** More chunks mean more cache keys to manage

**Best practices I follow:**
```javascript
// Parallel loading when multiple modules needed
const [auth, utils] = await Promise.all([
  import('./auth.js'),
  import('./utils.js')
]);

// Preload for predictable user flows
if (user.isAdmin) {
  import('./admin-panel.js'); // Fire and forget
}

// Chunk naming for debugging
const module = await import(
  /* webpackChunkName: "admin" */ './admin.js'
);
```

I use dynamic imports for features >50KB that aren't needed immediately, measuring the impact with Lighthouse's code splitting audit."

---

### 🎤 Question 4: How would you structure a large-scale application using modules?

**👶 Junior Answer:**
"I'd organize files by what they do—put all components in a components folder, all utilities in a utils folder, etc. Then use imports to connect them together."

**🎯 Senior Answer:**
"I advocate for **feature-based architecture** over type-based organization. Instead of grouping by technical type (components/, services/, utils/), I group by business domain:

```
src/
├── features/
│   ├── authentication/
│   │   ├── index.js              // Public API
│   │   ├── auth-service.js       // Internal
│   │   ├── auth-context.js       // Internal
│   │   └── __tests__/
│   │
│   ├── products/
│   │   ├── index.js
│   │   ├── product-list.js
│   │   ├── product-detail.js
│   │   └── product-service.js
│   │
├── shared/
│   ├── components/               // Cross-cutting UI
│   ├── hooks/
│   └── utils/
│
└── core/
    ├── api/                      // HTTP client
    ├── config/
    └── router/
```

**Key principles:**

**1. Index files as contracts:** Each feature exports a public API via `index.js`, hiding internals:
```javascript
// features/auth/index.js
export { login, logout } from './auth-service.js';
export { useAuth } from './auth-context.js';
// auth-storage.js NOT exported—internal implementation
```

**2. Dependency rules:**
- Features can import from `shared/` and `core/`
- Features CANNOT import from other features directly
- Cross-feature needs? Lift to `shared/` or use event bus

**3. Colocate related code:**
```
products/
├── index.js
├── ProductList.js
├── ProductList.test.js          // Tests alongside
├── ProductList.module.css        // Styles alongside
└── product-types.ts              // Types alongside
```

**4. Lazy load features:**
```javascript
const AuthModule = lazy(() => import('./features/authentication'));
const ProductsModule = lazy(() => import('./features/products'));
```

This scales to hundreds of features because each is self-contained, testable in isolation, and clearly owns its domain. New developers can contribute to one feature without understanding the entire codebase."

---

## 9. DEVTOOLS & DEBUGGING

### 🔍 Chrome DevTools for Module Debugging

#### **1. Network Tab - Module Loading Analysis**

**How to inspect:**
1. Open DevTools (`F12`)
2. Go to **Network** tab
3. Filter by **JS**
4. Reload page (`Ctrl+R`)

**What to look for:**
```
✅ Check module load order:
   main.js → utils.js → config.js

✅ Identify slow modules:
   Look for long "Waiting (TTFB)" times

✅ Verify code splitting:
   See separate chunks: main.chunk.js, admin.chunk.js

⚠️ Red flags:
   - Duplicate module loads (caching issue)
   - Very small chunks (<10KB) = too much splitting
   - Sequential waterfalls (should parallelize)
```

**Example Analysis:**
```
Name              Size    Time    Initiator
main.js           45KB    120ms   (index.html)
├─ utils.js       12KB    45ms    main.js:5
├─ api.js         8KB     38ms    main.js:6
└─ config.js      2KB     15ms    main.js:7
```

---

#### **2. Sources Tab - Setting Breakpoints in Modules**

**Debugging imported functions:**
```javascript
// math.js
export function calculate(x) {
  debugger; // ← Breakpoint here
  return x * 2;
}

// main.js
import { calculate } from './math.js';
calculate(5);
```

**In DevTools Sources:**
1. Open `math.js` from file tree (left panel)
2. Click line number to set breakpoint
3. Hover over variables to inspect values
4. Use **Call Stack** panel to see import chain

**Pro Tip:** Use **Conditional Breakpoints**
```javascript
// Right-click line number → Add conditional breakpoint
x > 100  // Only break when x exceeds 100
```

---

#### **3. Console - Module Introspection**

**Inspect module exports in real-time:**
```javascript
// In browser console (when using type="module")
import('./math.js').then(module => {
  console.log(module);
  // {add: ƒ, subtract: ƒ, PI: 3.14159, default: ƒ}
  
  console.log(Object.keys(module));
  // ['add', 'subtract', 'PI', 'default']
});
```

**Check if module is cached:**
```javascript
// Modules are cached after first import
const mod1 = await import('./utils.js');
const mod2 = await import('./utils.js');
console.log(mod1 === mod2); // true - same reference
```

**Debug import failures:**
```javascript
try {
  const mod = await import('./missing.js');
} catch (err) {
  console.error('Failed to load:', err.message);
  // TypeError: Failed to fetch dynamically imported module
}
```

---

#### **4. Performance Tab - Module Parse/Compile Times**

**Measure module impact:**
1. Open **Performance** tab
2. Click **Record** (●)
3. Load your page
4. Stop recording
5. Look for **"Evaluate Script"** entries

**What to analyze:**
```
Script Evaluation Timeline:
├─ Parse: 12ms        (Syntax analysis)
├─ Compile: 8ms       (Bytecode generation)
└─ Execute: 45ms      (Running top-level code)

⚠️ Red flags:
   - Parse >50ms = very large file, consider splitting
   - Execute >100ms = heavy top-level code, defer it
```

---

#### **5. Application Tab - Module Scripts**

**View loaded modules:**
1. **Application** tab → **Frames** → `top`
2. Expand **Scripts** section
3. See all loaded module files

**Useful for:**
- Verifying all modules loaded correctly
- Checking for unexpected module loads
- Inspecting service worker module caching

---

### 🛠️ Common Module Errors & How to Debug

#### **Error 1: "Failed to resolve module specifier"**
```
Uncaught TypeError: Failed to resolve module specifier 'utils'
```

**Cause:** Missing `./` or file extension
```javascript
import { add } from 'utils';     // ❌ Wrong
import { add } from './utils.js'; // ✅ Correct
```

**Debug Steps:**
1. Check Network tab—no request made
2. Verify import path matches file location
3. Ensure `.js` extension included

---

#### **Error 2: "CORS policy blocked"**
```
Access to script at 'file:///...' from origin 'null' has been blocked by CORS
```

**Cause:** Opening HTML directly (`file://`) instead of via server

**Fix:**
```bash
# Use a local server
npx serve .
# or
python -m http.server 8000
# or VS Code Live Server extension
```

**Debug in DevTools:**
- Network tab shows **(failed) net::ERR_FAILED**
- Console shows CORS error

---

#### **Error 3: "Cannot use import statement outside a module"**
```
Uncaught SyntaxError: Cannot use import statement outside a module
```

**Cause:** Missing `type="module"` in script tag
```html
<script src="main.js"></script>           <!-- ❌ -->
<script type="module" src="main.js"></script> <!-- ✅ -->
```

---

#### **Error 4: Circular Dependency (`undefined` imports)**
```javascript
// a.js imports from b.js
console.log(bValue); // undefined ⚠️
```

**Debug:**
1. Open Sources → Set breakpoint in both files
2. Check Call Stack to see import order
3. Hover over imported values—see which are `undefined`

**Console technique:**
```javascript
// Add at top of each module
console.log('Loading module A');
export const a = 'A';
console.log('Module A loaded, a =', a);
```

**Output reveals load order:**
```
Loading module A
Loading module B
Module B loaded, b = B
Loading module A (skipped - cached)
Module A loaded, a = A  ← A finishes after B needs it
```

---

### 🎯 Debugging Checklist

```
☐ Network Tab: All modules loaded? (200 status)
☐ Console: Any module resolution errors?
☐ Sources Tab: Can you see the module file?
☐ Breakpoints work in module files?
☐ Check import path: includes ./ and .js?
☐ Check HTML: has type="module"?
☐ Using local server? (not file://)
☐ Dynamic imports wrapped in try-catch?
☐ Check Call Stack for circular dependencies
```

---

### 💡 Pro Debugging Techniques

**1. Module Load Visualization**
```javascript
// Add to every module temporarily
const moduleName = import.meta.url.split('/').pop();
console.log(`📦 Loading: ${moduleName}`);
```

**2. Import Map (Browser Feature)**
```html
<script type="importmap">
{
  "imports": {
    "utils": "./src/utils.js",
    "lodash": "https://cdn.skypack.dev/lodash"
  }
}
</script>
<script type="module">
import { add } from 'utils'; // No path needed!
</script>
```

**3. Source Maps (for Bundled Code)**
- Webpack/Vite generate `.map` files
- DevTools automatically loads them
- Lets you debug original source, not bundled code

---

## 10. CHAPTER SUMMARY & NEXT STEPS

### 🎯 Quick Recap

**What You Mastered Today:**

✅ **Module Basics**
- ES6 modules split code into reusable files
- Use `export` to share, `import` to consume
- Require `type="module"` in HTML

✅ **Export Types**
- **Named exports:** Multiple per file, need `{ }`
- **Default exports:** One per file, no `{ }`
- **Mixed:** Combine both for flexibility

✅ **Import Patterns**
- **Named:** `import { x } from './file.js'`
- **Default:** `import X from './file.js'`
- **Namespace:** `import * as X from './file.js'`
- **Dynamic:** `await import('./file.js')`

✅ **Advanced Concepts**
- **Aliases:** Rename imports to avoid conflicts
- **Re-exports:** Create module hubs with index files
- **Tree shaking:** Remove unused code for smaller bundles
- **Dynamic imports:** Load modules on-demand with Promises

✅ **Architecture Patterns**
- Feature-based organization over type-based
- Index files as public APIs
- Lazy loading for performance
- Plugin systems for extensibility

---

### 🔗 Connecting to Day 33

**Tomorrow's Topic:** *JavaScript Map, Set, WeakMap, WeakSet - When & Why to Use Them!*

**The Connection:**
Today you learned how to **organize** your code with modules. Tomorrow, you'll learn how to **efficiently store and retrieve** data using modern JavaScript collections. Think of it this way:

- **Today:** How to structure files and share code
- **Tomorrow:** How to structure data within those files

Just like modules optimize code organization, `Map` and `Set` optimize data storage. You'll discover when to use:
- `Map` instead of objects for key-value pairs
- `Set` instead of arrays for unique values
- `WeakMap`/`WeakSet` for memory-efficient caching

**Preview Exercise:**
```javascript
// Today you learned modules can share state
// math-service.js
const cache = {}; // ❌ Object for caching - is this optimal?

export function fibonacci(n) {
  if (cache[n]) return cache[n];
  // ... calculate
  cache[n] = result;
  return result;
}

// Tomorrow you'll learn:
const cache = new Map(); // ✅ Better for caching!
// Why? You'll find out on Day 33!
```

---

### 📚 Additional Practice

**Before Day 33, Try:**
1. Refactor one of your old projects to use modules
2. Create a feature-based folder structure
3. Implement dynamic imports for a "heavy" feature
4. Build an index.js file that re-exports multiple modules

---

### 🎓 You're Ready For Day 33!

Congratulations! You've mastered JavaScript modules—a critical skill for modern web development. You can now:
- Structure scalable applications
- Optimize bundle sizes with tree shaking
- Implement lazy loading for performance
- Debug module-related issues like a pro

**Tomorrow awaits with even more powerful JavaScript features!** 🚀

---

**💬 Questions?** Review the practice problems, MCQs, and debug challenges. Share your module structures on Discord for feedback!

---

