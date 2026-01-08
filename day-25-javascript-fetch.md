# Day 25: JavaScript fetch() Explained Like Never Before


## 1. CHAPTER OVERVIEW

### 📺 In the Video
You learned how the `fetch()` API is the modern way to make HTTP requests in JavaScript, replacing the old XMLHttpRequest. We covered all HTTP methods (GET, POST, PUT, PATCH, DELETE), explored request/response handling, worked with query parameters using URLSearchParams, and mastered error handling patterns with async/await.

### ✅ Learning Checklist
By the end of this workbook, you will be able to:
- ✓ Make HTTP requests using fetch() with async/await syntax
- ✓ Implement all CRUD operations (GET, POST, PUT, PATCH, DELETE)
- ✓ Build dynamic URLs with query parameters using URLSearchParams
- ✓ Handle responses correctly and check for HTTP errors
- ✓ Implement proper error handling for network and HTTP errors
- ✓ Cancel ongoing requests using AbortController
- ✓ Work with custom headers for authentication and content types

### 📋 Prerequisites
From **Day 24 - Async/Await**, you should know:
- How async/await works with Promises
- The try-catch pattern for error handling
- Sequential vs parallel async operations
- How await pauses execution until Promise resolves
- Converting Promise chains to async/await syntax

---

## 2. LECTURE CHEAT SHEET

### 🎯 Core fetch() Syntax

```javascript
// Basic Pattern
const response = await fetch(url, options);
const data = await response.json();

// With Error Handling
try {
    const response = await fetch(url, options);
    
    if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const data = await response.json();
} catch (error) {
    console.error('Fetch failed:', error);
}
```

### 📊 HTTP Methods Quick Reference

```
┌─────────────────────────────────────────────────────────┐
│                   HTTP METHOD FLOW                      │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  GET     → Retrieve data (READ)                        │
│  POST    → Create new resource (CREATE)                │
│  PUT     → Replace entire resource (FULL UPDATE)       │
│  PATCH   → Update specific fields (PARTIAL UPDATE)     │
│  DELETE  → Remove resource (DELETE)                    │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 🔑 Key Terminology

| Term | Definition | Example |
|------|------------|---------|
| **fetch()** | Browser API for making HTTP requests | `fetch('https://api.com/data')` |
| **Response Object** | Object returned by fetch containing status, headers, body | `response.ok`, `response.status` |
| **response.ok** | Boolean indicating HTTP status 200-299 | `if (!response.ok) throw Error()` |
| **response.json()** | Parses response body as JSON | `await response.json()` |
| **Query Parameters** | URL parameters for filtering/sorting | `?userId=1&sort=desc` |
| **URLSearchParams** | API for building query strings | `new URLSearchParams({page: 1})` |
| **AbortController** | Interface to cancel fetch requests | `controller.abort()` |
| **Headers** | Metadata sent with request/response | `{'Content-Type': 'application/json'}` |

### 📋 HTTP Methods Comparison Table

| Method | Purpose | Body? | Idempotent? | Safe? | Use Case |
|--------|---------|-------|-------------|-------|----------|
| **GET** | Retrieve data | ❌ No | ✅ Yes | ✅ Yes | Fetch user profile |
| **POST** | Create resource | ✅ Yes | ❌ No | ❌ No | Submit form data |
| **PUT** | Replace resource | ✅ Yes | ✅ Yes | ❌ No | Update entire user profile |
| **PATCH** | Partial update | ✅ Yes | ⚠️ Maybe | ❌ No | Update email only |
| **DELETE** | Remove resource | ⚠️ Optional | ✅ Yes | ❌ No | Delete account |

**Key:**
- **Idempotent**: Same request multiple times = same result
- **Safe**: Doesn't modify server state

### 🎨 fetch() Options Object

```javascript
const options = {
    method: 'POST',              // HTTP method
    headers: {                   // Request headers
        'Content-Type': 'application/json',
        'Authorization': 'Bearer token123'
    },
    body: JSON.stringify(data),  // Request body (string)
    signal: controller.signal,   // AbortController signal
    mode: 'cors',                // CORS mode
    credentials: 'same-origin'   // Include cookies?
};
```

### 🔄 Response Properties & Methods

| Property/Method | Type | Description |
|----------------|------|-------------|
| `response.ok` | Boolean | True if status 200-299 |
| `response.status` | Number | HTTP status code (200, 404, etc.) |
| `response.statusText` | String | Status message ("OK", "Not Found") |
| `response.headers` | Headers | Response headers object |
| `response.json()` | Promise | Parse response as JSON |
| `response.text()` | Promise | Get response as text |
| `response.blob()` | Promise | Get binary data |

### 🌐 Query Parameters Pattern

```javascript
// Manual (NOT recommended)
const url = `${baseURL}?userId=1&sort=desc`;

// ✅ Using URLSearchParams (RECOMMENDED)
const params = new URLSearchParams({
    userId: 1,
    sort: 'desc',
    limit: 10
});
const url = `${baseURL}?${params.toString()}`;
// Result: baseURL?userId=1&sort=desc&limit=10
```

### 📊 URLSearchParams Methods

| Method | Purpose | Example |
|--------|---------|---------|
| `.get(key)` | Get single value | `params.get('userId')` → `"1"` |
| `.set(key, val)` | Set/update value | `params.set('page', 2)` |
| `.append(key, val)` | Add duplicate key | `params.append('tag', 'js')` |
| `.delete(key)` | Remove parameter | `params.delete('sort')` |
| `.has(key)` | Check if exists | `params.has('userId')` → `true` |
| `.toString()` | Convert to string | `params.toString()` → `"userId=1&sort=desc"` |

### 🚨 Critical fetch() Behavior

```javascript
// ❌ IMPORTANT: fetch() DOES NOT reject on HTTP errors!
try {
    const response = await fetch('/api/data');
    const data = await response.json(); // ❌ Might fail if 404!
} catch (error) {
    // Only catches NETWORK errors, not HTTP errors!
}

// ✅ CORRECT: Always check response.ok
try {
    const response = await fetch('/api/data');
    
    if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }
    
    const data = await response.json();
} catch (error) {
    // Catches both network AND HTTP errors
}
```

### 🎯 Common Status Codes

| Code | Meaning | Action |
|------|---------|--------|
| 200 | OK | Success - process data |
| 201 | Created | Resource created successfully |
| 204 | No Content | Success but no body to return |
| 400 | Bad Request | Check request data |
| 401 | Unauthorized | Authentication required |
| 403 | Forbidden | No permission |
| 404 | Not Found | Resource doesn't exist |
| 500 | Server Error | Server-side issue |

### 🔄 PUT vs PATCH Visual

```
User Profile: { name: "John", email: "john@email.com", age: 25 }

PUT /users/1                      PATCH /users/1
Body: {                          Body: {
  name: "John",                    age: 26
  email: "john@email.com",       }
  age: 26                        
}                                Result:
                                 { name: "John",    ← Kept
Result:                            email: "john@...", ← Kept
{ name: "John",                    age: 26 }         ← Updated
  email: "john@email.com",
  age: 26 }
  
If you forget email in PUT:      PATCH only updates
{ name: "John",                  what you send,
  age: 26 }  ← email LOST!       keeping the rest.
```

### ⚡ AbortController Pattern

```javascript
// Create controller
const controller = new AbortController();

// Pass signal to fetch
fetch(url, { signal: controller.signal })
    .then(response => response.json())
    .catch(error => {
        if (error.name === 'AbortError') {
            console.log('Fetch cancelled');
        }
    });

// Cancel the request
controller.abort();
```

---

## 3. PREDICT THE OUTPUT

### 🧠 Test Your Understanding

**Instructions:** Read each code snippet carefully. Think about HTTP behavior, async/await execution, and response handling before checking the answer.

---

#### **Snippet 1: Basic GET Request**
```javascript
async function test() {
    const response = await fetch('https://jsonplaceholder.typicode.com/users/1');
    console.log(response);
    const data = await response.json();
    console.log(data.name);
}
test();
```

**🤔 Think:** What gets logged first? What type is `response`?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
Response { type: "cors", url: "...", status: 200, ok: true, ... }
Leanne Graham
```

**Why:** 
- First `console.log` prints the Response object (not the data)
- Second `console.log` prints the parsed JSON data's name property
- Response object contains metadata (status, headers, etc.)
</details>

---

#### **Snippet 2: Forgetting await**
```javascript
async function test() {
    const response = fetch('https://jsonplaceholder.typicode.com/users/1');
    console.log(response);
    const data = response.json();
    console.log(data);
}
test();
```

**🤔 Think:** What happens when you forget `await`?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
Promise { <pending> }
Promise { <pending> }
```

**Why:** 
- Without `await`, `fetch()` returns a pending Promise, not the Response
- Calling `.json()` on a Promise (instead of Response) returns another Promise
- Both logs show Promise objects, not the actual data
</details>

---

#### **Snippet 3: Response.ok Check**
```javascript
async function test() {
    const response = await fetch('https://jsonplaceholder.typicode.com/users/999');
    console.log('Status:', response.status);
    console.log('OK:', response.ok);
    const data = await response.json();
    console.log('Data:', data);
}
test();
```

**🤔 Think:** What happens when the resource doesn't exist (404)?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
Status: 404
OK: false
Data: {}
```

**Why:** 
- fetch() doesn't throw an error for 404 (only network errors)
- `response.status` is 404
- `response.ok` is false (only true for 200-299)
- The API still returns a response body (empty object)
- **Critical:** No error was thrown - you must check `response.ok` manually!
</details>

---

