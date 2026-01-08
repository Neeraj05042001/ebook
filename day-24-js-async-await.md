# Day 24: Master JavaScript async/await & Simplify Promises Like a PRO


## 1. CHAPTER OVERVIEW

### 📹 In the Video
You learned how async/await provides syntactic sugar over Promises, transforming asynchronous code into a synchronous-looking structure. The lecture covered the async keyword, await operator, error handling with try/catch, and strategies for handling multiple asynchronous operations efficiently.

### ✅ Checklist: By the End of This Workbook, You Will Be Able To...
- [ ] Convert Promise-based code to async/await syntax
- [ ] Use try/catch blocks for proper error handling in async functions
- [ ] Identify when to use sequential vs parallel execution patterns
- [ ] Debug async/await code using Chrome DevTools
- [ ] Implement retry logic and timeout wrappers for production code
- [ ] Explain the differences between Promise.all() and Promise.allSettled()
- [ ] Recognize and avoid common async/await anti-patterns

### 📚 Prerequisites
From **Day 23 (Promises)**, you should understand:
- Promise states (pending, fulfilled, rejected)
- `.then()` and `.catch()` chaining
- Promise.all(), Promise.race(), Promise.allSettled()
- How the Event Loop handles asynchronous operations

---

## 2. LECTURE CHEAT SHEET

### 🔑 Key Terms Glossary

| Term | Definition |
|------|------------|
| **async function** | A function that always returns a Promise, even if you return a regular value |
| **await** | Pauses async function execution until a Promise settles (resolves/rejects) |
| **Syntactic Sugar** | Syntax designed to make code easier to read/write without adding new functionality |
| **Top-level await** | Using await outside async functions in ES modules (ES2022+) |
| **Sequential Execution** | Operations run one after another; each waits for the previous to complete |
| **Parallel Execution** | Operations start simultaneously; all run concurrently |

---

### 📊 async/await vs Promises Comparison

| Aspect | Promises | async/await |
|--------|----------|-------------|
| **Syntax** | `.then().catch()` chaining | `try/catch` blocks with `await` |
| **Readability** | Can become nested/complex | Reads like synchronous code |
| **Error Handling** | `.catch()` at end of chain | Traditional `try/catch` blocks |
| **Debugging** | Stack traces can be unclear | Better stack traces |
| **Multiple Operations** | `Promise.all([...])` | `await Promise.all([...])` |
| **Underlying Tech** | Native Promise implementation | Built on top of Promises |

---

### 🎯 async Keyword Syntax Reference

```javascript
// Function Declaration
async function myFunction() {
  return "value"; // Wrapped in Promise.resolve("value")
}

// Arrow Function
const myAsyncFn = async () => {
  return "value";
};

// Method in Object
const obj = {
  async myMethod() {
    return "value";
  }
};

// Method in Class
class MyClass {
  async myMethod() {
    return "value";
  }
}
```

**Key Rule:** An async function ALWAYS returns a Promise, regardless of what you return inside it.

```javascript
async function test() {
  return 42; // Returns Promise.resolve(42)
}

test().then(value => console.log(value)); // 42
```

---

### ⏳ await Keyword Rules

```javascript
// ✅ VALID: Inside async function
async function fetchData() {
  const result = await somePromise;
  return result;
}

// ❌ INVALID: Outside async function
function regularFunction() {
  const result = await somePromise; // SyntaxError!
}

// ✅ VALID: Top-level await (ES Modules only)
// In a .mjs file or <script type="module">
const data = await fetch('/api/data');
```

**await Behavior:**
1. Pauses the async function execution
2. Waits for the Promise to settle
3. Returns the resolved value (or throws if rejected)
4. **Does NOT block** the main thread—other code continues

---

### 🔄 Execution Flow Diagram

```
async function example() {
    console.log("A");           // ← Executes immediately
    const x = await promise;     // ← Function pauses here
    console.log("B", x);        // ← Resumes when promise settles
}

console.log("C");
example();
console.log("D");

Output Order:
C → A → D → B (when promise resolves)
```

**Why?** The function pauses at `await`, but the main thread continues. When the Promise resolves, the function resumes.

---

### 🛡️ Error Handling Patterns

```javascript
// Pattern 1: Basic try/catch
async function basic() {
  try {
    const data = await riskyOperation();
    return data;
  } catch (error) {
    console.error("Failed:", error.message);
    throw error; // Re-throw if caller needs it
  }
}

// Pattern 2: With finally
async function withCleanup() {
  try {
    const result = await operation();
    return result;
  } catch (error) {
    handleError(error);
  } finally {
    cleanup(); // Always runs
  }
}

// Pattern 3: Conditional error handling
async function conditional() {
  try {
    const data = await fetch(url);
    return await data.json();
  } catch (error) {
    if (error.name === 'NetworkError') {
      return retryWithBackoff();
    }
    throw error; // Can't handle, re-throw
  }
}
```

---

### ⚡ Sequential vs Parallel Execution

```javascript
// ❌ SEQUENTIAL (Slow): Each waits for previous
async function sequential() {
  const user = await fetchUser();      // 1s
  const posts = await fetchPosts();    // 1s  
  const comments = await fetchComments(); // 1s
  // Total: ~3 seconds
}

// ✅ PARALLEL (Fast): All start together
async function parallel() {
  const [user, posts, comments] = await Promise.all([
    fetchUser(),      // All start
    fetchPosts(),     // at the same
    fetchComments()   // time
  ]);
  // Total: ~1 second (slowest operation)
}
```

**When to use each:**
- **Sequential:** When operation B needs the result from operation A
- **Parallel:** When operations are independent

---

### 📦 Promise Utility Methods with async/await

| Method | Behavior | Use Case |
|--------|----------|----------|
| `Promise.all([...])` | Fails if ANY Promise rejects | All operations must succeed |
| `Promise.allSettled([...])` | Waits for all, never rejects | Want results from all, even failures |
| `Promise.race([...])` | Resolves/rejects with first settled | Timeout scenarios, fastest response |
| `Promise.any([...])` | Resolves with first fulfilled | First successful operation wins |

```javascript
// Example: allSettled for resilient operations
async function fetchMultiple() {
  const results = await Promise.allSettled([
    fetch('/api/data1').then(r => r.json()),
    fetch('/api/data2').then(r => r.json()),
    fetch('/api/data3').then(r => r.json())
  ]);
  
  results.forEach((result, i) => {
    if (result.status === 'fulfilled') {
      console.log(`API ${i+1} success:`, result.value);
    } else {
      console.log(`API ${i+1} failed:`, result.reason);
    }
  });
}
```

---

### 🎨 Code Conversion Quick Reference

```javascript
// BEFORE: Promise chain
function oldWay() {
  return fetch(url)
    .then(response => response.json())
    .then(data => processData(data))
    .then(result => saveResult(result))
    .catch(error => handleError(error));
}

// AFTER: async/await
async function newWay() {
  try {
    const response = await fetch(url);
    const data = await response.json();
    const result = await processData(data);
    await saveResult(result);
  } catch (error) {
    handleError(error);
  }
}
```

---

## 3. PREDICT THE OUTPUT

### Challenge 1
```javascript
async function test() {
  return "Hello";
}

console.log(test());
```

**🤔 Thinking Process:**
- `async` functions ALWAYS return a Promise
- Returning "Hello" wraps it in `Promise.resolve("Hello")`

**✅ Correct Output:**
```
Promise { 'Hello' }
```

**💡 Why:** The function itself returns a Promise object, not the string directly. To get "Hello", you'd need `test().then(console.log)` or `await test()`.

---

### Challenge 2
```javascript
async function order() {
  console.log("A");
  const result = await Promise.resolve("B");
  console.log(result);
  console.log("C");
}

console.log("D");
order();
console.log("E");
```

**🤔 Thinking Process:**
1. "D" logs first (synchronous)
2. `order()` is called, "A" logs immediately
3. Function pauses at `await`, main thread continues
4. "E" logs (main thread)
5. Promise resolves, function resumes
6. "B" then "C" log

**✅ Correct Output:**
```
D
A
E
B
C
```

**💡 Why:** The function pauses at await but doesn't block the main thread. The Event Loop handles resumption.

---

### Challenge 3
```javascript
async function fetchData() {
  throw new Error("Oops!");
}

async function main() {
  const result = await fetchData();
  console.log(result);
}

main();
console.log("Done");
```

**🤔 Thinking Process:**
- `fetchData()` throws an error
- No try/catch in `main()` to catch it
- Unhandled Promise rejection occurs
- "Done" logs first (synchronous)

**✅ Correct Output:**
```
Done
UnhandledPromiseRejectionWarning: Error: Oops!
```

**💡 Why:** Without try/catch, the error becomes an unhandled rejection. "Done" logs first because it's synchronous code outside the async function.

---

### Challenge 4
```javascript
async function parallel() {
  const p1 = Promise.resolve("One");
  const p2 = Promise.resolve("Two");
  
  console.log(await p1);
  console.log(await p2);
}

parallel();
```

