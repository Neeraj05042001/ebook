# Day 18: Learn DOM Manipulation with JavaScript Like a Pro

## 1. CHAPTER OVERVIEW

Welcome to **Day 18**! Yesterday, you learned how to access the DOM—how to reach into the HTML structure and "grab" elements. Today, we stop looking and start _doing_. You will learn how to surgically alter the webpage: creating new elements from thin air, changing text, styling dynamically, and moving elements around without reloading the page.

### What You'll Master Today

- **Element Creation:** How to generate new HTML tags (`div`, `li`, `span`) using JavaScript.
- **Content Injection:** The critical differences between `innerText`, `textContent`, and `innerHTML`.
- **Attribute Management:** Manipulating IDs, classes, and custom data attributes safely.
- **Style Manipulation:** Changing CSS properties dynamically using the `.style` object vs. `classList`.
- **DOM Architecture:** How to append, prepend, and insert elements exactly where you want them.
- **Element Removal:** How to safely delete nodes from the DOM.

### Prerequisites Check

Before diving in, ensure you have:

- [ ] Understood the DOM Tree structure (Day 17).
- [ ] Mastered selecting elements using `querySelector` and `getElementById` (Day 17).
- [ ] Knowledge of JavaScript Objects (Day 12) – _The DOM is just a giant object!_
- [ ] Basic HTML/CSS understanding.

### Why This Matters

This is the bridge between a static document and a software application.

- **Real-world usage:** When you scroll down on Instagram and new posts load, that is DOM manipulation (creating elements + appending them). When you mark a task as "done" in Trello and it turns green, that is style manipulation.
- **Career Impact:** You cannot build React, Vue, or Angular applications without understanding how the underlying DOM manipulation works. This is the engine under the hood of every modern framework.

---

## 2. QUICK REVISION SUMMARY

Here is your cheat sheet for modifying the DOM. These methods are methods available on DOM nodes.

### 1. Creating and Adding Nodes

| Method                          | Description                                                                                      |
| ------------------------------- | ------------------------------------------------------------------------------------------------ |
| `document.createElement('tag')` | Creates a new element node (e.g., `'div'`, `'p'`). It is **not** visible until added to the DOM. |
| `parent.append(node/string)`    | Adds a node or text string to the **end** of the parent. Supports multiple arguments.            |
| `parent.appendChild(node)`      | Adds a **single** node to the **end** of the parent. Does not support strings.                   |
| `parent.prepend(node)`          | Adds a node to the **beginning** of the parent.                                                  |
| `parent.insertBefore(new, ref)` | Inserts `new` node before the `ref` (reference) node.                                            |

### 2. Modifying Content

This is often a source of confusion. Here is the definitive decision guide:

| Property      | What it does                                                                                   | When to use                                                                                |
| ------------- | ---------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| `innerText`   | Gets/Sets visible text. Aware of CSS (ignores `display: none` text). Triggers reflow (slower). | When you need the text exactly as the user sees it.                                        |
| `textContent` | Gets/Sets all text, including hidden ones. Does not parse HTML. Faster.                        | **Preferred default** for setting plain text.                                              |
| `innerHTML`   | Parses string as HTML. Can create child tags.                                                  | Only when you need to render actual HTML tags inside a container. **Security Risk (XSS).** |

### 3. CSS Classes & Styles

Avoid setting `.className` directly as it overwrites everything. Use `classList`.

- **`element.classList.add('class-name')`**: Adds a class.
- **`element.classList.remove('class-name')`**: Removes a class.
- **`element.classList.toggle('class-name')`**: Adds if missing, removes if present.
- **`element.classList.contains('class-name')`**: Returns `true`/`false`.
- **`element.style.property`**: Inline style manipulation (e.g., `element.style.backgroundColor = 'red'`).

> **Note on Naming:** In CSS, property names use kebab-case (`background-color`). In JavaScript, we use camelCase (`backgroundColor`).

### 4. Attributes

| Method                        | Usage                                                                       |
| ----------------------------- | --------------------------------------------------------------------------- |
| `getAttribute('attr')`        | `img.getAttribute('src')`                                                   |
| `setAttribute('attr', 'val')` | `input.setAttribute('type', 'password')`                                    |
| `removeAttribute('attr')`     | `btn.removeAttribute('disabled')`                                           |
| `dataset` property            | Access `data-*` attributes. `<div data-id="123">` becomes `div.dataset.id`. |

---

## Section 3: PREDICT THE OUTPUT

Welcome to the prediction zone! Before we dive into writing code, let's sharpen your understanding by predicting what happens when code runs. This trains your brain to think like the JavaScript engine.

**How to Use This Section:**

1. Read the code carefully
2. Try to predict the output WITHOUT running it
3. Write down your prediction
4. Check the hint if you're stuck
5. Read the detailed explanation
6. Run the code to verify

---

### Challenge 1: Basic Element Selection 🟢 Easy

```javascript
const heading = document.getElementById("main-heading");
console.log(heading);
console.log(typeof heading);

// HTML: <h1 id="main-heading">Welcome</h1>
```

**🤔 Think About It:**
What type of value does `getElementById()` return? Is it a string, object, or something else?

**💡 Hint:** The DOM represents HTML elements as objects in JavaScript.

**✅ Answer & Explanation:**

```
[object HTMLHeadingElement]
object
```

**Why?**

- `getElementById()` returns an **HTMLElement object**, not a string
- Even though it looks like HTML, JavaScript converts the element into an object with properties and methods
- The `typeof` operator confirms it's an object
- This object gives you programmatic access to manipulate the element

**Key Takeaway:** DOM elements are JavaScript objects with properties (like `textContent`, `style`) and methods (like `addEventListener()`).

---

### Challenge 2: querySelector vs getElementById 🟢 Easy

```javascript
const elem1 = document.getElementById("box");
const elem2 = document.querySelector("#box");
const elem3 = document.querySelector(".box");

console.log(elem1 === elem2);
console.log(elem1 === elem3);

// HTML:
// <div id="box" class="box">First</div>
// <div class="box">Second</div>
```

**🤔 Think About It:**
When do two variables reference the same DOM element? What does `querySelector()` return when multiple elements match?

**💡 Hint:** `===` checks if two variables reference the exact same object in memory. `querySelector()` returns only the first match.

**✅ Answer & Explanation:**

```
true
true
```

**Why?**

