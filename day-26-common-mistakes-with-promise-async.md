# Day 26: 6 Common Mistakes with JavaScript Promises & Async Code


## 1. CHAPTER OVERVIEW

### In the Video
You learned about the six most common pitfalls developers face when working with Promises and async/await, including sequential vs parallel execution traps, promise chaining mistakes, improper error handling, missing callbacks, blocking operations, and over-engineered try/catch blocks.

### Checklist: By the End of This Workbook, You Will Be Able To...
- ✅ Identify when to use `Promise.all()` vs sequential `await` statements
- ✅ Write clean promise chains without falling into "promise hell"
- ✅ Implement proper error handling strategies for async operations
- ✅ Avoid accidentally blocking the event loop with synchronous operations
- ✅ Return and await promises correctly in all contexts
- ✅ Use try/catch blocks strategically without over-engineering

### Prerequisites
From **Day 25 (JavaScript fetch() Explained)**, you should understand:
- How to use the `fetch()` API for HTTP requests
- Basic Promise syntax (`.then()`, `.catch()`)
- The fundamentals of `async/await`
- How to parse JSON responses

---

## 2. LECTURE CHEAT SHEET

### Key Terms Glossary

| Term | Definition |
|------|------------|
| **Promise Hell** | Nested promise chains that are difficult to read and maintain (similar to callback hell) |
| **Sequential Execution** | Running async operations one after another, waiting for each to complete |
| **Parallel Execution** | Running multiple async operations simultaneously |
| **Event Loop Blocking** | Using synchronous operations that prevent other code from executing |
| **Unhandled Rejection** | A promise that fails without a `.catch()` or try/catch handler |
| **Microtask** | High-priority async operations (promises) that execute before regular callbacks |

---

### Mistake #1: Sequential vs Parallel Execution

```
❌ SLOW (Sequential)          ✅ FAST (Parallel)
┌─────────────────┐           ┌─────────────────┐
│  Request 1      │ 500ms     │  Request 1      │ 500ms
└─────────────────┘           │  Request 2      │ 500ms
┌─────────────────┐           │  Request 3      │ 500ms
│  Request 2      │ 500ms     └─────────────────┘
└─────────────────┘           Total: 500ms ⚡
┌─────────────────┐
│  Request 3      │ 500ms
└─────────────────┘
Total: 1500ms 🐌
```

**Syntax Comparison:**

```javascript
// ❌ Sequential - Takes 3x longer
async function fetchSequential(ids) {
  const results = [];
  for (const id of ids) {
    const response = await fetch(`/api/users/${id}`);
    const data = await response.json();
    results.push(data);
  }
  return results;
}

// ✅ Parallel - Much faster
async function fetchParallel(ids) {
  const promises = ids.map(id => 
    fetch(`/api/users/${id}`).then(res => res.json())
  );
  return Promise.all(promises);
}
```

---

### Mistake #2: Promise Chaining

**The Pyramid of Doom:**

```javascript
// ❌ Promise Hell - Hard to read
fetch('/api/user')
  .then(res => {
    return res.json().then(user => {
      return fetch(`/api/posts/${user.id}`).then(postsRes => {
        return postsRes.json().then(posts => {
          return { user, posts };
        });
      });
    });
  });

// ✅ Flat Chain - Clean
let userData;
fetch('/api/user')
  .then(res => res.json())
  .then(user => {
    userData = user;
    return fetch(`/api/posts/${user.id}`);
  })
  .then(res => res.json())
  .then(posts => ({ user: userData, posts }));

// ✅✅ Best - async/await
async function getUserData() {
  const userRes = await fetch('/api/user');
  const user = await userRes.json();
  const postsRes = await fetch(`/api/posts/${user.id}`);
  const posts = await postsRes.json();
  return { user, posts };
}
```

---

### Mistake #3: Error Handling Patterns

| Pattern | When to Use | Example |
|---------|-------------|---------|
| **Try/Catch** | With async/await | `try { await fetch() } catch(e) {}` |
| **.catch()** | With promise chains | `promise.then().catch()` |
| **Re-throw** | When adding context | `catch(e) { throw new Error('Context: ' + e) }` |
| **Global Handler** | Fallback safety net | `process.on('unhandledRejection')` |

**Error Handling Decision Tree:**

```
Is the error expected?
├─ YES → Handle it gracefully, provide fallback
└─ NO → Log it, re-throw with context

Can the operation be retried?
├─ YES → Implement retry logic with exponential backoff
└─ NO → Fail fast, inform user

Does the caller need to know?
├─ YES → Re-throw the error
└─ NO → Handle completely, return safe value
```

---

### Mistake #4: Missing Return/Await

```javascript
// ❌ Common Mistakes
function bad1() {
  fetch('/api/data')  // Missing return!
    .then(res => res.json());
}

async function bad2() {
  const items = [1, 2, 3];
  items.forEach(async (id) => {
    fetchUser(id);  // Not awaited!
  });
  console.log('Done'); // Lies! Runs immediately
}

// ✅ Correct Patterns
function good1() {
  return fetch('/api/data')  // Return the promise
    .then(res => res.json());
}

async function good2() {
  const items = [1, 2, 3];
  for (const id of items) {
    await fetchUser(id);  // Properly awaited
  }
  console.log('Done'); // Actually done
}
```

---

### Mistake #5: Blocking Operations

| Operation Type | Blocking (❌) | Non-Blocking (✅) |
|----------------|--------------|------------------|
| **File Read** | `fs.readFileSync()` | `fs.promises.readFile()` |
| **Sleep/Delay** | `while(Date.now() < end)` | `await new Promise(r => setTimeout(r, ms))` |
| **Large Computation** | Synchronous loop | Web Workers / `setImmediate()` |

**Performance Impact:**

```javascript
// ❌ Blocks event loop for 3 seconds
function blockingDelay(ms) {
  const start = Date.now();
  while(Date.now() - start < ms) {}
}

// ✅ Non-blocking sleep
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

// Usage
await sleep(3000); // Other operations can run during wait
```

---

### Mistake #6: Try/Catch Abuse

**When to Use Try/Catch:**