**🤔 Thinking Process:**
- Both Promises are already resolved (synchronous)
- First await gets "One" immediately
- Second await gets "Two" immediately
- No actual delay

**✅ Correct Output:**
```
One
Two
```

**💡 Why:** `Promise.resolve()` creates already-fulfilled Promises. await still pauses the function, but resumes immediately.

---

### Challenge 5
```javascript
async function test() {
  return await Promise.resolve(42);
}

console.log(test());
```

**🤔 Thinking Process:**
- `await Promise.resolve(42)` returns 42
- Function returns 42
- But async functions wrap return values in Promises

**✅ Correct Output:**
```
Promise { <pending> }
```
(Then quickly resolves to 42)

**💡 Why:** Even though you await inside, the function itself returns a Promise. The `await` is actually redundant here—`return Promise.resolve(42)` would be identical.

---

### Challenge 6
```javascript
async function sequential() {
  console.time("Time");
  
  await new Promise(resolve => setTimeout(resolve, 1000));
  await new Promise(resolve => setTimeout(resolve, 1000));
  
  console.timeEnd("Time");
}

sequential();
```

**🤔 Thinking Process:**
- First await waits 1000ms
- Second await waits another 1000ms
- Sequential execution

**✅ Correct Output:**
```
Time: ~2000ms
```

**💡 Why:** Each await waits for the previous Promise to resolve before starting the next timer.

---

### Challenge 7
```javascript
async function race() {
  const fast = new Promise(resolve => setTimeout(() => resolve("Fast"), 100));
  const slow = new Promise(resolve => setTimeout(() => resolve("Slow"), 500));
  
  const result = await Promise.race([fast, slow]);
  console.log(result);
}

race();
```

**🤔 Thinking Process:**
- `Promise.race()` resolves with the first settled Promise
- "fast" resolves after 100ms
- "slow" is ignored

**✅ Correct Output:**
```
Fast
```

**💡 Why:** race() returns as soon as the first Promise settles. The slow Promise still runs but its result is discarded.

---

### Challenge 8
```javascript
async function errorTest() {
  try {
    await Promise.reject("Error 1");
    console.log("After first");
  } catch (e) {
    console.log("Caught:", e);
  }
  
  try {
    await Promise.reject("Error 2");
  } catch (e) {
    console.log("Caught:", e);
  }
}

errorTest();
```

**🤔 Thinking Process:**
- First try/catch catches "Error 1", logs it
- Execution continues
- Second try/catch catches "Error 2", logs it

**✅ Correct Output:**
```
Caught: Error 1
Caught: Error 2
```

**💡 Why:** Each try/catch block is independent. After catching an error, the function continues normally.

---

### Challenge 9
```javascript
async function loop() {
  const numbers = [1, 2, 3];
  
  for (const num of numbers) {
    await new Promise(resolve => setTimeout(resolve, 1000));
    console.log(num);
  }
}

console.log("Start");
loop();
console.log("End");
```

**🤔 Thinking Process:**
- "Start" logs (synchronous)
- loop() is called, enters for loop
- First iteration: waits 1s, logs 1
- "End" logs while loop is waiting
- Process repeats for 2 and 3

**✅ Correct Output:**
```
Start
End
(1s delay) 1
(1s delay) 2
(1s delay) 3
```

**💡 Why:** The await inside the loop makes each iteration wait, but "End" logs immediately since it's on the main thread.

---

### Challenge 10
```javascript
async function test() {
  const result = await Promise.all([
    Promise.resolve(1),
    Promise.reject(2),
    Promise.resolve(3)
  ]);
  
  console.log(result);
}

test().catch(err => console.log("Error:", err));
```

**🤔 Thinking Process:**
- Promise.all() fails if ANY Promise rejects
- Second Promise rejects with 2
- Entire operation fails immediately

**✅ Correct Output:**
```
Error: 2
```

**💡 Why:** Promise.all() has "fail-fast" behavior. As soon as one Promise rejects, the entire operation rejects with that error.

---

### Challenge 11
```javascript
const delay = (ms) => new Promise(resolve => setTimeout(resolve, ms));

async function test() {
  console.log("A");
  await delay(0);
  console.log("B");
}

console.log("C");
test();
console.log("D");
```

**🤔 Thinking Process:**
- "C" logs (synchronous)
- test() called, "A" logs immediately
- await delay(0) queues a microtask
- "D" logs (main thread continues)
- Microtask queue processes, "B" logs

**✅ Correct Output:**
```
C
A
D
B
```

**💡 Why:** Even with 0ms delay, setTimeout queues a macrotask. The main thread finishes first, then the Promise resolves.

---

### Challenge 12
```javascript
async function getData() {
  return {
    name: "John",
    age: await Promise.resolve(25)
  };
}

getData().then(console.log);
```

**🤔 Thinking Process:**
- Object is returned
- await inside object literal waits for Promise to resolve
- Returns object with age: 25

**✅ Correct Output:**
```
{ name: 'John', age: 25 }
```

**💡 Why:** You can use await inside object literals. The function waits for all awaited values before returning.

---

### Challenge 13
```javascript
async function test() {
  const arr = [1, 2, 3];
  arr.forEach(async (num) => {
    await new Promise(resolve => setTimeout(resolve, 1000));
    console.log(num);
  });
  console.log("Done");
}

test();
```

**🤔 Thinking Process:**
- forEach doesn't wait for async callbacks
- All async functions start immediately
- "Done" logs first
- After 1s, all numbers log together

**✅ Correct Output:**
```
Done
(1s delay) 1
2
3
```

**💡 Why:** forEach doesn't await—it fires all callbacks immediately. Use `for...of` loop to await each iteration properly.

---

### Challenge 14
```javascript
async function test() {
  try {
    return await Promise.reject("Error");
  } catch (e) {
    return "Recovered";
  }
}

test().then(console.log);
```

**🤔 Thinking Process:**
- Promise rejects with "Error"
- catch block catches it
- Returns "Recovered"
- This becomes the fulfilled value

**✅ Correct Output:**
```
Recovered
```

**💡 Why:** Catching an error and returning a value converts the rejected Promise into a fulfilled one.

---

### Challenge 15
```javascript
async function test() {
  const promises = [1, 2, 3].map(n => 
    Promise.resolve(n * 2)
  );
  
  const results = [];
  for (const promise of promises) {
    results.push(await promise);
  }
  
  return results;
}

test().then(console.log);
```

**🤔 Thinking Process:**
- map creates 3 resolved Promises immediately
- for loop awaits each one sequentially
- Each returns 2, 4, 6
- Results array is [2, 4, 6]

**✅ Correct Output:**
```
[2, 4, 6]
```

**💡 Why:** Promises are created first (all at once), then awaited sequentially. Since they're already resolved, it's fast.

---

## 4. GUIDED PRACTICE PROBLEMS

### 🔥 Problem 1: The Warm-up - Delayed Message Logger

**📝 Task Description:**
Create an async function that logs three messages with delays between them. Message 1 appears immediately, Message 2 after 1 second, and Message 3 after 2 more seconds.

**🎯 Learning Goal:** Direct application of async/await with Promises.

**📦 Starter Code:**
```html
<!DOCTYPE html>
<html>
<head>
  <title>Delayed Logger</title>
</head>
<body>
  <h1>Check the Console</h1>
  <script>
    // Your code here
  </script>
</body>
</html>
```

**📋 Step-by-Step Requirements:**
1. Create a helper function `wait(ms)` that returns a Promise resolving after `ms` milliseconds
2. Create an async function `logMessages()` that:
   - Logs "Message 1" immediately
   - Waits 1 second
   - Logs "Message 2"
   - Waits 2 more seconds
   - Logs "Message 3"
3. Call the function and verify the timing in the console

---

**✅ The Solution:**
```javascript
// Helper function to create delay
function wait(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

// Async function with sequential delays
async function logMessages() {
  console.log("Message 1"); // Immediate
  
  await wait(1000); // Wait 1 second
  console.log("Message 2");
  
  await wait(2000); // Wait 2 more seconds
  console.log("Message 3");
  
  console.log("All messages logged!");
}

// Execute
logMessages();
```

**🎓 What You Learned:**
- Creating Promise-based delay functions
- Sequential async operations with await
- How await pauses execution but doesn't block the thread

---

**❌ Common Mistakes:**

1. **Forgetting to return the Promise:**
```javascript
// ❌ WRONG
function wait(ms) {
  setTimeout(() => {}, ms); // Nothing returned!
}

// ✅ CORRECT
function wait(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}
```

2. **Not using await:**
```javascript
// ❌ WRONG - All log immediately
async function logMessages() {
  console.log("Message 1");
  wait(1000); // Not awaited!
  console.log("Message 2");
  wait(2000);
  console.log("Message 3");
}

// ✅ CORRECT - Each waits
async function logMessages() {
  console.log("Message 1");
  await wait(1000);
  console.log("Message 2");
  await wait(2000);
  console.log("Message 3");
}
```

