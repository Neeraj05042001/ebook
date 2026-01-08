
# Day 22: MASTER JavaScript Callback with Asynchronous Programming 🤩

> 📌 MODULE 4 BEGINS HERE! 

Welcome to Module 4: Asynchronous JavaScript! This is where JavaScript gets truly powerful. In Module 3, you mastered DOM manipulation and events. Now, you'll learn how to handle operations that take time—like fetching data from servers, reading files, and building real-world applications that interact with APIs. Callbacks are your first step into this async world! 🚀
## 📋 CHAPTER OVERVIEW

### 🎬 In the Video, You Learned:
- ✅ **Synchronous vs Asynchronous Execution** - How JavaScript handles code line-by-line vs. non-blocking operations
- ✅ **JavaScript's Single-Threaded Nature** - One call stack, but multiple operations can happen "simultaneously"
- ✅ **The Event Loop** - How async operations get queued and executed
- ✅ **Callback Functions** - Functions passed as arguments to be executed later
- ✅ **Real-World API Simulation** - The Pizza Order App demonstrating nested callbacks
- ✅ **Callback Hell** - Why deeply nested callbacks become a maintenance nightmare

### ✅ Mastery Checklist
By the end of this workbook, you should be able to:
- [ ] Write both **synchronous** and **asynchronous** callbacks
- [ ] Predict the **execution order** of mixed sync/async code
- [ ] Understand the difference between **passing** a function vs. **invoking** it
- [ ] Simulate API calls using `setTimeout`
- [ ] Handle **error-first callbacks** properly
- [ ] Identify and explain **Callback Hell**
- [ ] Debug callback-related issues using DevTools
- [ ] Explain callbacks in a technical interview

### 🔧 Prerequisites
Before starting this chapter, make sure you're comfortable with:
- ✅ Function declarations and expressions
- ✅ ES6 Arrow functions
- ✅ Array methods like `.find()`, `.filter()`
- ✅ Basic `setTimeout` usage