```javascript
// ✅ Good use cases
try {
  const res = await fetch(url);
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return await res.json();
} catch (error) {
  if (error.message.includes('HTTP 404')) {
    return null; // Specific handling
  }
  logger.error('Fetch failed:', error);
  throw error; // Re-throw with context
}

// ❌ Unnecessary nesting
try {
  const a = await operation1();
  try {
    const b = await operation2(a);
    try {
      return await operation3(b);
    } catch (e3) { throw e3; } // Useless
  } catch (e2) { throw e2; } // Useless
} catch (e1) { throw e1; } // Useless
```

---

### Promise Utility Methods Comparison

| Method | Behavior | Use Case |
|--------|----------|----------|
| `Promise.all()` | Fails if ANY rejects | All operations must succeed |
| `Promise.allSettled()` | Never fails, waits for all | Process all regardless of failures |
| `Promise.race()` | Returns first to settle | Timeout implementations |
| `Promise.any()` | Returns first to resolve | Get fastest successful response |

```javascript
// Example: Fetch with timeout using Promise.race()
async function fetchWithTimeout(url, timeout = 5000) {
  const timeoutPromise = new Promise((_, reject) =>
    setTimeout(() => reject(new Error('Timeout')), timeout)
  );
  
  return Promise.race([
    fetch(url),
    timeoutPromise
  ]);
}
```

---

## 3. PREDICT THE OUTPUT

### Question 1
```javascript
async function mystery1() {
  console.log('A');
  await Promise.resolve();
  console.log('B');
}

console.log('C');
mystery1();
console.log('D');
```

**🤔 Thinking Process:**
- `console.log('C')` runs first (synchronous)
- `mystery1()` is called, logs 'A', then hits `await`
- Control returns to main thread, `console.log('D')` runs
- Microtask queue processes, 'B' logs last

**✅ Output:**
```
C
A
D
B
```

**💡 Why:** The `await` causes the rest of the function to be queued as a microtask after the current synchronous code completes.

---

### Question 2
```javascript
const promise = new Promise((resolve) => {
  console.log('1');
  resolve('2');
});

promise.then(value => console.log(value));
console.log('3');
```

**🤔 Thinking Process:**
- Promise executor runs immediately (synchronous)
- `.then()` callback is queued as microtask
- `console.log('3')` runs next
- Event loop processes microtask queue

**✅ Output:**
```
1
3
2
```

**💡 Why:** Promise executors run synchronously, but `.then()` callbacks are always asynchronous (microtasks).

---

### Question 3
```javascript
async function test() {
  const results = [];
  [1, 2, 3].forEach(async (num) => {
    const result = await Promise.resolve(num * 2);
    results.push(result);
  });
  return results;
}

test().then(console.log);
```

**🤔 Thinking Process:**
- `forEach` doesn't wait for async callbacks
- `test()` returns immediately with empty array
- The async operations complete later

**✅ Output:**
```
[]
```

**💡 Why:** `forEach` doesn't work with async/await. Use `for...of` loop or `Promise.all()` with `map()`.

---

### Question 4
```javascript
async function fetchData() {
  try {
    return await fetch('/api/data');
  } catch (error) {
    console.log('Caught:', error.message);
  }
}

fetchData();
```

**🤔 Thinking Process:**
- If fetch succeeds, Response object is returned
- If fetch fails (network error), error is caught and logged
- Function returns `undefined` after catch block

**✅ Output:**
- Success: No console output, returns Response
- Failure: Logs "Caught: [error message]", returns undefined

**💡 Why:** The catch only triggers on network failures, not HTTP errors (404, 500). Always check `response.ok`.

---

### Question 5
```javascript
Promise.resolve('A')
  .then(result => {
    console.log(result);
    return 'B';
  })
  .then(result => {
    console.log(result);
  })
  .then(result => {
    console.log(result);
  });
```

**🤔 Thinking Process:**
- First `.then()` logs 'A', returns 'B'
- Second `.then()` logs 'B', returns undefined (implicit)
- Third `.then()` logs undefined

**✅ Output:**
```
A
B
undefined
```

**💡 Why:** When you don't return a value from `.then()`, the next handler receives `undefined`.

---

### Question 6
```javascript
async function quickTest() {
  const p1 = Promise.resolve(1);
  const p2 = Promise.resolve(2);
  const p3 = Promise.resolve(3);
  
  const result = await Promise.all([p1, p2, p3]);
  console.log(result);
}

quickTest();
```

**🤔 Thinking Process:**
- All three promises are already resolved
- `Promise.all()` waits for all (instant in this case)
- Returns array of resolved values in order

**✅ Output:**
```
[1, 2, 3]
```

**💡 Why:** `Promise.all()` preserves the order of input promises in the result array.

---

### Question 7
```javascript
async function errorTest() {
  try {
    const result = Promise.reject('Error'); // Missing await!
    console.log('Success');
  } catch (error) {
    console.log('Caught:', error);
  }
}

errorTest();
```

**🤔 Thinking Process:**
- `Promise.reject()` returns a rejected promise
- WITHOUT `await`, it doesn't throw in try/catch
- Unhandled rejection occurs later

**✅ Output:**
```
Success
UnhandledPromiseRejectionWarning: Error
```

**💡 Why:** You must `await` a promise for try/catch to work. Without `await`, it's just assigning a promise to a variable.

---

### Question 8
```javascript
const urls = ['url1', 'url2', 'url3'];

async function processSequential() {
  for (const url of urls) {
    await fetch(url);
  }
  console.log('Sequential done');
}

async function processParallel() {
  const promises = urls.map(url => fetch(url));
  await Promise.all(promises);
  console.log('Parallel done');
}

processSequential();
processParallel();
```

**🤔 Thinking Process:**
- Both start simultaneously
- Parallel completes ~3x faster
- "Parallel done" logs first

**✅ Output:**
```
Parallel done
Sequential done
```

**💡 Why:** Parallel execution runs all fetches simultaneously, while sequential waits for each to complete.

---

### Question 9
```javascript
function delayedLog(msg, ms) {
  return new Promise(resolve => {
    setTimeout(() => {
      console.log(msg);
      resolve();
    }, ms);
  });
}

delayedLog('A', 1000);
delayedLog('B', 500);
delayedLog('C', 100);
```

**🤔 Thinking Process:**
- All three setTimeout calls start immediately
- They complete in order of delay duration
- C finishes first (100ms), then B (500ms), then A (1000ms)