3. **Using .then() unnecessarily:**
```javascript
// ❌ MIXING STYLES
async function logMessages() {
  console.log("Message 1");
  wait(1000).then(() => {
    console.log("Message 2");
  });
}

// ✅ CLEAN async/await
async function logMessages() {
  console.log("Message 1");
  await wait(1000);
  console.log("Message 2");
}
```

---

### 🧠 Problem 2: The Logic Builder - User Data Fetcher

**📝 Task Description:**
Fetch a user's data from JSONPlaceholder API, then fetch their posts. Display the user's name and the count of their posts. Handle errors gracefully and show a loading indicator.

**🎯 Learning Goal:** Combining async/await with DOM manipulation, error handling, and sequential API calls where the second depends on the first.

**📦 Starter Code:**
```html
<!DOCTYPE html>
<html>
<head>
  <title>User Data Fetcher</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 600px;
      margin: 50px auto;
      padding: 20px;
    }
    #output {
      margin-top: 20px;
      padding: 15px;
      border: 1px solid #ddd;
      border-radius: 5px;
    }
    .loading { color: #666; }
    .error { color: #d32f2f; }
    .success { color: #388e3c; }
  </style>
</head>
<body>
  <h1>User Data Fetcher</h1>
  <input type="number" id="userId" placeholder="Enter User ID (1-10)" min="1" max="10" value="1">
  <button id="fetchBtn">Fetch User Data</button>
  <div id="output"></div>

  <script>
    // Your code here
  </script>
</body>
</html>
```

**📋 Step-by-Step Requirements:**
1. When the button is clicked, show "Loading..." in the output div
2. Fetch user data from `https://jsonplaceholder.typicode.com/users/{userId}`
3. If successful, fetch that user's posts from `https://jsonplaceholder.typicode.com/posts?userId={userId}`
4. Display: "User: [name] has [count] posts"
5. If any error occurs, display a user-friendly error message
6. Use try/catch for error handling
7. Clear the loading state after completion

---

**✅ The Solution:**
```javascript
const fetchBtn = document.getElementById('fetchBtn');
const userIdInput = document.getElementById('userId');
const output = document.getElementById('output');

async function fetchUserData() {
  const userId = userIdInput.value;
  
  // Validation
  if (!userId || userId < 1 || userId > 10) {
    output.className = 'error';
    output.textContent = 'Please enter a valid User ID (1-10)';
    return;
  }
  
  try {
    // Show loading state
    output.className = 'loading';
    output.textContent = 'Loading user data...';
    
    // Fetch user data
    const userResponse = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`);
    
    if (!userResponse.ok) {
      throw new Error(`HTTP Error: ${userResponse.status}`);
    }
    
    const user = await userResponse.json();
    
    // Update loading message
    output.textContent = 'Loading posts...';
    
    // Fetch user's posts (depends on user data)
    const postsResponse = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`);
    
    if (!postsResponse.ok) {
      throw new Error(`HTTP Error: ${postsResponse.status}`);
    }
    
    const posts = await postsResponse.json();
    
    // Display success
    output.className = 'success';
    output.innerHTML = `
      <strong>User:</strong> ${user.name}<br>
      <strong>Email:</strong> ${user.email}<br>
      <strong>Posts:</strong> ${posts.length}
    `;
    
  } catch (error) {
    // Handle errors
    output.className = 'error';
    
    if (error.message.includes('HTTP')) {
      output.textContent = `Server Error: ${error.message}`;
    } else if (error.name === 'TypeError') {
      output.textContent = 'Network Error: Check your internet connection';
    } else {
      output.textContent = `Error: ${error.message}`;
    }
  }
}

// Event listener
fetchBtn.addEventListener('click', fetchUserData);

// Allow Enter key
userIdInput.addEventListener('keypress', (e) => {
  if (e.key === 'Enter') fetchUserData();
});
```

**🎓 What You Learned:**
- Sequential API calls where the second depends on the first
- Proper error handling with specific error types
- DOM manipulation within async functions
- User input validation before async operations
- Loading state management

---

**❌ Common Mistakes:**

1. **Not checking response.ok:**
```javascript
// ❌ WRONG - Doesn't catch HTTP errors
const response = await fetch(url);
const data = await response.json(); // Might fail silently

// ✅ CORRECT
const response = await fetch(url);
if (!response.ok) {
  throw new Error(`HTTP ${response.status}`);
}
const data = await response.json();
```

2. **Forgetting to await .json():**
```javascript
// ❌ WRONG
const response = await fetch(url);
const data = response.json(); // Returns a Promise!
console.log(data.name); // undefined

// ✅ CORRECT
const response = await fetch(url);
const data = await response.json();
console.log(data.name); // Works!
```

3. **Not clearing loading state on error:**
```javascript
// ❌ WRONG - Loading stays forever if error
async function fetch Data() {
  output.textContent = 'Loading...';
  const data = await fetch(url); // If this fails, loading never clears
}

// ✅ CORRECT - Use try/finally
async function fetchData() {
  try {
    output.textContent = 'Loading...';
    const data = await fetch(url);
  } catch (error) {
    output.textContent = 'Error!';
  } finally {
    // Clear loading state regardless
  }
}
```

4. **Running dependent calls in parallel:**
```javascript
// ❌ WRONG - Can't fetch posts without userId
const [user, posts] = await Promise.all([
  fetch(`/users/${userId}`).then(r => r.json()),
  fetch(`/posts?userId=${userId}`).then(r => r.json()) // userId not available yet!
]);

// ✅ CORRECT - Sequential when dependent
const user = await fetch(`/users/${userId}`).then(r => r.json());
const posts = await fetch(`/posts?userId=${user.id}`).then(r => r.json());
```

---

### 🏆 Problem 3: The Pro Challenge - Movie Explorer with Error Recovery

**📝 Task Description:**
Build a production-ready Movie Search App using the OMDB API. The app must handle network failures gracefully with automatic retries, show loading states, cache results to avoid redundant API calls, and display meaningful error messages.

**🎯 Learning Goal:** Real-world async/await patterns including retry logic, caching, debouncing, and complex error handling.

**API Details:**
- **Endpoint:** `http://www.omdbapi.com/?apikey=YOUR_KEY&s={searchTerm}`
- **Get your free key:** http://www.omdbapi.com/apikey.aspx
- **Rate Limit:** 1000 calls/day on free tier

**📦 Starter Code:**
```html
<!DOCTYPE html>
<html>
<head>
  <title>Movie Explorer Pro</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      min-height: 100vh;
      padding: 20px;
    }
    
    .container {
      max-width: 900px;
      margin: 0 auto;
      background: white;
      border-radius: 10px;
      padding: 30px;
      box-shadow: 0 10px 40px rgba(0,0,0,0.2);
    }
    
    h1 {
      text-align: center;
      color: #667eea;
      margin-bottom: 30px;
    }
    
    .search-box {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
    }
    
    #searchInput {
      flex: 1;
      padding: 12px 20px;
      border: 2px solid #ddd;
      border-radius: 5px;
      font-size: 16px;
    }
    
    #searchInput:focus {
      outline: none;
      border-color: #667eea;
    }
    
    button {
      padding: 12px 30px;
      background: #667eea;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
      transition: background 0.3s;
    }
    
    button:hover {
      background: #5568d3;
    }
    
    button:disabled {
      background: #ccc;
      cursor: not-allowed;
    }
    
    .status {
      text-align: center;
      padding: 15px;
      margin: 20px 0;
      border-radius: 5px;
    }
    
    .loading {
      background: #fff3cd;
      color: #856404;
    }
    
    .error {
      background: #f8d7da;
      color: #721c24;
    }
    
    .success {
      background: #d4edda;
      color: #155724;}
    
    .movies-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
      gap: 20px;
      margin-top: 20px;
    }
    
    .movie-card {
      background: #f8f9fa;
      border-radius: 8px;
      overflow: hidden;
      transition: transform 0.3s;
      cursor: pointer;
    }
    
    .movie-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 5px 20px rgba(0,0,0,0.2);
    }
    
    .movie-card img {
      width: 100%;
      height: 300px;
      object-fit: cover;
    }
    
    .movie-info {
      padding: 15px;
    }
    
    .movie-title {
      font-weight: bold;
      margin-bottom: 5px;
      color: #333;
    }
    
    .movie-year {
      color: #666;
      font-size: 14px;
    }
    
    .cache-info {
      text-align: center;
      font-size: 12px;
      color: #666;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>🎬 Movie Explorer Pro</h1>
    
    <div class="search-box">
      <input 
        type="text" 
        id="searchInput" 
        placeholder="Search for movies... (e.g., Matrix)"
      >
      <button id="searchBtn">Search</button>
    </div>
    
    <div id="status"></div>
    <div id="moviesContainer" class="movies-grid"></div>
    <div id="cacheInfo" class="cache-info"></div>
  </div>

  <script>
    // Your code here
  </script>
</body>
</html>
```