### ⚠️ CRITICAL REMINDER
**This chapter focuses ONLY on Callbacks.** We are NOT using:
- ❌ `.then()` (that's Promises - Day 23)
- ❌ `async/await` (that's Day 24)
- ❌ `fetch()` (we'll simulate APIs with `setTimeout`)

**Why?** Because you need to *feel the pain* of Callback Hell before you appreciate how Promises solve it! 😅

---

## 📖 LECTURE CHEAT SHEET

### 🔄 Visual Flow: How Callbacks Work

```
┌─────────────────────────────────────────────────┐
│  SYNCHRONOUS CALLBACK FLOW                      │
├─────────────────────────────────────────────────┤
│                                                 │
│  1. mainFunction() is called                    │
│       ↓                                         │
│  2. Receives callbackFunction as parameter      │
│       ↓                                         │
│  3. Does some work...                           │
│       ↓                                         │
│  4. Calls callbackFunction() immediately        │
│       ↓                                         │
│  5. Execution completes                         │
│                                                 │
└─────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────┐
│  ASYNCHRONOUS CALLBACK FLOW                     │
├─────────────────────────────────────────────────┤
│                                                 │
│  1. mainFunction() is called                    │
│       ↓                                         │
│  2. Receives callbackFunction as parameter      │
│       ↓                                         │
│  3. Starts async operation (setTimeout/API)     │
│       ↓                                         │
│  4. mainFunction() FINISHES (doesn't wait!)     │
│       ↓                                         │
│  5. Other code continues executing...           │
│       ↓                                         │
│  6. [Time Passes] ⏰                            │
│       ↓                                         │
│  7. Async operation completes                   │
│       ↓                                         │
│  8. callbackFunction() is executed NOW          │
│                                                 │
└─────────────────────────────────────────────────┘
```

### 📐 Syntax Reference

#### **Synchronous Callback**
```javascript
// Higher-Order Function (HOF)
function processData(data, callback) {
    const result = data * 2;
    callback(result);  // ← Callback executed immediately
}

// Callback Function
function displayResult(value) {
    console.log("Result:", value);
}

// Usage
processData(5, displayResult);  // Output: Result: 10
```

#### **Asynchronous Callback**
```javascript
// Simulating an API call
function fetchUserData(userId, callback) {
    console.log("Fetching user data...");
    
    setTimeout(() => {
        const userData = { id: userId, name: "John Doe" };
        callback(userData);  // ← Callback executed AFTER delay
    }, 2000);
    
    console.log("Request sent!");  // ← This runs before callback!
}

// Usage
fetchUserData(123, (data) => {
    console.log("User:", data.name);
});

// Output Order:
// "Fetching user data..."
// "Request sent!"
// [2 seconds pass]
// "User: John Doe"
```

### 🧬 The Callback Anatomy

```javascript
//        ┌─── Higher-Order Function (HOF)
//        │    (Receives callback as parameter)
//        ↓
function greetUser(name, callback) {
    console.log("Hello, " + name);
    //                    ┌─── Invoking the callback
    //                    ↓
    callback();  // ← Parentheses mean "execute now"
}
//                              ┌─── Callback Function
//                              │    (Passed as argument)
//                              ↓
greetUser("Alice", function() {
    console.log("Welcome to the app!");
});
```

**🔑 Key Distinction:**
```javascript
// ❌ WRONG: Passing vs Invoking
setTimeout(console.log("Hello"), 1000);  // Executes immediately!

// ✅ CORRECT: Pass the function, don't invoke it
setTimeout(() => console.log("Hello"), 1000);  // Executes after 1s
```

### 📚 Core Terms You Must Know

| Term | Definition | Example |
|------|------------|---------|
| **Blocking** | Code that stops execution until it completes | `alert()` blocks until you click OK |
| **Non-blocking** | Code that doesn't stop other code from running | `setTimeout()` lets other code run |
| **Call Stack** | Where JavaScript tracks function execution | Functions are pushed/popped from stack |
| **Callback Queue** | Where completed async operations wait | Callbacks wait here for the stack to clear |
| **Event Loop** | Monitors call stack and moves callbacks from queue to stack | Constantly checks: "Is stack empty? Move next callback!" |
| **HOF** | Higher-Order Function - accepts or returns functions | `array.map()`, `setTimeout()` |

### ⚡ The "Aha!" Moment

**Why can't we just do this?**
```javascript
function getData() {
    setTimeout(() => {
        return "Data from server";
    }, 1000);
}

const result = getData();
console.log(result);  // undefined 😱
```

**Why `undefined`?** Because `getData()` returns immediately (doesn't wait for setTimeout). The `return` inside setTimeout returns to... nowhere! 

**Solution? CALLBACKS!**
```javascript
function getData(callback) {
    setTimeout(() => {
        callback("Data from server");  // ← Pass data to callback
    }, 1000);
}

getData((result) => {
    console.log(result);  // "Data from server" ✅
});
```

---

## 🎯 PREDICT THE OUTPUT

**Instructions:** For each snippet, write down the **exact order** of console outputs. Then run the code to verify! No peeking! 👀

### Problem 1: Basic Async Order
```javascript
console.log("Start");

setTimeout(() => {
    console.log("Timeout");
}, 0);

console.log("End");
```

**Your Prediction:**
```
1. _________________
2. _________________
3. _________________
```

<details>
<summary>🔍 Click to Reveal Answer</summary>

**Correct Output:**
```
1. Start
2. End
3. Timeout
```

**Why?** Even with 0ms delay, `setTimeout` is asynchronous. It goes to the Web API, then to the callback queue. Meanwhile, synchronous code (`End`) executes first. Only after the call stack is empty does the Event Loop move `Timeout` to the stack.

</details>

---

### Problem 2: Multiple Timeouts
```javascript
console.log("A");

setTimeout(() => console.log("B"), 2000);

setTimeout(() => console.log("C"), 1000);

console.log("D");
```

**Your Prediction:**
```
1. _________________
2. _________________
3. _________________
4. _________________
```

<details>
<summary>🔍 Click to Reveal Answer</summary>

**Correct Output:**
```
1. A
2. D
3. C (after 1 second)
4. B (after 2 seconds)
```

**Why?** Synchronous code (A, D) runs first. Then async callbacks execute in order of their delay: C (1s) before B (2s).

</details>

---

### Problem 3: Callback Within Callback
```javascript
function first(callback) {
    console.log("First");
    callback();
}

function second() {
    console.log("Second");
}

console.log("Start");
first(second);
console.log("End");
```

**Your Prediction:**
```
1. _________________
2. _________________
3. _________________
4. _________________
```

<details>
<summary>🔍 Click to Reveal Answer</summary>

**Correct Output:**
```
1. Start
2. First
3. Second
4. End
```

**Why?** All synchronous! `first()` is called, which logs "First" then immediately invokes `second()`. No async delays here.

</details>

---

### Problem 4: The Tricky One
```javascript
for (var i = 1; i <= 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 1000);
}
```

**Your Prediction:**
```
After 1 second, output:
_________________
```

<details>
<summary>🔍 Click to Reveal Answer</summary>

**Correct Output:**
```
4
4
4
```

**Why?** By the time the callbacks execute (after 1s), the loop has finished and `i = 4`. All three callbacks reference the same `i` (closure issue with `var`).

**Fix with `let`:**
```javascript
for (let i = 1; i <= 3; i++) {  // ← let creates block scope
    setTimeout(() => console.log(i), 1000);
}
// Output: 1, 2, 3 ✅
```

</details>

---

### Problem 5: Sync vs Async Callbacks
```javascript
function process(arr, callback) {
    callback(arr[0]);
}

console.log("X");

process([10, 20], (num) => {
    console.log(num);
});

console.log("Y");
```

**Your Prediction:**
```
1. _________________
2. _________________
3. _________________
```

<details>
<summary>🔍 Click to Reveal Answer</summary>

**Correct Output:**
```
1. X
2. 10
3. Y
```

**Why?** This callback is **synchronous** (no setTimeout/async operation). It executes immediately when called, so `10` logs before `Y`.

</details>

---

### Problem 6: Nested Async Callbacks
```javascript
console.log("1");

setTimeout(() => {
    console.log("2");
    setTimeout(() => {
        console.log("3");
    }, 1000);
}, 1000);

console.log("4");
```

**Your Prediction:**
```
1. _________________
2. _________________
3. _________________
4. _________________
```

<details>
<summary>🔍 Click to Reveal Answer</summary>

**Correct Output:**
```
1. 1
2. 4
3. 2 (after 1 second)
4. 3 (after 2 seconds total)
```

**Why?** 
- Sync code (1, 4) runs first
- After 1s, first callback executes (logs "2" and schedules another timeout)
- After 1 more second (2s total), second callback executes (logs "3")

</details>

---

### Problem 7: The Error Handler Pattern
```javascript
function fetchData(success, callback) {
    setTimeout(() => {
        if (success) {
            callback(null, "Data loaded");
        } else {
            callback("Error occurred", null);
        }
    }, 1000);
}

console.log("Requesting...");

fetchData(true, (error, data) => {
    if (error) {
        console.log("Failed:", error);
    } else {
        console.log("Success:", data);
    }
});

console.log("Request sent");
```

**Your Prediction:**
```
1. _________________
2. _________________
3. _________________ (after 1 second)
```

<details>
<summary>🔍 Click to Reveal Answer</summary>

**Correct Output:**
```
1. Requesting...
2. Request sent
3. Success: Data loaded (after 1 second)
```

**Why?** Classic error-first callback pattern. Sync code runs first, then async callback executes with success path after 1s.

</details>

---

### Problem 8: The Order Challenge
```javascript
setTimeout(() => console.log("A"), 0);
Promise.resolve().then(() => console.log("B"));
console.log("C");
```

**Your Prediction:**
```
1. _________________
2. _________________
3. _________________
```

<details>
<summary>🔍 Click to Reveal Answer</summary>

**Correct Output:**
```
1. C
2. B
3. A
```

**Why?** 
- **C** is synchronous (runs first)
- **B** is a Promise (goes to **Microtask Queue** - higher priority than Callback Queue)
- **A** is setTimeout (goes to **Callback Queue** - lower priority)

Microtasks always execute before regular callbacks! (We'll cover Promises tomorrow, but remember this!)

</details>

---

### Problem 9: Callback Execution Count
```javascript
let count = 0;

function increment(callback) {
    count++;
    callback();
}

increment(() => {
    count++;
    console.log(count);
});

console.log(count);
```

**Your Prediction:**
```
1. _________________
2. _________________
```

<details>
<summary>🔍 Click to Reveal Answer</summary>

**Correct Output:**
```
1. 2
2. 2
```

**Why?** 
- `increment()` increases count to 1
- Callback executes immediately (synchronous), increases to 2, logs 2
- Second `console.log` also shows 2 (count hasn't changed)

</details>

---

### Problem 10: The Ultimate Test
```javascript
console.log("Start");

setTimeout(() => {
    console.log("Timeout 1");
}, 0);

function doWork(callback) {
    console.log("Working...");
    callback();
}

doWork(() => {
    console.log("Callback executed");
    setTimeout(() => {
        console.log("Timeout 2");
    }, 0);
});

console.log("End");
```

**Your Prediction:**
```
1. _________________
2. _________________
3. _________________
4. _________________
5. _________________
6. _________________
```

<details>
<summary>🔍 Click to Reveal Answer</summary>

**Correct Output:**
```
1. Start
2. Working...
3. Callback executed
4. End
5. Timeout 1
6. Timeout 2
```

**Why?** 
- All synchronous code runs first: Start → Working → Callback executed → End
- Both timeouts were scheduled during sync execution
- Timeout 1 was scheduled first, so it executes first
- Then Timeout 2 executes

</details>

---

## 🛠️ GUIDED PRACTICE PROBLEMS

### Problem 1: Simple Math Callback (Easy)

**🎯 Task:** Create a calculator that uses callbacks to log results.

**📋 Requirements:**
1. Create a function `calculate(a, b, operation, callback)`
2. The `operation` function should perform the math
3. The `callback` should log the result in a formatted way
4. Test with add, subtract, multiply, divide operations

**🔧 Starter Code:**
```javascript
function calculate(a, b, operation, callback) {
    // Your code here
}

function add(x, y) {
    // Your code here
}

function logResult(result) {
    // Your code here
}

// Test: calculate(10, 5, add, logResult);
// Expected Output: "Result: 15"
```

**💡 Hints:**
- `operation` is a function that should be called with `a` and `b`
- Pass the result of `operation` to the `callback`
- The callback should format the output nicely

<details>
<summary>✅ Solution</summary>

```javascript
function calculate(a, b, operation, callback) {
    const result = operation(a, b);
    callback(result);
}

function add(x, y) {
    return x + y;
}

function subtract(x, y) {
    return x - y;
}

function multiply(x, y) {
    return x * y;
}

function divide(x, y) {
    if (y === 0) {
        return "Cannot divide by zero";
    }
    return x / y;
}

function logResult(result) {
    console.log("Result:", result);
}

// Tests
calculate(10, 5, add, logResult);       // Result: 15
calculate(10, 5, subtract, logResult);  // Result: 5
calculate(10, 5, multiply, logResult);  // Result: 50
calculate(10, 5, divide, logResult);    // Result: 2
calculate(10, 0, divide, logResult);    // Result: Cannot divide by zero
```

**🎓 Why it works:**
- `calculate()` is a **Higher-Order Function** (accepts functions as parameters)
- `operation` is executed with the two numbers
- The result is immediately passed to `callback`
- This is a **synchronous callback** (no delays)

</details>

---

### Problem 2: Simulating a Data Fetch (Medium)

**🎯 Task:** Simulate an API call that fetches user data after a delay.

**📋 Requirements:**
1. Create `fetchUserData(userId, callback)` that simulates a 2-second API call
2. Return mock user data: `{ id, name, email }`
3. Handle invalid user IDs (id ≤ 0) with an error callback
4. Use the error-first callback pattern: `callback(error, data)`

**🔧 Starter Code:**
```javascript
function fetchUserData(userId, callback) {
    // Simulate API delay with setTimeout
    // Return user data or error
}

// Test cases:
fetchUserData(1, (error, data) => {
    if (error) {
        console.log("Error:", error);
    } else {
        console.log("User:", data);
    }
});

fetchUserData(-1, (error, data) => {
    // Should trigger error path
});
```

**💡 Hints:**
- Use `setTimeout` to create a 2-second delay
- Check `userId` validity before creating user data
- Error-first pattern: `callback(error, null)` or `callback(null, data)`

<details>
<summary>✅ Solution</summary>

```javascript
function fetchUserData(userId, callback) {
    console.log(`⏳ Fetching data for user ${userId}...`);
    
    setTimeout(() => {
        // Validate user ID
        if (userId <= 0) {
            callback("Invalid user ID: must be greater than 0", null);
            return;
        }
        
        // Simulate user not found
        if (userId > 100) {
            callback("User not found", null);
            return;
        }
        
        // Success: return mock user data
        const userData = {
            id: userId,
            name: `User${userId}`,
            email: `user${userId}@example.com`
        };
        
        callback(null, userData);
    }, 2000);
}

// Test Case 1: Valid user
fetchUserData(1, (error, data) => {
    if (error) {
        console.log("❌ Error:", error);
    } else {
        console.log("✅ User:", data);
        console.log(`   Name: ${data.name}`);
        console.log(`   Email: ${data.email}`);
    }
});

// Test Case 2: Invalid user ID
fetchUserData(-1, (error, data) => {
    if (error) {
        console.log("❌ Error:", error);  // Will execute this
    } else {
        console.log("✅ User:", data);
    }
});

// Test Case 3: User not found
fetchUserData(999, (error, data) => {
    if (error) {
        console.log("❌ Error:", error);  // Will execute this
    } else {
        console.log("✅ User:", data);
    }
});
```

**🎓 Why it works:**
- **Async simulation:** `setTimeout` mimics real API delay
- **Error-first pattern:** Industry standard for Node.js callbacks
- **Early return:** Prevents executing success code after error
- **Validation:** Checks data before performing expensive operations

</details>

---

### Problem 3: The Callback Hell Scenario (Hard)

**🎯 Task:** Build a user authentication flow that demonstrates Callback Hell.

**📋 Requirements:**
Create a sequential flow:
1. **Login** → Returns auth token after 1s
2. **Fetch Profile** → Uses token to get profile after 1.5s
3. **Get Posts** → Uses userId to fetch posts after 2s
4. Each step depends on the previous step's data
5. Handle errors at each step

**🔧 Starter Code:**
```javascript
function login(username, password, callback) {
    // Simulate login API call
}

function fetchProfile(token, callback) {
    // Simulate profile fetch
}

function getPosts(userId, callback) {
    // Simulate posts fetch
}

// Start the chain
login("john_doe", "pass123", (error, token) => {
    // Then fetch profile...
    // Then get posts...
    // Welcome to Callback Hell! 🔥
});
```

**💡 Hints:**
- Each function should validate inputs
- Use error-first callbacks throughout
- Notice how the code "pyramids" to the right
- This is **exactly** why we need Promises! (tomorrow's topic)

<details>
<summary>✅ Solution</summary>

```javascript
// Step 1: Login function
function login(username, password, callback) {
    console.log("🔐 Attempting login...");
    
    setTimeout(() => {
        if (!username || !password) {
            callback("Username and password required", null);
            return;
        }
        
        if (password.length < 6) {
            callback("Password too short", null);
            return;
        }
        
        // Success: return auth token
        const token = `TOKEN_${username}_${Date.now()}`;
        console.log("✅ Login successful!");
        callback(null, token);
    }, 1000);
}

// Step 2: Fetch profile function
function fetchProfile(token, callback) {
    console.log("👤 Fetching profile...");
    
    setTimeout(() => {
        if (!token || !token.startsWith("TOKEN_")) {
            callback("Invalid token", null);
            return;
        }
        
        // Success: return user profile
        const profile = {
            userId: 123,
            name: "John Doe",
            email: "john@example.com",
            token: token
        };
        console.log("✅ Profile loaded!");
        callback(null, profile);
    }, 1500);
}

// Step 3: Get posts function
function getPosts(userId, callback) {
    console.log("📝 Fetching posts...");
    
    setTimeout(() => {
        if (!userId) {
            callback("User ID required", null);
            return;
        }
        
        // Success: return mock posts
        const posts = [
            { id: 1, title: "My First Post", likes: 42 },
            { id: 2, title: "Learning Callbacks", likes: 87 },
            { id: 3, title: "JavaScript is Fun!", likes: 156 }
        ];
        console.log("✅ Posts loaded!");
        callback(null, posts);
    }, 2000);
}

// 🔥 CALLBACK HELL IN ACTION 🔥
console.log("🚀 Starting authentication flow...\n");

login("john_doe", "pass123", (error, token) => {
    if (error) {
        console.log("❌ Login failed:", error);
        return;
    }
    
    console.log("📄 Token:", token, "\n");
    
    fetchProfile(token, (error, profile) => {
        if (error) {
            console.log("❌ Profile fetch failed:", error);
            return;
        }
        
        console.log("👤 Profile:", profile, "\n");
        
        getPosts(profile.userId, (error, posts) => {
            if (error) {
                console.log("❌ Posts fetch failed:", error);
                return;
            }
            
            console.log("📝 Posts:", posts, "\n");
            console.log("🎉 Welcome", profile.name, "!");
            console.log("You have", posts.length, "posts");
        });
    });
});

// Notice the "pyramid" shape? →→→
// Each callback is nested deeper than the last
// This is CALLBACK HELL! 🔥🔥🔥
```

**🎓 Why this is problematic:**
1. **Readability:** Code "drifts" to the right (Pyramid of Doom)
2. **Maintainability:** Hard to add new steps or modify existing ones
3. **Error Handling:** Have to check errors at every level
4. **Testing:** Difficult to test individual functions in isolation
5. **Debugging:** Stack traces become confusing

**📊 Visualizing the Pyramid:**
```
login(..., (err, token) => {              // Level 1
    fetchProfile(token, (err, profile) => {  // Level 2
        getPosts(userId, (err, posts) => {     // Level 3
            // Imagine 5 more levels...         // 😱
        });
    });
});
```

**🎯 Tomorrow's Promise:** 
```javascript
// This is what we'll learn tomorrow!
login("john_doe", "pass123")
    .then(token => fetchProfile(token))
    .then(profile => getPosts(profile.userId))
    .then(posts => console.log("Posts:", posts))
    .catch(error => console.log("Error:", error));
// No pyramids! No callback hell! 🎉
```

</details>

---

## 🐛 DEBUG CHALLENGES

**Instructions:** Each snippet has a bug. Find it, explain why it's wrong, and fix it!

### Challenge 1: The Missing Invocation
```javascript
function greet(name, callback) {
    console.log("Hello, " + name);
    callback;  // 🐛 Bug here!
}

greet("Alice", () => console.log("Welcome!"));
```

**❓ What's wrong?**
_Write your answer here:_
<details>
<summary>🔍 Solution</summary>

**Bug:** `callback` is not being invoked (missing parentheses)

**Fix:**
```javascript
function greet(name, callback) {
    console.log("Hello, " + name);
    callback();  // ✅ Added ()
}
```

**Why it's wrong:** Without `()`, you're just referencing the function, not executing it. It's like saying "here's the recipe" instead of "cook the recipe."

</details>

---

### Challenge 2: The Scope Issue
```javascript
function fetchData(callback) {
    setTimeout(() => {
        var data = "Secret Data";
        callback(data);
    }, 1000);
}

fetchData((data) => {
    console.log(data);
});

console.log(data);  // 🐛 Bug here!
```

**❓ What's wrong?**
_Write your answer here:_

<details>
<summary>🔍 Solution</summary>

**Bug:** `data` is not accessible outside the `setTimeout` scope

**Fix:**
```javascript
function fetchData(callback) {
    setTimeout(() => {
        var data = "Secret Data";
        callback(data);
    }, 1000);
}

let result;  // Declare in outer scope

fetchData((data) => {
    result = data;  // Assign value
    console.log(result);  // ✅ Log inside callback
});

// ❌ Can't access 'result' here immediately (async!)
```

**Why it's wrong:** Variables declared with `var`/`let`/`const` inside a function are scoped to that function. Plus, you can't access async data synchronously!

**Better approach:**
```javascript
function fetchData(callback) {
    setTimeout(() => {
        const data = "Secret Data";
        callback(data);
    }, 1000);
}

fetchData((data) => {
    // Do everything you need with data HERE
    console.log(data);
    processData(data);
    saveData(data);
});
```

</details>

---

### Challenge 3: The Parameter Mix-up
```javascript
function calculate(a, b, callback) {
    const result = a + b;
    callback();  // 🐛 Bug here!
}

calculate(5, 10, (result) => {
    console.log("Sum:", result);
});
```

**❓ What's wrong?**
_Write your answer here:_

<details>
<summary>🔍 Solution</summary>

**Bug:** Not passing `result` to the callback

**Fix:**
```javascript
function calculate(a, b, callback) {
    const result = a + b;
    callback(result);  // ✅ Pass result as argument
}

calculate(5, 10, (result) => {
    console.log("Sum:", result);  // Output: Sum: 15
});
```

**Why it's wrong:** The callback expects a parameter (`result`), but you're calling it without any arguments, so `result` inside the callback is `undefined`.

</details>

---

### Challenge 4: The Error Handling Fail
```javascript
function divideNumbers(a, b, callback) {
    const result = a / b;
    callback(null, result);
}

divideNumbers(10, 0, (error, result) => {
    if (error) {
        console.log("Error:", error);
    } else {
        console.log("Result:", result);
    }
});
```

**❓ What's wrong?**
_Write your answer here:_

<details>
<summary>🔍 Solution</summary>

**Bug:** Dividing by zero isn't handled, returns `Infinity` instead of an error

**Fix:**
```javascript
function divideNumbers(a, b, callback) {
    if (b === 0) {
        callback("Cannot divide by zero", null);  // ✅ Error path
        return;
    }
    
    const result = a / b;
    callback(null, result);  // ✅ Success path
}

divideNumbers(10, 0, (error, result) => {
    if (error) {
        console.log("Error:", error);  // This will now execute
    } else {
        console.log("Result:", result);
    }
});
```

**Why it's wrong:**In JavaScript, `10 / 0 = Infinity` (not an error). You must explicitly validate and handle edge cases.

</details>

---

### Challenge 5: The Async Trap
```javascript
function getUsers() {
    let users;
    
    setTimeout(() => {
        users = ["Alice", "Bob", "Charlie"];
    }, 1000);
    
    return users;  // 🐛 Bug here!
}

const userList = getUsers();
console.log(userList);
```

**❓ What's wrong?**
_Write your answer here:_

<details>
<summary>🔍 Solution</summary>

**Bug:** Trying to `return` async data synchronously (returns `undefined`)

**Fix:**
```javascript
function getUsers(callback) {  // ✅ Accept callback
    setTimeout(() => {
        const users = ["Alice", "Bob", "Charlie"];
        callback(users);  // ✅ Pass data to callback
    }, 1000);
    // ❌ Don't return anything!
}

getUsers((userList) => {  // ✅ Handle data in callback
    console.log(userList);  // Output after 1s: ["Alice", "Bob", "Charlie"]
});

// ❌ Can't do this:
// const userList = getUsers();
// console.log(userList); // undefined!
```

**Why it's wrong:** The function returns **before** `setTimeout` completes. By the time `users` is assigned, the function has already returned `undefined`. **You can't return async data—you must use callbacks (or Promises)!**

</details>

---

## ⚠️ COMMON PITFALLS & ANTI-PATTERNS

### 1. The Pyramid of Doom 🔥

**❌ ANTI-PATTERN:**
```javascript
getData((data1) => {
    processData(data1, (data2) => {
        validateData(data2, (data3) => {
            saveData(data3, (data4) => {
                notifyUser(data4, (data5) => {
                    logEvent(data5, () => {
                        // 6 levels deep! 😱
                    });
                });
            });
        });
    });
});
```

**✅ BETTER (Using Named Functions):**
```javascript
function onEventLogged() {
    console.log("All done!");
}

function onUserNotified(data) {
    logEvent(data, onEventLogged);
}

function onDataSaved(data) {
    notifyUser(data, onUserNotified);
}

function onDataValidated(data) {
    saveData(data, onDataSaved);
}

function onDataProcessed(data) {
    validateData(data, onDataValidated);
}

function onDataReceived(data) {
    processData(data, onDataProcessed);
}

getData(onDataReceived);
```

---

### 2. Forgetting the Parentheses 🤦

**❌ WRONG:**
```javascript
setTimeout(console.log("Hello"), 1000);
// Executes immediately! 'Hello' is logged NOW, not after 1s
```

**✅ CORRECT:**
```javascript
setTimeout(() => console.log("Hello"), 1000);
// Logs after 1 second ✅
```

**🎓 Remember:** 
- `myFunction` = reference to the function
- `myFunction()` = invoke/execute the function

---

### 3. Modifying Variables Inside Loops ⚠️

**❌ WRONG:**
```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 1000);
}
// Output: 3, 3, 3 (not 0, 1, 2!)
```

**✅ FIX 1 (Using `let`):**
```javascript
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 1000);
}
// Output: 0, 1, 2 ✅
```

**✅ FIX 2 (Using IIFE):**
```javascript
for (var i = 0; i < 3; i++) {
    (function(index) {
        setTimeout(() => console.log(index), 1000);
    })(i);
}
// Output: 0, 1, 2 ✅
```

---

### 4. Not Handling Errors 🚫

**❌ DANGEROUS:**
```javascript
fetchData((data) => {
    console.log(data.name);  // What if data is null? 💥
});
```

**✅ SAFE:**
```javascript
fetchData((error, data) => {
    if (error) {
        console.error("Failed to fetch:", error);
        return;
    }
    
    if (!data) {
        console.error("No data received");
        return;
    }
    
    console.log(data.name);  // ✅ Safe to access
});
```

---

### 5. Calling Callbacks Multiple Times 😱

**❌ WRONG:**
```javascript
function riskyFunction(callback) {
    setTimeout(() => callback("First call"), 1000);
    setTimeout(() => callback("Second call"), 2000);
    // Callback is called TWICE! 🐛
}
```

**✅ CORRECT:**
```javascript
function safeFunction(callback) {
    let called = false;
    
    const safeCallback = (data) => {
        if (called) return;  // Prevent multiple calls
        called = true;
        callback(data);
    };
    
    setTimeout(() => safeCallback("Only once"), 1000);
}
```

---

## 📝 KNOWLEDGE CHECK MCQs

**Instructions:** Choose the best answer for each question.

### Question 1
**What is a callback function?**

A) A function that calls itself  
B) A function passed as an argument to another function  
C) A function that returns another function  
D) A function that has no parameters

<details>
<summary>Answer</summary>

**B) A function passed as an argument to another function**

A callback is a function you provide to another function, to be executed later (either immediately or after some operation completes).

</details>

---

### Question 2
**What will this code output?**
```javascript
console.log("A");
setTimeout(() => console.log("B"), 0);
console.log("C");
```

A) A, B, C  
B) A, C, B  
C) B, A, C  
D) C, B, A