#### **Snippet 4: Query Parameters**
```javascript
async function test() {
    const params = new URLSearchParams({ userId: 1, _limit: 2 });
    const url = `https://jsonplaceholder.typicode.com/posts?${params}`;
    console.log(url);
    
    const response = await fetch(url);
    const data = await response.json();
    console.log(data.length);
}
test();
```

**🤔 Think:** How does URLSearchParams affect the URL and results?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
https://jsonplaceholder.typicode.com/posts?userId=1&_limit=2
2
```

**Why:** 
- URLSearchParams automatically converts object to query string
- Template literal coerces params object to string (calls `.toString()`)
- Query parameters filter results: only posts from userId=1, limited to 2
- Final array has 2 items
</details>

---

#### **Snippet 5: POST Request Body**
```javascript
async function test() {
    const postData = { title: 'Test', body: 'Content', userId: 1 };
    
    const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
        method: 'POST',
        body: postData
    });
    
    const result = await response.json();
    console.log(result);
}
test();
```

**🤔 Think:** What's wrong with this POST request?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
{}  (or error depending on API)
```

**Why:** 
- **Bug:** `body` must be a string, not an object
- Should use `JSON.stringify(postData)`
- Missing `Content-Type` header
- API might return empty object or error

**Correct version:**
```javascript
body: JSON.stringify(postData),
headers: { 'Content-Type': 'application/json' }
```
</details>

---

#### **Snippet 6: Error Handling**
```javascript
async function test() {
    try {
        const response = await fetch('https://jsonplaceholder.typicode.com/invalid');
        const data = await response.json();
        console.log('Success:', data);
    } catch (error) {
        console.log('Error caught:', error.message);
    }
}
test();
```

**🤔 Think:** Will the catch block execute for a 404 response?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
Success: {}
```

**Why:** 
- **Surprise:** catch block does NOT execute!
- fetch() only rejects on NETWORK errors (no internet, DNS failure)
- 404 is a valid HTTP response - Promise resolves successfully
- To catch HTTP errors, you must check `response.ok` and throw manually
</details>

---

#### **Snippet 7: Parallel Requests**
```javascript
async function test() {
    console.log('Start');
    
    const user = fetch('https://jsonplaceholder.typicode.com/users/1')
        .then(r => r.json());
    const posts = fetch('https://jsonplaceholder.typicode.com/posts?userId=1')
        .then(r => r.json());
    
    const [userData, postsData] = await Promise.all([user, posts]);
    console.log('User:', userData.name);
    console.log('Posts:', postsData.length);
}
test();
```

**🤔 Think:** How do the requests execute - sequential or parallel?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
Start
User: Leanne Graham
Posts: 10
```

**Why:** 
- Both fetch calls start immediately (parallel execution)
- `Promise.all()` waits for both to complete
- Faster than awaiting each request sequentially
- Destructuring assigns results to variables
</details>

---

#### **Snippet 8: URLSearchParams Mutation**
```javascript
const params = new URLSearchParams();
params.append('tag', 'javascript');
params.append('tag', 'react');
params.set('page', 1);
console.log(params.toString());

params.set('tag', 'vue');
console.log(params.toString());
```

**🤔 Think:** How do `.append()` and `.set()` differ?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
tag=javascript&tag=react&page=1
tag=vue&page=1
```

**Why:** 
- `.append()` adds another value (allows duplicates)
- First output has two `tag` parameters
- `.set()` replaces ALL existing values for that key
- Second output has only one `tag` (both previous ones replaced)
</details>

---

#### **Snippet 9: AbortController Timing**
```javascript
async function test() {
    const controller = new AbortController();
    
    setTimeout(() => {
        controller.abort();
        console.log('Aborted');
    }, 100);
    
    try {
        const response = await fetch(
            'https://jsonplaceholder.typicode.com/users',
            { signal: controller.signal }
        );
        const data = await response.json();
        console.log('Data received:', data.length);
    } catch (error) {
        console.log('Error:', error.name);
    }
}
test();
```

**🤔 Think:** What happens if abort is called during the fetch?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:** (can vary based on network speed)
```
Aborted
Error: AbortError
```
**OR**
```
Data received: 10
Aborted
```

**Why:** 
- Race condition: depends on whether fetch completes before 100ms
- If fetch is slow: abort wins → throws AbortError
- If fetch is fast: completes before abort → data is logged
- AbortError has specific `name` property for identification
</details>

---

#### **Snippet 10: Response Parsing**
```javascript
async function test() {
    const response = await fetch('https://jsonplaceholder.typicode.com/users/1');
    
    const data1 = await response.json();
    console.log(data1.name);
    
    const data2 = await response.json();
    console.log(data2.name);
}
test();
```

**🤔 Think:** Can you call `.json()` twice on the same response?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
Leanne Graham
(Error: Failed to execute 'json' on 'Response': body stream already read)
```

**Why:** 
- Response body is a stream that can only be read ONCE
- First `.json()` consumes the stream
- Second `.json()` fails because stream is already consumed
- If you need data multiple times, store the result in a variable
</details>

---

#### **Snippet 11: PUT vs PATCH Behavior**
```javascript
async function test() {
    // Original data: { userId: 1, id: 1, title: "...", body: "..." }
    
    const patchResponse = await fetch('https://jsonplaceholder.typicode.com/posts/1', {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ title: 'Updated Title' })
    });
    
    const patchData = await patchResponse.json();
    console.log('PATCH - userId exists:', 'userId' in patchData);
    console.log('PATCH - title:', patchData.title);
}
test();
```

**🤔 Think:** Does PATCH preserve untouched fields?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
PATCH - userId exists: true
PATCH - title: Updated Title
```

**Why:** 
- PATCH updates only specified fields
- Other fields (userId, id, body) remain unchanged
- Server merges the update with existing data
- If this were PUT, you'd need to send ALL fields or they'd be lost
</details>

---

#### **Snippet 12: Status Code Handling**
```javascript
async function test() {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts/1');
    
    switch(response.status) {
        case 200:
            console.log('Success');
            break;
        case 201:
            console.log('Created');
            break;
        case 404:
            console.log('Not Found');
            break;
        default:
            console.log('Other:', response.status);
    }
    
    console.log('OK:', response.ok);
}
test();
```

**🤔 Think:** What's the relationship between status and ok?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
Success
OK: true
```

**Why:** 
- GET request to existing resource returns 200
- `response.ok` is true for status 200-299
- Status 200 specifically means "OK"
- Other success codes: 201 (Created), 204 (No Content)
</details>

---

#### **Snippet 13: Query Parameter Encoding**
```javascript
const search = 'hello world';
const params = new URLSearchParams({ q: search });
console.log(params.toString());

const manual = `q=${search}`;
console.log(manual);
```

**🤔 Think:** Why use URLSearchParams instead of template literals?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
q=hello+world
q=hello world
```

**Why:** 
- URLSearchParams automatically URL-encodes special characters
- Space becomes `+` (or `%20`)
- Manual string has unencoded space (invalid in URLs)
- Always use URLSearchParams or `encodeURIComponent()` for safety
</details>

---

#### **Snippet 14: DELETE Response**
```javascript
async function test() {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts/1', {
        method: 'DELETE'
    });
    
    console.log('Status:', response.status);
    console.log('OK:', response.ok);
    
    if (response.status === 204) {
        console.log('Deleted (no content)');
    } else {
        const data = await response.json();
        console.log('Response:', data);
    }
}
test();
```

**🤔 Think:** What status codes indicate successful deletion?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
Status: 200
OK: true
Response: {}
```

**Why:** 
- JSONPlaceholder returns 200 with empty object
- Real APIs often return 204 (No Content) with no body
- Both 200 and 204 indicate successful deletion
- Check status code to know if body is available
</details>

---

#### **Snippet 15: Async Function Return**
```javascript
async function fetchUser() {
    const response = await fetch('https://jsonplaceholder.typicode.com/users/1');
    return response.json();
}

const result = fetchUser();
console.log(result);

result.then(data => console.log(data.name));
```

**🤔 Think:** What does an async function return?

<details>
<summary><strong>✅ Answer</strong></summary>

**Output:**
```
Promise { <pending> }
Leanne Graham
```

**Why:** 
- Async functions ALWAYS return a Promise
- Even though we return `response.json()`, it's wrapped in a Promise
- First log shows pending Promise
- Must use `.then()` or `await` to access the data
</details>

---

## 4. GUIDED PRACTICE PROBLEMS

### 🔥 Problem 1: WARM-UP - Basic User Fetcher

**Difficulty:** ⭐ Easy  
**Concepts:** GET request, async/await, response.json()

#### 📋 Task Description
Create a function that fetches a single user from the JSONPlaceholder API and displays their name, email, and username in the console. Handle the case where the user doesn't exist (404).

#### 🎯 Requirements
1. Create an async function `fetchUserDetails(userId)`
2. Fetch from `https://jsonplaceholder.typicode.com/users/${userId}`
3. Check if response is successful
4. Parse JSON and log user details
5. Handle errors gracefully

#### 💻 Starter Code
```javascript
// Complete this function
async function fetchUserDetails(userId) {
    // Your code here
}

// Test it
fetchUserDetails(1);  // Should show user 1 details
fetchUserDetails(999); // Should handle non-existent user
```