- `getElementById('box')` returns the element with `id="box"`
- `querySelector('#box')` also returns the same element (# selects by ID)
- `querySelector('.box')` returns the **first element** with `class="box"`, which happens to be the same div
- All three variables point to the same object in memory, so `===` returns `true`

**Common Misconception:** People think `querySelector('.box')` might return a different element because there are multiple `.box` elements, but it always returns the **first match**.

---

### Challenge 3: textContent vs innerHTML 🟡 Medium

```javascript
const container = document.getElementById("content");
container.innerHTML = "<strong>Bold</strong> Text";

console.log(container.textContent);
console.log(container.innerHTML);

// HTML: <div id="content">Original</div>
```

**🤔 Think About It:**
What's the difference between `textContent` and `innerHTML`? Does one parse HTML tags while the other doesn't?

**💡 Hint:** `textContent` treats everything as plain text, while `innerHTML` interprets HTML tags.

**✅ Answer & Explanation:**

```
Bold Text
<strong>Bold</strong> Text
```

**Why?**

- `innerHTML = '<strong>Bold</strong> Text'` replaces the content with HTML that the browser **parses and renders**
- The `<strong>` tag is interpreted, making "Bold" appear bold in the browser
- `textContent` returns only the **visible text** without HTML tags
- `innerHTML` returns the **raw HTML markup** as a string

**Key Difference:**

```javascript
// textContent: Gets/sets plain text only
// innerHTML: Gets/sets HTML markup

// Reading textContent strips all tags
// Reading innerHTML preserves all tags
```

---

### Challenge 4: Chaining Properties 🟡 Medium

```javascript
const list = document.querySelector("ul");
console.log(list.children.length);
console.log(list.children[0].textContent);
console.log(list.firstElementChild.textContent);

// HTML:
// <ul>
//   <li>Apple</li>
//   <li>Banana</li>
//   <li>Cherry</li>
// </ul>
```

**🤔 Think About It:**
What does the `children` property return? How do we access individual children?

**💡 Hint:** `children` is an HTMLCollection (array-like), and `firstElementChild` is a direct property.

**✅ Answer & Explanation:**

```
3
Apple
Apple
```

**Why?**

1. `list.children` returns an **HTMLCollection** of all child elements (the three `<li>` elements)
2. `children.length` gives the count: **3**
3. `children[0]` accesses the first `<li>` element (zero-indexed)
4. `firstElementChild` is a property that directly references the first child element
5. Both `children[0]` and `firstElementChild` point to the same element

**Pro Tip:** Use `firstElementChild` and `lastElementChild` for cleaner code instead of `children[0]` and `children[children.length - 1]`.

---

### Challenge 5: Attribute Manipulation Gotcha 🟡 Medium

```javascript
const link = document.querySelector("a");

console.log(link.getAttribute("href"));
console.log(link.href);

// HTML: <a href="/about">About</a>
// Current URL: https://example.com/home
```

**🤔 Think About It:**
Is there a difference between `getAttribute('href')` and the `href` property? One returns a relative URL, but what does the other return?

**💡 Hint:** Properties like `href` return **resolved/absolute** URLs, while `getAttribute()` returns the **literal attribute value**.

**✅ Answer & Explanation:**

```
/about
https://example.com/about
```

**Why?**

- `getAttribute('href')` returns the **exact value** from the HTML attribute: `"/about"`
- The `href` **property** returns the **fully resolved absolute URL**: `"https://example.com/about"`
- The browser automatically resolves relative URLs when you access the property
- This difference is crucial when you need the actual attribute value vs. the computed URL

**Real-World Use Case:**

```javascript
// When you need the literal attribute (e.g., for comparison)
if (link.getAttribute("href") === "/about") {
  // Do something
}

// When you need the full URL (e.g., for navigation)
console.log("Navigating to:", link.href);
```

---

### Challenge 6: Creating and Appending Elements 🟡 Medium

```javascript
const container = document.getElementById("box");
const newDiv = document.createElement("div");

console.log(container.children.length);

newDiv.textContent = "New Element";
console.log(container.children.length);

container.appendChild(newDiv);
console.log(container.children.length);

// HTML: <div id="box"><p>Existing</p></div>
```

**🤔 Think About It:**
When does an element actually appear in the DOM? Is it when we create it or when we append it?

**💡 Hint:** `createElement()` creates an element in memory but doesn't add it to the page.

**✅ Answer & Explanation:**

```
1
1
2
```

**Why?**

1. Initially, `container` has 1 child (the `<p>` element)
2. `createElement('div')` creates an element **in memory only** – it's not yet in the DOM
3. Setting `textContent` modifies the in-memory element but doesn't add it to the page
4. Only when we call `appendChild()` does the element get added to the DOM
5. After appending, `children.length` becomes **2**

**Critical Understanding:** There are two stages:

- **Creation:** Element exists in JavaScript memory
- **Insertion:** Element appears in the DOM tree

```javascript
// This element exists but is invisible:
const ghost = document.createElement("div");

// This element is now visible in the DOM:
document.body.appendChild(ghost);
```

---

### Challenge 7: classList Behavior 🔴 Hard

```javascript
const box = document.querySelector(".box");

console.log(box.classList.contains("active"));

box.classList.add("active");
console.log(box.classList.contains("active"));

box.classList.toggle("active");
console.log(box.classList.contains("active"));

box.classList.toggle("active");
console.log(box.classList.contains("active"));

// HTML: <div class="box">Content</div>
```

**🤔 Think About It:**
How does `toggle()` work? What does it do if the class exists vs. doesn't exist?

**💡 Hint:** `toggle()` adds the class if it's missing, removes it if it's present.

**✅ Answer & Explanation:**

```
false
true
false
true
```

**Step-by-Step:**

1. Initially, the element only has class `"box"`, not `"active"` → `false`
2. `add('active')` adds the class → `contains()` returns `true`
3. First `toggle('active')` removes it (because it exists) → `false`
4. Second `toggle('active')` adds it back (because it's missing) → `true`

**Understanding toggle():**

```javascript
// If class exists → remove it
// If class doesn't exist → add it

// This is equivalent to:
if (element.classList.contains("active")) {
  element.classList.remove("active");
} else {
  element.classList.add("active");
}
```

**Real-World Usage:** Perfect for UI interactions like showing/hiding menus, toggling dark mode, or accordion panels.

---

### Challenge 8: Style Property Values 🔴 Hard

```javascript
const box = document.querySelector(".box");

box.style.backgroundColor = "red";
box.style.width = "100px";

console.log(box.style.backgroundColor);
console.log(box.style.width);
console.log(typeof box.style.width);

// HTML: <div class="box">Content</div>
```

**🤔 Think About It:**
When we set CSS properties via JavaScript, what data type are they stored as? Are numeric values stored as numbers?

**💡 Hint:** CSS properties set via the `style` object are always stored as strings.

**✅ Answer & Explanation:**

```
red
100px
string
```

**Why?**

- Even though we can write `box.style.width = '100px'` or `box.style.width = 100 + 'px'`, the stored value is always a **string**
- CSS values need units (px, em, %, etc.), so JavaScript stores them as strings
- When you read `box.style.width`, you get `"100px"`, not the number `100`
- If you need the numeric value, you must parse it: `parseInt(box.style.width)`

**Common Mistake:**

```javascript
// ❌ This doesn't work as expected:
box.style.width = 100; // Results in invalid CSS

// ✅ Always include units:
box.style.width = "100px";
box.style.width = 100 + "px";
```

---

### Challenge 9: Parent-Child Navigation 🔴 Hard

```javascript
const listItem = document.querySelector("li");

console.log(listItem.parentElement.tagName);
console.log(listItem.parentElement.children.length);
console.log(listItem.nextElementSibling.textContent);

// HTML:
// <ul>
//   <li>First</li>
//   <li>Second</li>
//   <li>Third</li>
// </ul>
```

**🤔 Think About It:**
How do we navigate the DOM tree? What's the relationship between `parentElement`, `children`, and siblings?

**💡 Hint:** `parentElement` goes up the tree, `children` accesses all child elements, and `nextElementSibling` moves horizontally.

**✅ Answer & Explanation:**

```
UL
3
Second
```

**Why?**

1. `querySelector('li')` selects the **first** `<li>` ("First")
2. `parentElement` returns the parent: the `<ul>` element
3. `tagName` gives the tag name in uppercase: `"UL"`
4. The `<ul>` has 3 children (all three `<li>` elements)
5. `nextElementSibling` moves to the next sibling: the second `<li>` ("Second")

**DOM Tree Visualization:**

```
<ul> ← parentElement
  ├─ <li>First</li> ← listItem
  ├─ <li>Second</li> ← nextElementSibling
  └─ <li>Third</li>
```

**Navigation Properties:**

- `parentElement`: Go up one level
- `children`: All child elements (HTMLCollection)
- `nextElementSibling`: Next sibling at same level
- `previousElementSibling`: Previous sibling at same level
- `firstElementChild`: First child
- `lastElementChild`: Last child

---

### Challenge 10: setAttribute vs Property Assignment 🔴 Hard

```javascript
const input = document.querySelector("input");

input.value = "JavaScript";
console.log(input.value);
console.log(input.getAttribute("value"));

input.setAttribute("value", "DOM");
console.log(input.value);
console.log(input.getAttribute("value"));

// HTML: <input type="text" value="Hello">
```

**🤔 Think About It:**
Is there a difference between setting a property and setting an attribute? Do they always stay in sync?

**💡 Hint:** For input elements, the `value` **property** represents the current value, while the `value` **attribute** represents the initial/default value.

**✅ Answer & Explanation:**

```
JavaScript
Hello
DOM
DOM
```

**Step-by-Step:**

1. Setting `input.value = 'JavaScript'` changes the **current value** (property)
2. The **attribute** still has the original HTML value: `"Hello"`
3. `getAttribute('value')` reads the attribute, which hasn't changed
4. `setAttribute('value', 'DOM')` changes **both** the attribute and the property
5. After `setAttribute()`, both are `"DOM"`

**Critical Difference:**

```javascript
// Property: Current state (what user sees/interacts with)
input.value = "New Value"; // Changes what's displayed

// Attribute: Initial HTML value (default)
input.getAttribute("value"); // Original HTML value

// setAttribute updates both:
input.setAttribute("value", "Reset"); // Updates property AND attribute
```

**Real-World Implication:**

```javascript
// To reset an input to its original value:
input.value = input.getAttribute("value");

// Or better, use defaultValue:
input.value = input.defaultValue;
```

---

## Section 4: GUIDED PRACTICE PROBLEMS

Time to get your hands dirty! Each problem builds your DOM manipulation skills progressively. Follow the structure: read the task, check requirements, write your solution, then compare with the provided solution.

---

### Problem 1: Element Content Updater 🟢 Easy

**📋 Task:**
Create a function `updateHeading()` that changes the text of an `h1` element with id `"page-title"` to "DOM Manipulation Master".

**✅ Requirements:**

- [ ] Select the element by ID
- [ ] Update its text content
- [ ] Verify the change

**🧪 Test Cases:**

```javascript
// HTML: <h1 id="page-title">Original Title</h1>

updateHeading();
// Expected: h1 should display "DOM Manipulation Master"

const heading = document.getElementById("page-title");
console.log(heading.textContent); // "DOM Manipulation Master"
```

**💡 Solution:**

```javascript
function updateHeading() {
  // Step 1: Select the element
  const heading = document.getElementById("page-title");

  // Step 2: Update the text content
  heading.textContent = "DOM Manipulation Master";
}

// Call the function
updateHeading();
```

**Why This Works:**

1. `getElementById()` directly targets elements by their unique ID (fast and specific)
2. `textContent` is the safest way to update plain text (no HTML injection risks)
3. Changes are immediate and reflected in the browser

**❌ Common Mistakes:**

```javascript
// Mistake 1: Using innerHTML for plain text
heading.innerHTML = "DOM Manipulation Master"; // Works but unnecessary

// Mistake 2: Forgetting to select the element
textContent = "DOM Manipulation Master"; // Error: textContent is not defined

// Mistake 3: Wrong selector
const heading = document.querySelector("page-title"); // Missing # for ID
```

**🎓 What You Learned:**

- How to select elements by ID (most efficient selector)
- Difference between `textContent` and `innerHTML`
- Basic DOM manipulation workflow

**🚀 Extension Challenge:**
Modify the function to accept a parameter for the new text:

```javascript
function updateHeading(newText) {
  const heading = document.getElementById("page-title");
  heading.textContent = newText;
}

updateHeading("Welcome to JavaScript!");
```

---

### Problem 2: Class Toggle Controller 🟢 Easy

**📋 Task:**
Create a function `toggleHighlight()` that adds or removes the class `"highlight"` from an element with id `"target"`.

**✅ Requirements:**

- [ ] Select the element
- [ ] Toggle the "highlight" class
- [ ] Function should work repeatedly (add if missing, remove if present)

**🧪 Test Cases:**

```javascript
// HTML: <div id="target">Click me!</div>
// CSS: .highlight { background-color: yellow; }

toggleHighlight(); // First call: adds highlight
toggleHighlight(); // Second call: removes highlight
toggleHighlight(); // Third call: adds highlight again

// After first call:
console.log(document.getElementById("target").classList.contains("highlight")); // true

// After second call:
console.log(document.getElementById("target").classList.contains("highlight")); // false
```

**💡 Solution:**

```javascript
function toggleHighlight() {
  // Step 1: Select the target element
  const element = document.getElementById("target");

  // Step 2: Toggle the highlight class
  element.classList.toggle("highlight");
}

// Usage:
toggleHighlight(); // Adds highlight
toggleHighlight(); // Removes highlight
```

**Why This Works:**

1. `classList.toggle()` is perfect for on/off states
2. It handles both adding and removing automatically
3. No need for conditional logic – the browser does it for you

**Alternative Approaches:**

```javascript
// Manual approach (more code, same result):
function toggleHighlight() {
  const element = document.getElementById("target");

  if (element.classList.contains("highlight")) {
    element.classList.remove("highlight");
  } else {
    element.classList.add("highlight");
  }
}

// Using toggle with a boolean (less common):
function toggleHighlight(shouldHighlight) {
  const element = document.getElementById("target");
  element.classList.toggle("highlight", shouldHighlight);
}
```

**❌ Common Mistakes:**

```javascript
// Mistake 1: Using className instead of classList
element.className = "highlight"; // Replaces ALL classes

// Mistake 2: Manually checking instead of using toggle
if (element.classList.contains("highlight")) {
  element.classList.remove("highlight");
} else {
  element.classList.add("highlight");
}
// Works but verbose; toggle() is cleaner

// Mistake 3: Typo in class name
element.classList.toggle("hilight"); // Wrong class name
```

**🎓 What You Learned:**

- `classList.toggle()` is the cleanest way to add/remove classes
- Classes can be dynamically controlled for UI states
- `classList` methods are better than manipulating `className`

**🚀 Extension Challenge:**
Create a function that highlights multiple elements:

```javascript
function toggleHighlightAll(selector) {
  const elements = document.querySelectorAll(selector);
  elements.forEach((element) => {
    element.classList.toggle("highlight");
  });
}

// Usage:
toggleHighlightAll(".item"); // Toggles all elements with class "item"
```

---

### Problem 3: Dynamic List Creator 🟡 Medium

**📋 Task:**
Create a function `createList(items)` that takes an array of strings and creates an unordered list (`<ul>`) with list items (`<li>`) for each string. Append the list to a div with id `"list-container"`.

**✅ Requirements:**

- [ ] Accept an array of strings as parameter
- [ ] Create a `<ul>` element
- [ ] Create `<li>` elements for each array item
- [ ] Append list items to the `<ul>`
- [ ] Append the `<ul>` to the container

**🧪 Test Cases:**

```javascript
// HTML: <div id="list-container"></div>

createList(["Apple", "Banana", "Cherry"]);

// Expected Output:
// <div id="list-container">
//   <ul>
//     <li>Apple</li>
//     <li>Banana</li>
//     <li>Cherry</li>
//   </ul>
// </div>

createList(["JavaScript", "Python"]);
// Should add another list with 2 items
```

**💡 Solution:**

```javascript
function createList(items) {
  // Step 1: Select the container
  const container = document.getElementById("list-container");

  // Step 2: Create the <ul> element
  const ul = document.createElement("ul");

  // Step 3: Loop through items and create <li> elements
  items.forEach((item) => {
    const li = document.createElement("li");
    li.textContent = item;
    ul.appendChild(li);
  });

  // Step 4: Append the complete list to the container
  container.appendChild(ul);
}

// Usage:
createList(["Apple", "Banana", "Cherry"]);
```

**Step-by-Step Breakdown:**

```javascript
// What happens in memory:
// 1. createElement('ul') creates: <ul></ul> (in memory, not visible)
// 2. First iteration:
//    - createElement('li') → <li></li>
//    - li.textContent = 'Apple' → <li>Apple</li>
//    - appendChild(li) → <ul><li>Apple</li></ul>
// 3. Second iteration:
//    - <ul><li>Apple</li><li>Banana</li></ul>
// 4. Third iteration:
//    - <ul><li>Apple</li><li>Banana</li><li>Cherry</li></ul>
// 5. appendChild(ul) → Now visible in the DOM!
```

**Why This Works:**

1. We build the entire structure in memory first (efficient)
2. Only one DOM insertion (container.appendChild) – minimizes reflows
3. forEach is clean and readable for iteration

**❌ Common Mistakes:**

```javascript
// Mistake 1: Appending to container in the loop
items.forEach((item) => {
  const li = document.createElement("li");
  li.textContent = item;
  container.appendChild(li); // ❌ Creates li's without ul
});

// Mistake 2: Not creating new elements each iteration
const li = document.createElement("li"); // ❌ Created once outside loop
items.forEach((item) => {
  li.textContent = item;
  ul.appendChild(li); // Only the last item appears!
});

// Mistake 3: Using innerHTML in a loop (performance issue)
items.forEach((item) => {
  ul.innerHTML += `<li>${item}</li>`; // ❌ Recreates all items each time
});
```

**🎓 What You Learned:**

- How to create elements programmatically
- Building DOM structures before inserting (performance)
- Using forEach to process arrays into DOM elements
- Parent-child relationships in DOM creation

**🚀 Extension Challenge:**
Add CSS classes and data attributes:

```javascript
function createList(items, listClass = "default-list") {
  const container = document.getElementById("list-container");
  const ul = document.createElement("ul");
  ul.classList.add(listClass);

  items.forEach((item, index) => {
    const li = document.createElement("li");
    li.textContent = item;
    li.setAttribute("data-index", index);
    li.classList.add("list-item");
    ul.appendChild(li);
  });

  container.appendChild(ul);
}

createList(["Apple", "Banana"], "fruit-list");
```

---

### Problem 4: Attribute Manager 🟡 Medium

**📋 Task:**
Create a function `setImageProperties(imageSrc, altText)` that finds an `<img>` element with id `"main-image"` and updates its `src` and `alt` attributes.

**✅ Requirements:**

- [ ] Select the image element
- [ ] Set the `src` attribute to the provided URL
- [ ] Set the `alt` attribute to the provided text
- [ ] Handle invalid inputs gracefully

**🧪 Test Cases:**

```javascript
// HTML: <img id="main-image" src="placeholder.jpg" alt="Placeholder">

setImageProperties("photo.jpg", "Beautiful landscape");
// Expected: src="photo.jpg", alt="Beautiful landscape"

const img = document.getElementById("main-image");
console.log(img.src); // Ends with "photo.jpg"
console.log(img.alt); // "Beautiful landscape"

setImageProperties("avatar.png", "User profile picture");
// Expected: Updates to new values
```

**💡 Solution:**

```javascript
function setImageProperties(imageSrc, altText) {
  // Step 1: Select the image element
  const image = document.getElementById("main-image");

  // Step 2: Validate element exists
  if (!image) {
    console.error("Image element not found");
    return;
  }

  // Step 3: Update attributes
  image.setAttribute("src", imageSrc);
  image.setAttribute("alt", altText);

  // Alternative: Use properties directly
  // image.src = imageSrc;
  // image.alt = altText;
}

// Usage:
setImageProperties("landscape.jpg", "Mountain view at sunset");
```

**Property vs. setAttribute:**

```javascript
// Both approaches work, but there are subtle differences:

// Using properties (recommended for common attributes):
image.src = "photo.jpg";
image.alt = "Description";
// Pros: Cleaner syntax, type-safe
// Cons: Only works for standard HTML properties

// Using setAttribute (works for any attribute):
image.setAttribute("src", "photo.jpg");
image.setAttribute("data-id", "123"); // Custom attributes
// Pros: Works for any attribute, including custom ones
// Cons: Slightly more verbose
```

**Why This Works:**

1. Both properties and `setAttribute()` update the DOM immediately
2. Changing `src` triggers the browser to load the new image
3. `alt` text is important for accessibility

**❌ Common Mistakes:**

```javascript
// Mistake 1: Forgetting to check if element exists
const image = document.getElementById("main-image");
image.src = imageSrc; // ❌ Error if element not found

// Mistake 2: Using incorrect attribute names
image.setAttribute("source", imageSrc); // ❌ Wrong attribute name
image.setAttribute("alternative", altText); // ❌ Should be 'alt'

// Mistake 3: Not handling relative vs absolute URLs
image.src = "/images/photo.jpg"; // Be aware of path resolution
```

**Better Solution with Validation:**

```javascript
function setImageProperties(imageSrc, altText) {
  // Validate inputs
  if (!imageSrc || typeof imageSrc !== "string") {
    console.error("Invalid image source provided");
    return false;
  }

  if (!altText || typeof altText !== "string") {
    console.error("Invalid alt text provided");
    return false;
  }

  const image = document.getElementById("main-image");

  if (!image) {
    console.error("Image element not found");
    return false;
  }

  image.src = imageSrc;
  image.alt = altText;

  return true; // Success indicator
}
```

**🎓 What You Learned:**

- How to update element attributes
- Difference between property assignment and setAttribute()
- Importance of validation in DOM manipulation
- Accessibility considerations (alt text)

**🚀 Extension Challenge:**
Create a comprehensive image loader with error handling:

```javascript
function loadImage(imageSrc, altText, options = {}) {
  const image = document.getElementById("main-image");

  if (!image) return;

  // Set loading state
  image.classList.add("loading");

  // Create a temporary image to preload
  const tempImg = new Image();

  tempImg.onload = () => {
    image.src = imageSrc;
    image.alt = altText;
    image.classList.remove("loading");
    image.classList.add("loaded");

    if (options.onSuccess) options.onSuccess();
  };

  tempImg.onerror = () => {
    image.classList.remove("loading");
    image.classList.add("error");

    if (options.onError) options.onError();
  };

  tempImg.src = imageSrc;
}

// Usage:
loadImage("photo.jpg", "Landscape", {
  onSuccess: () => console.log("Image loaded!"),
  onError: () => console.log("Failed to load image"),
});
```

---

### Problem 5: Style Manipulator 🟡 Medium

**📋 Task:**
Create a function `applyStyles(elementId, stylesObject)` that accepts an element ID and an object containing CSS properties, then applies those styles to the element.

**✅ Requirements:**

- [ ] Accept element ID as string
- [ ] Accept styles object with CSS properties
- [ ] Apply all styles from the object
- [ ] Handle camelCase CSS properties (e.g., backgroundColor)

**🧪 Test Cases:**

```javascript
// HTML: <div id="box">Styled Box</div>

applyStyles("box", {
  backgroundColor: "blue",
  color: "white",
  padding: "20px",
  borderRadius: "8px",
});

// Expected: Box should have blue background, white text, padding, and rounded corners

const box = document.getElementById("box");
console.log(box.style.backgroundColor); // "blue"
console.log(box.style.color); // "white"
```

**💡 Solution:**

```javascript
function applyStyles(elementId, stylesObject) {
  // Step 1: Select the element
  const element = document.getElementById(elementId);

  // Step 2: Check if element exists
  if (!element) {
    console.error(`Element with id "${elementId}" not found`);
    return;
  }

  // Step 3: Loop through styles object and apply each property
  for (const property in stylesObject) {
    element.style[property] = stylesObject[property];
  }
}

// Usage:
applyStyles("box", {
  backgroundColor: "blue",
  color: "white",
  padding: "20px",
  borderRadius: "8px",
  fontSize: "18px",
});
```

**Modern Approach Using Object.assign:**

```javascript
function applyStyles(elementId, stylesObject) {
  const element = document.getElementById(elementId);

  if (!element) {
    console.error(`Element with id "${elementId}" not found`);
    return;
  }

  // Object.assign merges stylesObject into element.style
  Object.assign(element.style, stylesObject);
}
```

**Why This Works:**

1. `for...in` iterates over all properties in the object
2. `element.style[property]` uses bracket notation to access CSS properties
3. JavaScript automatically converts camelCase to kebab-case in CSS
4. All styles are applied inline (highest specificity except `!important`)

**CSS Property Naming:**

```javascript
// In CSS:               In JavaScript:
// background-color  →  backgroundColor
// font-size         →  fontSize
// border-radius     →  borderRadius
// margin-top        →  marginTop

// Example:
element.style.backgroundColor = "red"; // Sets background-color
element.style["background-color"] = "red"; // Also works but not recommended
```

**❌ Common Mistakes:**

```javascript
// Mistake 1: Using kebab-case instead of camelCase
element.style['background-color'] = 'red'; // Works but inconsistent
element.style.background-color = 'red'; // ❌ Syntax error

// Mistake 2: Forgetting units for numeric values
element.style.width = 100; // ❌ Invalid, needs units
element.style.width = '100px'; // ✅ Correct

// Mistake 3: Not validating the styles object
for (const property in stylesObject) {
  element.style[property] = stylesObject[property]; // What if property is invalid?
}

// Mistake 4: Overwriting existing styles unintentionally
element.style = stylesObject; // ❌ Replaces entire style object
```

**Enhanced Solution with Validation:**

```javascript
function applyStyles(elementId, stylesObject) {
  const element = document.getElementById(elementId);

  if (!element) {
    console.error(`Element with id "${elementId}" not found`);
    return false;
  }

  if (typeof stylesObject !== "object" || stylesObject === null) {
    console.error("Invalid styles object provided");
    return false;
  }

  // Apply styles
  for (const property in stylesObject) {
    if (stylesObject.hasOwnProperty(property)) {
      try {
        element.style[property] = stylesObject[property];
      } catch (error) {
        console.warn(`Failed to apply style "${property}": ${error.message}`);
      }
    }
  }

  return true;
}
```

**🎓 What You Learned:**

- How to apply multiple styles programmatically
- CSS property naming conventions in JavaScript (camelCase)
- Using `for...in` and `Object.assign()` for object manipulation
- Inline styles have high specificity

**🚀 Extension Challenge:**
Create an animation function using styles:

```javascript
function animateBox(elementId) {
  const element = document.getElementById(elementId);
  let width = 100;
  let growing = true;

  const interval = setInterval(() => {
    if (growing) {
      width += 5;
      if (width >= 300) growing = false;
    } else {
      width -= 5;
      if (width <= 100) growing = true;
    }

    applyStyles(elementId, {
      width: width + "px",
      height: width + "px",
      backgroundColor: width > 200 ? "red" : "blue",
    });
  }, 50);

  // Stop after 5 seconds
  setTimeout(() => clearInterval(interval), 5000);
}

animateBox("box");
```

---

### Problem 6: Element Finder and Counter 🟡 Medium

**📋 Task:**
Create a function `countElements(className)` that counts how many elements have a specific class and returns an object with the count and a reference to all matching elements.

**✅ Requirements:**

- [ ] Accept a class name as parameter
- [ ] Use `querySelectorAll()` to find all matching elements
- [ ] Return an object with count and elements array
- [ ] Handle cases where no elements are found

**🧪 Test Cases:**

```javascript
// HTML:
// <div class="box">Box 1</div>
// <div class="box">Box 2</div>
// <div class="box">Box 3</div>
// <p class="text">Paragraph</p>

const result = countElements("box");
console.log(result.count); // 3
console.log(result.elements.length); // 3
console.log(result.elements[0].textContent); // "Box 1"

const noMatch = countElements("nonexistent");
console.log(noMatch.count); // 0
console.log(noMatch.elements.length); // 0
```

**💡 Solution:**

```javascript
function countElements(className) {
  // Step 1: Find all elements with the class
  const elements = document.querySelectorAll(`.${className}`);

  // Step 2: Convert NodeList to Array (optional but useful)
  const elementsArray = Array.from(elements);

  // Step 3: Return object with count and elements
  return {
    count: elementsArray.length,
    elements: elementsArray,
  };
}

// Usage:
const boxResult = countElements("box");
console.log(`Found ${boxResult.count} elements`);

boxResult.elements.forEach((element, index) => {
  console.log(`Element ${index + 1}: ${element.textContent}`);
});
```

**Understanding querySelectorAll:**

```javascript
// querySelectorAll returns a NodeList, not an Array
const elements = document.querySelectorAll(".box");

console.log(elements instanceof NodeList); // true
console.log(elements instanceof Array); // false

// NodeList has forEach, but not map, filter, reduce
elements.forEach((el) => console.log(el)); // ✅ Works

// Convert to Array for full array methods:
const array = Array.from(elements);
const texts = array.map((el) => el.textContent); // ✅ Now we can use map

// Or use spread operator:
const array2 = [...elements];
```

**Why This Works:**

1. `querySelectorAll()` finds all matching elements (not just the first)
2. Adding `.` prefix to className converts it to a valid CSS selector
3. `Array.from()` enables full array methods
4. Returning an object provides both count and access to elements

**❌ Common Mistakes:**

```javascript
// Mistake 1: Using querySelector instead of querySelectorAll
const element = document.querySelector(".box"); // Only returns first match
console.log(element.length); // ❌ undefined (element is not an array)

// Mistake 2: Forgetting the dot for class selector
const elements = document.querySelectorAll("box"); // ❌ Looks for <box> tag
const elements = document.querySelectorAll(".box"); // ✅ Looks for class="box"

// Mistake 3: Assuming NodeList is an Array
const elements = document.querySelectorAll(".box");
const texts = elements.map((el) => el.textContent); // ❌ map is not a function

// Mistake 4: Not handling empty results
const elements = document.querySelectorAll(".nonexistent");
console.log(elements[0].textContent); // ❌ Error: Cannot read property of undefined
```

**Enhanced Solution with More Features:**

```javascript
function countElements(className, options = {}) {
  // Validate input
  if (!className || typeof className !== "string") {
    console.error("Valid class name required");
    return { count: 0, elements: [] };
  }

  const elements = document.querySelectorAll(`.${className}`);
  const elementsArray = Array.from(elements);

  const result = {
    count: elementsArray.length,
    elements: elementsArray,
  };

  // Optional: Get text content of all elements
  if (options.includeText) {
    result.texts = elementsArray.map((el) => el.textContent.trim());
  }

  // Optional: Get specific attribute from all elements
  if (options.getAttribute) {
    result.attributes = elementsArray.map((el) =>
      el.getAttribute(options.getAttribute)
    );
  }

  return result;
}

// Usage:
const result = countElements("box", {
  includeText: true,
  getAttribute: "data-id",
});

console.log(result);
// {
//   count: 3,
//   elements: [div, div, div],
//   texts: ['Box 1', 'Box 2', 'Box 3'],
//   attributes: ['1', '2', '3']
// }
```

**🎓 What You Learned:**

- Difference between `querySelector()` and `querySelectorAll()`
- NodeList vs Array and how to convert between them
- How to return structured data from functions
- Defensive programming (handling empty results)

**🚀 Extension Challenge:**
Create a function that finds elements and applies operations:

```javascript
function processElements(className, operation) {
  const elements = Array.from(document.querySelectorAll(`.${className}`));

  return elements.map((element, index) => {
    return operation(element, index);
  });
}

// Usage examples:
// 1. Get all text contents
const texts = processElements("box", (el) => el.textContent);

// 2. Add index to each element
processElements("box", (el, index) => {
  el.textContent += ` (${index + 1})`;
});

// 3. Collect all IDs
const ids = processElements("box", (el) => el.id);
```

---

### Problem 7: Parent-Child Navigator 🔴 Hard

**📋 Task:**
Create a function `getElementInfo(elementId)` that returns an object containing information about an element: its tag name, text content, number of children, parent's tag name, and all sibling elements.

**✅ Requirements:**

- [ ] Select element by ID
- [ ] Get tag name
- [ ] Get text content
- [ ] Count direct children
- [ ] Get parent's tag name
- [ ] Get array of all siblings (excluding the element itself)
- [ ] Handle cases where element has no parent or siblings

**🧪 Test Cases:**

```javascript
// HTML:
// <div id="container">
//   <p>First paragraph</p>
//   <div id="target">Target Element</div>
//   <p>Second paragraph</p>
// </div>

const info = getElementInfo("target");
console.log(info.tagName); // "DIV"
console.log(info.textContent); // "Target Element"
console.log(info.childrenCount); // 0
console.log(info.parentTag); // "DIV"
console.log(info.siblings.length); // 2 (two <p> elements)
```

**💡 Solution:**

```javascript
function getElementInfo(elementId) {
  // Step 1: Select the element
  const element = document.getElementById(elementId);

  // Step 2: Validate element exists
  if (!element) {
    console.error(`Element with id "${elementId}" not found`);
    return null;
  }

  // Step 3: Get parent element
  const parent = element.parentElement;

  // Step 4: Get siblings (all parent's children except this element)
  let siblings = [];
  if (parent) {
    siblings = Array.from(parent.children).filter((child) => child !== element);
  }

  // Step 5: Build and return info object
  return {
    tagName: element.tagName,
    textContent: element.textContent.trim(),
    childrenCount: element.children.length,
    parentTag: parent ? parent.tagName : null,
    siblings: siblings,
    siblingsCount: siblings.length,
  };
}

// Usage:
const info = getElementInfo("target");
console.log(info);
```

**Breaking Down the DOM Navigation:**

```javascript
// Understanding the DOM tree:
// <div id="container">               ← parent
//   <p>First</p>                     ← sibling
//   <div id="target">Target</div>    ← element
//   <p>Second</p>                    ← sibling
// </div>

const element = document.getElementById("target");

// Navigate UP:
element.parentElement; // → <div id="container">
element.parentElement.parentElement; // → Further up the tree

// Navigate DOWN:
element.children; // → HTMLCollection of direct children
element.firstElementChild; // → First child element
element.lastElementChild; // → Last child element

// Navigate SIDEWAYS:
element.nextElementSibling; // → <p>Second</p>
element.previousElementSibling; // → <p>First</p>
```

**Why This Works:**

1. `parentElement` gives us access to the parent node
2. `parent.children` gives us all siblings (including our element)
3. `filter()` removes our element from the siblings list
4. Conditional checks handle edge cases (no parent, no siblings)

**Alternative Approach Using Sibling Navigation:**

```javascript
function getElementInfo(elementId) {
  const element = document.getElementById(elementId);

  if (!element) return null;

  // Get siblings by traversing manually
  const siblings = [];
  let sibling = element.parentElement?.firstElementChild;

  while (sibling) {
    if (sibling !== element) {
      siblings.push(sibling);
    }
    sibling = sibling.nextElementSibling;
  }

  return {
    tagName: element.tagName,
    textContent: element.textContent.trim(),
    childrenCount: element.children.length,
    parentTag: element.parentElement?.tagName || null,
    siblings: siblings,
    siblingsCount: siblings.length,
  };
}
```

**❌ Common Mistakes:**

```javascript
// Mistake 1: Using childNodes instead of children
console.log(element.childNodes); // Includes text nodes and comments
console.log(element.children); // Only element nodes ✅

// Mistake 2: Not filtering out the element itself from siblings
const siblings = Array.from(parent.children); // Includes the element
const siblings = Array.from(parent.children).filter((c) => c !== element); // ✅

// Mistake 3: Forgetting to check if parent exists
const parentTag = element.parentElement.tagName; // ❌ Error if no parent
const parentTag = element.parentElement?.tagName || null; // ✅

// Mistake 4: Using innerHTML instead of textContent
console.log(element.innerHTML); // Includes HTML tags
console.log(element.textContent); // Plain text only ✅
```

**Enhanced Solution with More Details:**

```javascript
function getElementInfo(elementId) {
  const element = document.getElementById(elementId);

  if (!element) {
    return { error: "Element not found" };
  }

  const parent = element.parentElement;
  const siblings = parent
    ? Array.from(parent.children).filter((c) => c !== element)
    : [];

  return {
    // Basic info
    tagName: element.tagName,
    id: element.id,
    classList: Array.from(element.classList),

    // Content info
    textContent: element.textContent.trim(),
    innerHTML: element.innerHTML.trim(),

    // Children info
    childrenCount: element.children.length,
    hasChildren: element.children.length > 0,
    childTags: Array.from(element.children).map((c) => c.tagName),

    // Parent info
    parentTag: parent?.tagName || null,
    parentId: parent?.id || null,
    hasParent: parent !== null,

    // Siblings info
    siblings: siblings,
    siblingsCount: siblings.length,
    siblingTags: siblings.map((s) => s.tagName),

    // Position info
    previousSibling: element.previousElementSibling?.tagName || null,
    nextSibling: element.nextElementSibling?.tagName || null,
    isFirstChild: element === parent?.firstElementChild,
    isLastChild: element === parent?.lastElementChild,
  };
}

// Usage:
const fullInfo = getElementInfo("target");
console.log(fullInfo);
```

**🎓 What You Learned:**

- DOM tree navigation (up, down, sideways)
- Difference between `childNodes` and `children`
- Optional chaining (`?.`) for safe property access
- Building complex data structures from DOM information
- Edge case handling (no parent, no siblings)

**🚀 Extension Challenge:**
Create a function that visualizes the DOM tree structure:

```javascript
function printDOMTree(elementId, depth = 0) {
  const element = document.getElementById(elementId);
  if (!element) return;

  const indent = "  ".repeat(depth);
  console.log(
    `${indent}<${element.tagName.toLowerCase()}${
      element.id ? ` id="${element.id}"` : ""
    }>`
  );

  Array.from(element.children).forEach((child) => {
    printDOMTree(child.id || child.tagName, depth + 1);
  });

  console.log(`${indent}</${element.tagName.toLowerCase()}>`);
}

// Usage:
printDOMTree("container");
// Output:
// <div id="container">
//   <p>
//   </p>
//   <div id="target">
//   </div>
//   <p>
//   </p>
// </div>
```

---

### Problem 8: Dynamic Form Builder 🔴 Hard

**📋 Task:**
Create a function `createForm(formConfig)` that dynamically creates a form with various input types based on a configuration object. The form should include labels, inputs, and a submit button.

**✅ Requirements:**

- [ ] Accept a configuration object with form fields
- [ ] Create labels and inputs for each field
- [ ] Support different input types (text, email, password, number)
- [ ] Add a submit button
- [ ] Append the complete form to a container
- [ ] Set appropriate attributes (name, placeholder, required)

**🧪 Test Cases:**

```javascript
// HTML: <div id="form-container"></div>

const formConfig = {
  fields: [
    {
      label: "Username",
      type: "text",
      name: "username",
      placeholder: "Enter username",
      required: true,
    },
    {
      label: "Email",
      type: "email",
      name: "email",
      placeholder: "Enter email",
      required: true,
    },
    {
      label: "Password",
      type: "password",
      name: "password",
      placeholder: "Enter password",
      required: true,
    },
    {
      label: "Age",
      type: "number",
      name: "age",
      placeholder: "Enter age",
      required: false,
    },
  ],
  submitText: "Register",
};

createForm(formConfig);

// Expected Output: A complete form with 4 input fields and a submit button
// Each input should have a label, proper type, and attributes
```

**💡 Solution:**

```javascript
function createForm(formConfig) {
  // Step 1: Select the container
  const container = document.getElementById("form-container");

  if (!container) {
    console.error("Form container not found");
    return;
  }

  // Step 2: Create form element
  const form = document.createElement("form");
  form.setAttribute("id", "dynamic-form");

  // Step 3: Create form fields
  formConfig.fields.forEach((field) => {
    // Create wrapper div for each field
    const fieldWrapper = document.createElement("div");
    fieldWrapper.classList.add("form-field");

    // Create label
    const label = document.createElement("label");
    label.textContent = field.label;
    label.setAttribute("for", field.name);

    // Create input
    const input = document.createElement("input");
    input.setAttribute("type", field.type);
    input.setAttribute("name", field.name);
    input.setAttribute("id", field.name);

    if (field.placeholder) {
      input.setAttribute("placeholder", field.placeholder);
    }

    if (field.required) {
      input.setAttribute("required", "true");
    }

    // Append label and input to wrapper
    fieldWrapper.appendChild(label);
    fieldWrapper.appendChild(input);

    // Append wrapper to form
    form.appendChild(fieldWrapper);
  });

  // Step 4: Create submit button
  const submitButton = document.createElement("button");
  submitButton.setAttribute("type", "submit");
  submitButton.textContent = formConfig.submitText || "Submit";
  submitButton.classList.add("submit-button");

  form.appendChild(submitButton);

  // Step 5: Append form to container
  container.appendChild(form);

  return form;
}

// Usage:
const formConfig = {
  fields: [
    {
      label: "Username",
      type: "text",
      name: "username",
      placeholder: "Enter username",
      required: true,
    },
    {
      label: "Email",
      type: "email",
      name: "email",
      placeholder: "Enter email",
      required: true,
    },
    {
      label: "Password",
      type: "password",
      name: "password",
      placeholder: "Enter password",
      required: true,
    },
    {
      label: "Age",
      type: "number",
      name: "age",
      placeholder: "Enter age",
      required: false,
    },
  ],
  submitText: "Register",
};

createForm(formConfig);
```

**Understanding the Structure:**

```javascript
// DOM structure created:
// <form id="dynamic-form">
//   <div class="form-field">
//     <label for="username">Username</label>
//     <input type="text" name="username" id="username" placeholder="Enter username" required>
//   </div>
//   <div class="form-field">
//     <label for="email">Email</label>
//     <input type="email" name="email" id="email" placeholder="Enter email" required>
//   </div>
//   ...
//   <button type="submit" class="submit-button">Register</button>
// </form>
```

**Why This Works:**

1. We build the complete structure before inserting into DOM (performance)
2. Each field is wrapped in a div for styling flexibility
3. Labels are properly associated with inputs using `for` and `id` attributes
4. Attributes are set conditionally (placeholder, required)
5. The function is data-driven – easy to create different forms

**❌ Common Mistakes:**

```javascript
// Mistake 1: Not wrapping fields in containers
form.appendChild(label);
form.appendChild(input); // ❌ Hard to style, no structure

// Better:
const wrapper = document.createElement("div");
wrapper.appendChild(label);
wrapper.appendChild(input);
form.appendChild(wrapper);

// Mistake 2: Using innerHTML for the entire form
form.innerHTML = `<input type="text">`; // ❌ Event listeners lost, security risk

// Mistake 3: Not associating labels with inputs
label.textContent = "Username";
// ❌ Missing: label.setAttribute('for', 'username');

// Mistake 4: Forgetting to set input type
const input = document.createElement("input");
// ❌ Missing: input.setAttribute('type', 'text');
```

**Enhanced Solution with Validation:**

```javascript
function createForm(formConfig) {
  const container = document.getElementById("form-container");

  if (!container) {
    console.error("Form container not found");
    return null;
  }

  // Validate config
  if (!formConfig.fields || !Array.isArray(formConfig.fields)) {
    console.error("Invalid form configuration");
    return null;
  }

  const form = document.createElement("form");
  form.setAttribute("id", "dynamic-form");

  formConfig.fields.forEach((field) => {
    const fieldWrapper = document.createElement("div");
    fieldWrapper.classList.add("form-field");

    // Label
    const label = document.createElement("label");
    label.textContent = field.label;
    label.setAttribute("for", field.name);
    if (field.required) {
      label.innerHTML += ' <span class="required">*</span>';
    }

    // Input
    const input = document.createElement("input");
    input.setAttribute("type", field.type || "text");
    input.setAttribute("name", field.name);
    input.setAttribute("id", field.name);

    if (field.placeholder) {
      input.setAttribute("placeholder", field.placeholder);
    }

    if (field.required) {
      input.setAttribute("required", "true");
      input.setAttribute("aria-required", "true"); // Accessibility
    }

    // Additional validation attributes
    if (field.type === "email") {
      input.setAttribute("pattern", "[a-z0-9._%+-]+@[a-z0-9.-]+\\.[a-z]{2,}$");
    }

    if (field.minLength) {
      input.setAttribute("minlength", field.minLength);
    }

    if (field.maxLength) {
      input.setAttribute("maxlength", field.maxLength);
    }

    fieldWrapper.appendChild(label);
    fieldWrapper.appendChild(input);
    form.appendChild(fieldWrapper);
  });

  // Submit button
  const submitButton = document.createElement("button");
  submitButton.setAttribute("type", "submit");
  submitButton.textContent = formConfig.submitText || "Submit";
  submitButton.classList.add("submit-button");

  form.appendChild(submitButton);

  // Add form submit handler (optional)
  form.addEventListener("submit", (e) => {
    e.preventDefault();
    const formData = new FormData(form);
    const data = Object.fromEntries(formData);
    console.log("Form submitted:", data);

    if (formConfig.onSubmit) {
      formConfig.onSubmit(data);
    }
  });

  container.appendChild(form);

  return form;
}

// Usage with submit handler:
createForm({
  fields: [
    {
      label: "Username",
      type: "text",
      name: "username",
      required: true,
      minLength: 3,
    },
    { label: "Email", type: "email", name: "email", required: true },
    {
      label: "Password",
      type: "password",
      name: "password",
      required: true,
      minLength: 8,
    },
  ],
  submitText: "Register",
  onSubmit: (data) => {
    console.log("Processing registration:", data);
    // Send to server, etc.
  },
});
```

**🎓 What You Learned:**

- Building complex DOM structures programmatically
- Creating forms dynamically from configuration objects
- Setting multiple attributes conditionally
- Proper label-input associations for accessibility
- Data-driven UI development patterns
- Form validation attributes

**🚀 Extension Challenge:**
Add support for select dropdowns and textareas:

```javascript
function createFormField(field) {
  const fieldWrapper = document.createElement("div");
  fieldWrapper.classList.add("form-field");

  const label = document.createElement("label");
  label.textContent = field.label;
  label.setAttribute("for", field.name);

  let input;

  if (field.type === "select") {
    input = document.createElement("select");
    input.setAttribute("name", field.name);
    input.setAttribute("id", field.name);

    field.options.forEach((option) => {
      const optionElement = document.createElement("option");
      optionElement.value = option.value;
      optionElement.textContent = option.text;
      input.appendChild(optionElement);
    });
  } else if (field.type === "textarea") {
    input = document.createElement("textarea");
    input.setAttribute("name", field.name);
    input.setAttribute("id", field.name);
    input.setAttribute("rows", field.rows || 4);
  } else {
    input = document.createElement("input");
    input.setAttribute("type", field.type);
    input.setAttribute("name", field.name);
    input.setAttribute("id", field.name);
  }

  if (field.placeholder) {
    input.setAttribute("placeholder", field.placeholder);
  }

  if (field.required) {
    input.setAttribute("required", "true");
  }

  fieldWrapper.appendChild(label);
  fieldWrapper.appendChild(input);

  return fieldWrapper;
}

// Usage:
const advancedConfig = {
  fields: [
    { label: "Name", type: "text", name: "name", required: true },
    {
      label: "Country",
      type: "select",
      name: "country",
      options: [
        { value: "us", text: "United States" },
        { value: "uk", text: "United Kingdom" },
        { value: "in", text: "India" },
      ],
      required: true,
    },
    {
      label: "Bio",
      type: "textarea",
      name: "bio",
      rows: 5,
      placeholder: "Tell us about yourself",
    },
  ],
  submitText: "Submit",
};
```

---

## 5. DEBUG CHALLENGES

These challenges simulate real-world scenarios where code is broken. Since we haven't covered Events (Day 19) yet, these scripts run immediately when the page loads.

### Challenge 1: The Disappearing Classes

**Context:** A developer is trying to add a "highlight" class to a selected item, but notices that the item loses all its previous styling.

**Broken Code:**

```javascript
// HTML: <div id="card" class="card-base shadow-lg">...</div>

const card = document.getElementById("card");

// Trying to add the highlight class
card.className = "highlight";
```

**Your Task:** Fix the code so the `highlight` class is added without removing `card-base` and `shadow-lg`.

**Solution:**

```javascript
const card = document.getElementById("card");

// INCORRECT: card.className = "highlight"; (Overwrites everything)

// CORRECT: Use classList
card.classList.add("highlight");
```

_Why it broke:_ `className` is a string setter. It replaces the entire string of classes. `classList.add` appends to the list.

---

### Challenge 2: The Infinite Clone

**Context:** You are trying to display the same "Warning Icon" in two different places on the page.

**Broken Code:**

```javascript
const icon = document.createElement("img");
icon.src = "warning.png";

const header = document.querySelector("header");
const footer = document.querySelector("footer");

header.appendChild(icon); // Put it in header
footer.appendChild(icon); // Put it in footer
```

**Your Task:** Why does the icon only appear in the footer? Fix it so it appears in both.

**Solution:**

```javascript
const icon = document.createElement("img");
icon.src = "warning.png";

const header = document.querySelector("header");
const footer = document.querySelector("footer");

header.appendChild(icon);
// CORRECT: You must clone the node to use it again
footer.appendChild(icon.cloneNode(true));
```

_Why it broke:_ A DOM Node is an object reference. It can only exist in one place in the DOM tree at a time. If you append an existing node to a new parent, it gets **moved**, not copied.

---

### Challenge 3: The Style Syntax Error

**Context:** A junior dev is trying to change the background color of a box using JavaScript.

**Broken Code:**

```javascript
const box = document.querySelector('.box');
box.style.background-color = 'blue';
box.style.margin-top = '20px';

```

**Your Task:** The console is throwing a syntax error. Fix the property names.

**Solution:**

```javascript
const box = document.querySelector(".box");

// CORRECT: Use camelCase for CSS properties in JS
box.style.backgroundColor = "blue";
box.style.marginTop = "20px";
```

_Why it broke:_ In JavaScript, the minus sign `-` is a subtraction operator. `background-color` is interpreted as `variable background` minus `variable color`.

---

### Challenge 4: The Dangerous Input

**Context:** You are building a user profile display. You accept a bio string and display it.

**Broken Code:**

```javascript
const bioContainer = document.querySelector("#user-bio");
const userBio = "I love coding! <img src='x' onerror='alert(\"Hacked!\")'>";

// Displaying the bio
bioContainer.innerHTML = userBio;
```

**Your Task:** Prevent the `alert` from executing while still displaying the text.

**Solution:**

```javascript
const bioContainer = document.querySelector("#user-bio");
const userBio = "I love coding! <img src='x' onerror='alert(\"Hacked!\")'>";

// CORRECT: Use textContent to treat input as plain text
bioContainer.textContent = userBio;
```

_Why it broke:_ `innerHTML` parses strings as HTML code. This allows Cross-Site Scripting (XSS) attacks. `textContent` escapes the HTML, rendering the tags as visible text rather than executing them.

---

### Challenge 5: Styling Non-Existent Elements

**Context:** You want to style a list item, but the script runs before the HTML is fully loaded (assuming script is in `<head>`).

**Broken Code:**

```javascript
// HTML is not parsed yet
const item = document.querySelector(".menu-item");
item.style.color = "red";
```

_Error:_ `Uncaught TypeError: Cannot read properties of null (reading 'style')`

**Your Task:** Add a safety check.

**Solution:**

```javascript
const item = document.querySelector(".menu-item");

// CORRECT: Always check if the element exists
if (item) {
  item.style.color = "red";
} else {
  console.warn("Menu item not found!");
}
```

_Why it broke:_ If `querySelector` doesn't find a match, it returns `null`. You cannot access `.style` on `null`.

---

## 6. COMMON PITFALLS & ANTI-PATTERNS

Creating clean DOM code is an art. Here are the mistakes beginners make that slow down websites or cause bugs.

### 1. The "innerHTML" Hammer

- **What Beginners Do:** Using `innerHTML` for everything, even simple text updates.

```javascript
// Bad
element.innerHTML = "Hello World";
```

- **Why Problematic:**

1. **Security:** Opens the door to XSS attacks if data is external.
2. **Performance:** The browser has to spin up the HTML parser, check for tags, and rebuild the DOM subtree, which is heavy.
3. **Destruction:** It wipes out all event listeners attached to child elements.

- **The Right Way:** Use `textContent` for text, and `createElement`/`append` for new elements.

```javascript
// Good
element.textContent = "Hello World";
```

- **How to Recognize:** If you aren't writing actual HTML tags (like `<strong>`), don't use `innerHTML`.

### 2. The Loop Reflow

- **What Beginners Do:** Appending elements one by one inside a loop.

```javascript
const list = document.querySelector("ul");
for (let i = 0; i < 100; i++) {
  const item = document.createElement("li"); // Create
  list.appendChild(item); // Write to DOM immediately
}
```

- **Why Problematic:** Every time you touch the DOM, the browser has to recalculate layout and paint (Reflow/Repaint). Doing this 100 times freezes the UI.
- **The Right Way:** Use a `DocumentFragment`. It's a lightweight container that isn't part of the DOM tree.

```javascript
const list = document.querySelector("ul");
const fragment = document.createDocumentFragment(); // Invisible container

for (let i = 0; i < 100; i++) {
  const item = document.createElement("li");
  fragment.appendChild(item); // Add to fragment (cheap)
}

list.appendChild(fragment); // Write to DOM once (efficient)
```

### 3. Inline Style Spaghetti

- **What Beginners Do:** Writing complex CSS in JavaScript.

```javascript
div.style.backgroundColor = "red";
div.style.color = "white";
div.style.padding = "20px";
div.style.borderRadius = "5px";
```

- **Why Problematic:** It mixes logic (JS) with presentation (CSS). It creates high specificities that are hard to override later.
- **The Right Way:** Define a class in CSS and toggle it in JS.

```css
/* style.css */
.alert-box {
  background: red;
  color: white;
  padding: 20px;
}
```

```javascript
// script.js
div.classList.add("alert-box");
```

### 4. Selecting inside Loops

- **What Beginners Do:**

```javascript
const data = [1, 2, 3];
data.forEach((num) => {
  // Querying the container 3 times!
  const container = document.getElementById("app");
  container.append(num);
});
```

- **Why Problematic:** DOM selection (`getElementById`, `querySelector`) is expensive. Don't force the browser to search the whole document repeatedly.
- **The Right Way:** Cache the selector outside the loop.

```javascript
const container = document.getElementById("app"); // Search once
data.forEach((num) => {
  container.append(num);
});
```

### 5. Ignoring Live vs. Static Collections

- **What Beginners Do:** Assuming a list of elements stays the same.
- **Why Problematic:** `getElementsByClassName` returns a **Live** HTMLCollection. If you change a class name while iterating over it, the list length changes in real-time, causing logic errors (skipping elements).
- **The Right Way:** Use `querySelectorAll` (returns a **Static** NodeList) or convert to an Array using `Array.from()`.

---

## Section 7: Knowledge Check MCQs

### Instructions

Test your understanding of DOM manipulation concepts. Choose the best answer for each question. Don't just guess—think through why each option is correct or incorrect.

---

### Question 1: Basic DOM Selection

**Difficulty: Easy**

What will `document.getElementById('header')` return if no element with id "header" exists?

A) `undefined`  
B) `null`  
C) An empty array `[]`  
D) Throws an error