<details>
<summary>Answer</summary>

**B) A, C, B**

Even with 0ms delay, `setTimeout` is asynchronous and goes to the callback queue. Synchronous code (A, C) executes first.

</details>

---

### Question 3
**Which is a synchronous callback?**

A) `setTimeout(() => {}, 1000)`  
B) `fetch(url).then(data => {})`  
C) `[1,2,3].map(n => n * 2)`  
D) `addEventListener("click", handler)`

<details>
<summary>Answer</summary>

**C) `[1,2,3].map(n => n * 2)`**

`.map()` executes the callback immediately for each element. The others are asynchronous (delays, network, events).

</details>

---

### Question 4
**What is "callback hell"?**

A) A function that never returns  
B) Deeply nested callbacks that are hard to read  
C) A callback that throws an error  
D) A callback without parameters

<details>
<summary>Answer</summary>

**B) Deeply nested callbacks that are hard to read**

Callback hell (Pyramid of Doom) refers to multiple nested callbacks that create difficult-to-maintain code.

</details>

---

### Question 5
**What's wrong with this code?**
```javascript
setTimeout(console.log("Hi"), 1000);
```

A) Nothing, it's correct  
B) `console.log` is being invoked immediately, not after 1s  
C) Missing parentheses  
D) `setTimeout` needs a callback name, not a function call

