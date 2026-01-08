# Day 23: MASTER JavaScript Promises - From Beginner to PRO


## 1. CHAPTER OVERVIEW

### 📺 In the Video
You learned how JavaScript Promises provide a cleaner way to handle asynchronous operations compared to callbacks. We covered Promise states, chaining rules, error handling, and methods like Promise.all() and Promise.race() to work with multiple asynchronous operations simultaneously.

### ✅ Learning Checklist
By the end of this workbook, you will be able to:
- ✓ Create and consume Promises using the Promise constructor
- ✓ Handle Promise resolution and rejection with .then() and .catch()
- ✓ Chain Promises correctly and understand the 5 golden rules of chaining
- ✓ Use Promise utility methods (Promise.all, Promise.race, Promise.any, Promise.allSettled)
- ✓ Convert callback-based code to Promise-based code
- ✓ Debug common Promise errors and avoid anti-patterns
- ✓ Apply Promises to real-world scenarios like API calls and data processing

### 📋 Prerequisites
From **Day 22 - Callbacks & Asynchronous Programming**, you should know:
- How JavaScript's event loop works (call stack, callback queue)
- The concept of asynchronous vs synchronous code
- Callback functions and callback hell problem
- setTimeout() and setInterval() basics

---

## 2. LECTURE CHEAT SHEET

### 🎯 Core Promise Syntax

```javascript
// Creating a Promise
const myPromise = new Promise((resolve, reject) => {
    // Asynchronous operation
    if (success) {
        resolve(value);  // ✅ Success
    } else {
        reject(error);   // ❌ Failure
    }
});

// Consuming a Promise
myPromise
    .then(result => { /* handle success */ })
    .catch(error => { /* handle error */ })
    .finally(() => { /* cleanup */ });
```

### 📊 Promise States & Transitions

```
┌─────────────────────────────────────────┐
│         PROMISE LIFECYCLE               │
├─────────────────────────────────────────┤
│                                         │
│    ⏳ PENDING (initial state)          │
│              │                          │
│              ├──────────┬───────────┐   │
│              │          │           │   │
│              ▼          ▼           │   │
│     ✅ FULFILLED    ❌ REJECTED    │   │
│        (success)      (error)       │   │
│              │          │           │   │
│              └──────────┴───────────┘   │
│                   │                     │
│                   ▼                     │
│            🔒 SETTLED                   │
│        (cannot change state)            │
└─────────────────────────────────────────┘
```

### 🔑 Key Terminology

| Term | Definition | Example |
|------|------------|---------|
| **Executor** | Function passed to `new Promise()` | `(resolve, reject) => { ... }` |
| **Resolve** | Function to call when operation succeeds | `resolve("Success!")` |
| **Reject** | Function to call when operation fails | `reject(new Error("Failed"))` |
| **Pending** | Initial state before settle | Promise just created |
| **Fulfilled** | Promise completed successfully | `resolve()` was called |
| **Rejected** | Promise completed with error | `reject()` was called |
| **Settled** | Promise is either fulfilled or rejected | Final state (immutable) |
| **Thenable** | Object with a `.then()` method | Any Promise |

### 🎭 Handler Methods Comparison

| Method | Purpose | When It Runs | Return Value |
|--------|---------|--------------|--------------|
| `.then()` | Handle success | Promise fulfilled | New Promise |
| `.catch()` | Handle error | Promise rejected | New Promise |
| `.finally()` | Cleanup code | Always (success or failure) | New Promise (passes through value) |

### ⚡ The 5 Golden Rules of Promise Chaining

```javascript
// RULE 1: Every fulfilled promise has .then(), rejected has .catch()
promise.then(successHandler).catch(errorHandler);

// RULE 2: Three things you can do in .then()
promise.then(value => {
    return anotherPromise;     // 1️⃣ Return Promise (async)
    return simpleValue;        // 2️⃣ Return value (sync)
    throw new Error("Oops");   // 3️⃣ Throw error
});

// RULE 3: You can rethrow from .catch()
promise.catch(err => {
    if (err.code === 401) throw err;  // Rethrow for next catch
    return fallbackValue;              // Or recover
});

// RULE 4: .finally() passes through the result
promise
    .finally(() => console.log("Cleanup"))
    .then(value => console.log(value));  // Still gets original value

// RULE 5: Multiple .then() ≠ Chaining
const p = Promise.resolve(10);
p.then(v => v + 1);  // ❌ Each gets 10
p.then(v => v + 2);  // ❌ Each gets 10

// ✅ TRUE CHAINING
p.then(v => v + 1)   // Gets 10, returns 11
 .then(v => v + 2);  // Gets 11, returns 13
```

### 🎪 Promise Utility Methods

| Method | Use Case | Behavior | Result |
|--------|----------|----------|--------|
| `Promise.all([p1, p2])` | All must succeed | Fails fast if any rejects | Array of all values |
| `Promise.race([p1, p2])` | First to complete wins | Returns first settled | Single value/error |
| `Promise.any([p1, p2])` | Need at least one success | Ignores rejections | First fulfilled value |
| `Promise.allSettled([p1, p2])` | Need all results | Never rejects | Array of `{status, value/reason}` |

```javascript
// Visual Comparison
const p1 = Promise.resolve(1);
const p2 = Promise.reject("Error");
const p3 = Promise.resolve(3);

Promise.all([p1, p2, p3])        // ❌ Rejects with "Error"
Promise.race([p1, p2, p3])       // ✅ Resolves with 1 (first)
Promise.any([p1, p2, p3])        // ✅ Resolves with 1 (first success)
Promise.allSettled([p1, p2, p3]) // ✅ Returns [{status: "fulfilled", value: 1}, ...]
```

### 🔄 Callback to Promise Conversion Pattern

```javascript
// ❌ CALLBACK HELL
getData(function(a) {
    getMoreData(a, function(b) {
        getEvenMoreData(b, function(c) {
            console.log(c);
        });
    });
});

// ✅ PROMISE CHAIN
getData()
    .then(a => getMoreData(a))
    .then(b => getEvenMoreData(b))
    .then(c => console.log(c))
    .catch(err => console.error(err));
```

### 🛡️ Promise vs Callback - Quick Comparison

| Feature | Callbacks | Promises |
|---------|-----------|----------|
| **Readability** | Nested (pyramid of doom) | Linear chain |
| **Error Handling** | Every callback needs try-catch | Single `.catch()` |
| **Composition** | Hard to combine | Easy with `.all()`, `.race()` |
| **Return Values** | Cannot return async values | Returns Promise |
| **Control Flow** | Inverted (library controls timing) | You control with `.then()` |

### 🎨 Promise Flow Diagram

```
┌──────────────────────────────────────────────┐
│  const promise = new Promise((res, rej) => { │
│      setTimeout(() => res("Done"), 1000)     │
│  })                                          │
└────────────────┬─────────────────────────────┘
                 │
                 ▼
        ⏳ State: Pending
         Result: undefined
                 │
      (after 1 second)
                 │
                 ▼
        ✅ State: Fulfilled
         Result: "Done"
                 │
                 ▼
┌────────────────────────────────────────────┐
│  promise.then(result => {                  │
│      console.log(result)  // "Done"        │
│  })                                        │
└────────────────────────────────────────────┘
```

---

## 3. PREDICT THE OUTPUT

### 🧠 Test Your Understanding

**Instructions:** Read each code snippet carefully. Think about the Promise states, timing, and chaining rules before checking the answer.

---

#### **Snippet 1: Basic Promise Resolution**
```javascript
const p = new Promise((resolve, reject) => {
    resolve("First");
    resolve("Second");
});

p.then(result => console.log(result));
```

**🤔 Think:** Can a Promise resolve twice?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:** `"First"`

**Why:** A Promise can only settle once. The first `resolve("First")` changes the state to fulfilled. The second `resolve("Second")` is ignored because the Promise is already settled.
</details>

---

#### **Snippet 2: Synchronous vs Asynchronous**
```javascript
console.log("Start");

const promise = Promise.resolve("Immediate");

promise.then(result => console.log(result));

console.log("End");
```

**🤔 Think:** Does `.then()` execute immediately or after current code?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
Start
End
Immediate
```

**Why:** Even though the Promise is already resolved, `.then()` callbacks are always asynchronous (placed in the microtask queue). The synchronous code (`console.log("End")`) runs first, then the microtask queue is processed.
</details>

---

#### **Snippet 3: Chaining with Return Values**
```javascript
Promise.resolve(5)
    .then(num => num * 2)
    .then(num => num + 3)
    .then(result => console.log(result));
```

**🤔 Think:** How do values flow through the chain?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:** `13`

**Why:** 
- First `.then()`: receives 5, returns 10
- Second `.then()`: receives 10, returns 13
- Third `.then()`: receives 13, logs it

Each `.then()` passes its return value to the next `.then()` in the chain.
</details>

---

#### **Snippet 4: Error Handling**
```javascript
Promise.reject("Error!")
    .then(result => console.log("Success:", result))
    .catch(error => console.log("Caught:", error))
    .then(() => console.log("Continues"));