**✓ Correct Answer: B) `null`**

**Explanation:**  
`getElementById()` returns `null` when no matching element is found. It never throws an error or returns undefined. This is important for error handling—always check if the result is `null` before manipulating the element.

```javascript
const header = document.getElementById("header");
if (header) {
  // Safe to manipulate
  header.textContent = "New Header";
} else {
  console.log("Element not found");
}
```

**Interview Tip:**  
Interviewers love asking about `null` vs `undefined`. Remember: DOM methods return `null` for "not found", not `undefined`.

**Key Concept:** Always validate DOM selections before manipulation to prevent runtime errors.

---

### Question 2: querySelector vs querySelectorAll

**Difficulty: Easy**

What's the main difference between `querySelector()` and `querySelectorAll()`?

A) `querySelector()` is faster than `querySelectorAll()`  
B) `querySelector()` returns the first match, `querySelectorAll()` returns all matches  
C) `querySelector()` only works with IDs, `querySelectorAll()` works with any selector  
D) `querySelectorAll()` is deprecated

**✓ Correct Answer: B) `querySelector()` returns the first match, `querySelectorAll()` returns all matches**

**Explanation:**  
`querySelector()` returns a single element (the first match) or `null`. `querySelectorAll()` always returns a NodeList containing all matches (which can be empty but never `null`).