**📋 Step-by-Step Requirements:**

1. **API Configuration:**
   - Store API key in a constant
   - Create base URL constant

2. **Retry Logic:**
   - Implement exponential backoff (1s, 2s, 4s)
   - Max 3 retry attempts
   - Log each retry attempt

3. **Caching System:**
   - Store search results in memory
   - Check cache before making API calls
   - Show cache hit/miss info to user

4. **Debouncing:**
   - Implement search debouncing (500ms delay)
   - Avoid API calls on every keystroke

5. **Error Handling:**
   - Network errors
   - HTTP errors (404, 500)
   - No results found
   - Invalid API key

6. **UI States:**
   - Loading spinner
   - Error messages
   - Success messages
   - Empty state

7. **Movie Display:**
   - Show poster (or placeholder if N/A)
   - Show title and year
   - Grid layout

---

**✅ The Solution:**

```javascript
// Configuration
const API_KEY = 'YOUR_API_KEY_HERE'; // Get from omdbapi.com
const BASE_URL = 'http://www.omdbapi.com/';
const DEBOUNCE_DELAY = 500;
const MAX_RETRIES = 3;

// DOM Elements
const searchInput = document.getElementById('searchInput');
const searchBtn = document.getElementById('searchBtn');
const status = document.getElementById('status');
const moviesContainer = document.getElementById('moviesContainer');
const cacheInfo = document.getElementById('cacheInfo');

// State
const cache = new Map();
let debounceTimer;

// Utility: Delay function
const delay = (ms) => new Promise(resolve => setTimeout(resolve, ms));

// Utility: Retry with exponential backoff
async function retryWithBackoff(asyncFn, maxRetries = MAX_RETRIES) {
  let lastError;
  
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      return await asyncFn();
    } catch (error) {
      lastError = error;
      
      if (attempt < maxRetries) {
        const delayMs = Math.pow(2, attempt) * 1000;
        showStatus(`Retry ${attempt}/${maxRetries} in ${delayMs/1000}s...`, 'loading');
        await delay(delayMs);
      }
    }
  }
  
  throw new Error(`Failed after ${maxRetries} attempts: ${lastError.message}`);
}

// Main: Fetch movies with caching and retry
async function fetchMovies(searchTerm) {
  // Check cache first
  if (cache.has(searchTerm)) {
    cacheInfo.textContent = '✅ Loaded from cache';
    return cache.get(searchTerm);
  }
  
  const url = `${BASE_URL}?apikey=${API_KEY}&s=${encodeURIComponent(searchTerm)}`;
  
  const fetchFn = async () => {
    const response = await fetch(url);
    
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }
    
    const data = await response.json();
    
    if (data.Response === 'False') {
      throw new Error(data.Error || 'No movies found');
    }
    
    return data.Search;
  };
  
  const movies = await retryWithBackoff(fetchFn);
  
  // Cache the result
  cache.set(searchTerm, movies);
  cacheInfo.textContent = '🌐 Fetched from API';
  
  return movies;
}

// UI: Show status message
function showStatus(message, type = 'loading') {
  status.className = `status ${type}`;
  status.textContent = message;
  status.style.display = 'block';
}

// UI: Hide status
function hideStatus() {
  status.style.display = 'none';
}

// UI: Display movies
function displayMovies(movies) {
  moviesContainer.innerHTML = '';
  
  movies.forEach(movie => {
    const card = document.createElement('div');
    card.className = 'movie-card';
    
    const poster = movie.Poster !== 'N/A' 
      ? movie.Poster 
      : 'https://via.placeholder.com/200x300?text=No+Poster';
    
    card.innerHTML = `
      <img src="${poster}" alt="${movie.Title}">
      <div class="movie-info">
        <div class="movie-title">${movie.Title}</div>
        <div class="movie-year">${movie.Year}</div>
      </div>
    `;
    
    moviesContainer.appendChild(card);
  });
}

// Main: Search handler
async function handleSearch() {
  const searchTerm = searchInput.value.trim();
  
  if (!searchTerm) {
    showStatus('Please enter a search term', 'error');
    return;
  }
  
  if (!API_KEY || API_KEY === 'YOUR_API_KEY_HERE') {
    showStatus('Please add your OMDB API key in the code', 'error');
    return;
  }
  
  try {
    // Disable button during search
    searchBtn.disabled = true;
    moviesContainer.innerHTML = '';
    cacheInfo.textContent = '';
    
    showStatus(`Searching for "${searchTerm}"...`, 'loading');
    
    const movies = await fetchMovies(searchTerm);
    
    hideStatus();
    displayMovies(movies);
    showStatus(`Found ${movies.length} movies`, 'success');
    
  } catch (error) {
    console.error('Search failed:', error);
    
    if (error.message.includes('No movies found')) {
      showStatus(`No results for "${searchTerm}". Try another search.`, 'error');
    } else if (error.message.includes('HTTP')) {
      showStatus('Server error. Please try again later.', 'error');
    } else if (error.name === 'TypeError') {
      showStatus('Network error. Check your connection.', 'error');
    } else {
      showStatus(`Error: ${error.message}`, 'error');
    }
    
    moviesContainer.innerHTML = '';
    
  } finally {
    searchBtn.disabled = false;
  }
}

// Debounced search on input
searchInput.addEventListener('input', () => {
  clearTimeout(debounceTimer);
  debounceTimer = setTimeout(() => {
    if (searchInput.value.trim()) {
      handleSearch();
    }
  }, DEBOUNCE_DELAY);
});

// Immediate search on button click
searchBtn.addEventListener('click', () => {
  clearTimeout(debounceTimer);
  handleSearch();
});

// Search on Enter key
searchInput.addEventListener('keypress', (e) => {
  if (e.key === 'Enter') {
    clearTimeout(debounceTimer);
    handleSearch();
  }
});

// Initial message
showStatus('Enter a movie name to search', 'success');
```

**🎓 What You Learned:**
- Production-ready error handling with retry logic
- Implementing exponential backoff for failed requests
- Caching strategies to reduce API calls
- Debouncing user input for better UX
- Complex async workflows with multiple error scenarios
- Managing UI state during async operations
- Code organization for maintainable async applications

---

**❌ Common Mistakes:**

1. **Not validating API key:**
```javascript
// ❌ WRONG - Fails silently with bad key
async function fetchMovies(term) {
  const url = `${BASE_URL}?apikey=${API_KEY}&s=${term}`;
  const response = await fetch(url);
  return response.json();
}

// ✅ CORRECT - Check and inform user
if (!API_KEY || API_KEY === 'YOUR_API_KEY_HERE') {
  showStatus('Please configure API key', 'error');
  return;
}
```

2. **Infinite retry loops:**
```javascript
// ❌ WRONG - Could retry forever
async function retryForever(fn) {
  while (true) {
    try {
      return await fn();
    } catch (error) {
      // Retries infinitely!
    }
  }
}

// ✅ CORRECT - Max retry limit
async function retryWithLimit(fn, maxRetries = 3) {
  for (let i = 1; i <= maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (i === maxRetries) throw error;
    }
  }
}
```

3. **Not cleaning up timers:**
```javascript
// ❌ WRONG - Creates memory leaks
searchInput.addEventListener('input', () => {
  setTimeout(() => search(), 500); // Never cleared!
});

// ✅ CORRECT - Clear previous timer
let timer;
searchInput.addEventListener('input', () => {
  clearTimeout(timer);
  timer = setTimeout(() => search(), 500);
});
```

4. **Caching without limits:**
```javascript
// ❌ WRONG - Cache grows infinitely
const cache = new Map();
cache.set(key, value); // No size limit!

// ✅ CORRECT - LRU cache or size limit
const MAX_CACHE_SIZE = 50;
if (cache.size >= MAX_CACHE_SIZE) {
  const firstKey = cache.keys().next().value;
  cache.delete(firstKey);
}
cache.set(key, value);
```

5. **Not disabling submit button:**
```javascript
// ❌ WRONG - User can trigger multiple searches
async function search() {
  const results = await fetchData(); // Takes 2s
  // User clicked button 5 times = 5 API calls!
}

// ✅ CORRECT - Disable during operation
async function search() {
  searchBtn.disabled = true;
  try {
    const results = await fetchData();
  } finally {
    searchBtn.disabled = false;
  }
}
```

---

## 5. DEBUG CHALLENGES

Fix the bugs in each code snippet. Each challenge focuses on common async/await mistakes.

---

### 🐛 Challenge 1: The Missing Return

```javascript
async function getUserData(userId) {
  try {
    const response = await fetch(`/api/users/${userId}`);
    response.json();
  } catch (error) {
    console.error(error);
  }
}

getUserData(1).then(data => console.log(data)); // Logs: undefined
```

**🔍 Your Task:** Why is `data` undefined? Fix the code.

<details>
<summary>💡 Hint</summary>

The problem is on line 4. What does `.json()` return?
</details>

---

**✅ Solution:**