```

**🤔 Think:** What happens after `.catch()` handles an error?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
Caught: Error!
Continues
```

**Why:** `.catch()` handles the rejection. After catching, the chain continues normally. The final `.then()` executes because `.catch()` returns a resolved Promise (unless it throws).
</details>

---

#### **Snippet 5: Multiple .then() on Same Promise**
```javascript
const promise = Promise.resolve(10);

promise.then(value => console.log(value + 1));
promise.then(value => console.log(value + 2));
promise.then(value => console.log(value + 3));
```

**🤔 Think:** Is this chaining? What value does each `.then()` receive?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
11
12
13
```

**Why:** This is NOT chaining (Rule 5). Each `.then()` is attached to the original promise independently. All three receive the original resolved value (10), not the result of the previous `.then()`.
</details>

---

#### **Snippet 6: Finally Behavior**
```javascript
Promise.resolve("Result")
    .finally(() => {
        console.log("Cleanup");
        return "Ignored";
    })
    .then(result => console.log(result));
```

**🤔 Think:** Does `.finally()` change the Promise value?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
Cleanup
Result
```

**Why:** `.finally()` passes through the original value (Rule 4). The return value `"Ignored"` is ignored. The next `.then()` receives the original `"Result"`.
</details>

---

#### **Snippet 7: Throwing in .then()**
```javascript
Promise.resolve(20)
    .then(num => {
        if (num > 10) throw new Error("Too big!");
        return num;
    })
    .then(result => console.log("Success:", result))
    .catch(error => console.log("Error:", error.message));
```

**🤔 Think:** What happens when you throw inside `.then()`?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:** `Error: Too big!`

**Why:** Throwing an error inside `.then()` rejects the Promise (Rule 2). The error skips the next `.then()` and goes directly to `.catch()`.
</details>

---

#### **Snippet 8: Promise.all() Failure**
```javascript
const p1 = Promise.resolve("One");
const p2 = Promise.reject("Two");
const p3 = Promise.resolve("Three");

Promise.all([p1, p2, p3])
    .then(results => console.log("Success:", results))
    .catch(error => console.log("Failed:", error));
```

**🤔 Think:** What happens when one Promise in `.all()` rejects?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:** `Failed: Two`

**Why:** `Promise.all()` fails fast. If ANY Promise rejects, the entire operation rejects immediately with that error. The other Promises are ignored.
</details>

---

#### **Snippet 9: Promise.race() Timing**
```javascript
const slow = new Promise(resolve => setTimeout(() => resolve("Slow"), 2000));
const fast = new Promise(resolve => setTimeout(() => resolve("Fast"), 500));

Promise.race([slow, fast])
    .then(result => console.log("Winner:", result));
```

**🤔 Think:** Which Promise wins the race?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:** `Winner: Fast` (after ~500ms)

**Why:** `Promise.race()` resolves/rejects with the first Promise to settle. `fast` settles first (500ms < 2000ms), so it wins.
</details>

---

#### **Snippet 10: Rethrowing from .catch()**
```javascript
Promise.reject(404)
    .catch(err => {
        console.log("First catch:", err);
        if (err === 404) throw err;
    })
    .then(() => console.log("Success"))
    .catch(err => console.log("Second catch:", err));
```

**🤔 Think:** Can errors be caught multiple times?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
First catch: 404
Second catch: 404
```

**Why:** The first `.catch()` logs the error, then rethrows it (Rule 3). The rethrrown error skips the `.then()` and is caught by the second `.catch()`.
</details>

---

#### **Snippet 11: Promise.resolve() with Thenable**
```javascript
const thenable = {
    then: function(resolve) {
        resolve("Thenable value");
    }
};

Promise.resolve(thenable)
    .then(result => console.log(result));
```

**🤔 Think:** What happens when Promise.resolve() receives an object with `.then()`?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:** `Thenable value`

**Why:** `Promise.resolve()` recognizes objects with a `.then()` method (thenables) and treats them as Promises, executing their `.then()` method.
</details>

---

#### **Snippet 12: Timing with Nested Promises**
```javascript
console.log("1");

Promise.resolve()
    .then(() => {
        console.log("2");
        return Promise.resolve();
    })
    .then(() => console.log("3"));

Promise.resolve()
    .then(() => console.log("4"))
    .then(() => console.log("5"));

console.log("6");
```

**🤔 Think:** What's the order of execution with microtasks?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
1
6
2
4
3
5
```

**Why:** 
- Sync code first: 1, 6
- First microtask queue cycle: 2, 4
- Returning a Promise in "2" adds an extra microtask
- Second cycle: 3, 5
</details>

---

#### **Snippet 13: Promise.allSettled() Results**
```javascript
const promises = [
    Promise.resolve("Success"),
    Promise.reject("Error"),
    Promise.resolve("Another Success")
];

Promise.allSettled(promises)
    .then(results => {
        results.forEach(r => console.log(r.status, r.value || r.reason));
    });
```

**🤔 Think:** How does `.allSettled()` format results?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
fulfilled Success
rejected Error
fulfilled Another Success
```

**Why:** `Promise.allSettled()` waits for all Promises and returns an array of objects. Each has `status` ("fulfilled" or "rejected") and either `value` (success) or `reason` (failure).
</details>

---

#### **Snippet 14: Implicit Return undefined**
```javascript
Promise.resolve(10)
    .then(num => {
        console.log(num * 2);
        // No return statement
    })
    .then(result => console.log("Result:", result));
```

**🤔 Think:** What happens when `.then()` doesn't return anything?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
20
Result: undefined
```

**Why:** If a `.then()` handler doesn't explicitly return a value, it implicitly returns `undefined`. The next `.then()` receives `undefined`.
</details>

---

#### **Snippet 15: Promise.any() with All Rejections**
```javascript
const p1 = Promise.reject("Error 1");
const p2 = Promise.reject("Error 2");
const p3 = Promise.reject("Error 3");

Promise.any([p1, p2, p3])
    .then(result => console.log("Success:", result))
    .catch(error => console.log("All failed:", error.errors));
```

**🤔 Think:** What happens when ALL Promises reject in `.any()`?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:** `All failed: ['Error 1', 'Error 2', 'Error 3']`

**Why:** `Promise.any()` rejects only if ALL Promises reject. The rejection is an `AggregateError` containing all rejection reasons in the `.errors` array.
</details>

---

## 4. GUIDED PRACTICE PROBLEMS

### 🔥 Problem 1: WARM-UP - Sequential Timer

**Difficulty:** ⭐ Easy  
**Concepts:** Promise creation, basic chaining, setTimeout

#### 📋 Task Description
Create a function `delayMessage(message, seconds)` that returns a Promise which resolves with the given message after the specified number of seconds. Then chain three of these to create a countdown: "3...", "2...", "1...", "GO!".

#### 🎯 Requirements
1. Create a `delayMessage()` function that uses Promise and setTimeout
2. The function should accept a message and delay in seconds
3. Chain four delays to create: "3..." (1s) → "2..." (1s) → "1..." (1s) → "GO!" (0s)
4. Log each message to the console as it resolves

#### 💻 Starter Code
```javascript
// Step 1: Create the delayMessage function
function delayMessage(message, seconds) {
    // Your code here
}

// Step 2: Chain the promises for countdown
// Your code here
```

#### ✅ Solution
```javascript
// Step 1: Create the Promise-based delay function
function delayMessage(message, seconds) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve(message);
        }, seconds * 1000);
    });
}

// Step 2: Chain promises for countdown
delayMessage("3...", 1)
    .then((msg) => {
        console.log(msg);
        return delayMessage("2...", 1);
    })
    .then((msg) => {
        console.log(msg);
        return delayMessage("1...", 1);
    })
    .then((msg) => {
        console.log(msg);
        return delayMessage("GO!", 0);
    })
    .then((msg) => {
        console.log(msg);
    })
    .catch((error) => {
        console.error("Countdown failed:", error);
    });

// Alternative: More elegant with arrow functions
delayMessage("3...", 1)
    .then(msg => (console.log(msg), delayMessage("2...", 1)))
    .then(msg => (console.log(msg), delayMessage("1...", 1)))
    .then(msg => (console.log(msg), delayMessage("GO!", 0)))
    .then(msg => console.log(msg))
    .catch(error => console.error("Countdown failed:", error));
```

#### ⚠️ Common Mistakes
1. **Forgetting to return the Promise in .then()**
   ```javascript
   // ❌ WRONG - breaks the chain
   .then(msg => {
       console.log(msg);
       delayMessage("2...", 1); // Not returned!
   })
   
   // ✅ CORRECT
   .then(msg => {
       console.log(msg);
       return delayMessage("2...", 1);
   })
   ```