**✅ Output:**
```
C (after 100ms)
B (after 500ms)
A (after 1000ms)
```

**💡 Why:** setTimeout delays are independent; shortest delay completes first.

---

### Question 10
```javascript
async function test() {
  return 'Hello';
}

console.log(test());
```

**🤔 Thinking Process:**
- Async functions ALWAYS return a Promise
- Even when you return a non-promise value
- The value is automatically wrapped in a resolved promise

**✅ Output:**
```
Promise { 'Hello' }
```

**💡 Why:** To get the actual value, you need to `await test()` or use `.then()`.

---

### Question 11
```javascript
Promise.resolve()
  .then(() => console.log('1'))
  .then(() => console.log('2'));

Promise.resolve()
  .then(() => console.log('3'))
  .then(() => console.log('4'));
```

**🤔 Thinking Process:**
- Both chains start in the same microtask queue
- First `.then()` of each chain runs in same microtask batch
- Then second `.then()` of each chain runs

**✅ Output:**
```
1
3
2
4
```

**💡 Why:** Microtasks from the same "level" of promise chains execute together before moving to the next level.

---

### Question 12
```javascript
async function test() {
  const arr = [1, 2, 3];
  arr.forEach(async (num) => {
    await Promise.resolve(num);
    console.log(num);
  });
  console.log('Done');
}

test();
```

**🤔 Thinking Process:**
- forEach doesn't wait for async callbacks
- 'Done' logs immediately
- Numbers log after in undefined order

**✅ Output:**
```
Done
1
2
3
```

**💡 Why:** forEach doesn't support async/await. Use `for...of` for sequential or `Promise.all()` with `map()` for parallel.

---

### Question 13
```javascript
const promise = fetch('/api/data')
  .then(res => res.json())
  .catch(err => console.log('Error'));

promise.then(data => console.log(data));
```

**🤔 Thinking Process:**
- If fetch succeeds, data logs normally
- If fetch fails, 'Error' logs, then `undefined` is passed to second `.then()`
- `.catch()` returns a resolved promise with undefined value

**✅ Output (on error):**
```
Error
undefined
```

**💡 Why:** After `.catch()`, the chain continues with a resolved promise (unless you re-throw).

---

### Question 14
```javascript
async function mystery() {
  console.log('Start');
  
  const promise = new Promise((resolve) => {
    console.log('Promise executor');
    resolve('Resolved');
  });
  
  console.log('Middle');
  const result = await promise;
  console.log(result);
}

mystery();
console.log('End');
```

**🤔 Thinking Process:**
- 'Start' logs (sync)
- Promise executor runs immediately (sync), logs 'Promise executor'
- 'Middle' logs (sync)
- `await` pauses function, control returns to main
- 'End' logs
- Microtask completes, 'Resolved' logs

**✅ Output:**
```
Start
Promise executor
Middle
End
Resolved
```

**💡 Why:** Promise executors are synchronous; the async behavior comes from `.then()` or `await`.

---

### Question 15
```javascript
Promise.race([
  new Promise(resolve => setTimeout(() => resolve('Fast'), 100)),
  new Promise(resolve => setTimeout(() => resolve('Slow'), 500))
]).then(console.log);
```

**🤔 Thinking Process:**
- Both promises start simultaneously
- `Promise.race()` returns value of first settled promise
- 'Fast' resolves at 100ms, before 'Slow'

**✅ Output:**
```
Fast
```

**💡 Why:** `Promise.race()` settles as soon as the fastest promise settles.

---


## 4. DEBUG CHALLENGES

### Debug Challenge 1: The Missing Await

```javascript
// 🐛 This code doesn't work as expected
async function getUserName(userId) {
  const user = fetch(`/api/users/${userId}`)
    .then(res => res.json());
  
  return user.name;
}

getUserName(1).then(name => console.log(name));
```

**❓ What's wrong?**

<details>
<summary>Click to see the solution</summary>

**🔧 The Bug:**
- Missing `await` before the fetch call
- Trying to access `.name` on a Promise object instead of the resolved user data

**✅ Fixed Code:**
```javascript
async function getUserName(userId) {
  const response = await fetch(`/api/users/${userId}`);
  const user = await response.json();
  
  return user.name;
}

// Or with promise chain:
async function getUserName(userId) {
  const user = await fetch(`/api/users/${userId}`)
    .then(res => res.json());
  
  return user.name;
}
```

**💡 Explanation:** When using `async/await`, you must `await` promises before accessing their resolved values. Otherwise, you're working with the Promise object itself.

</details>

---

### Debug Challenge 2: forEach Hell

```javascript
// 🐛 This prints "Done" before processing any users
async function processUsers(userIds) {
  userIds.forEach(async (id) => {
    const user = await fetchUser(id);
    console.log(`Processed: ${user.name}`);
  });
  
  console.log('Done processing all users');
}

processUsers([1, 2, 3]);
```

**❓ What's wrong?**

<details>
<summary>Click to see the solution</summary>

**🔧 The Bug:**
- `forEach` doesn't wait for async callbacks
- The function continues immediately after starting the forEach loop

**✅ Fixed Code (Sequential):**
```javascript
async function processUsers(userIds) {
  for (const id of userIds) {
    const user = await fetchUser(id);
    console.log(`Processed: ${user.name}`);
  }
  
  console.log('Done processing all users');
}
```

**✅ Fixed Code (Parallel):**
```javascript
async function processUsers(userIds) {
  const promises = userIds.map(async (id) => {
    const user = await fetchUser(id);
    console.log(`Processed: ${user.name}`);
  });
  
  await Promise.all(promises);
  console.log('Done processing all users');
}
```

**💡 Explanation:** `forEach`, `map`, `filter` etc. don't work with async/await. Use `for...of` for sequential or `Promise.all()` with `map()` for parallel execution.

</details>

---

### Debug Challenge 3: Silent Error Swallowing

```javascript
// 🐛 Errors disappear into the void
async function fetchData(url) {
  try {
    const response = await fetch(url);
    const data = await response.json();
    return data;
  } catch (error) {
    console.log('Something went wrong');
    return null;
  }
}

// User never knows why this fails
fetchData('/api/data').then(data => {
  console.log(data.results); // TypeError: Cannot read property 'results' of null
});
```