```javascript
async function getUserData(userId) {
  try {
    const response = await fetch(`/api/users/${userId}`);
    return await response.json(); // ✅ Added return and await
  } catch (error) {
    console.error(error);
  }
}

getUserData(1).then(data => console.log(data));
```

**📝 Explanation:**
- `.json()` returns a **Promise**, not the parsed data
- Without `await`, you get a Promise object
- Without `return`, the function returns `undefined`
- **Fix:** Always `await` async methods like `.json()` and `return` the result

---

### 🐛 Challenge 2: The forEach Trap

```javascript
async function processItems(items) {
  console.log("Start processing");
  
  items.forEach(async (item) => {
    await delay(1000);
    console.log(`Processed: ${item}`);
  });
  
  console.log("Done processing");
}

function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

processItems(['A', 'B', 'C']);

// Output:
// Start processing
// Done processing
// (1s later) Processed: A
// Processed: B
// Processed: C
```

**🔍 Your Task:** Why does "Done processing" appear before the items are processed? Fix it.

<details>
<summary>💡 Hint</summary>

`forEach` doesn't understand `async/await`. Use a different loop.
</details>

---

**✅ Solution:**

```javascript
async function processItems(items) {
  console.log("Start processing");
  
  // ✅ Option 1: for...of loop
  for (const item of items) {
    await delay(1000);
    console.log(`Processed: ${item}`);
  }
  
  /* ✅ Option 2: Promise.all for parallel
  await Promise.all(
    items.map(async (item) => {
      await delay(1000);
      console.log(`Processed: ${item}`);
    })
  );
  */
  
  console.log("Done processing");
}

function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

processItems(['A', 'B', 'C']);

// Correct Output:
// Start processing
// (1s) Processed: A
// (1s) Processed: B
// (1s) Processed: C
// Done processing
```

**📝 Explanation:**
- `forEach`, `map`, `filter` don't wait for async callbacks
- They fire all callbacks immediately and continue
- **Use `for...of`** for sequential async operations
- **Use `Promise.all(array.map(async...))`** for parallel operations

---

### 🐛 Challenge 3: The Unhandled Rejection

```javascript
async function fetchData() {
  const response = await fetch('https://invalid-url-xyz.com/api');
  const data = await response.json();
  return data;
}

async function main() {
  const result = fetchData(); // No await!
  console.log("Fetching data...");
}

main();
```

**🔍 Your Task:** This code has TWO bugs. Find and fix both.

<details>
<summary>💡 Hint</summary>

1. Is `fetchData()` being awaited?
2. What happens if the fetch fails?
</details>

---

**✅ Solution:**

```javascript
async function fetchData() {
  try {
    const response = await fetch('https://invalid-url-xyz.com/api');
    
    // ✅ Bug 2 Fix: Check response.ok
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }
    
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Fetch failed:', error.message);
    throw error; // Re-throw so caller can handle
  }
}

async function main() {
  try {
    const result = await fetchData(); // ✅ Bug 1 Fix: Added await
    console.log("Data:", result);
  } catch (error) {
    console.log("Could not fetch data");
  }
}

main();
```

**📝 Explanation:**
- **Bug 1:** Missing `await` means `result` is a Promise, not the data
- **Bug 2:** Not checking `response.ok` means HTTP errors (404, 500) aren't caught
- **Always:**
  - `await` async function calls
  - Check `response.ok` before calling `.json()`
  - Wrap in try/catch

---

### 🐛 Challenge 4: The Race Condition

```javascript
let userData = null;

async function loadUser() {
  await delay(2000);
  userData = { name: "John", age: 30 };
}

function displayUser() {
  console.log(userData.name); // Error: Cannot read property 'name' of null
}

function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

loadUser();
displayUser(); // Called immediately!
```

**🔍 Your Task:** Why does this crash? Fix it so `displayUser` waits for the data.

<details>
<summary>💡 Hint</summary>

`displayUser` runs before `loadUser` finishes. How can you ensure order?
</details>

---

**✅ Solution:**

```javascript
let userData = null;

async function loadUser() {
  await delay(2000);
  userData = { name: "John", age: 30 };
}

function displayUser() {
  if (!userData) {
    console.log("No user data available");
    return;
  }
  console.log(userData.name);
}

function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

// ✅ Option 1: Await the load
async function main() {
  await loadUser();
  displayUser();
}
main();

// ✅ Option 2: Make displayUser async and return it
async function loadAndDisplay() {
  await loadUser();
  displayUser();
}
loadAndDisplay();

// ✅ Option 3: Use .then()
loadUser().then(() => displayUser());
```

**📝 Explanation:**
- Async functions don't block—code continues immediately
- If function B depends on function A's result, **you must await A**
- Never assume global state is ready without waiting
- **Best Practice:** Chain dependent operations explicitly

---

### 🐛 Challenge 5: The Promise.all Failure

```javascript
async function fetchMultipleUsers() {
  const userIds = [1, 2, 999, 4]; // 999 doesn't exist
  
  const users = await Promise.all(
    userIds.map(id => 
      fetch(`https://jsonplaceholder.typicode.com/users/${id}`)
        .then(res => res.json())
    )
  );
  
  console.log(users);
}

fetchMultipleUsers().catch(err => console.log("Failed:", err));
```

**🔍 Your Task:** User 999 returns a 404 error, but the code doesn't handle it. Some users load successfully. How can you get ALL results (both successes and failures)?

<details>
<summary>💡 Hint</summary>

`Promise.all()` fails fast. Is there a method that waits for all Promises regardless of success/failure?
</details>

---

**✅ Solution:**

```javascript
async function fetchMultipleUsers() {
  const userIds = [1, 2, 999, 4];
  
  // ✅ Use Promise.allSettled instead
  const results = await Promise.allSettled(
    userIds.map(async (id) => {
      const res = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`);
      
      if (!res.ok) {
        throw new Error(`User ${id} not found (${res.status})`);
      }
      
      return res.json();
    })
  );
  
  // Process results
  results.forEach((result, index) => {
    if (result.status === 'fulfilled') {
      console.log(`User ${userIds[index]}:`, result.value.name);
    } else {
      console.log(`User ${userIds[index]}:`, result.reason.message);
    }
  });
}

fetchMultipleUsers();

// Output:
// User 1: Leanne Graham
// User 2: Ervin Howell
// User 999: User 999 not found (404)
// User 4: Patricia Lebsack
```

**📝 Explanation:**
- `Promise.all()` **fails fast**—if one Promise rejects, the entire operation fails
- `Promise.allSettled()` **waits for all**, never rejects
- Returns array with `{status: 'fulfilled', value}` or `{status: 'rejected', reason}`
- **Use `allSettled`** when you need results from all operations, even if some fail
- **Use `all`** when all operations must succeed or none should

---

## 6. COMMON PITFALLS & ANTI-PATTERNS

### ❌ Anti-Pattern 1: Unnecessary async/await

```javascript
// ❌ DON'T DO THIS
async function getUser(id) {
  return await fetch(`/api/users/${id}`);
}

// ✅ DO THIS
async function getUser(id) {
  return fetch(`/api/users/${id}`);
  // Still returns a Promise, no need for await
}

// ✅ OR JUST THIS (no async needed)
function getUser(id) {
  return fetch(`/api/users/${id}`);
}
```

**Why it matters:** The `await` before `return` is redundant. The function already returns a Promise.

**Exception:** Keep `await` if you need try/catch:
```javascript
async function getUser(id) {
  try {
    return await fetch(`/api/users/${id}`); // await needed for catch to work
  } catch (error) {
    console.error(error);
    return null;
  }
}
```

---

### ❌ Anti-Pattern 2: Sequential When Parallel is Possible

```javascript
// ❌ SLOW: 6 seconds total
async function loadDashboard() {
  const user = await fetchUser();        // 2s
  const posts = await fetchPosts();      // 2s
  const comments = await fetchComments(); // 2s
  return { user, posts, comments };
}

// ✅ FAST: 2 seconds total
async function loadDashboard() {
  const [user, posts, comments] = await Promise.all([
    fetchUser(),
    fetchPosts(),
    fetchComments()
  ]);
  return { user, posts, comments };
}
```

**Why it matters:** Performance! If operations don't depend on each other, run them in parallel.

---

### ❌ Anti-Pattern 3: Mixing async/await with .then()

```javascript
// ❌ CONFUSING: Mixing styles
async function getData() {
  const response = await fetch('/api/data');
  return response.json().then(data => {
    return data.value;
  });
}

// ✅ CONSISTENT: Pure async/await
async function getData() {
  const response = await fetch('/api/data');
  const data = await response.json();
  return data.value;
}
```

**Why it matters:** Code readability. Pick one style and stick with it.

---

### ❌ Anti-Pattern 4: Not Checking response.ok

```javascript
// ❌ DANGEROUS: 404 errors not caught
async function fetchUser(id) {
  const response = await fetch(`/api/users/${id}`);
  return await response.json(); // Fails silently on 404
}

// ✅ SAFE: Check response status
async function fetchUser(id) {
  const response = await fetch(`/api/users/${id}`);
  
  if (!response.ok) {
    throw new Error(`HTTP ${response.status}: ${response.statusText}`);
  }
  
  return await response.json();
}
```

**Why it matters:** `fetch()` only rejects on network errors, not HTTP errors (404, 500, etc.).

---

### ❌ Anti-Pattern 5: Forgetting Error Boundaries

```javascript
// ❌ UNHANDLED: Errors crash silently
async function processData() {
  const data = await fetchData();
  const result = await transform(data);
  await saveResult(result);
}

processData(); // If this fails, no one knows!

// ✅ HANDLED: Proper error boundary
async function processData() {
  try {
    const data = await fetchData();
    const result = await transform(data);
    await saveResult(result);
  } catch (error) {
    console.error('Process failed:', error);
    notifyUser('Operation failed');
    logToServer(error);
  }
}

processData().catch(err => {
  // Last resort error handler
  console.error('Critical error:', err);
});
```

**Why it matters:** Unhandled Promise rejections can crash Node.js apps and cause silent failures in browsers.

---

### ❌ Anti-Pattern 6: Async Functions in Array Methods

```javascript
// ❌ DOESN'T WORK: map doesn't wait
async function processAll(items) {
  const results = items.map(async (item) => {
    return await processItem(item);
  });
  console.log(results); // Array of Promises, not values!
}

// ✅ CORRECT: Await Promise.all
async function processAll(items) {
  const results = await Promise.all(
    items.map(async (item) => {
      return await processItem(item);
    })
  );
  console.log(results); // Array of actual values
}

// ✅ ALTERNATIVE: for...of for sequential
async function processAll(items) {
  const results = [];
  for (const item of items) {
    results.push(await processItem(item));
  }
  return results;
}
```

**Why it matters:** Array methods like `map`, `filter`, `forEach` don't wait for async operations.

---

### ❌ Anti-Pattern 7: Creating Promises in Loops

```javascript
// ❌ INEFFICIENT: Creates promises sequentially
async function fetchUsers(ids) {
  const users = [];
  for (const id of ids) {
    users.push(await fetchUser(id)); // One at a time
  }
  return users;
}

// ✅ EFFICIENT: Create all promises first, then await
async function fetchUsers(ids) {
  const promises = ids.map(id => fetchUser(id)); // All start
  return await Promise.all(promises); // Wait together
}
```

**Why it matters:** The first approach takes `n * time`, the second takes `max(time)`.

---

### ❌ Anti-Pattern 8: Ignoring Cleanup

```javascript
// ❌ RESOURCE LEAK: No cleanup
async function uploadFile(file) {
  const reader = new FileReader();
  const data = await readFile(reader, file);
  await uploadToServer(data);
  // What if uploadToServer fails? Reader stays open!
}

// ✅ PROPER CLEANUP: Use finally
async function uploadFile(file) {
  const reader = new FileReader();
  try {
    const data = await readFile(reader, file);
    await uploadToServer(data);
  } catch (error) {
    console.error('Upload failed:', error);
    throw error;
  } finally {
    reader.abort(); // Always cleanup
    releaseResources();
  }
}
```

**Why it matters:** Always clean up resources (file handles, connections, timers) in `finally` blocks.

---

## 7. KNOWLEDGE CHECK MCQs

### Question 1
What does an `async` function always return?

A) The value you specify in the return statement  
B) A Promise  
C) undefined  
D) A callback function