2. **Using setTimeout without Promise**
   ```javascript
   // ❌ WRONG - setTimeout doesn't return a Promise
   function delayMessage(message, seconds) {
       setTimeout(() => message, seconds * 1000);
   }
   ```

3. **Not handling errors**
   - Always add `.catch()` at the end of Promise chains

---

### 🔥 Problem 2: THE LOGIC BUILDER - User Authentication Flow

**Difficulty:** ⭐⭐ Medium  
**Concepts:** Promise chaining, conditional logic, error handling, Promise.reject

#### 📋 Task Description
Simulate a complete user authentication system with three steps:
1. Validate credentials (username & password)
2. Fetch user data from "database"
3. Load user permissions

Each step should be a separate Promise. If any step fails, the entire flow should stop and handle the error appropriately.

#### 🎯 Requirements
1. `validateCredentials(username, password)` - Returns Promise
   - Resolves if username is "admin" and password is "password123"
   - Rejects with "Invalid credentials" otherwise
   
2. `fetchUserData(username)` - Returns Promise
   - Simulates 1-second delay
   - Resolves with user object: `{id: 1, name: username, email: "user@example.com"}`
   
3. `loadPermissions(userId)` - Returns Promise
   - Simulates 0.5-second delay
   - Resolves with permissions array: `["read", "write", "delete"]`
   
4. Chain all three functions and log final result with user data and permissions

#### 💻 Starter Code
```javascript
// Function 1: Validate credentials
function validateCredentials(username, password) {
    // Your code here
}

// Function 2: Fetch user data
function fetchUserData(username) {
    // Your code here
}

// Function 3: Load permissions
function loadPermissions(userId) {
    // Your code here
}

// Chain them together
// Your code here
```

#### ✅ Solution
```javascript
// Function 1: Validate credentials
function validateCredentials(username, password) {
    return new Promise((resolve, reject) => {
        // Simulate credential check
        setTimeout(() => {
            if (username === "admin" && password === "password123") {
                resolve(username);
            } else {
                reject("Invalid credentials");
            }
        }, 500);
    });
}

// Function 2: Fetch user data
function fetchUserData(username) {
    return new Promise((resolve) => {
        setTimeout(() => {
            const user = {
                id: 1,
                name: username,
                email: `${username}@example.com`
            };
            resolve(user);
        }, 1000);
    });
}

// Function 3: Load permissions
function loadPermissions(userId) {
    return new Promise((resolve) => {
        setTimeout(() => {
            const permissions = ["read", "write", "delete"];
            resolve(permissions);
        }, 500);
    });
}

// Complete authentication flow
function authenticateUser(username, password) {
    let userData;
    
    validateCredentials(username, password)
        .then((validatedUsername) => {
            console.log("✅ Credentials validated for:", validatedUsername);
            return fetchUserData(validatedUsername);
        })
        .then((user) => {
            console.log("✅ User data fetched:", user);
            userData = user; // Store for later use
            return loadPermissions(user.id);
        })
        .then((permissions) => {
            console.log("✅ Permissions loaded:", permissions);
            console.log("🎉 Authentication complete!");
            console.log("Final user object:", {
                ...userData,
                permissions: permissions
            });
        })
        .catch((error) => {
            console.error("❌ Authentication failed:", error);
        });
}

// Test with correct credentials
authenticateUser("admin", "password123");

// Test with wrong credentials
// authenticateUser("admin", "wrongpassword");
```

#### ⚠️ Common Mistakes
1. **Not preserving data across chain**
   ```javascript
   // ❌ WRONG - userData is lost
   .then(user => loadPermissions(user.id))
   .then(permissions => {
       console.log(userData); // undefined!
   })
   
   // ✅ CORRECT - Store in outer scope
   let userData;
   .then(user => {
       userData = user;
       return loadPermissions(user.id);
   })
   ```

2. **Handling errors at wrong level**
   ```javascript
   // ❌ WRONG - Individual catches prevent chain continuation
   validateCredentials()
       .catch(err => console.log(err))
       .then(() => fetchUserData()) // Still runs!
   
   // ✅ CORRECT - Single catch at end
   validateCredentials()
       .then(() => fetchUserData())
       .catch(err => console.log(err))
   ```

3. **Forgetting to reject on validation failure**
   - Always use `reject()` for error conditions

---

### 🔥 Problem 3: THE PRO CHALLENGE - Parallel API Request Manager

**Difficulty:** ⭐⭐⭐ Hard  
**Concepts:** Promise.all, Promise.allSettled, Promise.race, error recovery, real-world patterns

#### 📋 Task Description
Build a robust API request manager that fetches data from multiple endpoints in parallel. The system should:
- Try multiple backup servers (fail over to next if one fails)
- Handle partial failures gracefully
- Include a timeout mechanism
- Provide detailed status for each request

#### 🎯 Requirements
1. `fetchFromAPI(url, delay, shouldFail)` - Simulates API call
   - Returns Promise that resolves/rejects based on `shouldFail`
   - Uses provided delay
   
2. `fetchWithTimeout(url, timeout)` - Adds timeout to fetch
   - Rejects if request takes longer than timeout
   - Uses `Promise.race()`
   
3. `fetchWithFallback(urls)` - Tries backup servers
   - Attempts each URL until one succeeds
   - Uses `Promise.any()`
   
4. `fetchAll Partners(urls)` - Fetches from all endpoints
   - Continues even if some fail
   - Returns status for each request
   - Uses `Promise.allSettled()`

#### 💻 Starter Code
```javascript
// Simulated API fetch
function fetchFromAPI(url, delay = 1000, shouldFail = false) {
    // Your code here
}

// Fetch with timeout
function fetchWithTimeout(url, timeout = 3000) {
    // Your code here
}

// Fetch with fallback servers
function fetchWithFallback(urls) {
    // Your code here
}

// Fetch from all partners
function fetchAllPartners(urls) {
    // Your code here
}

// Test your functions
// Your code here
```

#### ✅ Solution
```javascript
// 1. Simulated API fetch
function fetchFromAPI(url, delay = 1000, shouldFail = false) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (shouldFail) {
                reject(`Failed to fetch from ${url}`);
            } else {
                resolve({
                    url: url,
                    data: `Data from ${url}`,
                    timestamp: Date.now()
                });
            }
        }, delay);
    });
}

// 2. Fetch with timeout using Promise.race()
function fetchWithTimeout(url, timeout = 3000) {
    const fetchPromise = fetchFromAPI(url, 2000, false);
    
    const timeoutPromise = new Promise((_, reject) => {
        setTimeout(() => {
            reject(`Timeout: ${url} took longer than ${timeout}ms`);
        }, timeout);
    });
    
    return Promise.race([fetchPromise, timeoutPromise]);
}

// 3. Fetch with fallback servers using Promise.any()
function fetchWithFallback(urls) {
    const fetchPromises = urls.map((url, index) => {
        // Each server has different behavior
        const shouldFail = index < urls.length - 1; // Last one succeeds
        return fetchFromAPI(url, 1000, shouldFail);
    });
    
    return Promise.any(fetchPromises)
        .then(result => {
            console.log("✅ Successfully fetched from:", result.url);
            return result;
        })
        .catch(error => {
            console.error("❌ All servers failed");
            throw new Error("All backup servers unavailable");
        });
}

// 4. Fetch from all partners using Promise.allSettled()
function fetchAllPartners(urls) {
    const fetchPromises = urls.map((url, index) => 
        fetchFromAPI(url, 1000 + (index * 500), index % 2 === 0)
    );
    
    return Promise.allSettled(fetchPromises)
        .then(results => {
            console.log("\n📊 Detailed Status Report:");
            console.log("=" .repeat(50));
            
            const report = results.map((result, index) => {
                if (result.status === "fulfilled") {
                    console.log(`✅ ${urls[index]}: SUCCESS`);
                    console.log(`   Data: ${result.value.data}`);
                    return {
                        url: urls[index],
                        status: "success",data: result.value.data
                    };
                } else {
                    console.log(`❌ ${urls[index]}: FAILED`);
                    console.log(`   Reason: ${result.reason}`);
                    return {
                        url: urls[index],
                        status: "failed",
                        error: result.reason
                    };
                }
            });
            
            const successCount = report.filter(r => r.status === "success").length;
            console.log("=" .repeat(50));
            console.log(`📈 Summary: ${successCount}/${urls.length} requests succeeded\n`);
            
            return report;
        });
}

// === DEMO & TESTS ===

console.log("🚀 DEMO 1: Basic API Fetch");
fetchFromAPI("https://api.example.com/data", 1000, false)
    .then(result => console.log("Received:", result))
    .catch(error => console.error(error));

setTimeout(() => {
    console.log("\n⏱️ DEMO 2: Fetch with Timeout");
    fetchWithTimeout("https://slow-api.com/data", 1500)
        .then(result => console.log("Success:", result.url))
        .catch(error => console.error(error));
}, 2000);

setTimeout(() => {
    console.log("\n🔄 DEMO 3: Fetch with Fallback Servers");
    const backupServers = [
        "https://api1.example.com/data",  // Will fail
        "https://api2.example.com/data",  // Will fail
        "https://api3.example.com/data"   // Will succeed
    ];
    fetchWithFallback(backupServers);
}, 4000);

setTimeout(() => {
    console.log("\n🌐 DEMO 4: Fetch All Partners");
    const partnerAPIs = [
        "https://partner1.com/api",
        "https://partner2.com/api",
        "https://partner3.com/api",
        "https://partner4.com/api"
    ];
    fetchAllPartners(partnerAPIs);
}, 6000);
```