#### ✅ Solution
```javascript
async function fetchUserDetails(userId) {
    const API_URL = `https://jsonplaceholder.typicode.com/users/${userId}`;
    
    try {
        const response = await fetch(API_URL);
        
        // Check if request was successful
        if (!response.ok) {
            throw new Error(`User not found! Status: ${response.status}`);
        }
        
        const user = await response.json();
        
        // Display user details
        console.log('=== User Details ===');
        console.log('Name:', user.name);
        console.log('Email:', user.email);
        console.log('Username:', user.username);
        
        return user;
        
    } catch (error) {
        console.error('❌ Error fetching user:', error.message);
        return null;
    }
}

// Test cases
fetchUserDetails(1);   // ✅ Valid user
fetchUserDetails(999); // ❌ Invalid user
```

#### ⚠️ Common Mistakes
1. **Forgetting to check response.ok**
   ```javascript
   // ❌ WRONG - doesn't handle 404
   const user = await response.json();
   
   // ✅ CORRECT
   if (!response.ok) {
       throw new Error(`HTTP error! status: ${response.status}`);
   }
   const user = await response.json();
   ```

2. **Not using try-catch**
   ```javascript
   // ❌ WRONG - unhandled errors crash the app
   const response = await fetch(url);
   const user = await response.json();
   
   // ✅ CORRECT
   try {
       const response = await fetch(url);
       const user = await response.json();
   } catch (error) {
       console.error('Error:', error);
   }
   ```

3. **Forgetting await on response.json()**
   ```javascript
   // ❌ WRONG - returns Promise, not data
   const user = response.json();
   
   // ✅ CORRECT
   const user = await response.json();
   ```

---

### 🔥 Problem 2: THE LOGIC BUILDER - Post Manager with Filters

**Difficulty:** ⭐⭐ Medium  
**Concepts:** Query parameters, URLSearchParams, filtering, multiple conditions

#### 📋 Task Description
Build a post manager that can fetch posts with various filters: by user ID, limit results, and sort by title. Use URLSearchParams to build clean query strings dynamically.

#### 🎯 Requirements
1. Create function `fetchFilteredPosts(options)` that accepts filter object
2. Support filters: `userId`, `_limit`, `_sort`, `_order`
3. Use URLSearchParams to build query string
4. Log how many posts were found
5. Return the filtered posts array

#### 💻 Starter Code
```javascript
async function fetchFilteredPosts(options = {}) {
    // Build URL with query parameters
    // Fetch and return filtered posts
}

// Test cases
fetchFilteredPosts({ userId: 1, _limit: 5 });
fetchFilteredPosts({ _sort: 'title', _order: 'asc', _limit: 3 });
fetchFilteredPosts({ userId: 2 });
```

#### ✅ Solution
```javascript
async function fetchFilteredPosts(options = {}) {
    const BASE_URL = 'https://jsonplaceholder.typicode.com/posts';
    
    try {
        // Build query string from options
        const params = new URLSearchParams();
        
        // Add each filter if provided
        if (options.userId) params.set('userId', options.userId);
        if (options._limit) params.set('_limit', options._limit);
        if (options._sort) params.set('_sort', options._sort);
        if (options._order) params.set('_order', options._order);
        
        // Construct full URL
        const queryString = params.toString();
        const url = queryString ? `${BASE_URL}?${queryString}` : BASE_URL;
        
        console.log('📡 Fetching from:', url);
        
        // Fetch data
        const response = await fetch(url);
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const posts = await response.json();
        
        // Display results
        console.log(`✅ Found ${posts.length} posts`);
        console.log('First post title:', posts[0]?.title || 'N/A');
        
        return posts;
        
    } catch (error) {
        console.error('❌ Error fetching posts:', error.message);
        return [];
    }
}

// Test cases with different filters
console.log('\n--- Test 1: User 1, limit 5 ---');
await fetchFilteredPosts({ userId: 1, _limit: 5 });

console.log('\n--- Test 2: Sorted by title, first 3 ---');
await fetchFilteredPosts({ _sort: 'title', _order: 'asc', _limit: 3 });

console.log('\n--- Test 3: All posts from user 2 ---');
await fetchFilteredPosts({ userId: 2 });

console.log('\n--- Test 4: No filters (all posts) ---');
const allPosts = await fetchFilteredPosts();
console.log('Total posts:', allPosts.length);
```

#### ⚠️ Common Mistakes
1. **Building query string manually**
   ```javascript
   // ❌ WRONG - error-prone, doesn't encode properly
   const url = `${BASE_URL}?userId=${options.userId}&limit=${options.limit}`;
   
   // ✅ CORRECT - handles encoding and missing values
   const params = new URLSearchParams(options);
   const url = `${BASE_URL}?${params.toString()}`;
   ```

2. **Not handling missing parameters**
   ```javascript
   // ❌ WRONG - creates invalid query string
   const url = `${BASE_URL}?userId=${options.userId}`;
   // If userId is undefined: "?userId=undefined"
   
   // ✅ CORRECT - check before adding
   const params = new URLSearchParams();
   if (options.userId) params.set('userId', options.userId);
   ```

3. **Forgetting optional chaining**
   ```javascript
   // ❌ WRONG - crashes if posts array is empty
   console.log('First post:', posts[0].title);
   
   // ✅ CORRECT - safe access
   console.log('First post:', posts[0]?.title || 'N/A');
   ```

---

### 🔥 Problem 3: THE PRO CHALLENGE - Complete CRUD Blog Manager

**Difficulty:** ⭐⭐⭐ Hard  
**Concepts:** All HTTP methods, error handling, request cancellation, real-world patterns

#### 📋 Task Description
Build a complete blog post manager with create, read, update, and delete functionality. Include proper error handling, loading states, and request cancellation for search functionality.

#### 🎯 Requirements
1. Create a `BlogManager` class with methods for all CRUD operations
2. Implement: `getAllPosts()`, `getPost(id)`, `createPost(data)`, `updatePost(id, data)`, `patchPost(id, data)`, `deletePost(id)`
3. Add `searchPosts(query)` with AbortController to cancel previous searches
4. Handle all errors with specific error messages
5. Track loading state and display appropriate messages

#### 💻 Starter Code
```javascript
class BlogManager {
    constructor(baseURL) {
        this.baseURL = baseURL;
        this.searchController = null;
    }
    
    // Implement CRUD methods here
}

// Usage
const blog = new BlogManager('https://jsonplaceholder.typicode.com/posts');
```

#### ✅ Solution
```javascript
class BlogManager {
    constructor(baseURL) {
        this.baseURL = baseURL;
        this.searchController = null;
    }
    
    // Helper method for fetch with error handling
    async _fetch(url, options = {}) {
        try {
            const response = await fetch(url, options);
            
            if (!response.ok) {
                const errorMessages = {
                    400: 'Bad Request - Check your data',
                    401: 'Unauthorized - Authentication required',
                    403: 'Forbidden - Access denied',
                    404: 'Not Found - Resource doesn\'t exist',
                    500: 'Server Error - Try again later'
                };
                
                const message = errorMessages[response.status] || 
                               `HTTP Error ${response.status}`;
                throw new Error(message);
            }
            
            // Handle 204 No Content
            if (response.status === 204) {
                return { success: true, message: 'Operation completed' };
            }
            
            return await response.json();
            
        } catch (error) {
            if (error.name === 'AbortError') {
                console.log('Request cancelled');
                return null;
            }
            throw error;
        }
    }
    
    // READ - Get all posts
    async getAllPosts(limit = 10) {
        console.log(`📖 Fetching ${limit} posts...`);
        
        const params = new URLSearchParams({ _limit: limit });
        const url = `${this.baseURL}?${params.toString()}`;
        
        const posts = await this._fetch(url);
        console.log(`✅ Retrieved ${posts.length} posts`);
        return posts;
    }
    
    // READ - Get single post
    async getPost(id) {
        console.log(`📖 Fetching post #${id}...`);
        
        const post = await this._fetch(`${this.baseURL}/${id}`);
        console.log(`✅ Retrieved: "${post.title}"`);
        return post;
    }
    
    // CREATE - New post
    async createPost(postData) {
        console.log('✏️ Creating new post...');
        
        const post = await this._fetch(this.baseURL, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(postData)
        });
        
        console.log(`✅ Created post #${post.id}: "${post.title}"`);
        return post;
    }
    
    // UPDATE - Replace entire post (PUT)
    async updatePost(id, postData) {
        console.log(`🔄 Updating post #${id} (full replacement)...`);
        
        const post = await this._fetch(`${this.baseURL}/${id}`, {
            method: 'PUT',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({...postData, id}) // Include ID
        });
        
        console.log(`✅ Updated post #${id}`);
        return post;
    }
    
    // UPDATE - Partial update (PATCH)
    async patchPost(id, updates) {
        console.log(`🩹 Patching post #${id}...`);
        
        const post = await this._fetch(`${this.baseURL}/${id}`, {
            method: 'PATCH',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(updates)
        });
        
        console.log(`✅ Patched post #${id}`);
        return post;
    }
    
    // DELETE - Remove post
    async deletePost(id) {
        console.log(`🗑️ Deleting post #${id}...`);
        
        const result = await this._fetch(`${this.baseURL}/${id}`, {
            method: 'DELETE'
        });
        
        console.log(`✅ Deleted post #${id}`);
        return result;
    }
    
    // SEARCH - With cancellation
    async searchPosts(query) {
        // Cancel previous search
        if (this.searchController) {
            this.searchController.abort();
        }
        
        // Create new controller
        this.searchController = new AbortController();
        
        console.log(`🔍 Searching for: "${query}"...`);
        
        try {
            const params = new URLSearchParams({ q: query });
            const response = await fetch(
                `${this.baseURL}?${params.toString()}`,
                { signal: this.searchController.signal }
            );
            
            if (!response.ok) throw new Error('Search failed');
            
            const results = await response.json();
            console.log(`✅ Found ${results.length} results`);
            return results;
            
        } catch (error) {
            if (error.name === 'AbortError') {
                console.log('⚠️ Search cancelled');
                return null;
            }
            throw error;
        }
    }
}