```javascript
// Returns first paragraph
const firstP = document.querySelector("p");

// Returns NodeList of all paragraphs
const allPs = document.querySelectorAll("p");
console.log(allPs.length); // Number of paragraphs
```

**Interview Tip:**  
Mention that `querySelectorAll()` returns a **static NodeList**, not a live collection. Changes to the DOM won't update the NodeList.

**Key Concept:** Choose `querySelector()` when you need one element, `querySelectorAll()` when you need multiple.

---

### Question 3: innerHTML Security

**Difficulty: Medium**

Why is using `innerHTML` with user input potentially dangerous?

A) It's slower than other methods  
B) It can cause XSS (Cross-Site Scripting) attacks  
C) It doesn't work with modern browsers  
D) It only accepts strings

**✓ Correct Answer: B) It can cause XSS (Cross-Site Scripting) attacks**

**Explanation:**  
`innerHTML` parses and executes any HTML/JavaScript in the string, including malicious scripts. If user input isn't sanitized, attackers can inject harmful code.

```javascript
// DANGEROUS - Never do this with user input
const userInput = "<img src=x onerror=\"alert('Hacked!')\">";
element.innerHTML = userInput; // Script executes!

// SAFE - Use textContent for plain text
element.textContent = userInput; // Renders as text, doesn't execute
```