#### ⚠️ Common Mistakes
1. **Incorrect Promise.race() usage**
   ```javascript
   // ❌ WRONG - Both resolve, race returns first
   Promise.race([
       Promise.resolve("fast"),
       Promise.reject("slow")
   ])
   
   // ✅ CORRECT - Timeout should reject
   Promise.race([
       fetchPromise,
       new Promise((_, reject) => 
           setTimeout(() => reject("Timeout"), 3000)
       )
   ])
   ```

2. **Not handling Promise.any() failure**
   ```javascript
   // ❌ WRONG - Doesn't handle AggregateError
   Promise.any(promises).then(result => console.log(result))
   
   // ✅ CORRECT
   Promise.any(promises)
       .then(result => console.log(result))
       .catch(err => console.log("All failed:", err.errors))
   ```

3. **Confusing Promise.all() with Promise.allSettled()**
   - Use `.all()` when ALL must succeed
   - Use `.allSettled()` when you need results regardless

---

## 5. DEBUG CHALLENGES

**Instructions:** Each code snippet below has a bug related to Promises. Find the bug, explain what's wrong, and provide the corrected code.

---

### 🐛 Challenge 1: The Silent Failure

```javascript
function getUserData(userId) {
    return new Promise((resolve, reject) => {
        if (userId <= 0) {
            reject("Invalid user ID");
        }
        
        setTimeout(() => {
            resolve({ id: userId, name: "John Doe" });
        }, 1000);
    });
}

getUserData(-1)
    .then(user => console.log("User:", user));
```

<details>
<summary>🔍 Click to see the bug & solution</summary>

#### ❌ The Bug
**Missing error handler!** When the Promise rejects with "Invalid user ID", there's no `.catch()` to handle it. This causes an unhandled Promise rejection.

#### ✅ Fixed Code
```javascript
function getUserData(userId) {
    return new Promise((resolve, reject) => {
        if (userId <= 0) {
            reject("Invalid user ID");
        }
        
        setTimeout(() => {
            resolve({ id: userId, name: "John Doe" });
        }, 1000);
    });
}

getUserData(-1)
    .then(user => console.log("User:", user))
    .catch(error => console.error("Error:", error)); // Added error handler
```

#### 📚 Lesson
**ALWAYS add `.catch()` to handle Promise rejections.** Unhandled rejections can crash your application in Node.js or cause warnings in browsers.
</details>

---

### 🐛 Challenge 2: The Broken Chain

```javascript
function step1() {
    return Promise.resolve("Step 1 complete");
}

function step2(previousResult) {
    return Promise.resolve(previousResult + " -> Step 2 complete");
}

function step3(previousResult) {
    return Promise.resolve(previousResult + " -> Step 3 complete");
}

step1()
    .then(result1 => {
        console.log(result1);
        step2(result1);
    })
    .then(result2 => {
        console.log(result2);
        return step3(result2);
    })
    .then(result3 => {
        console.log(result3);
    });
```

<details>
<summary>🔍 Click to see the bug & solution</summary>

#### ❌ The Bug
**Missing return statement in the first `.then()`!** The call to `step2(result1)` is not returned, so the Promise chain breaks. The second `.then()` receives `undefined` instead of the result from `step2()`.

#### ✅ Fixed Code
```javascript
function step1() {
    return Promise.resolve("Step 1 complete");
}

function step2(previousResult) {
    return Promise.resolve(previousResult + " -> Step 2 complete");
}

function step3(previousResult) {
    return Promise.resolve(previousResult + " -> Step 3 complete");
}

step1()
    .then(result1 => {
        console.log(result1);
        return step2(result1); // Added return!
    })
    .then(result2 => {
        console.log(result2);
        return step3(result2);
    })
    .then(result3 => {
        console.log(result3);
    });
```

#### 📚 Lesson
**Always return Promises in `.then()` handlers** when chaining. Without the return, the next `.then()` won't wait for the Promise and will receive `undefined`.
</details>

---

### 🐛 Challenge 3: The State Confusion

```javascript
let myPromise = new Promise((resolve, reject) => {
    resolve("First resolution");
    
    setTimeout(() => {
        resolve("Second resolution");
    }, 1000);
    
    reject("This is an error");
});

myPromise
    .then(result => console.log("Result:", result))
    .catch(error => console.error("Error:", error));
```

<details>
<summary>🔍 Click to see the bug & solution</summary>

#### ❌ The Bug
**Misunderstanding Promise state immutability.** The developer expects all three statements (two `resolve()` and one `reject()`) to have an effect, but only the first `resolve("First resolution")` matters.

**Expected behavior:** Might think it logs multiple results or errors  
**Actual behavior:** Only logs "Result: First resolution"

#### ✅ Corrected Understanding
```javascript
// This is the correct behavior, not a bug to "fix"
let myPromise = new Promise((resolve, reject) => {
    resolve("First resolution"); // ✅ This takes effect
    
    // ❌ These are all ignored - Promise is already settled
    setTimeout(() => {
        resolve("Second resolution"); // Ignored
    }, 1000);
    
    reject("This is an error"); // Ignored
});

myPromise
    .then(result => console.log("Result:", result)) // Logs: "Result: First resolution"
    .catch(error => console.error("Error:", error)); // Never runs
```

#### 📚 Lesson
**A Promise can only settle ONCE.** Once `resolve()` or `reject()` is called, the Promise state is locked forever. All subsequent calls to `resolve()` or `reject()` are silently ignored.
</details>

---

### 🐛 Challenge 4: The Finally Fallacy

```javascript
function processData() {
    let loading = true;
    
    return fetch('https://api.example.com/data')
        .then(response => response.json())
        .finally(() => {
            loading = false;
            return "Processing complete"; // Try to return result
        })
        .then(result => {
            console.log("Final result:", result);
        });
}
```

<details>
<summary>🔍 Click to see the bug & solution</summary>

#### ❌ The Bug
**Misunderstanding `.finally()` behavior.** The developer expects the return value `"Processing complete"` from `.finally()` to be passed to the next `.then()`, but `.finally()` passes through the original Promise value, not its return value.

**What they expect:** "Final result: Processing complete"  
**What actually happens:** "Final result: [the parsed JSON data]"

#### ✅ Fixed Code
```javascript
function processData() {
    let loading = true;
    
    return fetch('https://api.example.com/data')
        .then(response => response.json())
        .then(data => {
            // Process the data and return final result
            return {
                data: data,
                message: "Processing complete"
            };
        })
        .finally(() => {
            loading = false; // Cleanup happens here
            // Don't try to return values from finally
        })
        .then(result => {
            console.log("Final result:", result);
        });
}
```

#### 📚 Lesson
**`.finally()` passes through the original value/error** (Rule 4). Use it only for cleanup like closing connections, stopping loaders, etc. If you need to transform the result, use `.then()` before `.finally()`.
</details>

---

### 🐛 Challenge 5: The Async Trap

```javascript
function getUsersData() {
    const users = [1, 2, 3, 4, 5];
    const results = [];
    
    for (let userId of users) {
        fetchUser(userId)
            .then(user => {
                results.push(user);
            });
    }
    
    return results;
}

function fetchUser(id) {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve({ id: id, name: `User ${id}` });
        }, 100);
    });
}

console.log("Users:", getUsersData());
```

<details>
<summary>🔍 Click to see the bug & solution</summary>

#### ❌ The Bug
**Returning before Promises resolve!** The function returns the `results` array immediately (empty), before any of the `fetchUser()` Promises have resolved. The async operations are still in progress when the return statement executes.

**What they expect:** Array of 5 users  
**What actually happens:** Empty array `[]`

#### ✅ Fixed Code
```javascript
// Solution 1: Return a Promise with Promise.all()
function getUsersData() {
    const users = [1, 2, 3, 4, 5];
    
    const promises = users.map(userId => fetchUser(userId));
    
    return Promise.all(promises);
}

function fetchUser(id) {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve({ id: id, name: `User ${id}` });
        }, 100);
    });
}

// Must use .then() because getUsersData() returns a Promise
getUsersData()
    .then(users => console.log("Users:", users));

// Solution 2: Using async/await (preview for next chapter)
async function getUsersDataAsync() {
    const users = [1, 2, 3, 4, 5];
    const promises = users.map(userId => fetchUser(userId));
    const results = await Promise.all(promises);
    return results;
}
```