**❓ What's wrong?**

<details>
<summary>Click to see the solution</summary>

**🔧 The Bug:**
- Catching errors but not re-throwing them
- Returning `null` hides the real problem from the caller
- Generic error message provides no debugging information
- Caller has no way to handle the error appropriately

**✅ Fixed Code:**
```javascript
async function fetchData(url) {
  try {
    const response = await fetch(url);
    
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }
    
    const data = await response.json();
    return data;
    
  } catch (error) {
    console.error('Failed to fetch data:', {
      url,
      error: error.message,
      stack: error.stack
    });
    throw error; // Re-throw for caller to handle
  }
}

// Now the caller can handle errors appropriately
fetchData('/api/data')
  .then(data => {
    if (data && data.results) {
      console.log(data.results);
    }
  })
  .catch(error => {
    console.error('Error fetching data:', error.message);
    // Show user-friendly error message
  });
```

**💡 Explanation:** Always re-throw errors after logging them. Silent failures make debugging impossible. Let the caller decide how to handle errors.

</details>

---

### Debug Challenge 4: Promise Hell Revival

```javascript
// 🐛 This is callback hell with promises
function getUserData(userId) {
  return fetch(`/api/users/${userId}`)
    .then(response => {
      return response.json().then(user => {
        return fetch(`/api/posts/${user.id}`)
          .then(postsResponse => {
            return postsResponse.json().then(posts => {
              return fetch(`/api/comments/${posts[0].id}`)
                .then(commentsResponse => {
                  return commentsResponse.json().then(comments => {
                    return { user, posts, comments };
                  });
                });
            });
          });
      });
    });
}
```

**❓ What's wrong?**

<details>
<summary>Click to see the solution</summary>

**🔧 The Bug:**
- Nested promise chains (promise hell)
- Difficult to read and maintain
- Error handling becomes complex
- Hard to add logging or modify flow

**✅ Fixed Code (Promise Chaining):**
```javascript
function getUserData(userId) {
  let userData, postsData;
  
  return fetch(`/api/users/${userId}`)
    .then(response => response.json())
    .then(user => {
      userData = user;
      return fetch(`/api/posts/${user.id}`);
    })
    .then(response => response.json())
    .then(posts => {
      postsData = posts;
      return fetch(`/api/comments/${posts[0].id}`);
    })
    .then(response => response.json())
    .then(comments => ({
      user: userData,
      posts: postsData,
      comments
    }));
}
```

**✅ Fixed Code (async/await - Best):**
```javascript
async function getUserData(userId) {
  const userResponse = await fetch(`/api/users/${userId}`);
  const user = await userResponse.json();
  
  const postsResponse = await fetch(`/api/posts/${user.id}`);
  const posts = await postsResponse.json();
  
  const commentsResponse = await fetch(`/api/comments/${posts[0].id}`);
  const comments = await commentsResponse.json();
  
  return { user, posts, comments };
}
```

**💡 Explanation:** Flatten promise chains or use async/await for better readability. Nested `.then()` calls are an anti-pattern.

</details>

---

### Debug Challenge 5: Race Condition

```javascript
// 🐛 Counter doesn't reach the expected value
let counter = 0;

async function incrementCounter() {
  const current = counter;
  await new Promise(resolve => setTimeout(resolve, 100)); // Simulate async work
  counter = current + 1;
}

// Expected: counter = 5, Actual: counter = 1 or 2
Promise.all([
  incrementCounter(),
  incrementCounter(),
  incrementCounter(),
  incrementCounter(),
  incrementCounter()
]);

setTimeout(() => console.log('Counter:', counter), 500);
```

**❓ What's wrong?**

<details>
<summary>Click to see the solution</summary>

**🔧 The Bug:**
- Classic race condition
- Multiple async operations read the same initial value before any writes occur
- All operations write back nearly the same value

**Flow of the bug:**
```
Time 0ms:   All 5 functions read counter = 0
Time 100ms: All 5 functions write counter = 1
Result:     counter = 1 (instead of 5)
```

**✅ Fixed Code (Sequential Execution):**
```javascript
let counter = 0;

async function incrementCounter() {
  const current = counter;
  await new Promise(resolve => setTimeout(resolve, 100));
  counter = current + 1;
}

// Run sequentially
async function runIncrements() {
  await incrementCounter();
  await incrementCounter();
  await incrementCounter();
  await incrementCounter();
  await incrementCounter();
  console.log('Counter:', counter); // Now correctly 5
}

runIncrements();
```

**✅ Fixed Code (Using a Mutex Pattern):**
```javascript
class Counter {
  constructor() {
    this.value = 0;
    this.lock = Promise.resolve();
  }

  async increment() {
    // Queue this operation after previous ones
    this.lock = this.lock.then(async () => {
      const current = this.value;
      await new Promise(resolve => setTimeout(resolve, 100));
      this.value = current + 1;
    });
    
    return this.lock;
  }

  getValue() {
    return this.value;
  }
}

// Usage
const counter = new Counter();

Promise.all([
  counter.increment(),
  counter.increment(),
  counter.increment(),
  counter.increment(),
  counter.increment()
]).then(() => {
  console.log('Counter:', counter.getValue()); // Correctly 5
});
```

**✅ Fixed Code (Atomic Operations):**
```javascript
let counter = 0;

async function incrementCounter() {
  await new Promise(resolve => setTimeout(resolve, 100));
  counter++; // This is atomic in JavaScript's single-threaded model
}

Promise.all([
  incrementCounter(),
  incrementCounter(),
  incrementCounter(),
  incrementCounter(),
  incrementCounter()
]).then(() => {
  console.log('Counter:', counter); // Correctly 5
});
```

**💡 Explanation:** Race conditions occur when multiple async operations access shared state. Solutions include: sequential execution, mutex/lock patterns, or making operations truly atomic.

</details>

---

## 5. COMMON PITFALLS & ANTI-PATTERNS

### ❌ Don't Do This | ✅ Do This Instead

---

### Pitfall 1: Using forEach with async/await