**Interview Tip:**  
Show security awareness by mentioning alternatives: `textContent` for text, `createElement()` with `appendChild()` for structured content.

**Key Concept:** Never use `innerHTML` with unsanitized user input. Prefer `textContent` or DOM creation methods.

---

### Question 4: NodeList vs Array

**Difficulty: Medium**

What's true about NodeLists returned by `querySelectorAll()`?

A) They are regular JavaScript arrays  
B) They have all array methods like `map()`, `filter()`, `reduce()`  
C) They are array-like but don't have most array methods  
D) They automatically update when DOM changes

**✓ Correct Answer: C) They are array-like but don't have most array methods**

**Explanation:**  
NodeLists have `length` and indexed access but lack most array methods. They only have `forEach()`, `entries()`, `keys()`, and `values()`. To use array methods, convert them first.

```javascript
const divs = document.querySelectorAll("div");

// This WON'T work
// divs.map(div => div.textContent); // Error!

// Convert to array first
const divsArray = Array.from(divs);
// OR: [...divs]

// Now array methods work
const texts = divsArray.map((div) => div.textContent);
```

**Interview Tip:**  
Mention both conversion methods: `Array.from()` and spread operator `[...]`. Show you know the difference between NodeList and HTMLCollection.

**Key Concept:** Convert NodeList to array when you need array methods beyond `forEach()`.