<details>
<summary>Show Answer</summary>

**Answer: B) A Promise**

**Explanation:** Async functions ALWAYS return a Promise, even if you return a regular value. The value gets automatically wrapped in `Promise.resolve()`.

```javascript
async function test() {
  return 42; // Returns Promise.resolve(42)
}
```
</details>

---

### Question 2
Where can you use the `await` keyword?

A) Anywhere in your code  
B) Only inside async functions  
C) Inside async functions or at the top level of ES modules  
D) Only inside Promise .then() callbacks

<details>
<summary>Show Answer</summary>

**Answer: C) Inside async functions or at the top level of ES modules**

**Explanation:** `await` can be used:
1. Inside functions declared with `async`
2. At the top level of ES modules (top-level await, ES2022+)

It CANNOT be used in regular functions or in most other contexts.
</details>

---

### Question 3
What happens when you forget to `await` an async function?

```javascript
async function getData() {
  return "data";
}

const result = getData();
console.log(result);
```

A) Logs "data"  
B) Logs a Promise object  
C) Throws an error  
D) Logs undefined

<details>
<summary>Show Answer</summary>

**Answer: B) Logs a Promise object**

**Explanation:** Without `await`, you get the Promise itself, not the resolved value. To get "data", you need:
```javascript
const result = await getData();
// OR
getData().then(result => console.log(result));
```
</details>

---

### Question 4
Which is faster?

```javascript
// Option A
async function optionA() {
  const a = await fetch('/api/a');
  const b = await fetch('/api/b');
  return [a, b];
}

// Option B
async function optionB() {
  const [a, b] = await Promise.all([
    fetch('/api/a'),
    fetch('/api/b')
  ]);
  return [a, b];
}
```

A) Option A  
B) Option B  
C) They're the same speed  
D) It depends on the API

<details>
<summary>Show Answer</summary>

**Answer: B) Option B**

**Explanation:** 
- **Option A (Sequential):** Waits for A to complete, then starts B. Total time = A_time + B_time
- **Option B (Parallel):** Starts both immediately. Total time = max(A_time, B_time)

If each takes 1 second, A takes 2 seconds total, B takes 1 second total.
</details>

---

### Question 5
What's wrong with this code?

```javascript
async function process() {
  items.forEach(async (item) => {
    await processItem(item);
  });
  console.log("Done");
}
```

A) Nothing, it works fine  
B) forEach doesn't wait for async callbacks  
C) You can't use async functions in forEach  
D) await is used incorrectly

<details>
<summary>Show Answer</summary>

**Answer: B) forEach doesn't wait for async callbacks**

**Explanation:** `forEach` fires all callbacks immediately and doesn't wait. "Done" logs before any items are processed.

**Fix:** Use `for...of` loop:
```javascript
for (const item of items) {
  await processItem(item);
}
console.log("Done"); // Now logs after all items
```
</details>

---

### Question 6
How do you handle errors in async/await?

A) .catch() method  
B) try/catch blocks  
C) error callback  
D) Promise.catch()

<details>
<summary>Show Answer</summary>

**Answer: B) try/catch blocks**

**Explanation:** async/await uses traditional try/catch for error handling:
```javascript
async function example() {
  try {
    const result = await riskyOperation();
  } catch (error) {
    console.error(error);
  }
}
```

You CAN also use `.catch()` on the returned Promise, but try/catch is the idiomatic approach.
</details>

---

### Question 7
What does this code output?

```javascript
async function test() {
  console.log("A");
  await Promise.resolve();
  console.log("B");
}

console.log("C");
test();
console.log("D");
```

A) A, B, C, D  
B) C, A, D, B  
C) C, D, A, B  
D) A, C, D, B

<details>
<summary>Show Answer</summary>

**Answer: B) C, A, D, B**

**Explanation:**
1. "C" logs (synchronous)
2. `test()` called, "A" logs immediately
3. Function pauses at `await`
4. "D" logs (main thread continues)
5. Promise resolves, "B" logs

The function pauses at await but doesn't block the thread.
</details>

---

### Question 8
What's the difference between Promise.all() and Promise.allSettled()?

A) They're the same  
B) all() fails if any Promise rejects; allSettled() waits for all  
C) allSettled() is faster  
D) all() is for errors, allSettled() is for success

<details>
<summary>Show Answer</summary>

**Answer: B) all() fails if any Promise rejects; allSettled() waits for all**

**Explanation:**
- **Promise.all():** Rejects immediately if ANY Promise rejects ("fail-fast")
- **Promise.allSettled():** Waits for ALL Promises (never rejects), returns status for each

Use `all()` when all must succeed. Use `allSettled()` when you want all results regardless.
</details>

---

### Question 9
Is this `await` necessary?

```javascript
async function getData() {
  return await fetch('/api/data');
}
```

A) Yes, always needed  
B) No, it's redundant  
C) Yes, but only sometimes  
D) It depends on the API

<details>
<summary>Show Answer</summary>

**Answer: C) Yes, but only sometimes**

**Explanation:**
- **Redundant** if you don't need try/catch (just return the Promise)
- **Necessary** if you need error handling:

```javascript
// Redundant
async function getData() {
  return await fetch('/api/data'); // Unnecessary await
}

// Necessary (for catch to work)
async function getData() {
  try {
    return await fetch('/api/data'); // Needed!
  } catch (error) {
    return null;
  }
}
```
</details>

---

### Question 10
What's wrong here?

```javascript
async function fetchData() {
  const response = await fetch('/api/data');
  return response.json();
}
```