<details>
<summary>Answer</summary>

**B) `console.log` is being invoked immediately, not after 1s**

`console.log("Hi")` executes right away and returns `undefined`. You're passing `undefined` to `setTimeout`, not a function.

**Fix:** `setTimeout(() => console.log("Hi"), 1000)`

</details>

---

### Question 6
**In the error-first callback pattern, which is correct?**

A) `callback(data, error)`  
B) `callback(error, data)`  
C) `callback(error || data)`  
D) `callback({error, data})`

<details>
<summary>Answer</summary>

**B) `callback(error, data)`**

By convention (from Node.js), error comes first. If no error, pass `null` as first argument.

</details>

---

### Question 7
**What does the Event Loop do?**

A) Loops through arrays  
B) Moves callbacks from queue to call stack when stack is empty  
C) Creates infinite loops  
D) Handles syntax errors

<details>
<summary>Answer</summary>

**B) Moves callbacks from queue to call stack when stack is empty**

The Event Loop constantly checks: "Is the call stack empty? If yes, move the next callback from the queue to the stack."

</details>

---

### Question 8
**Functions in JavaScript are:**

A) Second-class citizens  
B) First-class citizens  
C) Not objects  
D) Cannot be passed as arguments

<details>
<summary>Answer</summary>

**B) First-class citizens**