// ============ USAGE EXAMPLES ============

async function demo() {
    const blog = new BlogManager('https://jsonplaceholder.typicode.com/posts');
    
    try {
        // 1. Get all posts
        console.log('\n=== GET ALL POSTS ===');
        await blog.getAllPosts(5);
        
        // 2. Get single post
        console.log('\n=== GET SINGLE POST ===');
        await blog.getPost(1);
        
        // 3. Create new post
        console.log('\n=== CREATE POST ===');
        const newPost = await blog.createPost({
            title: 'My New Post',
            body: 'This is the content',
            userId: 1
        });
        
        // 4. Update post (PUT)
        console.log('\n=== UPDATE POST (PUT) ===');
        await blog.updatePost(1, {
            title: 'Updated Title',
            body: 'Updated body',
            userId: 1
        });
        
        // 5. Patch post (PATCH)
        console.log('\n=== PATCH POST ===');
        await blog.patchPost(1, {
            title: 'Only Title Changed'
        });
        
        // 6. Delete post
        console.log('\n=== DELETE POST ===');
        await blog.deletePost(1);
        
        // 7. Search with cancellation
        console.log('\n=== SEARCH WITH CANCELLATION ===');
        blog.searchPosts('first query');
        // This will cancel the previous search
        setTimeout(() => blog.searchPosts('second query'), 100);
        
    } catch (error) {
        console.error('❌ Demo error:', error.message);
    }
}

// Run demo
demo();
```

#### ⚠️ Common Mistakes
1. **Not cancelling previous searches**
   ```javascript
   // ❌ WRONG - multiple searches run simultaneously
   async searchPosts(query) {
       const response = await fetch(url);
   }
   
   // ✅ CORRECT - cancel previous before starting new
   async searchPosts(query) {
       if (this.searchController) {
           this.searchController.abort();
       }
       this.searchController = new AbortController();
       const response = await fetch(url, { 
           signal: this.searchController.signal 
       });
   }
   ```

2. **Forgetting Content-Type header for POST/PUT/PATCH**
   ```javascript
   // ❌ WRONG - server might not parse JSON
   await fetch(url, {
       method: 'POST',
       body: JSON.stringify(data)
   });
   
   // ✅ CORRECT
   await fetch(url, {
       method: 'POST',
       headers: { 'Content-Type': 'application/json' },
       body: JSON.stringify(data)
   });
   ```

3. **Not handling 204 No Content**
   ```javascript
   // ❌ WRONG - crashes on 204 response
   const data = await response.json();
   
   // ✅ CORRECT - check status first
   if (response.status === 204) {
       return { success: true };
   }
   const data = await response.json();
   ```

---


## 5. DEBUG CHALLENGES

Fix the bugs in each code snippet. Each challenge focuses on common fetch() API mistakes.

---

### 🐛 Challenge 1: The Missing await

```javascript
async function getUser() {
  try {
    const response = fetch('https://jsonplaceholder.typicode.com/users/1');
    const user = response.json();
    console.log(user.name);
  } catch (error) {
    console.error('Error:', error);
  }
}

getUser();
```

**🔍 Your Task:** This code has TWO missing `await` keywords. Find them and explain why both are needed.

<details>
<summary>💡 Hint</summary>

Both `fetch()` and `.json()` return Promises. What happens when you don't await them?
</details>

---

**✅ Solution:**

```javascript
async function getUser() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users/1');
    const user = await response.json();
    console.log(user.name);
  } catch (error) {
    console.error('Error:', error);
  }
}

getUser();
```

**📝 Explanation:**
- **Bug 1:** Missing `await` before `fetch()` means `response` is a Promise, not a Response object
- **Bug 2:** Missing `await` before `.json()` means `user` is a Promise, not the parsed data
- **Result:** Without both awaits, `user.name` tries to access a property on a Promise, which is `undefined`
- **Always remember:** Both `fetch()` and response methods (`.json()`, `.text()`, `.blob()`) return Promises

---

### 🐛 Challenge 2: The response.ok Oversight

```javascript
async function deletePost(id) {
  try {
    const response = await fetch(
      `https://jsonplaceholder.typicode.com/posts/${id}`,
      { method: 'DELETE' }
    );
    
    const result = await response.json();
    console.log('Deleted:', result);
  } catch (error) {
    console.error('Delete failed:', error);
  }
}

deletePost(999); // Non-existent post
```

**🔍 Your Task:** This code doesn't handle HTTP errors properly. What's wrong and how do you fix it?

<details>
<summary>💡 Hint</summary>

What does fetch() consider a "successful" request? Does it automatically throw errors for 404?
</details>

---

**✅ Solution:**

```javascript
async function deletePost(id) {
  try {
    const response = await fetch(
      `https://jsonplaceholder.typicode.com/posts/${id}`,
      { method: 'DELETE' }
    );
    
    // ✅ Check response status
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    // ✅ Handle 204 No Content
    if (response.status === 204) {
      console.log('Deleted successfully (no content)');
      return { success: true };
    }
    
    const result = await response.json();
    console.log('Deleted:', result);
    return result;
  } catch (error) {
    console.error('Delete failed:', error.message);
    throw error;
  }
}

deletePost(999);
```

**📝 Explanation:**
- **Bug:** fetch() doesn't reject on HTTP errors (404, 500, etc.)—only on network failures
- **Fix 1:** Always check `response.ok` before parsing the response
- **Fix 2:** DELETE requests often return 204 (No Content), which has no body to parse
- **Key Point:** `response.json()` will fail on empty responses, so check status first

---

### 🐛 Challenge 3: The POST Body Mistake

```javascript
async function createPost(title, body) {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
      method: 'POST',
      body: {
        title: title,
        body: body,
        userId: 1
      }
    });
    
    const newPost = await response.json();
    console.log('Created:', newPost);
  } catch (error) {
    console.error('Error:', error);
  }
}

createPost('Test', 'This is a test');
```

**🔍 Your Task:** This POST request is missing TWO critical things. What are they?

<details>
<summary>💡 Hint</summary>

1. What format should the body be in?
2. Does the server know what type of data you're sending?
</details>

---

**✅ Solution:**

```javascript
async function createPost(title, body) {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json' // ✅ Added header
      },
      body: JSON.stringify({ // ✅ Stringified body
        title: title,
        body: body,
        userId: 1
      })
    });
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const newPost = await response.json();
    console.log('Created:', newPost);
    return newPost;
  } catch (error) {
    console.error('Error:', error);
    throw error;
  }
}

createPost('Test', 'This is a test');
```

**📝 Explanation:**
- **Bug 1:** Body must be a string, not an object—use `JSON.stringify()`
- **Bug 2:** Must set `Content-Type: application/json` header so server knows it's JSON
- **Why both?** The server needs to know WHAT format (header) and needs proper string format (stringified body)
- **Remember:** Headers tell the server HOW to interpret the body

---

### 🐛 Challenge 4: The Query Parameter Confusion

```javascript
async function searchPosts(userId, limit) {
  const url = 'https://jsonplaceholder.typicode.com/posts?userId=' + 
              userId + '&limit=' + limit;
  
  const response = await fetch(url);
  const posts = await response.json();
  console.log(posts);
}

searchPosts(1, 5);
```

**🔍 Your Task:** This code works but has THREE issues: no error handling, no response.ok check, and poor query string building. Fix all three.

<details>
<summary>💡 Hint</summary>

Use URLSearchParams for cleaner query strings. Always wrap fetch in try/catch.
</details>

---

**✅ Solution:**

```javascript
async function searchPosts(userId, limit) {
  try {
    // ✅ Use URLSearchParams for clean query building
    const params = new URLSearchParams({
      userId: userId,
      _limit: limit // JSONPlaceholder uses _limit
    });
    
    const url = `https://jsonplaceholder.typicode.com/posts?${params.toString()}`;
    
    const response = await fetch(url);
    
    // ✅ Check response status
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const posts = await response.json();
    console.log(posts);
    return posts;
  } catch (error) {
    console.error('Search failed:', error.message);
    throw error;
  }
}

searchPosts(1, 5);
```

**📝 Explanation:**
- **Issue 1:** String concatenation is error-prone and doesn't handle special characters
- **Issue 2:** No try/catch means unhandled rejections on network errors
- **Issue 3:** No `response.ok` check means HTTP errors aren't caught
- **Best Practice:** Use URLSearchParams—it handles encoding, special characters, and is more maintainable

---

### 🐛 Challenge 5: The AbortController Memory Leak

```javascript
let controller;

function fetchWithCancel(url) {
  controller = new AbortController();
  
  fetch(url, { signal: controller.signal })
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => {
      console.log('Error:', error.message);
    });
}

// User rapidly clicks search button
document.getElementById('searchBtn').addEventListener('click', () => {
  fetchWithCancel('https://jsonplaceholder.typicode.com/posts/1');
});
```

**🔍 Your Task:** This code creates new controllers but never cancels old requests. Fix it so clicking the button cancels any pending request.

<details>
<summary>💡 Hint</summary>

You need to abort the PREVIOUS controller before creating a new one.
</details>

---

**✅ Solution:**

```javascript
let controller;