#### 📚 Lesson
**You cannot return async results synchronously.** When dealing with Promises, your function must also return a Promise. Use `Promise.all()` to wait for multiple Promises and return a single Promise that resolves with all results.
</details>

---

## 6. COMMON PITFALLS & ANTI-PATTERNS

### ❌ Anti-Pattern 1: Nested Promises (Promise Hell)
```javascript
// ❌ DON'T DO THIS - Nested like callbacks
getData()
    .then(data => {
        processData(data)
            .then(processed => {
                saveData(processed)
                    .then(result => {
                        console.log(result);
                    });
            });
    });

// ✅ DO THIS - Flat chain
getData()
    .then(data => processData(data))
    .then(processed => saveData(processed))
    .then(result => console.log(result))
    .catch(error => console.error(error));
```
**Why it matters:** Nested Promises defeat the purpose of using Promises. Keep chains flat for better readability and error handling.

---

### ❌ Anti-Pattern 2: The Unnecessary Promise Constructor
```javascript
// ❌ DON'T DO THIS - Wrapping Promises unnecessarily
function getUser(id) {
    return new Promise((resolve, reject) => {
        fetchUserFromAPI(id) // Already returns a Promise!
            .then(user => resolve(user))
            .catch(error => reject(error));
    });
}

// ✅ DO THIS - Return the Promise directly
function getUser(id) {
    return fetchUserFromAPI(id);
}
```
**Why it matters:** Don't wrap Promises in Promises. If a function already returns a Promise, just return it directly.

---

### ❌ Anti-Pattern 3: Forgetting to Return in .then()
```javascript
// ❌ DON'T DO THIS - Chain breaks
promise
    .then(result => {
        anotherPromise(); // Not returned!
    })
    .then(result => {
        console.log(result); // undefined
    });

// ✅ DO THIS - Return the Promise
promise
    .then(result => {
        return anotherPromise();
    })
    .then(result => {
        console.log(result); // Correct value
    });

// ✅ EVEN BETTER - Arrow function implicit return
promise
    .then(result => anotherPromise())
    .then(result => console.log(result));
```
**Why it matters:** Breaking the chain causes subsequent `.then()` handlers to receive `undefined` instead of the Promise result.

---

### ❌ Anti-Pattern 4: Using .then() for Side Effects Only
```javascript
// ❌ DON'T DO THIS - Side effect without return
fetchData()
    .then(data => {
        console.log(data); // Just logging
    })
    .then(result => {
        // result is undefined!
        saveData(result);
    });

// ✅ DO THIS - Return the value after side effect
fetchData()
    .then(data => {
        console.log(data);
        return data; // Pass it along
    })
    .then(result => {
        saveData(result);
    });
```
**Why it matters:** If you need to both use a value and pass it along, remember to return it.

---

### ❌ Anti-Pattern 5: Creating Promises in Loops (Serial Instead of Parallel)
```javascript
// ❌ DON'T DO THIS - Runs serially (slow)
async function processItems(items) {
    for (let item of items) {
        await processItem(item); // Waits for each one
    }
}

// ✅ DO THIS - Run in parallel (fast)
function processItems(items) {
    const promises = items.map(item => processItem(item));
    return Promise.all(promises);
}
```
**Why it matters:** If operations are independent, run them in parallel for better performance. Use `Promise.all()` instead of sequential awaits.

---

### ❌ Anti-Pattern 6: Catching Too Early
```javascript
// ❌ DON'T DO THIS - Catches prevent chain continuation
fetchData()
    .catch(err => console.log("Fetch failed"))
    .then(data => processData(data)) // Still runs even if fetch failed!

// ✅ DO THIS - Catch at the end
fetchData()
    .then(data => processData(data))
    .then(result => saveResult(result))
    .catch(err => console.log("Operation failed:", err));
```
**Why it matters:** `.catch()` in the middle recovers from the error and continues the chain. Put `.catch()` at the end unless you specifically want to recover and continue.

---

### ⚡ Performance Tip: Avoid Promise Constructor When Not Needed
```javascript
// ❌ SLOWER - Unnecessary Promise creation
function delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

// ✅ BETTER - Use Promise.resolve() for already-known values
function getStaticData() {
    return Promise.resolve({ name: "John", age: 30 });
}

// Instead of:
function getStaticData() {
    return new Promise(resolve => {
        resolve({ name: "John", age: 30 });
    });
}
```

---

## 7. KNOWLEDGE CHECK MCQs

**Instructions:** Choose the best answer for each question.

---

### Question 1
What is the initial state of a newly created Promise?

A) Fulfilled  
B) Rejected  
C) Pending  
D) Settled

<details>
<summary>✅ Answer</summary>

**C) Pending**

Every Promise starts in the "pending" state when created. It transitions to either "fulfilled" (resolved) or "rejected" based on the outcome of the asynchronous operation.
</details>

---

### Question 2
What happens if you call both `resolve()` and `reject()` in the same Promise executor?

A) Both are executed in order  
B) Only the first call takes effect  
C) An error is thrown  
D) The Promise enters an error state

<details>
<summary>✅ Answer</summary>

**B) Only the first call takes effect**

A Promise can only settle once. Whichever is called first (resolve or reject) determines the final state. All subsequent calls are ignored because the Promise is already settled (immutable).
</details>

---

### Question 3
Which method should you use to handle Promise rejection?

A) .then()  
B) .catch()  
C) .finally()  
D) .reject()

<details>
<summary>✅ Answer</summary>

**B) .catch()**

`.catch()` is specifically designed to handle Promise rejections. While `.then()` can also accept an error handler as its second argument, `.catch()` is clearer and recommended for error handling.
</details>

---

### Question 4
What does `.finally()` return?

A) undefined  
B) The original Promise result  
C) A new Promise that passes through the original value  
D) null

<details>
<summary>✅ Answer</summary>

**C) A new Promise that passes through the original value**

`.finally()` returns a new Promise but passes through the original Promise's value or error. Its return value is ignored (unless it throws an error).
</details>

---

### Question 5
```javascript
const p = Promise.resolve(10);
p.then(v => v + 1);
p.then(v => v + 2);
```
What values are logged if we console.log both `.then()` results?

A) 11, 13  
B) 11, 12  
C) 10, 10  
D) 11, 11

<details>
<summary>✅ Answer</summary>

**B) 11, 12**

This is NOT chaining (Rule 5). Each `.then()` is called independently on the original Promise `p`, which resolved to 10. The first `.then()` receives 10 and returns 11. The second `.then()` also receives 10 and returns 12. They don't affect each other.
</details>

---

### Question 6
What does `Promise.all()` do if one Promise rejects?

A) Waits for all Promises to complete  
B) Immediately rejects with that error  
C) Ignores the rejection  
D) Retries the failed Promise

<details>
<summary>✅ Answer</summary>

**B) Immediately rejects with that error**

`Promise.all()` uses "fail-fast" behavior. If ANY Promise rejects, the entire operation immediately rejects with that error, without waiting for other Promises.
</details>

---

### Question 7
Which Promise method would you use to fetch data from multiple backup servers, needing only ONE to succeed?

A) Promise.all()  
B) Promise.race()  
C) Promise.any()  
D) Promise.allSettled()

<details>
<summary>✅ Answer</summary>

**C) Promise.any()**

`Promise.any()` resolves with the first Promise that fulfills. It ignores rejections and only fails if ALL Promises reject. This is perfect for fallback/backup scenarios.
</details>

---

### Question 8
What is the purpose of `Promise.race()`?

A) To execute Promises sequentially  
B) To return the result of the fastest Promise  
C) To compare Promise execution times  
D) To retry failed Promises

<details>
<summary>✅ Answer</summary>

**B) To return the result of the fastest Promise**

`Promise.race()` resolves or rejects with the first Promise that settles (either fulfilled or rejected), whichever completes first.
</details>

---

### Question 9
```javascript
Promise.resolve(5)
    .then(x => x * 2)
    .then(x => { x + 10 })
    .then(result => console.log(result));
```
What is logged?

A) 20  
B) 10  
C) undefined  
D) 15

<details>
<summary>✅ Answer</summary>

**C) undefined**

The second `.then()` has `{ x + 10 }` which is a block, not an expression. It doesn't return anything, so it implicitly returns `undefined`. The third `.then()` receives `undefined`.

**Correct version would be:** `.then(x => x + 10)` (no braces, implicit return)
</details>

---

### Question 10
When should you use `Promise.allSettled()` instead of `Promise.all()`?

A) When you need better performance  
B) When you need results from all Promises regardless of success/failure  
C) When you want to fail fast  
D) When working with only two Promises

<details>
<summary>✅ Answer</summary>

**B) When you need results from all Promises regardless of success/failure**

`Promise.allSettled()` waits for all Promises to complete and returns status for each (fulfilled or rejected). Use it when partial failures are acceptable and you need to know what happened with each Promise.
</details>

---

### Question 11
What does `Promise.resolve(promise)` return?