In JavaScript, functions are first-class citizens, meaning they can be:
- Assigned to variables
- Passed as arguments
- Returned from functions
- Stored in data structures

</details>

---

### Question 9
**What will this output?**
```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 0);
}
```

A) 0, 1, 2  
B) 3, 3, 3  
C) 2, 2, 2  
D) undefined, undefined, undefined

<details>
<summary>Answer</summary>

**B) 3, 3, 3**

By the time callbacks execute, the loop has finished and `i = 3`. All three callbacks reference the same `i` (closure with `var`).

**Fix:** Use `let` instead of `var`

</details>

---

### Question 10
**Which is NOT a benefit of callbacks?**

A) Enable asynchronous programming  
B) Allow code reusability  
C) Make code more readable with deep nesting  
D) Enable event-driven programming

<details>
<summary>Answer</summary>

**C) Make code more readable with deep nesting**

Deep nesting (callback hell) is actually a **problem** with callbacks, not a benefit!

</details>

---

### Question 11
**What's the difference between these?**
```javascript
// Version A
doSomething(myFunction);

// Version B
doSomething(myFunction());
```

A) No difference  
B) A passes the function, B invokes it immediately  
C) B is correct, A is wrong  
D) Both are syntax errors

<details>
<summary>Answer</summary>

**B) A passes the function, B invokes it immediately**