```javascript
// ❌ WRONG: forEach doesn't wait
async function bad() {
  const ids = [1, 2, 3];
  ids.forEach(async (id) => {
    await processItem(id);
  });
  console.log('Done'); // Lies! Not done yet
}

// ✅ CORRECT: Use for...of
async function good() {
  const ids = [1, 2, 3];
  for (const id of ids) {
    await processItem(id);
  }
  console.log('Done'); // Actually done
}

// ✅ ALSO CORRECT: Parallel execution
async function goodParallel() {
  const ids = [1, 2, 3];
  await Promise.all(ids.map(id => processItem(id)));
  console.log('Done'); // All completed
}
```

**💡 Rule:** Never use `forEach`, `map`, `filter` with async callbacks. Use `for...of` or `Promise.all()`.

---

### Pitfall 2: Not checking response.ok

```javascript
// ❌ WRONG: Assumes all HTTP responses are successful
async function bad(url) {
  const response = await fetch(url);
  return response.json(); // 404 errors won't be caught!
}

// ✅ CORRECT: Always check response.ok
async function good(url) {
  const response = await fetch(url);
  
  if (!response.ok) {
    throw new Error(`HTTP ${response.status}: ${response.statusText}`);
  }
  
  return response.json();
}
```

**💡 Rule:** `fetch()` only rejects on network errors, not HTTP errors. Always check `response.ok`.

---

### Pitfall 3: Sequential execution when parallel is possible

```javascript
// ❌ WRONG: Takes 3 seconds total
async function bad() {
  const user = await fetchUser(); // 1 second
  const posts = await fetchPosts(); // 1 second
  const comments = await fetchComments(); // 1 second
  return { user, posts, comments };
}

// ✅ CORRECT: Takes 1 second total
async function good() {
  const [user, posts, comments] = await Promise.all([
    fetchUser(),
    fetchPosts(),
    fetchComments()
  ]);
  return { user, posts, comments };
}
```

**💡 Rule:** If operations don't depend on each other, run them in parallel with `Promise.all()`.

---

### Pitfall 4: Catching errors and returning null

```javascript
// ❌ WRONG: Hides errors from caller
async function bad(url) {
  try {
    const response = await fetch(url);
    return response.json();
  } catch (error) {
    console.log('Error occurred');
    return null; // Caller has no idea what went wrong
  }
}

// ✅ CORRECT: Re-throw with context
async function good(url) {
  try {
    const response = await fetch(url);
    
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }
    
    return response.json();
  } catch (error) {
    console.error(`Failed to fetch ${url}:`, error.message);
    throw error; // Let caller decide how to handle
  }
}
```

**💡 Rule:** Don't swallow errors. Log them and re-throw so callers can handle appropriately.

---

### Pitfall 5: Mixing async patterns

```javascript
// ❌ WRONG: Mixing callbacks, promises, and async/await
async function bad(callback) {
  fetch('/api/data')
    .then(response => response.json())
    .then(async data => {
      const processed = await processData(data);
      callback(processed); // Don't mix callbacks with promises!
    });
}

// ✅ CORRECT: Stick to one pattern (async/await)
async function good() {
  const response = await fetch('/api/data');
  const data = await response.json();
  const processed = await processData(data);
  return processed;
}
```

**💡 Rule:** Pick one async pattern (preferably async/await) and stick with it throughout your codebase.

---

### Pitfall 6: Not handling Promise.all failures properly

```javascript
// ❌ WRONG: One failure kills everything
async function bad(urls) {
  const promises = urls.map(url => fetch(url));
  return Promise.all(promises); // Fails if ANY request fails
}

// ✅ CORRECT: Handle individual failures gracefully
async function good(urls) {
  const promises = urls.map(url =>
    fetch(url)
      .then(res => ({ status: 'success', data: res }))
      .catch(err => ({ status: 'failed', error: err.message }))
  );
  return Promise.all(promises); // Never fails, returns all results
}

// ✅ ALSO CORRECT: Use Promise.allSettled
async function goodSettled(urls) {
  const promises = urls.map(url => fetch(url));
  const results = await Promise.allSettled(promises);
  
  return results.map((result, index) => ({
    url: urls[index],
    ...result
  }));
}
```

**💡 Rule:** Use `Promise.allSettled()` when you want results from all operations, even if some fail.

---

### Pitfall 7: Forgetting to return in promise chains

```javascript
// ❌ WRONG: Missing return statement
function bad() {
  fetch('/api/data')
    .then(response => response.json()) // Forgot to return!
    .then(data => console.log(data)); // Gets undefined
}

// ✅ CORRECT: Always return promises
function good() {
  return fetch('/api/data')
    .then(response => response.json())
    .then(data => {
      console.log(data);
      return data; // Return value for next handler
    });
}
```

**💡 Rule:** Always `return` promises from functions and values from `.then()` handlers.

---

### Pitfall 8: Using blocking operations in async functions

```javascript
// ❌ WRONG: Blocks the event loop
const fs = require('fs');

async function bad() {
  const data = fs.readFileSync('file.txt'); // Blocks everything!
  return data;
}

// ✅ CORRECT: Use async versions
const fs = require('fs').promises;

async function good() {
  const data = await fs.readFile('file.txt', 'utf8');
  return data;
}
```

**💡 Rule:** Never use "Sync" methods (`readFileSync`, etc.) in async code. Always use async versions.

---

## 6. KNOWLEDGE CHECK MCQs

### Question 1
What happens if you don't `await` a promise inside an async function?

- A) The function waits automatically
- B) The function continues without waiting
- C) A syntax error occurs
- D) The promise is automatically cancelled

<details>
<summary>Show Answer</summary>

**✅ Answer: B) The function continues without waiting**

**Explanation:** Without `await`, the async function treats the promise as just another value and continues executing. The promise will still run, but the function won't wait for it to complete.

```javascript
async function example() {
  fetch('/api/data'); // Not awaited - function doesn't wait
  console.log('This runs immediately');
}
```
</details>

---

### Question 2
Which method should you use when you want results from all promises, even if some fail?

- A) `Promise.all()`
- B) `Promise.race()`
- C) `Promise.allSettled()`
- D) `Promise.any()`

<details>
<summary>Show Answer</summary>

**✅ Answer: C) Promise.allSettled()**

**Explanation:** `Promise.allSettled()` waits for all promises to complete and returns results for both successful and failed promises. `Promise.all()` fails immediately if any promise rejects.