A) Nothing  
B) Missing await before response.json()  
C) Missing error handling  
D) fetch should not be awaited

<details>
<summary>Show Answer</summary>

**Answer: B) Missing await before response.json()**

**Explanation:** `.json()` returns a Promise and must be awaited:

```javascript
async function fetchData() {
  const response = await fetch('/api/data');
  return await response.json(); // ✅ Must await
}
```

Without `await`, the function returns a Promise, not the parsed data.
</details>

---

### Question 11
What does "top-level await" mean?

A) Using await at the beginning of a function  
B) Using await outside any function (ES modules only)  
C) Using await with the highest priority  
D) A performance optimization

<details>
<summary>Show Answer</summary>

**Answer: B) Using await outside any function (ES modules only)**

**Explanation:** Top-level await (ES2022) allows using `await` at the module's top level:

```javascript
// In an ES module (.mjs or type="module")
const data = await fetch('/api/data'); // No async function needed!
console.log(data);
```

Only works in ES modules, not regular scripts.
</details>

---

### Question 12
Which method should you use when you need the first successful result?

A) Promise.all()  
B) Promise.race()  
C) Promise.any()  
D) Promise.allSettled()

<details>
<summary>Show Answer</summary>

**Answer: C) Promise.any()**

**Explanation:**
- **Promise.race():** Returns first settled (resolved OR rejected)
- **Promise.any():** Returns first FULFILLED (ignores rejections until all fail)

```javascript
// First successful response wins
const fastest = await Promise.any([
  fetch('/api/server1'),
  fetch('/api/server2'),
  fetch('/api/server3')
]);
```
</details>

---

### Question 13
What's the output?

```javascript
async function test() {
  throw new Error("Oops");
}

test();
console.log("Done");
```

A) Error, then "Done"  
B) "Done", then unhandled rejection  
C) "Done" only  
D) Error only

<details>
<summary>Show Answer</summary>

**Answer: B) "Done", then unhandled rejection**

**Explanation:**
1. `test()` is called but not awaited
2. "Done" logs immediately (synchronous)
3. Error occurs asynchronously
4. Unhandled rejection warning appears

To handle properly:
```javascript
test().catch(err => console.error(err));
```
</details>

---

### Question 14
When should you use sequential await instead of Promise.all()?

A) Never, parallel is always better  
B) When operation B needs the result from operation A  
C) When you want slower code  
D) When working with arrays

<details>
<summary>Show Answer</summary>

**Answer: B) When operation B needs the result from operation A**

**Explanation:**

```javascript
// ✅ Sequential: B needs userId from A
const user = await fetchUser();
const posts = await fetchPosts(user.id); // Needs user.id

// ✅ Parallel: Independent operations
const [user, posts] = await Promise.all([
  fetchUser(),
  fetchAllPosts() // Doesn't need specific user
]);
```
</details>

---

### Question 15
What's the best way to retry a failed async operation?

A) Use a while loop  
B) Recursion  
C) A for loop with try/catch and exponential backoff  
D) setTimeout

<details>
<summary>Show Answer</summary>

**Answer: C) A for loop with try/catch and exponential backoff**

**Explanation:**

```javascript
async function retryOperation(fn, maxRetries = 3) {
  for (let i = 1; i <= maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (i === maxRetries) throw error;
      
      const delay = Math.pow(2, i) * 1000; // Exponential backoff
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }
}
```

This provides controlled retries with increasing delays (1s, 2s, 4s...).
</details>

---

## 8. REAL INTERVIEW QUESTIONS

### 🎯 Question 1: Explain async/await and how it differs from Promises

**Junior Answer:**
"async/await makes asynchronous code easier to read. Instead of using `.then()`, you use `await` to wait for a Promise. It's syntactic sugar over Promises."

**Senior Answer:**
"async/await is syntactic sugar built on top of Promises that makes asynchronous code read like synchronous code. Key differences:

1. **Syntax**: async/await uses try/catch instead of .then()/.catch() chains
2. **Readability**: Eliminates Promise hell and reduces nesting
3. **Debugging**: Provides better stack traces since the code flow is linear
4. **Error handling**: Centralized error handling with try/catch vs distributed .catch() calls
5. **Control flow**: More intuitive with standard constructs like loops and conditionals

Under the hood, async functions always return Promises, and await pauses function execution (but not the thread) until the Promise settles. It's important to note that await can only be used inside async functions, except for top-level await in ES modules.

Performance-wise, they're equivalent—the choice is about code organization and readability."

---

### 🎯 Question 2: How would you handle multiple independent async operations efficiently?

**Junior Answer:**
"You can use Promise.all() to run multiple operations at once:
```javascript
const results = await Promise.all([operation1(), operation2()]);
```
"

**Senior Answer:**
"The approach depends on the requirements:

**1. All must succeed (fail-fast):**
```javascript
const [users, posts, comments] = await Promise.all([
  fetchUsers(),
  fetchPosts(),
  fetchComments()
]);
// If ANY fails, entire operation fails
```

**2. Need all results regardless of failures:**
```javascript
const results = await Promise.allSettled([
  fetchUsers(),
  fetchPosts(),
  fetchComments()
]);

results.forEach(result => {
  if (result.status === 'fulfilled') {
    processSuccess(result.value);
  } else {
    logError(result.reason);
  }
});
```

**3. First successful result:**
```javascript
const data = await Promise.any([
  fetchFromPrimary(),
  fetchFromBackup(),
  fetchFromCache()
]);
// Returns first fulfilled, ignores rejections
```

**4. Rate-limited processing:**
```javascript
async function batchProcess(items, batchSize = 5) {
  for (let i = 0; i < items.length; i += batchSize) {
    const batch = items.slice(i, i + batchSize);
    await Promise.all(batch.map(item => processItem(item)));
  }
}
```

Choice depends on: failure tolerance, performance requirements, and business logic."

---

### 🎯 Question 3: What are common mistakes when using async/await?

**Junior Answer:**
"Common mistakes include forgetting to use `await`, not handling errors with try/catch, and using async functions in array methods like forEach."

**Senior Answer:**
"Here are the most critical mistakes I've seen in production:

**1. Not awaiting async functions:**
```javascript
// ❌ Returns Promise, not data
const data = getData();

// ✅ Waits for resolution
const data = await getData();
```

**2. Sequential when parallel is possible:**
```javascript
// ❌ 3 seconds total
const a = await fetch('/a'); // 1s
const b = await fetch('/b'); // 1s
const c = await fetch('/c'); // 1s

// ✅ 1 second total
const [a, b, c] = await Promise.all([
  fetch('/a'), fetch('/b'), fetch('/c')
]);
```

**3. Using async with Array.prototype methods:**
```javascript
// ❌ Doesn't wait
items.forEach(async item => await process(item));

// ✅ Waits for each
for (const item of items) {
  await process(item);
}
```

**4. Forgetting to check response.ok:**
```javascript
// ❌ 404 not caught
const res = await fetch(url);
const data = await res.json();

// ✅ Handles HTTP errors
const res = await fetch(url);
if (!res.ok) throw new Error(`HTTP ${res.status}`);
const data = await res.json();
```

**5. No error boundaries:**
```javascript
// ❌ Silent failure
async function process() {await riskyOperation();
}
process(); // If fails, no one knows

// ✅ Handled
process().catch(handleError);
```

**6. Creating Promises in loops inefficiently:**
```javascript
// ❌ Sequential creation
for (const id of ids) {
  users.push(await fetchUser(id));
}

// ✅ Parallel creation
const promises = ids.map(id => fetchUser(id));
const users = await Promise.all(promises);
```

These mistakes can cause performance issues, silent failures, and race conditions in production."

---

### 🎯 Question 4: How would you implement a timeout for an async operation?

**Junior Answer:**
"I would use setTimeout to reject the Promise after a certain time."

**Senior Answer:**
"There are several approaches, each with tradeoffs:

**1. Promise.race() with timeout Promise:**
```javascript
function withTimeout(promise, ms) {
  const timeout = new Promise((_, reject) =>
    setTimeout(() => reject(new Error(`Timeout after ${ms}ms`)), ms)
  );
  return Promise.race([promise, timeout]);
}

// Usage
try {
  const data = await withTimeout(fetch('/api/data'), 5000);
} catch (error) {
  if (error.message.includes('Timeout')) {
    handleTimeout();
  }
}
```

**2. AbortController (preferred for fetch):**
```javascript
async function fetchWithTimeout(url, ms) {
  const controller = new AbortController();
  const timeout = setTimeout(() => controller.abort(), ms);
  
  try {
    const response = await fetch(url, {
      signal: controller.signal
    });
    clearTimeout(timeout);
    return response;
  } catch (error) {
    if (error.name === 'AbortError') {
      throw new Error('Request timeout');
    }
    throw error;
  }
}
```