- **A:** Passes `myFunction` as a callback (correct for callbacks)
- **B:** Executes `myFunction()` NOW and passes its return value

</details>

---

### Question 12
**In this code, when does "Done" log?**
```javascript
setTimeout(() => console.log("Async"), 0);
console.log("Done");
```

A) After "Async"  
B) Before "Async"  
C) At the same time  
D) It depends

<details>
<summary>Answer</summary>

**B) Before "Async"**

"Done" is synchronous and logs first. "Async" is in the callback queue and executes after the call stack is empty.

</details>

---

### Question 13
**What does a Higher-Order Function do?**

A) Runs faster than normal functions  
B) Takes a function as argument OR returns a function  
C) Only works with arrays  
D) Cannot have parameters

<details>
<summary>Answer</summary>

**B) Takes a function as argument OR returns a function**

Examples: `.map()`, `.filter()`, `setTimeout()`, or any function that returns a function.

</details>

---

### Question 14
**What's the best way to avoid callback hell?**

A) Use more callbacks  
B) Use named functions instead of anonymous ones  
C) Avoid callbacks entirely  
D) Use only synchronous code

<details>
<summary>Answer</summary>

**B) Use named functions instead of anonymous ones**

Breaking callbacks into named functions improves readability. (Promises and async/await are even better, but that's Day 23!)

</details>

---

### Question 15
**JavaScript is:**

A) Multi-threaded  
B) Single-threaded but can handle async operations  
C) Cannot handle async operations  
D) Multi-threaded when using callbacks

<details>
<summary>Answer</summary>

**B) Single-threaded but can handle async operations**

JavaScript has one call stack (single-threaded), but uses Web APIs, callback queue, and the Event Loop to handle asynchronous operations.

</details>

---

## 🎤 REAL INTERVIEW QUESTIONS

### Question 1: Explain Callbacks
**Interviewer:** "Can you explain what a callback function is and why we use them in JavaScript?"

<details>
<summary>💡 Sample Answer</summary>

**Answer:**

"A callback function is a function that's passed as an argument to another function and is executed after some operation completes.

**Why we use them:**
1. **Asynchronous Operations:** JavaScript is single-threaded, so we use callbacks to handle operations that take time (like API calls) without blocking the main thread.
2. **Reusability:** We can pass different functions to handle results in different ways.
3. **Event Handling:** Callbacks are fundamental to event-driven programming.

**Example:**
```javascript
function fetchData(callback) {
    setTimeout(() => {
        const data = { id: 1, name: "User" };
        callback(data);
    }, 1000);
}

fetchData((result) => {
    console.log("Data received:", result);
});
```