```javascript
const results = await Promise.allSettled([
  fetch('/api/1'), // might fail
  fetch('/api/2'), // might fail
  fetch('/api/3')  // might fail
]);
// Still get results from all three
```
</details>

---

### Question 3
What's wrong with this code?

```javascript
userIds.forEach(async (id) => {
  await processUser(id);
});
console.log('Done');
```

- A) Nothing, it works correctly
- B) forEach doesn't work with async callbacks
- C) The await is unnecessary
- D) async can't be used with arrow functions

<details>
<summary>Show Answer</summary>

**✅ Answer: B) forEach doesn't work with async callbacks**

**Explanation:** `forEach` doesn't wait for async callbacks to complete. The "Done" message prints before any users are processed. Use `for...of` or `Promise.all()` instead.

```javascript
// Correct way:
for (const id of userIds) {
  await processUser(id);
}
console.log('Done'); // Actually done
```
</details>

---

### Question 4
When does `fetch()` reject its promise?

- A) On any HTTP error (404, 500, etc.)
- B) Only on network errors
- C) When response.json() fails
- D) It never rejects

<details>
<summary>Show Answer</summary>

**✅ Answer: B) Only on network errors**

**Explanation:** `fetch()` only rejects on network failures. HTTP errors like 404 or 500 still resolve successfully. You must check `response.ok` to detect HTTP errors.

```javascript
const response = await fetch(url);
if (!response.ok) { // Manual check required
  throw new Error(`HTTP ${response.status}`);
}
```
</details>

---

### Question 5
What's the output of this code?

```javascript
async function test() {
  return 'Hello';
}
console.log(test());
```

- A) `Hello`
- B) `Promise { 'Hello' }`
- C) `undefined`
- D) A syntax error

<details>
<summary>Show Answer</summary>

**✅ Answer: B) Promise { 'Hello' }**

**Explanation:** Async functions always return a promise, even when you return a non-promise value. To get the actual value, use `await` or `.then()`.

```javascript
const result = await test(); // Now result is 'Hello'
// or
test().then(console.log); // Logs 'Hello'
```
</details>

---

### Question 6
Which is faster for independent operations?

```javascript
// Option A
const a = await fetch('/api/1');
const b = await fetch('/api/2');

// Option B
const [a, b] = await Promise.all([
  fetch('/api/1'),
  fetch('/api/2')
]);
```

- A) Option A
- B) Option B
- C) They're the same speed
- D) It depends on the server

<details>
<summary>Show Answer</summary>

**✅ Answer: B) Option B**

**Explanation:** Option A runs sequentially (waits for first to finish before starting second). Option B runs in parallel (both start immediately). Option B is roughly twice as fast.
</details>

---

### Question 7
What should you always check after calling `fetch()`?

- A) `response.status`
- B) `response.ok`
- C) `response.headers`
- D) `response.body`

<details>
<summary>Show Answer</summary>

**✅ Answer: B) response.ok**

**Explanation:** `response.ok` is `true` for successful status codes (200-299) and `false` for errors. It's the simplest way to check if the request succeeded.

```javascript
if (!response.ok) {
  throw new Error(`HTTP ${response.status}`);
}
```
</details>

---

### Question 8
What happens if you catch an error but don't re-throw it?

- A) The error propagates automatically
- B) The promise chain continues with `undefined`
- C) The application crashes
- D) Nothing, errors are always silent

<details>
<summary>Show Answer</summary>

**✅ Answer: B) The promise chain continues with undefined**

**Explanation:** When you catch an error without re-throwing, the promise resolves successfully with whatever you return (or `undefined` if you return nothing). This can hide errors from calling code.

```javascript
promise
  .catch(err => {
    console.log(err);
    // No throw or return = next .then() gets undefined
  })
  .then(data => console.log(data)); // undefined
```
</details>

---

### Question 9
Which pattern avoids "promise hell"?

- A) Nested `.then()` callbacks
- B) async/await or flat promise chains
- C) Using `.catch()` everywhere
- D) Mixing promises with callbacks

<details>
<summary>Show Answer</summary>

**✅ Answer: B) async/await or flat promise chains**

**Explanation:** async/await or properly flattened promise chains are much more readable than nested `.then()` calls. They avoid the "pyramid of doom" problem.
</details>

---

### Question 10
What's the purpose of try/catch in async functions?

- A) To make code run faster
- B) To handle promise rejections
- C) To create new promises
- D) To make functions synchronous

<details>
<summary>Show Answer</summary>

**✅ Answer: B) To handle promise rejections**

**Explanation:** try/catch in async functions catches promise rejections, allowing you to handle errors gracefully. Without it, unhandled rejections can crash Node.js applications.
</details>

---

### Question 11
Can you use `await` outside of an async function?

- A) Yes, always
- B) No, never
- C) Only at the top level in ES modules
- D) Only in Node.js

<details>
<summary>Show Answer</summary>

**✅ Answer: C) Only at the top level in ES modules**

**Explanation:** Modern JavaScript (ES2022+) allows top-level `await` in ES modules. Otherwise, `await` must be inside an async function.

```javascript
// In an ES module (.mjs or type="module")
const data = await fetch('/api/data'); // Valid top-level await
```
</details>

---

### Question 12
What does `Promise.race()` return?

- A) All promise results
- B) The result of the first promise to settle
- C) Only successful promises
- D) An array of all results

<details>
<summary>Show Answer</summary>

**✅ Answer: B) The result of the first promise to settle**

**Explanation:** `Promise.race()` returns as soon as the first promise settles (either resolves or rejects), ignoring the others.

```javascript
const fastest = await Promise.race([
  fetch('/server1/data'),
  fetch('/server2/data')
]); // Returns whichever responds first
```
</details>

---

### Question 13
Which is a blocking operation that should be avoided?

- A) `await fetch()`
- B) `fs.readFileSync()`
- C) `setTimeout()`
- D) `Promise.resolve()`

<details>
<summary>Show Answer</summary>

**✅ Answer: B) fs.readFileSync()**

**Explanation:** Synchronous methods like `readFileSync()` block the event loop, preventing other operations from running. Always use async versions (`fs.promises.readFile()`).
</details>

---

### Question 14
What's the best way to handle errors from multiple independent promises?

- A) Wrap each in its own try/catch
- B) Use Promise.all() with one catch
- C) Use Promise.allSettled()
- D) Ignore errors