async function fetchWithCancel(url) {
  try {
    // ✅ Cancel previous request if exists
    if (controller) {
      controller.abort();
    }
    
    // Create new controller
    controller = new AbortController();
    
    const response = await fetch(url, { signal: controller.signal });
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const data = await response.json();
    console.log(data);
    return data;
  } catch (error) {
    // ✅ Don't log AbortError as real errors
    if (error.name === 'AbortError') {
      console.log('Request cancelled');
    } else {
      console.error('Error:', error.message);
    }
  }
}

// User rapidly clicks search button
document.getElementById('searchBtn').addEventListener('click', () => {
  fetchWithCancel('https://jsonplaceholder.typicode.com/posts/1');
});
```

**📝 Explanation:**
- **Bug:** Creates new controllers without aborting old ones—previous requests keep running
- **Fix:** Always call `controller.abort()` before creating a new controller
- **Why important:** Prevents wasted network requests and potential race conditions
- **Best Practice:** Check if controller exists before aborting to avoid errors
- **AbortError handling:** Don't treat cancellations as errors—they're intentional

---

## 6. COMMON PITFALLS & ANTI-PATTERNS

### ❌ Anti-Pattern 1: Not Checking response.ok

```javascript
// ❌ DON'T DO THIS
async function getUser(id) {
  const response = await fetch(`/api/users/${id}`);
  const user = await response.json(); // Might fail on 404!
  return user;
}

// ✅ DO THIS
async function getUser(id) {
  const response = await fetch(`/api/users/${id}`);
  
  if (!response.ok) {
    throw new Error(`User not found: ${response.status}`);
  }
  
  const user = await response.json();
  return user;
}
```

**Why it matters:** fetch() only rejects on network errors, not HTTP errors. A 404 is still a "successful" request from fetch's perspective.

---

### ❌ Anti-Pattern 2: Forgetting to Stringify POST Body

```javascript
// ❌ DON'T DO THIS
await fetch('/api/users', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: { name: 'John', age: 30 } // Objects aren't valid!
});

// ✅ DO THIS
await fetch('/api/users', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ name: 'John', age: 30 })
});
```

**Why it matters:** The body must be a string. Sending an object will result in `[object Object]` being sent to the server.

---

### ❌ Anti-Pattern 3: Parsing Empty Responses

```javascript
// ❌ DON'T DO THIS
async function deletePost(id) {
  const response = await fetch(`/api/posts/${id}`, {
    method: 'DELETE'
  });
  const result = await response.json(); // 204 has no body!
  return result;
}

// ✅ DO THIS
async function deletePost(id) {
  const response = await fetch(`/api/posts/${id}`, {
    method: 'DELETE'
  });
  
  if (response.status === 204) {
    return { success: true, message: 'Deleted successfully' };
  }
  
  return await response.json();
}
```

**Why it matters:** DELETE requests often return 204 No Content. Trying to parse an empty body throws an error.

---

### ❌ Anti-Pattern 4: Manual Query String Building

```javascript
// ❌ DON'T DO THIS
const search = "hello world";
const url = `/api/search?q=${search}&limit=10`;
// Spaces and special chars break this!

// ✅ DO THIS
const params = new URLSearchParams({
  q: "hello world",
  limit: 10
});
const url = `/api/search?${params.toString()}`;
// Properly encoded: q=hello+world&limit=10
```

**Why it matters:** Manual concatenation doesn't handle special characters, spaces, or encoding. URLSearchParams does it correctly.

---

### ❌ Anti-Pattern 5: Not Using AbortController for Cancellation

```javascript
// ❌ DON'T DO THIS
let lastRequest;

function search(query) {
  lastRequest = fetch(`/api/search?q=${query}`)
    .then(r => r.json())
    .then(data => displayResults(data));
  // Old request still running!
}

// ✅ DO THIS
let controller;

function search(query) {
  if (controller) {
    controller.abort(); // Cancel previous
  }
  
  controller = new AbortController();
  
  fetch(`/api/search?q=${query}`, { signal: controller.signal })
    .then(r => r.json())
    .then(data => displayResults(data))
    .catch(err => {
      if (err.name !== 'AbortError') {
        console.error(err);
      }
    });
}
```

**Why it matters:** Without cancellation, rapid searches create race conditions. The last request might not be the one that finishes last!

---

### ❌ Anti-Pattern 6: Mixing .then() with async/await

```javascript
// ❌ DON'T DO THIS (confusing mix)
async function getData() {
  const response = await fetch('/api/data');
  return response.json().then(data => {
    return data.value;
  });
}

// ✅ DO THIS (consistent async/await)
async function getData() {
  const response = await fetch('/api/data');
  const data = await response.json();
  return data.value;
}
```

**Why it matters:** Mixing styles reduces code readability. Pick one style and stick with it (async/await is preferred).

---

### ❌ Anti-Pattern 7: Setting Content-Type for FormData

```javascript
// ❌ DON'T DO THIS
const formData = new FormData();
formData.append('file', fileInput.files[0]);

await fetch('/upload', {
  method: 'POST',
  headers: {
    'Content-Type': 'multipart/form-data' // Browser sets this!
  },
  body: formData
});

// ✅ DO THIS
const formData = new FormData();
formData.append('file', fileInput.files[0]);

await fetch('/upload', {
  method: 'POST',
  // No Content-Type header!
  body: formData
});
```

**Why it matters:** Browser automatically sets the correct Content-Type with boundary for FormData. Setting it manually breaks file uploads.

---

### ❌ Anti-Pattern 8: Not Handling Network Errors

```javascript
// ❌ DON'T DO THIS
async function loadData() {
  const response = await fetch('/api/data');
  const data = await response.json();
  displayData(data);
  // What if there's no internet?
}

// ✅ DO THIS
async function loadData() {
  try {
    const response = await fetch('/api/data');
    
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }
    
    const data = await response.json();
    displayData(data);
  } catch (error) {
    if (error.name === 'TypeError') {
      showError('Network error - check your connection');
    } else {
      showError(`Failed to load: ${error.message}`);
    }
  }
}
```

**Why it matters:** Network errors (no internet, DNS failure) throw TypeErrors. Always wrap fetch in try/catch and handle both network AND HTTP errors.

---

## 7. KNOWLEDGE CHECK MCQs

### Question 1
What does `fetch()` return?

A) The response data directly  
B) A Promise that resolves to a Response object  
C) A string containing the API data  
D) An XMLHttpRequest object

<details>
<summary>Show Answer</summary>

**Answer: B) A Promise that resolves to a Response object**

**Explanation:** fetch() is Promise-based and returns a Promise that resolves to a Response object (not the data itself). You need to call methods like `.json()` or `.text()` to extract the actual data from the Response.

```javascript
const response = await fetch('/api/data'); // Response object
const data = await response.json(); // Actual data
```
</details>

---

### Question 2
When does `fetch()` reject its Promise?

A) When the HTTP status is 404 or 500  
B) Only on network failures (no internet, DNS errors)  
C) When the response is empty  
D) When the response is not JSON

<details>
<summary>Show Answer</summary>

**Answer: B) Only on network failures**

**Explanation:** fetch() ONLY rejects on network-level failures. HTTP error status codes (404, 500) are considered successful requests. You must check `response.ok` or `response.status` manually to handle HTTP errors.

```javascript
// This does NOT throw for 404!
try {
  const response = await fetch('/nonexistent');
  // response.status = 404, but no error thrown
  
  if (!response.ok) { // Must check manually
    throw new Error('HTTP error');
  }
} catch (error) {
  // Only catches network errors
}
```
</details>

---

### Question 3
What's wrong with this code?

```javascript
const response = await fetch('/api/data');
const data = response.json();
console.log(data.name);
```

A) Nothing, it works fine  
B) Missing await before `.json()`  
C) Should use `.text()` instead  
D) Missing error handling

<details>
<summary>Show Answer</summary>

**Answer: B) Missing await before `.json()`**

**Explanation:** `.json()` returns a Promise, not the parsed data. Without `await`, `data` is a Promise object, so `data.name` is `undefined`.

```javascript
// ❌ Wrong
const data = response.json(); // Promise object
console.log(data.name); // undefined

// ✅ Correct
const data = await response.json(); // Parsed data
console.log(data.name); // Works!
```
</details>

---

### Question 4
Which header must you set when sending JSON data in a POST request?

A) `Accept: application/json`  
B) `Content-Type: application/json`  
C) `Data-Type: json`  
D) No header needed

<details>
<summary>Show Answer</summary>

**Answer: B) Content-Type: application/json**

**Explanation:** The `Content-Type` header tells the server what format the request body is in. For JSON data, it must be set to `application/json`.

```javascript
await fetch('/api/data', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json' // Required!
  },
  body: JSON.stringify({ name: 'John' })
});
```

Note: `Accept` header tells server what format YOU want back, not what you're sending.
</details>

---

### Question 5
What's the difference between PUT and PATCH?

A) They're the same  
B) PUT is for complete replacement, PATCH is for partial updates  
C) PATCH is faster than PUT  
D) PUT requires authentication, PATCH doesn't

<details>
<summary>Show Answer</summary>

**Answer: B) PUT is for complete replacement, PATCH is for partial updates**

**Explanation:**
- **PUT**: Replaces the entire resource with new data (send all fields)
- **PATCH**: Updates only specific fields (send only changed fields)

```javascript
// PUT - must send ALL fields
await fetch('/api/users/1', {
  method: 'PUT',
  body: JSON.stringify({
    name: 'John',
    email: 'john@example.com',
    age: 30
  })
});