In this example, the arrow function is the callback, executed after 1 second when data is ready."

</details>

---

### Question 2: Synchronous vs Asynchronous Callbacks
**Interviewer:** "What's the difference between synchronous and asynchronous callbacks? Give examples."

<details>
<summary>💡 Sample Answer</summary>

**Answer:**

"**Synchronous callbacks** execute immediately within the function that calls them, blocking further execution until they complete.

**Example:**
```javascript
[1, 2, 3].forEach((num) => {
    console.log(num);
});
console.log("Done");
// Output: 1, 2, 3, Done (in order)
```

**Asynchronous callbacks** are executed later, after some operation completes (like a timer or API call), allowing other code to run in the meantime.

**Example:**
```javascript
setTimeout(() => {
    console.log("Async callback");
}, 1000);
console.log("Done");
// Output: Done (immediately), then Async callback (after 1s)
```

The key difference is **when** the callback executes relative to the rest of the code."

</details>

---

### Question 3: Callback Hell
**Interviewer:** "What is callback hell and how would you solve it?"

<details>
<summary>💡 Sample Answer</summary>

**Answer:**

"Callback hell, also called the Pyramid of Doom, occurs when we have multiple nested callbacks, making code hard to read and maintain.

**Example of Callback Hell:**
```javascript
getData((a) => {
    processData(a, (b) => {
        validateData(b, (c) => {
            saveData(c, (d) => {
                console.log("Done");
            });
        });
    });
});
```

**Solutions:**

1. **Named Functions:** Break callbacks into separate, named functions
```javascript
function onDataReceived(a) {
    processData(a, onDataProcessed);
}

function onDataProcessed(b) {
    validateData(b, onDataValidated);
}

getData(onDataReceived);
```

2. **Promises** (modern approach):
```javascript
getData()
    .then(processData)
    .then(validateData)
    .then(saveData)
    .catch(handleError);
```

3. **Async/Await** (cleanest):
```javascript
try {
    const a = await getData();
    const b = await processData(a);
    const c = await validateData(b);
    await saveData(c);
} catch (error) {
    handleError(error);
}
```

In modern JavaScript, we typically use Promises or async/await to avoid callback hell."

</details>

---

### Question 4: Error Handling
**Interviewer:** "How do you handle errors in callback-based code?"

<details>
<summary>💡 Sample Answer</summary>

**Answer:**

"The standard pattern is the **error-first callback**, popularized by Node.js.

**Convention:**
- First parameter is for errors
- Second parameter is for successful data
- If no error, pass `null` as first argument

**Example:**
```javascript
function fetchUser(id, callback) {
    setTimeout(() => {
        if (id <= 0) {
            callback(new Error("Invalid ID"), null);
        } else {
            callback(null, { id, name: "User" });
        }
    }, 1000);
}

// Usage
fetchUser(5, (error, user) => {
    if (error) {
        console.error("Error:", error.message);
        return;
    }
    console.log("User:", user);
});
```

**Key Points:**
1. Always check for errors first
2. Return early on error to prevent further execution
3. Handle both error and success cases
4. Be consistent with error-first pattern across your codebase"

</details>

---

### Question 5: Event Loop
**Interviewer:** "Can you explain how the Event Loop works in JavaScript?"

<details>
<summary>💡 Sample Answer</summary>

**Answer:**

"The Event Loop is what enables JavaScript to perform non-blocking operations despite being single-threaded.

**Components:**
1. **Call Stack:** Where function execution contexts are pushed and popped
2. **Web APIs:** Browser-provided APIs (setTimeout, fetch, DOM events) that run outside the main thread
3. **Callback Queue:** Where completed async callbacks wait
4. **Microtask Queue:** Higher priority queue for Promises
5. **Event Loop:** Continuously checks if call stack is empty, then moves callbacks from queue to stack

**Execution Flow:**
```javascript
console.log("1");                    // → Call Stack → Execute
setTimeout(() => console.log("2"), 0);  // → Web API → Callback Queue
console.log("3");                    // → Call Stack → Execute
// Output: 1, 3, 2
```

**What happens:**
1. "1" logs (synchronous, call stack)
2. `setTimeout` goes to Web API
3. "3" logs (synchronous, call stack)
4. Call stack is now empty
5. Event Loop moves setTimeout callback to call stack
6. "2" logs

Even with 0ms delay, `setTimeout` is async and goes through the queue."

</details>

---

### Question 6: Practical Scenario
**Interviewer:** "Write a function that simulates three API calls in sequence using callbacks."

<details>
<summary>💡 Sample Answer</summary>

**Answer:**

```javascript
// Simulate API calls with delays
function getUser(userId, callback) {
    console.log("Fetching user...");
    setTimeout(() => {
        callback(null, { userId, name: "John Doe" });
    }, 1000);
}

function getOrders(userId, callback) {
    console.log("Fetching orders...");
    setTimeout(() => {
        callback(null, [
            { id: 1, item: "Book" },
            { id: 2, item: "Laptop" }
        ]);
    }, 1000);
}

function getOrderDetails(orderId, callback) {
    console.log("Fetching order details...");
    setTimeout(() => {
        callback(null, {
            orderId,
            price: 999,
            status: "Shipped"
        });
    }, 1000);
}

// Execute in sequence
function getUserOrderDetails(userId) {
    getUser(userId, (error, user) => {
        if (error) {
            console.error("User error:", error);
            return;
        }
        
        console.log("User:", user.name);
        
        getOrders(user.userId, (error, orders) => {
            if (error) {
                console.error("Orders error:", error);
                return;
            }
            
            console.log("Orders:", orders.length);
            
            getOrderDetails(orders[0].id, (error, details) => {
                if (error) {
                    console.error("Details error:", error);
                    return;
                }
                
                console.log("Order details:", details);
            });
        });
    });
}

getUserOrderDetails(123);
```

**Explanation:**
- Each function simulates an API call with `setTimeout`
- Uses error-first callback pattern
- Checks for errors at each step
- This demonstrates callback nesting (which Promises would clean up)"

</details>

---

## 🛠️ DEVTOOLS & DEBUGGING

### Using the Call Stack in Chrome DevTools

**Step 1: Open DevTools**
- Press `F12` or `Ctrl+Shift+I` (Windows/Linux)
- Press `Cmd+Option+I` (Mac)