---

### Question 5: getAttribute vs Property Access

**Difficulty: Medium**

What's the difference between `element.getAttribute('id')` and `element.id`?

A) They're identical—use either one  
B) `getAttribute()` returns the HTML attribute value, property access returns the current property value  
C) Property access is slower  
D) `getAttribute()` only works for custom attributes

**✓ Correct Answer: B) `getAttribute()` returns the HTML attribute value, property access returns the current property value**

**Explanation:**  
Properties can differ from attributes. Properties reflect current state, attributes show initial HTML values. For some attributes like `href`, the property returns an absolute URL while the attribute returns what's written in HTML.

```javascript
<a href="/about" id="link">
  About
</a>;

const link = document.getElementById("link");

// Property - absolute URL
console.log(link.href); // "http://example.com/about"

// Attribute - exactly what's in HTML
console.log(link.getAttribute("href")); // "/about"

// For id, they're usually the same
console.log(link.id); // "link"
console.log(link.getAttribute("id")); // "link"
```

**Interview Tip:**  
Mention that `getAttribute()` is necessary for custom data attributes and when you need the exact HTML attribute value.

**Key Concept:** Use property access for standard properties, `getAttribute()` for custom attributes or when you need exact HTML values.

---

### Question 6: Creating Elements

**Difficulty: Easy**