// PATCH - send only changed fields
await fetch('/api/users/1', {
  method: 'PATCH',
  body: JSON.stringify({
    age: 31 // Only update age
  })
});
```
</details>

---

### Question 6
How do you properly build query parameters?

A) Manual string concatenation  
B) Using URLSearchParams  
C) Adding them to the body  
D) Using JSON.stringify()

<details>
<summary>Show Answer</summary>

**Answer: B) Using URLSearchParams**

**Explanation:** URLSearchParams automatically handles encoding, special characters, and proper formatting.

```javascript
// ❌ Manual (error-prone)
const url = `/search?q=${query}&limit=${limit}`;

// ✅ URLSearchParams (safe)
const params = new URLSearchParams({ q: query, limit: limit });
const url = `/search?${params.toString()}`;
```
</details>

---

### Question 7
What happens if you try to parse JSON from a 204 No Content response?

A) Returns an empty object  
B) Returns null  
C) Throws an error  
D) Returns undefined

<details>
<summary>Show Answer</summary>

**Answer: C) Throws an error**

**Explanation:** 204 No Content means there's no response body. Calling `.json()` on an empty body throws a parsing error.

```javascript
const response = await fetch('/api/delete/1', {
  method: 'DELETE'
});

// If response.status === 204
await response.json(); // ❌ Error: Unexpected end of JSON input

// ✅ Correct handling
if (response.status === 204) {
  return { success: true };
}
```
</details>

---

### Question 8
How do you cancel an ongoing fetch request?

A) Call fetch.cancel()  
B) Use AbortController  
C) Use clearTimeout()  
D) You can't cancel fetch requests

<details>
<summary>Show Answer</summary>

**Answer: B) Use AbortController**

**Explanation:** AbortController provides the standard way to cancel fetch requests.

```javascript
const controller = new AbortController();

fetch('/api/data', { signal: controller.signal })
  .then(r => r.json())
  .catch(err => {
    if (err.name === 'AbortError') {
      console.log('Request cancelled');
    }
  });

// Cancel the request
controller.abort();
```
</details>

---

### Question 9
What's the correct format for the body in a POST request?

A) JavaScript object  
B) JSON string  
C) Array  
D) Number

<details>
<summary>Show Answer</summary>

**Answer: B) JSON string**

**Explanation:** The body must be a string (or FormData, Blob, etc.), not a plain JavaScript object. Use `JSON.stringify()` to convert objects.

```javascript
// ❌ Wrong
body: { name: 'John' } // [object Object] sent to server

// ✅ Correct
body: JSON.stringify({ name: 'John' })
```
</details>

---

### Question 10
Which response method would you use to download a file?

A) response.json()  
B) response.text()  
C) response.blob()  
D) response.file()

<details>
<summary>Show Answer</summary>

**Answer: C) response.blob()**

**Explanation:** `.blob()` returns binary data, which is what you need for files, images, PDFs, etc.

```javascript
const response = await fetch('/download/file.pdf');
const blob = await response.blob();

// Create download link
const url = URL.createObjectURL(blob);
const a = document.createElement('a');
a.href = url;
a.download = 'file.pdf';
a.click();
```
</details>

---

### Question 11
What does `response.ok` check?

A) If the response has content  
B) If status code is 200-299  
C) If the request was cancelled  
D) If the response is JSON

<details>
<summary>Show Answer</summary>

**Answer: B) If status code is 200-299**

**Explanation:** `response.ok` is a boolean shorthand for checking if the status code is in the success range (200-299).

```javascript
const response = await fetch('/api/data');

// These are equivalent:
if (response.ok) { }
if (response.status >= 200 && response.status < 300) { }
```
</details>

---

### Question 12
When sending FormData, should you set Content-Type header?

A) Yes, always set it to multipart/form-data  
B) No, the browser sets it automatically  
C) Only for file uploads  
D) Only for POST requests

<details>
<summary>Show Answer</summary>

**Answer: B) No, the browser sets it automatically**

**Explanation:** Browser automatically sets Content-Type with proper boundary for FormData. Setting it manually breaks uploads.

```javascript
const formData = new FormData();
formData.append('file', fileInput.files[0]);

// ❌ DON'T set Content-Type
await fetch('/upload', {
  method: 'POST',
  // headers: { 'Content-Type': '...' }, // ❌ No!
  body: formData
});
```
</details>

---

### Question 13
What's the best way to handle both network AND HTTP errors?

A) Only use try/catch  
B) Only check response.ok  
C) Use both try/catch AND check response.ok  
D) Use .catch() method

<details>
<summary>Show Answer</summary>

**Answer: C) Use both try/catch AND check response.ok**

**Explanation:** try/catch catches network errors, response.ok catches HTTP errors.

```javascript
try {
  const response = await fetch('/api/data');
  
  // Catches HTTP errors (404, 500)
  if (!response.ok) {
    throw new Error(`HTTP ${response.status}`);
  }
  
  const data = await response.json();
} catch (error) {
  // Catches network errors AND thrown HTTP errors
  console.error(error);
}
```
</details>

---

### Question 14
Which is the correct order of operations?

A) fetch → parse → check status  
B) fetch → check status → parse  
C) parse → fetch → check status  
D) check status → fetch → parse

<details>
<summary>Show Answer</summary>

**Answer: B) fetch → check status → parse**

**Explanation:** Always check response status BEFORE parsing to avoid errors on bad responses.

```javascript
// ✅ Correct order
const response = await fetch(url);      // 1. Fetch
if (!response.ok) throw new Error();    // 2. Check status
const data = await response.json();     // 3. Parse
```
</details>

---

### Question 15
What does URLSearchParams.toString() return?

A) A JavaScript object  
B) A query string like "key1=value1&key2=value2"  
C) A JSON string  
D) An array of parameters

<details>
<summary>Show Answer</summary>

**Answer: B) A query string like "key1=value1&key2=value2"**

**Explanation:** `.toString()` converts URLSearchParams to a properly formatted query string.

```javascript
const params = new URLSearchParams({
  search: 'hello world',
  limit: 10,
  page: 1
});

console.log(params.toString());
// "search=hello+world&limit=10&page=1"

const url = `/api/search?${params.toString()}`;
```
</details>

---

## 8. REAL INTERVIEW QUESTIONS

### 🎯 Question 1: Explain how fetch() works and why it only rejects on network errors

**Junior Answer:**
"fetch() is used to make HTTP requests. It returns a Promise that gives you the response. It only fails when there's no internet or the server is down."

**Senior Answer:**
"fetch() is a Promise-based API for making HTTP requests. It initiates a network request and returns a Promise that resolves to a Response object containing the status, headers, and body.

The key distinction is that fetch() only rejects on **network-level failures**—cases where the request couldn't be completed at all, such as:
- No internet connection
- DNS resolution failure
- Request blocked by browser (CORS)
- Server completely unreachable

HTTP error status codes (404, 500, etc.) are considered **successful requests** from fetch's perspective because the server did respond. This design decision treats any server response as 'successful communication,' leaving it to developers to interpret the semantic meaning of status codes.

To handle HTTP errors, you must explicitly check `response.ok` (which is true for 200-299) or `response.status`:

```javascript
try {
  const response = await fetch('/api/data');
  
  if (!response.ok) {
    throw new Error(`HTTP ${response.status}: ${response.statusText}`);
  }
  
  const data = await response.json();
  return data;
} catch (error) {
  // Handles BOTH network errors and thrown HTTP errors
  if (error.name === 'TypeError') {
    // Network failure
    console.error('Network error:', error);
  } else {
    // HTTP error or other
    console.error('Request failed:', error);
  }
}
```

This two-layer error handling—network errors via catch, HTTP errors via response.ok—is essential for robust applications."

---

### 🎯 Question 2: How would you implement request cancellation and why is it important?

**Junior Answer:**
"You can use AbortController to cancel fetch requests. It's useful when the user navigates away or changes their search query."

**Senior Answer:**
"Request cancellation is critical for preventing race conditions, reducing unnecessary network usage, and improving UX. The standard mechanism is AbortController:

```javascript
class RequestManager {
  constructor() {
    this.controller = null;
  }
  