**Step 2: Set Breakpoints**
```javascript
function fetchData(callback) {
    setTimeout(() => {
        debugger;  // 🔍 Execution will pause here
        const data = "User Data";
        callback(data);
    }, 1000);
}

fetchData((data) => {
    console.log(data);
});
```

**Step 3: Observe the Call Stack**
1. When `debugger` hits, look at the right panel
2. **Call Stack** shows:
   ```
   (anonymous) ← Current position
   fetchData
   (anonymous)
   ```
3. Notice how the callback is in the stack but not where you called it!

**Step 4: Step Through Code**
- **Step Over (F10):** Execute current line, move to next
- **Step Into (F11):** Go inside function calls
- **Step Out (Shift+F11):** Exit current function

---

### Measuring Async Performance

```javascript
console.time("Data Fetch");  // Start timer

function fetchData(callback) {
    setTimeout(() => {
        console.timeEnd("Data Fetch");  // End timer
        callback("Data loaded");
    }, 2000);
}

fetchData((data) => {
    console.log(data);
});

// Output:
// Data Fetch: 2003.45ms (approximately 2000ms)
// Data loaded
```

**Advanced Timing:**
```javascript
function timedOperation(operationName, callback, delay) {
    const label = `⏱️ ${operationName}`;
    
    console.time(label);
    console.log(`🚀 ${operationName} started`);
    
    setTimeout(() => {
        console.timeEnd(label);
        console.log(`✅ ${operationName} completed`);
        callback();
    }, delay);
}

timedOperation("Fetch User", () => {
    console.log("User data received");
}, 1000);

// Output:
// 🚀 Fetch User started
// ⏱️ Fetch User: 1002.34ms
// ✅ Fetch User completed
// User data received
```

---

### Debugging Async Flow

**Problem: Lost Track of Execution?**

Use labeled console logs:
```javascript
function step1(callback) {
    console.log("📍 Step 1: Starting");
    setTimeout(() => {
        console.log("✅ Step 1: Complete");
        callback();
    }, 1000);
}

function step2(callback) {
    console.log("📍 Step 2: Starting");
    setTimeout(() => {
        console.log("✅ Step 2: Complete");
        callback();
    }, 1000);
}

function step3() {
    console.log("📍 Step 3: Starting");
    setTimeout(() => {
        console.log("✅ Step 3: Complete");
    }, 1000);
}

step1(() => {
    step2(() => {
        step3();
    });
});

// Output Timeline:
// 📍 Step 1: Starting
// [1 second]
// ✅ Step 1: Complete
// 📍 Step 2: Starting
// [1 second]
// ✅ Step 2: Complete
// 📍 Step 3: Starting
// [1 second]
// ✅ Step 3: Complete
```

---

### Network Tab for API Debugging

When working with real APIs:

1. **Open Network Tab** in DevTools
2. **Filter by XHR/Fetch**
3. **Click on a request** to see:
   - **Headers:** Request/response headers
   - **Preview:** Formatted response data
   - **Response:** Raw response
   - **Timing:** How long each phase took

**Simulating in Console:**
```javascript
// Simulate API monitoring
function monitoredFetch(url, callback) {
    console.log(`📡 [REQUEST] ${url}`);
    const startTime = performance.now();
    
    setTimeout(() => {
        const endTime = performance.now();
        const duration = (endTime - startTime).toFixed(2);
        
        console.log(`✅ [RESPONSE] ${url} (${duration}ms)`);
        callback({ status: 200, data: "Mock data" });
    }, Math.random() * 2000);  // Random delay
}

monitoredFetch("api/users", (response) => {
    console.log("Data:", response.data);
});
```

---

## 📚 CHAPTER SUMMARY & NEXT STEPS

### 🎓 What You Mastered Today

**✅ Execution Flow Understanding**
- You can now **predict** the order of mixed sync/async code
- You understand why `setTimeout(..., 0)` doesn't execute immediately
- You know how the **Event Loop** orchestrates async operations

**✅ Callback Mastery**
- Writing both **synchronous** and **asynchronous** callbacks
- Implementing the **error-first callback pattern**
- Using **Higher-Order Functions** effectively
- Debugging callback chains using DevTools

**✅ Problem Recognition**
- Identifying **Callback Hell** (Pyramid of Doom)
- Understanding why deeply nested callbacks are problematic
- Knowing when to break callbacks into named functions

**✅ Real-World Skills**
- Simulating API calls with `setTimeout`
- Handling errors gracefully at each async step
- Sequencing dependent operations

---

### 🔥 The Pain You Just Felt

That feeling of nesting callbacks 5 levels deep? The frustration of tracking execution order? The headache of error handling at every level?

**That's intentional.** 

You needed to *experience* the problem that Promises solve!

---

### 🚀 Tomorrow: The Solution

**Day 23: JavaScript Promises** will feel like a breath of fresh air! 🌬️

**Instead of this:**
```javascript
login((err, user) => {
    if (err) return handleError(err);
    getProfile(user, (err, profile) => {
        if (err) return handleError(err);
        getPosts(profile, (err, posts) => {
            if (err) return handleError(err);
            console.log(posts);
        });
    });
});
```

**You'll write this:**
```javascript
login()
    .then(user => getProfile(user))
    .then(profile => getPosts(profile))
    .then(posts => console.log(posts))
    .catch(error => handleError(error));
```

**And then Day 24, even better:**
```javascript
try {
    const user = await login();
    const profile = await getProfile(user);
    const posts = await getPosts(profile);
    console.log(posts);
} catch (error) {
    handleError(error);
}
```

---

### 🎯 Before Tomorrow's Class

**Practice Exercise:**
Try to refactor one of today's nested callback examples using **only named functions**—no anonymous callbacks. Feel how much more readable it becomes. This sets you up perfectly for understanding why Promises are even better!



---

### 💬 Discussion Question

Head to **Discord** and answer this:

**"What was the most confusing part about callbacks for you? And what finally made it click?"**

Share your "aha moment"—it might help someone else! 💡

---

## 🎉 CONGRATULATIONS!

You've completed Day 22! You now understand:
- ✅ How JavaScript handles asynchronous operations
- ✅ Why callbacks are powerful but messy
- ✅ How to write, debug, and refactor callback code

**Tomorrow, you'll learn how Promises make all this pain go away!** 🚀

---

> 🌟 *"First, you learn the hard way. Then, you learn the smart way. Both are necessary."* 🌟

---



**Type "Continue" if you'd like me to generate additional practice problems or expand any section!**