A) A new Promise wrapping the original  
B) The same Promise (unchanged)  
C) undefined  
D) A resolved value

<details>
<summary>✅ Answer</summary>

**B) The same Promise (unchanged)**

If you pass a Promise to `Promise.resolve()`, it returns that Promise as-is without wrapping it. This is useful for normalizing values that might or might not be Promises.
</details>

---

### Question 12
Which is the correct way to chain Promises returned from functions?

A) `getData().then(data => { processData(data); })`  
B) `getData().then(data => processData(data))`  
C) `getData().then(processData(data))`  
D) `getData(processData())`

<details>
<summary>✅ Answer</summary>

**B) `getData().then(data => processData(data))`**

You must return the Promise from `processData()` to continue the chain. Option B correctly passes the data and returns the resulting Promise. Option A doesn't return, breaking the chain.
</details>

---

### Question 13
What happens if you throw an error inside a `.then()` handler?

A) The program crashes  
B) The error is ignored  
C) The Promise rejects and goes to `.catch()`  
D) The error is logged to console

<details>
<summary>✅ Answer</summary>

**C) The Promise rejects and goes to `.catch()`**

Throwing an error inside `.then()` automatically rejects the returned Promise (Rule 2). The error skips subsequent `.then()` handlers and is caught by the next `.catch()` in the chain.
</details>

---

### Question 14
Can you convert a callback-based function to return a Promise?

A) No, it's not possible  
B) Yes, by wrapping it in a Promise constructor  
C) Only if the original function supports it  
D) Yes, but only with async/await

<details>
<summary>✅ Answer</summary>

**B) Yes, by wrapping it in a Promise constructor**

You can promisify any callback-based function by wrapping it in a Promise:
```javascript
function promisify(fn) {
    return function(...args) {
        return new Promise((resolve, reject) => {
            fn(...args, (err, result) => {
                if (err) reject(err);
                else resolve(result);
            });
        });
    };
}
```
</details>

---

### Question 15
What is the main advantage of Promises over callbacks?

A) Promises are faster  
B) Promises use less memory  
C) Promises avoid callback hell and provide better error handling  
D) Promises don't require error handling

<details>
<summary>✅ Answer</summary>

**C) Promises avoid callback hell and provide better error handling**

Promises provide:
- Flat, chainable syntax (no deep nesting)
- Unified error handling with `.catch()`
- Better composition with methods like `.all()` and `.race()`
- More readable and maintainable code
</details>

---

## 8. REAL INTERVIEW QUESTIONS

---

### 📌 Question 1: Explain the Promise lifecycle and states

**Difficulty:** ⭐⭐ Medium

#### 👨‍💼 Junior Answer
```
A Promise has three states: pending, fulfilled, and rejected. 
When you create a Promise, it starts as pending. Then it either 
becomes fulfilled when it succeeds or rejected when it fails.
```

#### 🎯 Senior Answer
```
A Promise represents an asynchronous operation and has three states:

1. PENDING (initial state):
   - The operation hasn't completed yet
   - Result is undefined
   - Can transition to fulfilled or rejected

2. FULFILLED (resolved):
   - The operation completed successfully
   - Has a resolved value
   - State is immutable - cannot change anymore

3. REJECTED (failed):
   - The operation failed
   - Has a rejection reason (error)
   - State is immutable - cannot change anymore

Once a Promise transitions to fulfilled or rejected, we say it's 
"settled." A settled Promise's state is permanent - you cannot 
resolve a Promise that's already rejected, or vice versa.

This immutability is crucial for predictable async behavior and 
prevents race conditions where multiple async operations might 
try to settle the same Promise.
```

#### 💻 Code Example
```javascript
// Demonstrate state transitions
const promise = new Promise((resolve, reject) => {
    console.log("State: pending");
    
    setTimeout(() => {
        resolve("Success!");
        // State: fulfilled (immutable from here)
        
        // These are ignored - Promise already settled
        reject("This won't work");
        resolve("Neither will this");
    }, 1000);
});

promise
    .then(result => {
        console.log("Fulfilled with:", result);
        // State remains: fulfilled
    })
    .catch(error => {
        // Never executes because Promise resolved
        console.log("Rejected with:", error);
    });
```

#### 🎤 Follow-up Questions to Anticipate
- **Q:** "What's the difference between fulfilled and resolved?"
  - **A:** Technically, "resolved" can mean the Promise is settled with a value OR with another Promise/thenable. "Fulfilled" specifically means it has a final value. In practice, most developers use them interchangeably.

- **Q:** "Can you check a Promise's state from outside?"
  - **A:** No, there's no built-in way to synchronously check a Promise's state. This is intentional - it forces you to use `.then()/.catch()` which properly handles async timing.

---

### 📌 Question 2: What's the difference between Promise.all(), Promise.race(), Promise.any(), and Promise.allSettled()?

**Difficulty:** ⭐⭐⭐ Hard

#### 👨‍💼 Junior Answer
```
Promise.all() waits for all Promises to finish.
Promise.race() returns whichever Promise finishes first.
Promise.any() and Promise.allSettled() are newer methods 
that handle Promises differently.
```

#### 🎯 Senior Answer
```
These methods handle multiple Promises with different strategies:

1. Promise.all([p1, p2, p3])
   - Use case: All operations must succeed (loading multiple required resources)
   - Behavior: Fail-fast - rejects immediately if ANY Promise rejects
   - Returns: Array of all resolved values (in order)
   - Rejects with: The first rejection reason

2. Promise.race([p1, p2, p3])
   - Use case: Need the fastest result (timeout implementations, competing APIs)
   - Behavior: Settles with the first Promise to settle (resolve OR reject)
   - Returns: Single value from the fastest Promise
   - Useful for: Timeouts, racing requests to multiple servers

3. Promise.any([p1, p2, p3])
   - Use case: Need at least ONE success (fallback servers, redundant APIs)
   - Behavior: Ignores rejections, resolves with first fulfillment
   - Returns: First successful value
   - Rejects only if: ALL Promises reject (returns AggregateError)

4. Promise.allSettled([p1, p2, p3])
   - Use case: Need all results regardless of success/failure (analytics, logging)
   - Behavior: Never rejects - always waits for all to settle
   - Returns: Array of objects: {status: "fulfilled"/"rejected", value/reason}
   - Useful for: Partial failures are acceptable, need complete status report
```

#### 💻 Code Example
```javascript
// Setup: Mix of fast/slow, success/failure Promises
const fast = Promise.resolve("Fast success");
const slow = new Promise(resolve => setTimeout(() => resolve("Slow success"), 2000));
const failing = Promise.reject("Fast failure");

// 1. Promise.all() - Fails fast
console.log("--- Promise.all() ---");
Promise.all([fast, slow, failing])
    .then(results => console.log("All succeeded:", results))
    .catch(error => console.log("❌ Failed:", error)); // Logs immediately

// 2. Promise.race() - First to settle wins
console.log("\n--- Promise.race() ---");
Promise.race([fast, slow, failing])
    .then(result => console.log("✅ Winner:", result))
    .catch(error => console.log("❌ Winner:", error)); // Depends which is faster

// 3. Promise.any() - First success wins
console.log("\n--- Promise.any() ---");
Promise.any([failing, slow, fast])
    .then(result => console.log("✅ First success:", result)) // "Fast success"
    .catch(error => console.log("All failed:", error.errors));

// 4. Promise.allSettled() - Complete report
console.log("\n--- Promise.allSettled() ---");
Promise.allSettled([fast, slow, failing])
    .then(results => {
        results.forEach((result, i) => {
            if (result.status === "fulfilled") {
                console.log(`✅ Promise ${i}: ${result.value}`);
            } else {
                console.log(`❌ Promise ${i}: ${result.reason}`);
            }
        });
    });
```

#### 📊 Decision Matrix
```
Need ALL to succeed?          → Promise.all()
Need fastest (success/fail)?  → Promise.race()
Need at least ONE success?    → Promise.any()
Need complete status report?  → Promise.allSettled()
```

#### 🎤 Follow-up Questions to Anticipate
- **Q:** "When would you use Promise.race() for timeouts?"
  - **A:** 
  ```javascript
  function fetchWithTimeout(url, timeout = 3000) {
      return Promise.race([
          fetch(url),
          new Promise((_, reject) => 
              setTimeout(() => reject(new Error('Timeout')), timeout)
          )
      ]);
  }
  ```

- **Q:** "What's an AggregateError in Promise.any()?"
  - **A:** It's a special error type that contains an `.errors` array with all rejection reasons when ALL Promises fail. Introduced with ES2021.

---

### 📌 Question 3: How do you handle errors in Promise chains? Show different approaches.

**Difficulty:** ⭐⭐⭐ Hard

#### 👨‍💼 Junior Answer
```
You use .catch() at the end of the Promise chain to handle errors. 
If any Promise rejects, the catch block will handle it.
```