  async fetch(url, options = {}) {
    // Cancel any pending request
    if (this.controller) {
      this.controller.abort();
    }
    
    // Create new controller for this request
    this.controller = new AbortController();
    
    try {
      const response = await fetch(url, {
        ...options,
        signal: this.controller.signal
      });
      
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}`);
      }
      
      return await response.json();
    } catch (error) {
      // Don't treat intentional cancellations as errors
      if (error.name === 'AbortError') {
        console.log('Request cancelled');
        return null;
      }
      throw error;
    }
  }
}
```

**Key use cases:**

1. **Search/Autocomplete**: Cancel previous searches when user types
```javascript
let searchController;

async function search(query) {
  if (searchController) searchController.abort();
  searchController = new AbortController();
  
  const results = await fetch(`/search?q=${query}`, {
    signal: searchController.signal
  });
}
```

2. **Component Unmounting**: Prevent state updates on unmounted components
```javascript
useEffect(() => {
  const controller = new AbortController();
  
  fetchData({ signal: controller.signal });
  
  return () => controller.abort(); // Cleanup
}, []);
```

3. **Timeouts**: Enforce maximum request duration
```javascript
const controller = new AbortController();
const timeoutId = setTimeout(() => controller.abort(), 5000);

try {
  const response = await fetch(url, { signal: controller.signal });
  clearTimeout(timeoutId);
} catch (error) {
  if (error.name === 'AbortError') {
    throw new Error('Request timeout');
  }
}
```

The abort signal is a **cooperative cancellation** mechanism—it signals intent to cancel, but the server may have already processed the request."

---

### 🎯 Question 3: How do you handle different response types and HTTP methods with fetch()?

**Junior Answer:**
"You use `.json()` for JSON data and set the method to 'POST', 'PUT', or 'DELETE' in the options object. You also need to stringify the body for POST requests."

**Senior Answer:**
"fetch() provides a flexible interface for handling various content types and HTTP methods. Here's a comprehensive approach:

**Response Type Handling:**

```javascript
async function fetchData(url) {
  const response = await fetch(url);
  
  if (!response.ok) {
    throw new Error(`HTTP ${response.status}`);
  }
  
  // Determine response type from Content-Type header
  const contentType = response.headers.get('content-type');
  
  if (contentType?.includes('application/json')) {
    return await response.json();
  } else if (contentType?.includes('text/')) {
    return await response.text();
  } else if (contentType?.includes('multipart/form-data')) {
    return await response.formData();
  } else {
    // Binary data (images, PDFs, etc.)
    return await response.blob();
  }
}
```

**HTTP Methods Pattern:**

```javascript
class APIClient {
  constructor(baseURL) {
    this.baseURL = baseURL;
  }
  
  async request(endpoint, options = {}) {
    const url = `${this.baseURL}${endpoint}`;
    
    const config = {
      ...options,
      headers: {
        'Content-Type': 'application/json',
        ...options.headers
      }
    };
    
    // Stringify body for methods that support it
    if (config.body && typeof config.body === 'object') {
      config.body = JSON.stringify(config.body);
    }
    
    const response = await fetch(url, config);
    
    if (!response.ok) {
      const error = await response.json().catch(() => ({}));
      throw new Error(error.message || `HTTP ${response.status}`);
    }
    
    // Handle 204 No Content
    if (response.status === 204) {
      return null;
    }
    
    return await response.json();
  }
  
  get(endpoint) {
    return this.request(endpoint, { method: 'GET' });
  }
  
  post(endpoint, data) {
    return this.request(endpoint, {
      method: 'POST',
      body: data
    });
  }
  
  put(endpoint, data) {
    return this.request(endpoint, {
      method: 'PUT',
      body: data
    });
  }
  
  patch(endpoint, data) {
    return this.request(endpoint, {
      method: 'PATCH',
      body: data
    });
  }
  
  delete(endpoint) {
    return this.request(endpoint, { method: 'DELETE' });
  }
}

// Usage
const api = new APIClient('https://api.example.com');
const users = await api.get('/users');
const newUser = await api.post('/users', { name: 'John' });
await api.delete('/users/1');
```

**Key Considerations:**
- Always check Content-Type before parsing
- Handle 204 No Content responses (no body to parse)
- PUT vs PATCH: PUT replaces entire resource, PATCH updates specific fields
- DELETE may or may not return a body
- FormData shouldn't have Content-Type set (browser handles it)
- For file uploads, don't stringify the body or set Content-Type"

---

### 🎯 Question 4: How would you implement retry logic and error recovery for fetch requests?

**Junior Answer:**
"You can use a loop to retry the request a few times if it fails, with a delay between attempts."

**Senior Answer:**
"Production-grade retry logic requires exponential backoff, selective retry (not all errors should retry), and proper cleanup. Here's a robust implementation:

```javascript
class RetryableRequest {
  constructor(options = {}) {
    this.maxRetries = options.maxRetries || 3;
    this.initialDelay = options.initialDelay || 1000;
    this.maxDelay = options.maxDelay || 10000;
    this.backoffMultiplier = options.backoffMultiplier || 2;
    this.retryableStatuses = options.retryableStatuses || [408, 429, 500, 502, 503, 504];
  }
  
  async fetch(url, options = {}, retryCount = 0) {
    try {
      const response = await fetch(url, options);
      
      // Don't retry on client errors (4xx except specific ones)
      if (!response.ok && !this.shouldRetry(response.status)) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`);
      }
      
      // Retry on specific server errors
      if (!response.ok && this.shouldRetry(response.status)) {
        if (retryCount < this.maxRetries) {
          await this.delay(retryCount);
          return this.fetch(url, options, retryCount + 1);
        }
        throw new Error(`Failed after ${this.maxRetries} retries: ${response.status}`);
      }
      
      return response;
    } catch (error) {
      // Retry on network errors
      if (error.name === 'TypeError' && retryCount < this.maxRetries) {
        console.log(`Retry ${retryCount + 1}/${this.maxRetries} after network error`);
        await this.delay(retryCount);
        return this.fetch(url, options, retryCount + 1);
      }
      
      throw error;
    }
  }
  
  shouldRetry(status) {
    return this.retryableStatuses.includes(status);
  }
  
  async delay(retryCount) {
    // Exponential backoff with jitter
    const exponentialDelay = Math.min(
      this.initialDelay * Math.pow(this.backoffMultiplier, retryCount),
      this.maxDelay
    );
    
    // Add random jitter (0-25% of delay) to prevent thundering herd
    const jitter = exponentialDelay * 0.25 * Math.random();
    const totalDelay = exponentialDelay + jitter;
    
    await new Promise(resolve => setTimeout(resolve, totalDelay));
  }
}

// Usage
const retryFetch = new RetryableRequest({
  maxRetries: 3,
  initialDelay: 1000,
  backoffMultiplier: 2
});

try {
  const response = await retryFetch.fetch('/api/data');
  const data = await response.json();
} catch (error) {
  // Failed after all retries
  console.error('Request failed:', error);
}
```

**Advanced: Circuit Breaker Pattern**

```javascript
class CircuitBreaker {
  constructor(options = {}) {
    this.failureThreshold = options.failureThreshold || 5;
    this.timeout = options.timeout || 60000;
    this.state = 'CLOSED'; // CLOSED, OPEN, HALF_OPEN
    this.failures = 0;
    this.nextAttempt = Date.now();
  }
  