What method creates a new DOM element?

A) `document.newElement('div')`  
B) `document.createElement('div')`  
C) `document.makeElement('div')`  
D) `new Element('div')`

**✓ Correct Answer: B) `document.createElement('div')`**

**Explanation:**  
`createElement()` is the standard method to create new elements. The created element exists in memory but isn't visible until you append it to the DOM.

```javascript
// Create element
const newDiv = document.createElement("div");

// Configure it
newDiv.textContent = "Hello World";
newDiv.className = "box";

// Add to DOM (now it's visible)
document.body.appendChild(newDiv);
```

**Interview Tip:**  
Show you understand the two-step process: create the element, then attach it to the DOM. Elements are useless until appended.

**Key Concept:** Created elements must be appended to the DOM to be visible.

---

### Question 7: classList Methods

**Difficulty: Medium**

Which `classList` method checks if a class exists without modifying it?

A) `classList.has('classname')`  
B) `classList.includes('classname')`  
C) `classList.contains('classname')`  
D) `classList.check('classname')`

**✓ Correct Answer: C) `classList.contains('classname')`**

**Explanation:**  
`classList.contains()` returns `true` or `false` without modifying classes. This is useful for conditional logic.

```javascript
const button = document.querySelector("button");

if (button.classList.contains("active")) {
  console.log("Button is active");
} else {
  button.classList.add("active");
}

// Toggle is even simpler
button.classList.toggle("active"); // Adds if absent, removes if present
```

**Interview Tip:**  
Mention all four main classList methods: `add()`, `remove()`, `toggle()`, `contains()`. Show you know `toggle()` returns a boolean.

**Key Concept:** `classList` provides cleaner class manipulation than string manipulation of `className`.

---

### Question 8: textContent vs innerText

**Difficulty: Medium**

What's the key difference between `textContent` and `innerText`?

A) `textContent` is faster and returns all text including hidden elements  
B) They're identical—use either one  
C) `innerText` is newer and more reliable  
D) `textContent` only works in modern browsers

**✓ Correct Answer: A) `textContent` is faster and returns all text including hidden elements**

**Explanation:**  
`textContent` returns all text, even from hidden elements (`display: none`), and doesn't trigger reflows. `innerText` considers CSS styling and only returns visible text, making it slower.

```javascript
<div id="test">
  Visible text
  <span style="display: none;">Hidden text</span>
</div>;

const div = document.getElementById("test");

console.log(div.textContent);
// "Visible text Hidden text" (includes hidden)

console.log(div.innerText);
// "Visible text" (excludes hidden)
```

**Interview Tip:**  
Prefer `textContent` for performance unless you specifically need to respect CSS visibility. Most use cases should use `textContent`.

**Key Concept:** Use `textContent` by default—it's faster and more predictable.

---

### Question 9: Removing Elements

**Difficulty: Easy**

What's the modern way to remove an element from the DOM?

A) `element.delete()`  
B) `element.remove()`  
C) `element.removeElement()`  
D) `document.remove(element)`

**✓ Correct Answer: B) `element.remove()`**

**Explanation:**  
The `.remove()` method is the modern, simple way. The older approach requires accessing the parent: `element.parentNode.removeChild(element)`.

```javascript
const oldWay = document.querySelector(".old-element");
oldWay.parentNode.removeChild(oldWay); // Old way

const newWay = document.querySelector(".new-element");
newWay.remove(); // Modern, cleaner way
```

**Interview Tip:**  
Mention that `.remove()` is ES6+ but widely supported. Show you know the older `removeChild()` pattern for legacy code.

**Key Concept:** Use `.remove()` for modern code—it's simpler and more intuitive.

---

### Question 10: appendChild vs append

**Difficulty: Medium**

What can `append()` do that `appendChild()` cannot?

A) Append multiple elements at once  
B) Append text directly without creating text nodes  
C) Both A and B  
D) They're completely identical

**✓ Correct Answer: C) Both A and B**

**Explanation:**  
`append()` is more flexible: it accepts multiple arguments (elements or strings) and returns `undefined`. `appendChild()` only accepts one Node and returns it.

```javascript
const container = document.querySelector(".container");

// appendChild - one element only, returns the element
const div = document.createElement("div");
const returned = container.appendChild(div);
console.log(returned === div); // true

// append - multiple elements/strings, returns undefined
container.append(
  document.createElement("p"),
  "Some text",
  document.createElement("span")
);
```

**Interview Tip:**  
Mention that `append()` is newer (ES6) and doesn't return the appended element, while `appendChild()` does. Choose based on whether you need the return value.

**Key Concept:** Use `append()` for flexibility, `appendChild()` when you need the return value for chaining.

---

### Question 11: Style Manipulation

**Difficulty: Easy**

How do you set an element's background color to red using JavaScript?

A) `element.style.background-color = 'red'`  
B) `element.style.backgroundColor = 'red'`  
C) `element.style['background-color'] = 'red'`  
D) Both B and C

**✓ Correct Answer: D) Both B and C**

**Explanation:**  
CSS properties with hyphens become camelCase in JavaScript (`backgroundColor`), but you can also use bracket notation with the hyphenated name.

```javascript
const box = document.querySelector(".box");

// CamelCase (preferred)
box.style.backgroundColor = "red";
box.style.fontSize = "20px";

// Bracket notation (works but less common)
box.style["background-color"] = "blue";
box.style["font-size"] = "24px";
```

**Interview Tip:**  
Mention that inline styles have high specificity and override CSS files. For multiple styles, consider toggling classes instead of setting styles directly.

**Key Concept:** CamelCase is standard for style properties in JavaScript.

---

### Question 12: Data Attributes

**Difficulty: Medium**

How do you access a custom `data-user-id="123"` attribute?

A) `element.data.userId`  
B) `element.dataset.userId`  
C) `element.getAttribute('userId')`  
D) `element.dataAttributes.userId`

**✓ Correct Answer: B) `element.dataset.userId`**

**Explanation:**  
The `dataset` property provides access to all `data-*` attributes. The attribute name is converted to camelCase (remove `data-` prefix and convert hyphens).

```javascript
<div id="user" data-user-id="123" data-user-name="John"></div>;

const user = document.getElementById("user");

// Reading data attributes
console.log(user.dataset.userId); // "123"
console.log(user.dataset.userName); // "John"

// Setting data attributes
user.dataset.userAge = "30";
// Creates: data-user-age="30"

// Alternative (but more verbose)
console.log(user.getAttribute("data-user-id")); // "123"
```

**Interview Tip:**  
Explain that `data-*` attributes are great for storing custom data in HTML without cluttering JavaScript variables. They're also accessible via CSS for styling.

**Key Concept:** Use `dataset` for clean, semantic access to custom data attributes.

---

### Question 13: Document Ready

**Difficulty: Medium**

What's the modern alternative to jQuery's `$(document).ready()`?

A) `window.onload = function() {}`  
B) `document.addEventListener('DOMContentLoaded', function() {})`  
C) `document.ready(function() {})`  
D) `window.addEventListener('load', function() {})`

**✓ Correct Answer: B) `document.addEventListener('DOMContentLoaded', function() {})`**

**Explanation:**  
`DOMContentLoaded` fires when HTML is parsed and DOM is built (excluding images/stylesheets). `window.onload` waits for ALL resources including images. `DOMContentLoaded` is usually what you want.

```javascript
// Fires when DOM is ready (fast)
document.addEventListener("DOMContentLoaded", () => {
  console.log("DOM ready");
  // Manipulate DOM safely here
});

// Fires when everything loads (slower)
window.addEventListener("load", () => {
  console.log("All resources loaded");
  // Use when you need images/stylesheets loaded
});
```

**Interview Tip:**  
Mention that if your script is at the bottom of `<body>` with `defer`, you might not need these events at all—DOM is already loaded.

**Key Concept:** Use `DOMContentLoaded` for DOM manipulation, `load` when you need all resources loaded.

---

### Question 14: getAttribute with Non-existent Attributes

**Difficulty: Easy**

What does `element.getAttribute('nonexistent')` return?

A) `undefined`  
B) `null`  
C) `""` (empty string)  
D) Throws an error