#### 🎯 Senior Answer
```
There are multiple strategies for error handling in Promise chains, 
each with different use cases:

1. SINGLE .catch() AT THE END (most common):
   - Catches all errors in the chain
   - Clean and simple
   - Cannot recover and continue

2. INTERMEDIATE .catch() FOR RECOVERY:
   - Catch errors midway and provide fallback values
   - Chain continues after recovery
   - Useful for non-critical failures

3. MULTIPLE .catch() FOR SPECIFIC ERRORS:
   - Different handlers for different error types
   - Can rethrow to propagate specific errors
   - More granular control

4. .then() WITH TWO ARGUMENTS:
   - Less common, but valid
   - Separates success/error handlers
   - .catch() is generally preferred for clarity

5. FINALLY FOR CLEANUP:
   - Always runs regardless of success/failure
   - Doesn't affect the Promise value
   - Perfect for cleanup operations (closing connections, stopping loaders)

Key principle: Errors propagate down the chain until caught. 
After catching, the chain continues with a resolved Promise 
unless you rethrow the error.
```

#### 💻 Code Examples

```javascript
// PATTERN 1: Single .catch() at end (Standard)
fetchUser(userId)
    .then(user => fetchUserPosts(user.id))
    .then(posts => filterActivePosts(posts))
    .then(activePosts => displayPosts(activePosts))
    .catch(error => {
        console.error("Operation failed:", error);
        showErrorToUser(error.message);
    });

// PATTERN 2: Intermediate .catch() for recovery
fetchPrimaryAPI()
    .catch(error => {
        console.log("Primary failed, trying backup");
        return fetchBackupAPI(); // Recover with fallback
    })
    .then(data => processData(data))
    .catch(error => {
        console.error("All APIs failed:", error);
    });

// PATTERN 3: Specific error handling with rethrowing
authenticateUser(credentials)
    .catch(error => {
        if (error.code === 401) {
            console.log("Unauthorized - redirecting to login");
            redirectToLogin();
            throw error; // Rethrow to stop chain
        } else if (error.code === 500) {
            console.log("Server error - retrying");
            return authenticateUser(credentials); // Retry
        }
        throw error; // Rethrow unknown errors
    })
    .then(user => loadUserData(user))
    .catch(error => {
        console.error("Authentication flow failed:", error);
    });

// PATTERN 4: Error handling per operation
fetchData()
    .then(
        data => processData(data),           // Success handler
        error => console.log("Fetch failed") // Error handler
    )
    .then(
        result => saveResult(result),
        error => console.log("Process failed")
    );

// PATTERN 5: Using .finally() for cleanup
let isLoading = false;

function loadUserProfile(userId) {
    isLoading = true;
    showLoadingSpinner();
    
    return fetchUser(userId)
        .then(user => enrichUserData(user))
        .then(enrichedUser => displayProfile(enrichedUser))
        .catch(error => {
            console.error("Profile load failed:", error);
            showErrorMessage(error.message);
        })
        .finally(() => {
            isLoading = false;
            hideLoadingSpinner();
            // Cleanup happens whether success or failure
        });
}

// PATTERN 6: Graceful degradation (advanced)
function loadPageData() {
    const essentialData = fetchEssentialData()
        .catch(error => {
            throw new Error("Cannot load page - essential data failed");
        });
    
    const optionalData = fetchOptionalData()
        .catch(error => {
            console.warn("Optional data failed, continuing anyway");
            return null; // Provide default instead of failing
        });
    
    return Promise.all([essentialData, optionalData])
        .then(([essential, optional]) => {
            return {
                essential,
                optional: optional || getDefaultOptionalData()
            };
        });
}
```

#### ⚠️ Common Error Handling Mistakes

```javascript
// ❌ MISTAKE 1: Catching too early stops the chain
getData()
    .catch(err => console.log("Error:", err))
    .then(data => process(data)) // Still runs even if getData failed!

// ✅ CORRECT: Catch at the end OR rethrow
getData()
    .catch(err => {
        console.log("Error:", err);
        throw err; // Rethrow to stop chain
    })
    .then(data => process(data));

// ❌ MISTAKE 2: Swallowing errors silently
getData()
    .catch(err => {
        // Just logging, not rethrowing or handling
        console.log(err);
    });

// ✅ CORRECT: Properly handle or rethrow
getData()
    .catch(err => {
        console.error("Data fetch failed:", err);
        notifyErrorService(err);
        return getDefaultData(); // Or rethrow if you can't recover
    });

// ❌ MISTAKE 3: Not handling Promise rejections at all
getData()
    .then(data => process(data)); // Unhandled rejection if getData fails!

// ✅ CORRECT: Always add .catch()
getData()
    .then(data => process(data))
    .catch(err => console.error("Operation failed:", err));
```

#### 🎤 Follow-up Questions to Anticipate

- **Q:** "What's the difference between throwing an error and returning Promise.reject()?"
  - **A:** They're functionally equivalent inside a `.then()` handler - both reject the returned Promise. `throw` is more concise and readable. `Promise.reject()` is useful when you need to reject outside of try/catch blocks or want to be explicit.
  ```javascript
  // These are equivalent:
  .then(() => { throw new Error("Failed"); })
  .then(() => Promise.reject(new Error("Failed")))
  ```

- **Q:** "How do you handle errors in Promise.all()?"
  - **A:** 
  ```javascript
  // Option 1: Wrap each Promise to handle individually
  const safePromises = promises.map(p => 
      p.catch(error => ({ error }))
  );
  
  Promise.all(safePromises).then(results => {
      results.forEach(result => {
          if (result.error) {
              console.log("Failed:", result.error);
          } else {
              console.log("Success:", result);
          }
      });
  });
  
  // Option 2: Use Promise.allSettled() instead
  Promise.allSettled(promises).then(results => {
      // Handle each result's status
  });
  ```

- **Q:** "Can you recover from an error in a Promise chain?"
  - **A:** Yes! `.catch()` doesn't just handle errors - it can recover by returning a value. The chain continues with that value.
  ```javascript
  fetchPrimaryAPI()
      .catch(error => {
          console.log("Primary failed, using cache");
          return getCachedData(); // Chain continues with this
      })
      .then(data => processData(data)); // Works with either source
  ```

---

### 📌 Question 4: Write a function to promisify a callback-based function

**Difficulty:** ⭐⭐⭐ Hard

#### 👨‍💼 Junior Answer
```javascript
function promisify(fn) {
    return function() {
        return new Promise((resolve, reject) => {
            fn((err, result) => {
                if (err) reject(err);
                else resolve(result);
            });
        });
    };
}
```

#### 🎯 Senior Answer
```javascript
/**
 * Universal promisify function that handles:
 * - Multiple arguments
 * - Context (this) binding
 * - Error-first callbacks (Node.js convention)
 * - Edge cases
 */
function promisify(fn) {
    return function(...args) {
        return new Promise((resolve, reject) => {
            // Append callback as last argument
            fn.call(this, ...args, (err, result) => {
                if (err) {
                    reject(err);
                } else {
                    resolve(result);
                }
            });
        });
    };
}

// Usage examples:
// 1. Simple callback function
function readFileCallback(filename, callback) {
    setTimeout(() => {
        if (filename === "error.txt") {
            callback(new Error("File not found"));
        } else {
            callback(null, `Content of ${filename}`);
        }
    }, 100);
}

const readFile = promisify(readFileCallback);

readFile("test.txt")
    .then(content => console.log(content))
    .catch(error => console.error(error));

// 2. Method promisification with context
const obj = {
    value: 42,
    getValue(callback) {
        setTimeout(() => {
            callback(null, this.value);
        }, 100);
    }
};

obj.getValueAsync = promisify(obj.getValue);
obj.getValueAsync.call(obj)
    .then(val => console.log(val)); // 42

// 3. Advanced: Handle multiple callback arguments
function promisifyMultiArg(fn) {
    return function(...args) {
        return new Promise((resolve, reject) => {
            fn.call(this, ...args, (err, ...results) => {
                if (err) {
                    reject(err);
                } else {
                    // Return array if multiple results
                    resolve(results.length > 1 ? results : results[0]);
                }
            });
        });
    };
}
```

#### 💡 Why This Matters in Real Projects
```
In real-world projects, you'll encounter:
- Legacy codebases with callback-based APIs (fs.readFile, etc.)
- Third-party libraries that don't support Promises
- Need to gradually migrate callback code to Promise-based code

Node.js provides util.promisify() for this, but understanding 
how to build it yourself demonstrates deep Promise knowledge 
and is a common interview question.
```

#### 🎤 Follow-up Questions to Anticipate

- **Q:** "What if the callback doesn't follow the error-first convention?"
  - **A:** You'd need a custom promisify that matches the callback signature:
  ```javascript
  function promisifySuccess(fn) {
      return function(...args) {
          return new Promise((resolve) => {
              fn(...args, (result) => resolve(result));
          });
      };
  }
  ```