**3. Reusable wrapper with cleanup:**
```javascript
function createTimeoutWrapper(timeoutMs) {
  return async function(asyncFn) {
    let timeoutId;
    
    const timeoutPromise = new Promise((_, reject) => {
      timeoutId = setTimeout(() => {
        reject(new Error(`Operation timed out after ${timeoutMs}ms`));
      }, timeoutMs);
    });
    
    try {
      const result = await Promise.race([
        asyncFn(),
        timeoutPromise
      ]);
      clearTimeout(timeoutId);
      return result;
    } catch (error) {
      clearTimeout(timeoutId);
      throw error;
    }
  };
}

// Usage
const withTimeout5s = createTimeoutWrapper(5000);
const data = await withTimeout5s(() => fetch('/api/data'));
```

The AbortController approach is preferred for fetch requests as it actually cancels the network request, saving resources."

---

## 9. DEVTOOLS & DEBUGGING

### 🔍 Debugging async/await in Chrome DevTools

#### 1. **Setting Breakpoints in Async Functions**

**How to:**
1. Open DevTools (F12)
2. Go to **Sources** tab
3. Find your file in the file tree
4. Click line number to set breakpoint

**Key Points:**
- Breakpoints work normally in async functions
- When execution hits `await`, the function pauses
- Use "Step Over" (F10) to move to next await
- Use "Step Into" (F11) to enter awaited function

```javascript
async function debugExample() {
  console.log("Start"); // Set breakpoint here
  const data = await fetchData(); // Execution pauses here
  console.log(data); // And resumes here when Promise resolves
  return data;
}
```

---

#### 2. **Async Stack Traces**

Modern browsers show the full async call stack:

**Enable:**
1. DevTools → Sources
2. Check "Async Stack Traces" (usually enabled by default)

**What you see:**
```
Error: Something failed
    at processData (app.js:45)
    at async loadUser (app.js:30)
    at async main (app.js:10)
```

Without async stack traces, you'd only see the immediate function, losing context.

---

#### 3. **Console Debugging Tricks**

```javascript
// Use console.log with labels
async function fetchUser(id) {
  console.log('[fetchUser] Starting:', id);
  const response = await fetch(`/api/users/${id}`);
  console.log('[fetchUser] Response:', response.status);
  const data = await response.json();
  console.log('[fetchUser] Data:', data);
  return data;
}

// Log Promise states
const promise = fetchUser(1);
console.log('Immediate:', promise); // Promise { <pending> }

promise.then(data => {
  console.log('Resolved:', data);
});

// Use console.time for performance
console.time('fetchUsers');
const users = await Promise.all([
  fetchUser(1),
  fetchUser(2),
  fetchUser(3)
]);
console.timeEnd('fetchUsers'); // fetchUsers: 234.5ms
```

---

#### 4. **Network Tab for Fetch Debugging**

**When to use:** Debugging API calls with fetch()

**Steps:**
1. DevTools → Network tab
2. Perform the async operation
3. Look for the request in the list

**What to check:**
- **Status Code**: 200 (OK), 404 (Not Found), 500 (Server Error)
- **Response**: Click the request → Response tab
- **Timing**: See how long the request took
- **Headers**: Check request/response headers

---

#### 5. **Debugging Promise Rejections**

**Enable "Pause on caught exceptions":**
1. Sources tab → Pause button (bottom right)
2. Check "Pause on caught exceptions"

Now DevTools will pause whenever a Promise rejects, even if caught.

```javascript
async function example() {
  try {
    await riskyOperation(); // Debugger pauses here if it fails
  } catch (error) {
    console.error(error);
  }
}
```

---

#### 6. **Using `debugger` Statement**

```javascript
async function complexOperation() {
  const data = await fetchData();
  
  debugger; // Execution pauses here (if DevTools open)
  
  const processed = await processData(data);
  return processed;
}
```

When execution hits `debugger`, DevTools automatically opens and pauses.

---

#### 7. **Inspecting Pending Promises**

```javascript
// In console:
const promise = fetch('/api/data');
console.log(promise); // Promise { <pending> }

// Wait and check again:
await promise; // Now you can inspect the resolved value
```

Or use the **Console** to inspect:
```javascript
// Store in a global variable for inspection
window.myPromise = fetch('/api/data');

// Later, in console:
await myPromise.then(r => r.json());
```

---

#### 8. **Debugging Race Conditions**

**Technique:** Add artificial delays to expose timing issues:

```javascript
async function debugRace() {
  // Add delay to expose race condition
  await new Promise(resolve => setTimeout(resolve, 100));
  
  // Your code here
  userData.name = await fetchUserName();
}
```

---

#### 9. **Performance Profiling**

**Using Performance Tab:**
1. DevTools → Performance
2. Click Record
3. Trigger your async operations
4. Stop recording
5. Analyze timeline

**Look for:**
- Long-running async operations
- Blocked main thread
- Unnecessary sequential waits

---

#### 10. **Common Console Commands**

```javascript
// Check if function is async
console.log(myFunction.constructor.name); // "AsyncFunction"

// Monitor all fetch calls
monitor(fetch);

// Copy response data to clipboard
copy(await fetch('/api/data').then(r => r.json()));

// Log all network requests
$$('fetch').forEach(f => console.log(f));
```

---

### 🛠️ Debugging Checklist

When debugging async/await issues, check:

- [ ] Is the function declared with `async`?
- [ ] Is `await` used before async calls?
- [ ] Are errors handled with try/catch?
- [ ] Is `response.ok` checked after fetch?
- [ ] Are independent operations running in parallel?
- [ ] Is forEach being used with async? (Use for...of instead)
- [ ] Are Promises being awaited in the right order?
- [ ] Is there an unhandled Promise rejection?

---

## 10. CHAPTER SUMMARY & NEXT STEPS

### 🎓 Quick Recap

**Key Concepts Mastered:**

1. **async Keyword**
   - Makes functions return Promises automatically
   - Enables use of `await` inside the function

2. **await Keyword**
   - Pauses async function execution
   - Waits for Promise to settle
   - Returns resolved value or throws on rejection

3. **Error Handling**
   - Use try/catch blocks (not .catch() chains)
   - Always handle potential errors
   - Use `finally` for cleanup

4. **Execution Patterns**
   - **Sequential**: When operations depend on each other
   - **Parallel**: When operations are independent (use Promise.all())
   - Choose based on dependencies and performance needs

5. **Common Utilities**
   - `Promise.all()`: All must succeed (fail-fast)
   - `Promise.allSettled()`: Get all results regardless
   - `Promise.race()`: First to settle wins
   - `Promise.any()`: First to fulfill wins

6. **Best Practices**
   - Check `response.ok` after fetch
   - Avoid async in forEach/map (use for...of or Promise.all)
   - Don't mix .then() with async/await
   - Run independent operations in parallel
   - Always clean up resources in finally blocks

---

### ✅ Self-Assessment

Can you confidently:
- [ ] Convert Promise-based code to async/await?
- [ ] Choose between sequential and parallel execution?
- [ ] Handle errors properly with try/catch?
- [ ] Debug async code using Chrome DevTools?
- [ ] Implement retry logic and timeouts?
- [ ] Explain when to use Promise.all vs Promise.allSettled?
- [ ] Avoid common async/await pitfalls?

If you checked all boxes, you're ready for Day 25! 🎉

---

### 🔗 Connection to Next Topic

**Tomorrow: JavaScript fetch() Explained Like Never Before**

Now that you've mastered async/await, you're perfectly positioned to dive deep into the Fetch API. You'll learn:

- How fetch() works under the hood
- Request and Response objects in detail
- Setting headers, handling different HTTP methods
- Working with FormData, JSON, and file uploads
- Request interceptors and middleware patterns
- Building a complete API client

**Preview:**
```javascript
// You now understand this perfectly!
async function createUser(userData) {
  try {
    const response = await fetch('/api/users', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(userData)
    });
    
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }
    
    return await response.json();
  } catch (error) {
    console.error('User creation failed:', error);
    throw error;
  }
}

// Tomorrow: You'll master ALL of this ↓
// - Custom headers and authentication
// - Request/response interceptors
// - File uploads with progress tracking
// - Caching strategies
// - Error recovery patterns
```

---

### 📚 Additional Resources


**Practice:**
- Complete all tasks given.
- Build the Movie Explorer project
- Try the Debug Challenges without looking at solutions

**Challenge Yourself:**
- Implement a retry mechanism with exponential backoff
- Build a rate-limited API wrapper
- Create an async queue that processes tasks sequentially

---

### 🎯 Ready for Day 25?

You've completed Day 24 - async/await! 🎉

**What's Next:**
- Review any sections where you struggled
- Complete the practice problems in README.md
- Build the Movie Explorer project
- Join the Discord to discuss questions

**See you on Day 25!** 🚀

---

<div align="center">

### 💬 Questions or Stuck?

Join the [Discord Community](#) | [Report Issues](#) | [View Full Course](#)

**Remember:** Master these concepts through practice, not just reading. Build things! Break things! Debug things! That's how you truly learn async/await.

---

*Happy Coding! 💻*

</div>