<details>
<summary>Show Answer</summary>

**✅ Answer: C) Use Promise.allSettled()**

**Explanation:** `Promise.allSettled()` gives you results from all promises, including which succeeded and which failed, allowing you to handle each appropriately.
</details>

---

### Question 15
When should you use sequential `await` instead of `Promise.all()`?

- A) Never, Promise.all() is always better
- B) When operations depend on previous results
- C) When you want better performance
- D) When dealing with arrays

<details>
<summary>Show Answer</summary>

**✅ Answer: B) When operations depend on previous results**

**Explanation:** Use sequential `await` when each operation needs data from the previous one. Use `Promise.all()` when operations are independent.

```javascript
// Sequential (necessary):
const user = await fetchUser(id);
const posts = await fetchUserPosts(user.id); // Needs user

// Parallel (better):
const [user, settings] = await Promise.all([
  fetchUser(id),
  fetchSettings(id) // Independent
]);
```
</details>

---

## 7. REAL INTERVIEW QUESTIONS

### Interview Question 1: "Explain the difference between Promise.all() and Promise.race()"

**👶 Junior Answer:**
"Promise.all() waits for all promises to finish and Promise.race() returns the first one that finishes."

**👨‍💼 Senior Answer:**
"Both methods handle multiple promises, but with different strategies:

**Promise.all():**
- Waits for ALL promises to resolve
- Fails fast - if any promise rejects, the entire operation fails immediately
- Returns an array of results in the same order as input promises
- Use case: When you need all operations to succeed (e.g., fetching required data from multiple endpoints)

**Promise.race():**
- Returns as soon as the FIRST promise settles (resolve OR reject)
- Doesn't cancel other promises - they continue running
- Returns the value/error of the fastest promise
- Use case: Implementing timeouts, getting fastest response from multiple servers, or building fallback mechanisms

Example implementation:

```javascript
// Promise.all example: All must succeed
const [user, posts, comments] = await Promise.all([
  fetchUser(id),
  fetchPosts(id),
  fetchComments(id)
]);
// If ANY fails, entire operation fails

// Promise.race example: Timeout pattern
const dataOrTimeout = await Promise.race([
  fetch('/api/data'),
  new Promise((_, reject) => 
    setTimeout(() => reject(new Error('Timeout')), 5000)
  )
]);
```

Additionally, there's Promise.any() which returns the first successful result (ignoring rejections) and Promise.allSettled() which never fails and returns status of all promises."

**🎯 Key Interview Points:**
- Mention fail-fast behavior of Promise.all()
- Give real-world use cases
- Bonus: Mention Promise.any() and Promise.allSettled()

---

### Interview Question 2: "How do you handle errors in async/await vs Promise chains?"

**👶 Junior Answer:**
"You use try/catch for async/await and .catch() for promises."

**👨‍💼 Senior Answer:**
"Both patterns handle errors, but with different syntax and implications:

**async/await (try/catch):**
```javascript
async function fetchUserData(id) {
  try {
    const response = await fetch(`/api/users/${id}`);
    
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }
    
    const user = await response.json();
    return user;
    
  } catch (error) {
    // Can distinguish error types
    if (error.message.includes('HTTP')) {
      logger.error('API error:', error);
    } else {
      logger.error('Network error:', error);
    }
    throw error; // Re-throw for caller to handle
  }
}
```

**Promise chains (.catch()):**
```javascript
function fetchUserData(id) {
  return fetch(`/api/users/${id}`)
    .then(response => {
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}`);
      }
      return response.json();
    })
    .catch(error => {
      logger.error('Fetch failed:', error);
      throw error; // Re-throw
    });
}
```

**Key Differences:**
1. **Readability:** async/await is more readable for sequential operations
2. **Error scope:** try/catch wraps multiple operations; .catch() can be placed at different chain points
3. **Mixing:** Can use both - async functions can return promises with .catch()

**Best Practices:**
- Always check `response.ok` - fetch doesn't reject on HTTP errors
- Re-throw errors after logging unless you can fully recover
- Use specific error types for different failure scenarios
- Consider a global error handler for unhandled rejections:

```javascript
// Node.js
process.on('unhandledRejection', (reason, promise) => {
  logger.error('Unhandled Rejection:', reason);
});

// Browser
window.addEventListener('unhandledrejection', event => {
  logger.error('Unhandled Rejection:', event.reason);
});
```
"

**🎯 Key Interview Points:**
- Show both patterns with proper error handling
- Mention response.ok check
- Discuss re-throwing vs handling
- Bonus: Global error handlers

---

### Interview Question 3: "Why doesn't forEach work with async/await? How would you fix it?"

**👶 Junior Answer:**
"forEach doesn't wait for async functions. You should use a for loop instead."

**👨‍💼 Senior Answer:**
"The issue stems from how `forEach` handles callbacks. It doesn't await promises returned from async callbacks, making it impossible to know when all operations complete.

**Why it fails:**
```javascript
async function processItems(items) {
  items.forEach(async (item) => {
    await processItem(item); // forEach doesn't wait
  });
  console.log('Done'); // Logs immediately, before processing
}
```

**The Problem:**
- `forEach` executes callbacks synchronously
- It doesn't collect or wait for returned promises
- Async callbacks return promises that `forEach` ignores

**Solutions based on requirements:**

**1. Sequential Processing (one at a time):**
```javascript
async function processItemsSequential(items) {
  for (const item of items) {
    await processItem(item);
  }
  console.log('Done'); // Truly done
}
```

**2. Parallel Processing (all at once):**
```javascript
async function processItemsParallel(items) {
  await Promise.all(
    items.map(item => processItem(item))
  );
  console.log('Done'); // All completed
}
```

**3. Parallel with Error Handling:**
```javascript
async function processItemsSafe(items) {
  const results = await Promise.allSettled(
    items.map(item => 
      processItem(item).catch(err => ({
        error: err,
        item
      }))
    )
  );
  
  const successful = results.filter(r => r.status === 'fulfilled');
  const failed = results.filter(r => r.status === 'rejected');
  
  console.log(`Processed: ${successful.length}, Failed: ${failed.length}`);
  return { successful, failed };
}
```

**4. Rate-Limited Processing (batch processing):**
```javascript
async function processItemsBatched(items, batchSize = 5) {
  for (let i = 0; i < items.length; i += batchSize) {
    const batch = items.slice(i, i + batchSize);
    await Promise.all(batch.map(item => processItem(item)));
    console.log(`Processed ${Math.min(i + batchSize, items.length)}/${items.length}`);
  }
}
```

**Performance Comparison:**
- Sequential: Slowest, but predictable order and memory usage
- Parallel: Fastest, but can overwhelm resources
- Batched: Balanced approach for large datasets

The choice depends on whether operations must be ordered, if resources are constrained, and if individual failures should stop the entire process."

**🎯 Key Interview Points:**
- Explain WHY forEach fails (doesn't handle promises)
- Show multiple solutions (sequential, parallel, batched)
- Discuss trade-offs (performance vs resource usage)
- Bonus: Mention error handling strategies

---

### Interview Question 4: "What is a race condition in async code and how do you prevent it?"

**👶 Junior Answer:**
"A race condition is when two async operations try to change the same data at the same time. You fix it by using await."

**👨‍💼 Senior Answer:**
"A race condition occurs when multiple async operations access shared state, and the final result depends on unpredictable timing. This is especially dangerous in JavaScript despite being single-threaded, because of the event loop.

**Classic Example:**
```javascript
let counter = 0;