- **Q:** "How does Node.js's util.promisify() work?"
  - **A:** Similar concept, but with additional features:
    - Checks for `fn[util.promisify.custom]` first (custom promisify)
    - Handles multiple return values
    - Better error handling
  ```javascript
  const util = require('util');
  const fs = require('fs');
  const readFile = util.promisify(fs.readFile);
  ```

---

## 9. DEVTOOLS & DEBUGGING

### 🔍 Debugging Promises in Chrome DevTools

#### 1. **Using Console for Quick Testing**

```javascript
// Test Promises directly in console
Promise.resolve("test")
    .then(console.log)
    .catch(console.error);

// Inspect Promise state (Chrome shows [[PromiseState]] and [[PromiseResult]])
const p = new Promise(resolve => setTimeout(() => resolve("done"), 2000));
console.log(p); // Expand to see internal state
```

**Console Output:**
```
Promise {<pending>}
  [[Prototype]]: Promise
  [[PromiseState]]: "pending"
  [[PromiseResult]]: undefined

// After 2 seconds:
Promise {<fulfilled>: "done"}
  [[PromiseState]]: "fulfilled"
  [[PromiseResult]]: "done"
```

---

#### 2. **Sources Tab - Debugging Promise Chains**

**Step 1:** Open Sources tab → Set breakpoints in `.then()` handlers

```javascript
fetchData()
    .then(data => {
        debugger; // Breakpoint here
        return processData(data);
    })
    .then(result => {
        debugger; // And here
        return saveResult(result);
    })
    .catch(error => {
        debugger; // Check why it failed
        console.error(error);
    });
```

**Step 2:** Use the Call Stack panel to trace Promise execution

```
Call Stack when paused in .then():
  (anonymous)          ← Your .then() handler
  Promise.then
  (async)
  (anonymous)          ← Where Promise was created
```

---

#### 3. **Async Stack Traces**

Chrome DevTools automatically shows async stack traces for Promises!

**Enable:** Settings (⚙️) → Enable "Async stack traces"

```javascript
function level1() {
    return Promise.resolve()
        .then(() => level2());
}

function level2() {
    return Promise.resolve()
        .then(() => level3());
}

function level3() {
    throw new Error("Deep error!");
}

level1().catch(console.error);
```

**Console Output with Async Stack:**
```
Error: Deep error!
    at level3 (script.js:12)
    at async level2 (script.js:8)
    at async level1 (script.js:4)
```

---

#### 4. **Network Tab - Tracking Promise-Based API Calls**

When debugging `fetch()` or API calls wrapped in Promises:

1. Open Network tab
2. Filter by "Fetch/XHR"
3. Click on request → Preview/Response tabs
4. See timing in "Timing" tab

**Pro Tip:** Use "Preserve log" to keep requests across page reloads

---

#### 5. **Unhandled Promise Rejection Warnings**

Chrome automatically warns about unhandled rejections:

```javascript
// This will show warning in console
Promise.reject("Unhandled error");

// Console shows:
// Uncaught (in promise) Unhandled error
```

**How to debug:**
- Click the error → jumps to Sources tab
- Check Call Stack for where Promise was created
- Add `.catch()` handler

---

#### 6. **Performance Tab - Promise Timing**

Record performance while Promises execute:

1. Open Performance tab
2. Click Record (●)
3. Execute your Promise code
4. Stop recording
5. Analyze "Timings" section

**What to look for:**
- Long Promise chains (yellow bars)
- Blocking operations
- Microtask queue timing

---

#### 7. **Common Debugging Patterns**

```javascript
// Pattern 1: Log at each step
fetchData()
    .then(data => {
        console.log("Step 1 - Data fetched:", data);
        return processData(data);
    })
    .then(processed => {
        console.log("Step 2 - Data processed:", processed);
        return saveData(processed);
    })
    .then(saved => {
        console.log("Step 3 - Data saved:", saved);
    })
    .catch(error => {
        console.error("❌ Error at:", error);
        console.trace(); // Shows full stack trace
    });

// Pattern 2: Add Promise state inspector
function inspectPromise(promise, label) {
    console.log(`${label} - State:`, promise);
    return promise
        .then(result => {
            console.log(`${label} - Resolved:`, result);
            return result;
        })
        .catch(error => {
            console.error(`${label} - Rejected:`, error);
            throw error;
        });
}

// Usage:
inspectPromise(fetchData(), "API Call")
    .then(data => inspectPromise(processData(data), "Processing"))
    .then(result => inspectPromise(saveData(result), "Saving"));
```

---

#### 8. **Debugging Promise.all() Failures**

```javascript
// Hard to debug - which Promise failed?
Promise.all([promise1, promise2, promise3])
    .catch(error => console.error("One failed:", error));

// ✅ Better - Track each Promise
const promises = [
    { name: "API 1", promise: promise1 },
    { name: "API 2", promise: promise2 },
    { name: "API 3", promise: promise3 }
];

Promise.all(promises.map(p => 
    p.promise.catch(error => ({
        name: p.name,
        error: error
    }))
)).then(results => {
    results.forEach(result => {
        if (result.error) {
            console.error(`❌ ${result.name} failed:`, result.error);
        } else {
            console.log(`✅ ${result.name} succeeded`);
        }
    });
});
```

---

## 10. CHAPTER SUMMARY & NEXT STEPS

### 🎯 Quick Recap

In this chapter, you mastered JavaScript Promises:

✅ **Promise Fundamentals**
- Created Promises using the Promise constructor
- Understood the three states: pending, fulfilled, rejected
- Learned that Promises are immutable once settled

✅ **Promise Consumption**
- Used `.then()` for success handling
- Used `.catch()` for error handling
- Used `.finally()` for cleanup operations

✅ **The 5 Golden Rules of Chaining**
- Every fulfilled Promise has `.then()`, rejected has `.catch()`
- Three things you can do in `.then()`: return Promise, return value, throw error
- You can rethrow from `.catch()` to propagate errors
- `.finally()` passes through the original value
- Multiple `.then()` on same Promise ≠ chaining

✅ **Multiple Promise Handling**
- `Promise.all()` - All must succeed
- `Promise.race()` - First to settle wins
- `Promise.any()` - First success wins
- `Promise.allSettled()` - Get all results

✅ **Real-World Applications**
- Error handling strategies
- Promisifying callback functions
- Debugging with DevTools
- Performance considerations

---

### 🔗 Connection to Next Topic

**As discussed in the video**, Promises give us a much better way to handle async operations compared to callbacks, but the syntax can still feel a bit verbose with all those `.then()` chains.

**Coming up in Day 24 - Async/Await:**

You'll learn how to make Promise-based code look and behave more like synchronous code using `async` and `await` keywords. Instead of writing:

```javascript
fetchUser()
    .then(user => fetchPosts(user.id))
    .then(posts => displayPosts(posts))
    .catch(error => console.error(error));
```

You'll write:
```javascript
try {
    const user = await fetchUser();
    const posts = await fetchPosts(user.id);
    displayPosts(posts);
} catch (error) {
    console.error(error);
}
```

**The beauty?** Under the hood, it's still Promises - but with syntax sugar that makes async code much easier to read and write!

---

### ✨ You're Now Ready For:
- Writing production-ready async code with Promises
- Handling complex Promise chains confidently
- Debugging Promise issues in real projects
- Interview questions about Promises
- **Day 24: Async/Await** (the evolution of Promises!)

---

### 📚 Additional Practice Resources


1. **Practice Challenges**: Complete the tasks from the provided tasks document

2. **Real Project Idea**: Build a "Multi-API Weather Dashboard"
   - Fetch from 3 different weather APIs
   - Use Promise.any() for fastest response
   - Use Promise.allSettled() to show comparison
   - Implement timeout with Promise.race()

---

### 🎓 Final Challenge

Before moving to Day 24, complete this capstone exercise:

**Build a Promise-based Task Queue System**

Requirements:
- Process tasks sequentially (one after another)
- Each task returns a Promise
- Handle individual task failures without stopping the queue
- Track success/failure count
- Add timeout for each task
- Log detailed execution report

**Starter Template:**
```javascript
class TaskQueue {
    constructor() {
        this.tasks = [];
        this.results = [];
    }
    
    addTask(taskFn, timeout = 5000) {
        // Your code here
    }
    
    async processQueue() {
        // Your code here
    }
    
    getReport() {
        // Your code here
    }
}

// Usage:
const queue = new TaskQueue();
queue.addTask(() => fetchUserData(1));
queue.addTask(() => fetchUserData(2));
queue.addTask(() => fetchUserData(3));
queue.processQueue().then(() => {
    console.log(queue.getReport());
});
```

---

### 🚀 Ready for Day 24?

You've built a solid foundation with Promises. Next, you'll learn how async/await makes this even more elegant!

**See you in the next chapter!** 💪

---

<div align="center">

**🌟 Remember: Practice makes perfect! 🌟**

Complete the tasks, experiment in DevTools, and build real projects.

**Happy Coding!** 🚀

</div>