  async fetch(url, options) {
    if (this.state === 'OPEN') {
      if (Date.now() < this.nextAttempt) {
        throw new Error('Circuit breaker is OPEN');
      }
      this.state = 'HALF_OPEN';
    }
    
    try {
      const response = await fetch(url, options);
      
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}`);
      }
      
      this.onSuccess();
      return response;
    } catch (error) {
      this.onFailure();
      throw error;
    }
  }
  
  onSuccess() {
    this.failures = 0;
    this.state = 'CLOSED';
  }
  
  onFailure() {
    this.failures++;
    
    if (this.failures >= this.failureThreshold) {
      this.state = 'OPEN';
      this.nextAttempt = Date.now() + this.timeout;
      console.log('Circuit breaker opened');
    }
  }
}
```

**Key principles:**
1. **Selective Retry**: Only retry transient errors (network, 5xx), not client errors (4xx)
2. **Exponential Backoff**: Delay increases exponentially (1s, 2s, 4s, 8s...)
3. **Jitter**: Add randomness to prevent all clients retrying simultaneously
4. **Max Delay Cap**: Prevent delays from growing indefinitely
5. **Circuit Breaker**: Stop trying when service is definitely down
6. **Proper Logging**: Track retry attempts for debugging"

---

## 9. DEVTOOLS & DEBUGGING

### 🔍 Debugging fetch() Requests in Chrome DevTools

#### 1. **Network Tab - Your Primary Tool**

**How to Access:**
1. Open DevTools (F12 or Cmd+Option+I on Mac)
2. Click **Network** tab
3. Refresh page or trigger your fetch request

**What You See:**
```
Name              Status  Type    Size    Time
──────────────────────────────────────────────
posts             200     fetch   10.2KB  234ms
users/1           404     fetch   125B    145ms
```

**Key Columns to Monitor:**
| Column | What It Shows | Debugging Use |
|--------|---------------|---------------|
| **Name** | Request URL | Verify correct endpoint |
| **Status** | HTTP status code | Check for errors (404, 500) |
| **Type** | Request type (fetch, xhr) | Confirm it's fetch |
| **Size** | Response size | Detect unexpectedly large responses |
| **Time** | Duration | Find slow requests |
| **Waterfall** | Timing breakdown | See DNS, connection, wait times |

---

#### 2. **Inspecting a Specific Request**

Click on any request to see detailed information:

**Headers Tab:**
```
General
  Request URL: https://api.example.com/users/1
  Request Method: GET
  Status Code: 200 OK
  
Request Headers
  Accept: application/json
  Authorization: Bearer eyJ...
  Content-Type: application/json
  
Response Headers
  Content-Type: application/json
  Content-Length: 1247
  Cache-Control: max-age=3600
```

**Use this to debug:**
- ✅ Verify correct URL (typos common!)
- ✅ Check if authentication header is sent
- ✅ Confirm Content-Type is set for POST/PUT
- ✅ See what headers server sent back

---

**Payload Tab (for POST/PUT/PATCH):**
```javascript
// View what was sent in the body
{
  "title": "My Post",
  "body": "Content here",
  "userId": 1
}
```

**Use this to debug:**
- ✅ Verify data was stringified correctly
- ✅ Check for missing or incorrect fields
- ✅ Ensure no `[object Object]` was sent

---

**Response Tab:**
```javascript
// View what server returned
{
  "id": 101,
  "title": "My Post",
  "body": "Content here",
  "userId": 1
}
```

**Use this to debug:**
- ✅ See actual server response
- ✅ Check for error messages
- ✅ Verify data structure matches expectations

---

**Preview Tab:**
- Shows formatted JSON (easier to read than Response tab)
- Expandable object tree
- Perfect for exploring nested data

---

#### 3. **Filtering Requests**

```
Filter box at top of Network tab:
─────────────────────────────────
🔍 [Fetch/XHR] posts
```

**Useful Filters:**
- `Fetch/XHR`: Show only AJAX requests
- `status-code:404`: Show only 404 errors
- `domain:api.example.com`: Filter by domain
- `larger-than:1M`: Find large responses
- `method:POST`: Show only POST requests

---

#### 4. **Console Debugging Techniques**

**Basic Logging:**
```javascript
async function fetchUser(id) {
  console.log('[fetchUser] Starting:', id);
  
  const url = `https://api.example.com/users/${id}`;
  console.log('[fetchUser] URL:', url);
  
  const response = await fetch(url);
  console.log('[fetchUser] Response:', {
    status: response.status,
    ok: response.ok,
    headers: Object.fromEntries(response.headers)
  });
  
  const data = await response.json();
  console.log('[fetchUser] Data:', data);
  
  return data;
}
```

**Timing Performance:**
```javascript
async function timedFetch(url) {
  console.time(`Fetch ${url}`);
  
  const response = await fetch(url);
  console.timeLog(`Fetch ${url}`, 'Response received');
  
  const data = await response.json();
  console.timeEnd(`Fetch ${url}`);
  
  return data;
}

// Output:
// Fetch /api/data: 234.5ms Response received
// Fetch /api/data: 456.7ms
```

---

#### 5. **Breakpoint Debugging**

**Set Breakpoints:**
1. Go to **Sources** tab
2. Find your JavaScript file
3. Click line number to set breakpoint
4. Trigger the fetch request

**Debugger pauses at breakpoint:**
```javascript
async function fetchData() {
  debugger; // Execution pauses here
  const response = await fetch('/api/data');
  debugger; // And here after fetch completes
  const data = await response.json();
  return data;
}
```

**Inspect Variables:**
- Hover over variables to see values
- Use **Scope** panel to see all variables
- Use **Call Stack** to see function call chain

---

#### 6. **XHR/Fetch Breakpoints**

**Automatically pause on ALL fetch requests:**
1. Sources tab → Right sidebar
2. Expand "XHR/fetch Breakpoints"
3. Click `+` and enter URL pattern (or leave blank for all)
4. Check "Pause on all XHR/fetch requests"

**Execution pauses before every fetch:**
```javascript
// Debugger stops here automatically
const response = await fetch('/api/data');
```

---

#### 7. **Network Throttling**

Test how your app handles slow connections:

1. Network tab → Throttling dropdown (top right)
2. Select preset:
   - Fast 3G
   - Slow 3G
   - Offline

Or create custom profile:
```
Download: 500 Kb/s
Upload: 100 Kb/s
Latency: 400ms
```

**Use this to test:**
- Loading states
- Timeout handling
- User experience on slow connections

---

#### 8. **Offline Mode**

Test network error handling:

1. Network tab → Check "Offline" checkbox
2. Or: DevTools → Settings → Throttling → Add custom profile "Offline"

```javascript
// This will throw TypeError: Failed to fetch
try {
  await fetch('/api/data');
} catch (error) {
  console.log(error.name); // "TypeError"
  console.log('No internet!');
}
```

---

#### 9. **Copy as fetch()**

Replay requests in console:

1. Network tab → Right-click request
2. Copy → Copy as fetch
3. Paste in Console tab

```javascript
// Copied code:
await fetch("https://api.example.com/users/1", {
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer token..."
  },
  "method": "GET"
});
```

**Use this to:**
- Test modified requests
- Debug authentication issues
- Experiment with different parameters

---

#### 10. **Common Debugging Scenarios**

**Scenario 1: "Why is my request not working?"**
```
1. Network tab → Find the request
2. Check Status column → 404? 500?
3. Click request → Response tab → Read error message
4. Headers tab → Verify URL is correct
5. If POST/PUT → Payload tab → Check body format
```

**Scenario 2: "Request succeeds but data is wrong"**
```
1. Network tab → Click request
2. Preview tab → Inspect response structure
3. Console → Log the parsed data: console.log(await response.json())
4. Verify data transformation logic
```

**Scenario 3: "Request is too slow"**
```
1. Network tab → Check Time column
2. Click request → Timing tab
3. Look for:
   - DNS Lookup (slow? DNS issue)
   - Initial Connection (slow? server far away)
   - Waiting (TTFB) (slow? server processing)
   - Content Download (slow? large response)
```

**Scenario 4: "CORS error"**
```
Console shows: Access to fetch has been blocked by CORS policy

1. Network tab → Click failed request
2. Headers tab → Check:
   - Request: Origin header
   - Response: Access-Control-Allow-Origin header
3. If missing → Server needs to add CORS headers
4. If present but doesn't match → Check domain spelling
```

---

#### 11. **Preserve Log**

Don't lose requests on page navigation:

1. Network tab → Check "Preserve log"
2. Requests persist across page loads
3. Essential for debugging redirects

---

### 🛠️ Debugging Checklist

When debugging fetch issues:

- [ ] Network tab open before making request
- [ ] Check Status column for HTTP errors
- [ ] Verify Request URL in Headers tab
- [ ] For POST/PUT: Check Payload tab shows stringified JSON
- [ ] For POST/PUT: Verify Content-Type header is set
- [ ] Check Response tab for error messages
- [ ] Use Console to log response object
- [ ] Try "Copy as fetch" to replay in console
- [ ] Test with throttling to simulate slow network
- [ ] Check browser console for JavaScript errors

---

## 10. CHAPTER SUMMARY & NEXT STEPS

### 🎓 Quick Recap

**You've mastered the fetch() API!** Here's what you now know:

#### Core Concepts:
1. **fetch() Fundamentals**
   - Returns Promise<Response>, not the data directly
   - Only rejects on network errors, not HTTP errors
   - Always check `response.ok` before parsing

2. **Response Handling**
   - `.json()` for JSON data
   - `.text()` for plain text
   - `.blob()` for binary data (files, images)
   - All response methods return Promises

3. **HTTP Methods**
   - **GET**: Retrieve data (no body)
   - **POST**: Create resources (requires body + headers)
   - **PUT**: Replace entire resource
   - **PATCH**: Partial update
   - **DELETE**: Remove resource (often returns 204)

4. **Request Configuration**
   - Set method via options object
   - Add headers for Content-Type, Authorization
   - Stringify body with `JSON.stringify()`
   - Use URLSearchParams for query strings

5. **Error Handling**
   - Wrap in try/catch for network errors
   - Check response.ok for HTTP errors
   - Handle both types separately
   - Provide meaningful error messages

6. **Advanced Patterns**
   - AbortController for request cancellation
   - Request objects for reusable templates
   - Proper handling of 204 No Content
   - FormData for file uploads (no Content-Type!)

---

### ✅ Self-Assessment

Can you confidently:
- [ ] Make GET requests and parse JSON responses?
- [ ] Send POST requests with proper headers and body?
- [ ] Distinguish between PUT and PATCH?
- [ ] Check response.ok before parsing?
- [ ] Build query parameters with URLSearchParams?
- [ ] Cancel ongoing requests with AbortController?
- [ ] Handle both network AND HTTP errors?
- [ ] Debug fetch requests using Chrome DevTools?
- [ ] Explain why fetch() doesn't reject on 404?
- [ ] Create a reusable API client class?

If you checked all boxes, you're ready for Day 26! 🎉

---

### 🔗 Connection to Next Topic

**Tomorrow: 6 Common Mistakes with JavaScript Promises & Async Code**

Now that you've mastered fetch() and understand how to work with Promises in practice, you're ready to level up your skills by learning what NOT to do.

Tomorrow you'll discover:
- The hidden pitfalls that even experienced developers fall into
- Why `Promise.all()` might not be what you need
- How to avoid race conditions in async code
- The subtle bugs that come from mixing callbacks and Promises
- Performance traps that slow down your applications
- How to write bulletproof async error handling

**Preview:**
```javascript
// You know fetch() now, but do you see the bug here?
async function loadDashboard() {
  try {
    const user = await fetchUser();
    const posts = await fetchPosts();
    const comments = await fetchComments();
    // This works... but is it optimal? 🤔
  } catch (error) {
    console.log(error);
    // Is this enough error handling?
  }
}

// Tomorrow: You'll learn why this might bite you in production
```

---

### 📚 Practice Before Day 26

**Complete these challenges:**
1. Build a CRUD application using JSONPlaceholder
2. Implement search with AbortController cancellation
3. Create an API client class with retry logic
4. Handle file uploads with FormData
5. Build a request queue system

**Test your understanding:**
- What happens if you forget to stringify the body?
- When would you use PATCH instead of PUT?
- How do you detect network errors vs HTTP errors?
- Why shouldn't you set Content-Type for FormData?

---

### 🎯 Ready for Day 26?

You've completed Day 25 - fetch() API! 🎉

**What's Next:**
- Review the debug challenges
- Complete practice tasks from TASK.md
- Build the Movie Explorer project
- Practice with different APIs (GitHub, OpenWeather)

**See you on Day 26** where we'll explore the common mistakes that trip up even experienced developers! 🚀

---

<div align="center">

### 💬 Questions or Stuck?

Having trouble with fetch()? Common issues:
- **CORS errors?** → Check server CORS headers
- **undefined data?** → Missing `await` or `response.ok` check
- **[object Object] sent?** → Forgot `JSON.stringify()`
- **Can't parse response?** → Check if status is 204 No Content

Join the Discord to discuss! | [Report Issues](#) | [View Solutions](#)

---

**Remember:** fetch() is your gateway to building interactive, data-driven web applications. Master it through practice, not just reading. Build things! Break things! Debug things!

*Happy Coding! 💻*

</div>

---