async function incrementCounter() {
  const current = counter; // Read
  await someAsyncOperation(); // Other code can run here!
  counter = current + 1; // Write
}

// Race condition:
Promise.all([
  incrementCounter(), // Reads 0
  incrementCounter(), // Also reads 0
  incrementCounter()  // Also reads 0
]);
// All write back 1, but we expected 3
```

**Prevention Strategies:**

**1. Sequential Execution:**
```javascript
// Force operations to run one at a time
async function safeIncrement() {
  for (let i = 0; i < 3; i++) {
    await incrementCounter();
  }
}
```

**2. Mutex/Lock Pattern:**
```javascript
class Mutex {
  constructor() {
    this.queue = Promise.resolve();
  }

  async lock(fn) {
    this.queue = this.queue.then(fn);
    return this.queue;
  }
}

const mutex = new Mutex();

async function safeIncrement() {
  await mutex.lock(async () => {
    const current = counter;
    await someAsyncOperation();
    counter = current + 1;
  });
}
```

**3. Atomic State Updates:**
```javascript
// Use functional updates that don't rely on timing
class Counter {
  constructor() {
    this.value = 0;
  }

  async increment() {
    await someAsyncOperation();
    this.value++; // Atomic in JavaScript's single thread
  }
}
```

**4. Immutable State:**
```javascript
// Return new state instead of mutating
async function increment(currentState) {
  await someAsyncOperation();
  return { ...currentState, counter: currentState.counter + 1 };
}
```

**Real-World Examples:**

**Database Race Condition:**
```javascript
// Bad: Read-modify-write race
async function addToCart(userId, productId) {
  const cart = await db.getCart(userId);
  cart.items.push(productId);
  await db.saveCart(cart); // Another request might have modified cart!
}

// Good: Atomic database operation
async function addToCart(userId, productId) {
  await db.atomicUpdate('carts', userId, {
    $push: { items: productId }
  });
}
```

**UI State Race Condition:**
```javascript
// Bad: Multiple clicks cause multiple requests
button.addEventListener('click', async () => {
  const data = await fetchData(); // User clicks again before this completes!
  updateUI(data);
});

// Good: Disable during operation
button.addEventListener('click', async () => {
  button.disabled = true;
  try {
    const data = await fetchData();
    updateUI(data);
  } finally {
    button.disabled = false;
  }
});
```

**Key Prevention Principles:**
1. Minimize shared mutable state
2. Use locks/mutexes for critical sections
3. Make operations atomic when possible
4. Use immutable data structures
5. Implement proper debouncing/throttling for user actions"

**🎯 Key Interview Points:**
- Explain with clear example showing the problem
- Show multiple prevention strategies
- Give real-world scenarios (database, UI)
- Discuss trade-offs between approaches

---


## 8. CHAPTER SUMMARY & NEXT STEPS

### Quick Recap

**The 6 Critical Mistakes:**

1. **Sequential vs Parallel** - Use `Promise.all()` when operations are independent
2. **Promise Chaining** - Avoid nesting, keep chains flat, or use async/await
3. **Error Handling** - Always handle rejections, check `response.ok`, re-throw with context
4. **Missing Callbacks** - Return promises from functions, await all async operations
5. **Blocking Operations** - Never use synchronous APIs in async code
6. **Try/Catch Abuse** - Use try/catch strategically, not everywhere

**Golden Rules:**
- ✅ Use `for...of` for sequential, `Promise.all()` for parallel
- ✅ Always check `response.ok` after fetch()
- ✅ Re-throw errors after logging (unless you can fully recover)
- ✅ Return promises from functions
- ✅ Use async versions of Node.js APIs
- ✅ One try/catch per logical operation group

---

### What You've Mastered

By completing this workbook, you can now:
- Identify and fix the 6 most common async mistakes
- Write performant parallel async code
- Implement robust error handling strategies
- Debug complex async issues using DevTools
- Handle race conditions and memory leaks
- Write production-quality async code

---

### Connection to Next Topic

You've learned how to write async code correctly, but **how does it actually work under the hood?**

**Day 27 Preview: "How Your Async Code Works | JavaScript Event Loop Simplified!"**

You'll discover:
- How the Event Loop processes your async code
- What microtasks and macrotasks are
- Why `console.log` order can be surprising
- How `await` actually pauses function execution
- The magic behind Promises and async/await

**Teaser Question for Day 27:**
```javascript
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
console.log('4');
```
What's the output and WHY? You'll understand this completely after Day 27! 

---

### Ready for Day 27?

Before moving forward, make sure you can:
- [ ] Explain when to use sequential vs parallel execution
- [ ] Write clean promise chains without nesting
- [ ] Handle errors properly with try/catch and .catch()
- [ ] Debug async code using Chrome DevTools
- [ ] Identify and fix race conditions

**You're doing great! The Event Loop awaits... 🚀**

---

<div align="center">



**🎯 Next:** Day 27 - How Your Async Code Works | JavaScript Event Loop Simplified!

</div>