**✓ Correct Answer: B) `null`**

**Explanation:**  
Just like `getElementById()`, `getAttribute()` returns `null` for non-existent attributes, not `undefined` or an error.

```javascript
const div = document.querySelector("div");

console.log(div.getAttribute("data-missing")); // null
console.log(div.getAttribute("id")); // actual id value or null

// Safe checking
const customValue = div.getAttribute("data-custom");
if (customValue !== null) {
  console.log("Attribute exists:", customValue);
}
```

**Interview Tip:**  
Show consistency knowledge: DOM methods return `null` for "not found" scenarios. This is different from JavaScript's `undefined`.

**Key Concept:** Always check for `null` when using `getAttribute()` on potentially missing attributes.

---

### Question 15: insertAdjacentHTML Position

**Difficulty: Medium**

What does `insertAdjacentHTML('beforeend', html)` do?

A) Inserts HTML before the element  
B) Inserts HTML after the element  
C) Inserts HTML as the first child inside the element  
D) Inserts HTML as the last child inside the element

**✓ Correct Answer: D) Inserts HTML as the last child inside the element**

**Explanation:**  
`insertAdjacentHTML()` has four positions: `'beforebegin'` (before element), `'afterbegin'` (first child), `'beforeend'` (last child), `'afterend'` (after element).

```javascript
<div id="container">
  <p>Existing content</p>
</div>;

const container = document.getElementById("container");

// beforeend - adds as last child (like appendChild)
container.insertAdjacentHTML("beforeend", "<p>New last child</p>");

// afterbegin - adds as first child
container.insertAdjacentHTML("afterbegin", "<p>New first child</p>");

/* Result:
<div id="container">
    <p>New first child</p>
    <p>Existing content</p>
    <p>New last child</p>
</div>
*/
```

**Interview Tip:**  
Mention that `insertAdjacentHTML()` is more flexible than `innerHTML` because it doesn't replace existing content. Remember the four positions with "begin/end" and "before/after".

**Key Concept:** Use `insertAdjacentHTML()` when you need precise control over insertion position.

---

### Question 16: Live vs Static Collections

**Difficulty: Hard**

Which returns a live collection that automatically updates when DOM changes?

A) `querySelectorAll()`  
B) `getElementsByClassName()`  
C) Both return live collections  
D) Neither returns live collections

**✓ Correct Answer: B) `getElementsByClassName()`**

**Explanation:**  
`getElementsByClassName()`, `getElementsByTagName()`, and `children` return **live HTMLCollections** that update automatically. `querySelectorAll()` returns a **static NodeList** that doesn't update.

```javascript
<div class="box">Box 1</div>;

// Live collection
const liveBoxes = document.getElementsByClassName("box");
console.log(liveBoxes.length); // 1

// Static collection
const staticBoxes = document.querySelectorAll(".box");
console.log(staticBoxes.length); // 1

// Add a new box
document.body.insertAdjacentHTML("beforeend", '<div class="box">Box 2</div>');

console.log(liveBoxes.length); // 2 (auto-updated!)
console.log(staticBoxes.length); // 1 (still old value)
```

**Interview Tip:**  
This is a favorite interview question! Explain that live collections can cause bugs if you're modifying the DOM while iterating. Static NodeLists are safer for most use cases.

**Key Concept:** Prefer `querySelectorAll()` (static) unless you specifically need live updates.

---

### Question 17: Chaining DOM Operations

**Difficulty: Medium**

Which statement about chaining DOM methods is TRUE?

A) All DOM methods can be chained  
B) Only methods that return the element can be chained  
C) Chaining is not possible with DOM methods  
D) Chaining only works with jQuery

**✓ Correct Answer: B) Only methods that return the element can be chained**

**Explanation:**  
Methods like `appendChild()` return the appended element, allowing chaining. Methods like `append()` or `remove()` return `undefined` and can't be chained.

```javascript
// appendChild returns the element - chainable
const div = document.createElement("div");
document.body
  .appendChild(div)
  .appendChild(document.createElement("span")).textContent = "Chained!";

// append returns undefined - NOT chainable
const container = document.querySelector(".container");
container.append(div); // Can't chain after this

// setAttribute returns undefined - NOT chainable
div.setAttribute("class", "box"); // Can't chain
```

**Interview Tip:**  
Mention that while chaining can be elegant, excessive chaining reduces readability. Balance cleverness with clarity.

**Key Concept:** Check return values in MDN before attempting to chain DOM methods.

---

### Question 18: replaceChild vs replaceWith

**Difficulty: Medium**

What's the difference between `replaceChild()` and `replaceWith()`?

A) They're identical  
B) `replaceChild()` requires parent element, `replaceWith()` doesn't  
C) `replaceWith()` is deprecated  
D) `replaceChild()` is faster

**✓ Correct Answer: B) `replaceChild()` requires parent element, `replaceWith()` doesn't**

**Explanation:**  
`replaceWith()` is the modern method called directly on the element to be replaced. `replaceChild()` is called on the parent and requires both old and new elements.

```javascript
// Old way - replaceChild (requires parent)
const parent = document.querySelector(".parent");
const oldElement = document.querySelector(".old");
const newElement = document.createElement("div");

parent.replaceChild(newElement, oldElement);

// Modern way - replaceWith
const oldElement2 = document.querySelector(".old2");
const newElement2 = document.createElement("div");

oldElement2.replaceWith(newElement2); // Cleaner!
```

**Interview Tip:**  
Show you know modern DOM APIs by preferring `replaceWith()`, but acknowledge `replaceChild()` might appear in legacy code.

**Key Concept:** Use `replaceWith()` for cleaner, more intuitive element replacement.

---

### Question 19: Element vs Node

**Difficulty: Hard**

What's the difference between `childNodes` and `children`?

A) They return the same thing  
B) `childNodes` includes text nodes and comments, `children` only includes element nodes  
C) `children` is deprecated  
D) `childNodes` is faster

**✓ Correct Answer: B) `childNodes` includes text nodes and comments, `children` only includes element nodes**

**Explanation:**  
`childNodes` returns all nodes (elements, text, comments). `children` returns only element nodes (HTMLCollection), which is usually what you want.

```javascript
<div id="parent">
    Some text
    <span>Child element</span>
    <!-- Comment -->
</div>

const parent = document.getElementById('parent');

console.log(parent.childNodes.length);
// 5 (text, span, text, comment, text)

console.log(parent.children.length);
// 1 (only the span)

// Safer to use children for most cases
Array.from(parent.children).forEach(child => {
    console.log(child.tagName); // Only element nodes
});
```

**Interview Tip:**  
This distinction trips up many developers! Explain that whitespace in HTML creates text nodes, which is why `childNodes` often has more items than expected.

**Key Concept:** Use `children` when iterating over child elements; it ignores text nodes and comments.

---

### Question 20: Attribute vs Property Reflection

**Difficulty: Hard**

For an `<input type="text" value="initial">`, what happens when a user types "new text"?

A) Both `value` property and attribute change  
B) Only the `value` property changes  
C) Only the `value` attribute changes  
D) Neither changes

**✓ Correct Answer: B) Only the `value` property changes**

**Explanation:**  
The `value` attribute sets the initial value, but the `value` property reflects the current value. User interaction updates the property, not the attribute. The attribute remains "initial".

```javascript
<input type="text" value="initial" id="myInput">

const input = document.getElementById('myInput');

console.log(input.value); // "initial"
console.log(input.getAttribute('value')); // "initial"

// User types "new text"
// Simulating: input.value = 'new text';

console.log(input.value); // "new text" (current)
console.log(input.getAttribute('value')); // "initial" (unchanged)

// To reset to initial:
input.value = input.getAttribute('value');
```

**Interview Tip:**  
This shows deep understanding of DOM behavior. Mention this is why form reset buttons work—they restore the attribute values. Also applies to `checked` for checkboxes.

**Key Concept:** Attributes represent initial HTML state; properties represent current state.

---

## 8. CHAPTER SUMMARY & NEXT STEPS

### Skills Mastered

- [ ] **DOM Creation:** You can spawn new elements (`createElement`).
- [ ] **Content Control:** You know when to use `textContent` vs `innerHTML`.
- [ ] **DOM Injection:** You can insert elements exactly where needed (`append`, `prepend`, `insertBefore`).
- [ ] **Styling:** You can add/remove classes (`classList`) and apply inline styles (`.style`).
- [ ] **Optimization:** You understand why `DocumentFragment` is better than looping appends.

### Key Takeaways

1. **The DOM is an Object:** Treat elements like objects. They are reference types.
2. **Minimize DOM touches:** Use `DocumentFragment` or build strings before inserting to avoid layout thrashing.
3. **Security First:** avoid `innerHTML` whenever handling user input.
4. **Classes over `.style`:** Only use inline styles for dynamic coordinates or user-customizable values. For everything else, toggle CSS classes.
5. **Append Moves Nodes:** If you append an existing node, it moves location; it doesn't copy.

### Industry Standards

- **React/Vue/Angular:** Under the hood, these frameworks use `createElement` and `appendChild` exactly as you learned today, but they do it via a "Virtual DOM" to batch updates efficiently.
- **Accessibility:** When manipulating the DOM, always ensure you aren't destroying semantic meaning or focus states for screen readers.

### Connection to Other Chapters

- **Previous (Day 17 - Intro to DOM):** You used the selectors you learned yesterday to target the elements you modified today.
- **Previous (Day 12 - Objects):** You applied object manipulation concepts (dot notation, properties) to DOM nodes.
- **Next (Day 19 - Events):** Today, your scripts ran automatically. Tomorrow, you will learn **Events**—how to make these changes happen _only when_ a user clicks a button, submits a form, or hovers over an image.

### Self-Assessment Quiz

1. If I want to replace the HTML content of a div entirely, which property do I use?
2. What is the difference between `element.className = 'active'` and `element.classList.add('active')`?
3. How do you create a new `<li>` element but not add it to the page yet?
4. If I have `div.dataset.userId = "55"`, how does that look in the HTML?

### Still Struggling?

- **Confused by `append` vs `appendChild`?** Just stick to `append`—it's newer, more flexible (accepts strings), and easier to use.
- **Nodes not showing up?** 99% of the time, you created the element but forgot to `append` it to a parent.

### Tomorrow's Preview

**Day 19: Master JavaScript Events like a Pro**
We will breathe life into your DOM. You will learn:

- `addEventListener`
- Click, Hover, and Keyboard events
- Event Bubbling and Capturing (The tricky part!)

### Community Engagement

Build a simple script that creates a "Todo List" structure (Title, Input, Empty List) strictly using JavaScript (no HTML body content). Share a screenshot of your code and the result!

**Next Step for You:** Open your code editor. Create an empty `<body>` tag. Try to recreate the Google Homepage layout (just the logo and search bar) using **only** JavaScript `createElement` and `append`. No HTML allowed!
