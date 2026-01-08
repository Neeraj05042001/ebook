

<div align="center">

# Day 4: MASTERING Control Flow in JavaScript

</div>

## 1. CHAPTER OVERVIEW

Welcome to Day 4! Today you'll master one of the most crucial skills in programming: **controlling the flow of your code**. Think of control flow as the traffic signals of your program—they determine which path your code takes based on different conditions.

### 🎯 What You'll Master Today

- **Conditional Logic**: Make your programs intelligent by teaching them to make decisions
- **if-else Statements**: Handle simple to complex conditions with confidence
- **switch-case Statements**: Write clean code for multiple value comparisons
- **Ternary Operators**: Master the shorthand for quick conditional assignments
- **Nested Conditions**: Build multi-layered decision-making logic
- **Control Flow Best Practices**: Know when to use each control structure for optimal code
- **Debugging Conditional Logic**: Identify and fix common control flow bugs

### ✅ Prerequisites Check

Before diving in, make sure you're comfortable with:

- [ ] Variables (let, const) and their scope
- [ ] All primitive data types (numbers, strings, booleans)
- [ ] Comparison operators (==, ===, !=, !==, >, <, >=, <=)
- [ ] Logical operators (&&, ||, !)
- [ ] Basic operator precedence
- [ ] Using console.log() for output

If any checkbox feels uncertain, quickly review Day 3 before proceeding.

### 💼 Why This Matters

**Career Impact**: Every technical interview includes control flow questions. It's foundational to:
- Building user authentication systems
- Creating form validations
- Implementing business logic
- Developing game mechanics
- Writing API response handlers

**Real-World Usage**: 
- **E-commerce**: "If cart total > $50, apply free shipping"
- **Social Media**: "If user is verified, show blue checkmark"
- **Banking**: "If balance < withdrawal amount, deny transaction"
- **Gaming**: "If player health <= 0, trigger game over"

According to Stack Overflow's 2024 Developer Survey, understanding control flow is among the top 5 skills employers look for in JavaScript developers. You'll use these concepts in literally every project you build.

---

## 2. QUICK REVISION SUMMARY

### Key Concepts at a Glance

| Concept | Purpose | Best For | Syntax Pattern |
|---------|---------|----------|----------------|
| **if-else** | Test conditions and execute different code blocks | Complex conditions, range checking, multiple different tests | `if (condition) { } else { }` |
| **else if** | Test multiple different conditions sequentially | Grading systems, tier-based logic | `if { } else if { } else { }` |
| **switch-case** | Compare one value against multiple possibilities | Menu selections, day/month mappings, discrete values | `switch (expr) { case val: }` |
| **Ternary** | Quick conditional assignment | Simple true/false decisions, inline conditionals | `condition ? true : false` |

### Syntax Quick Reference

```javascript
// if-else
if (condition) {
    // runs if condition is true
} else {
    // runs if condition is false
}

// if-else if-else
if (condition1) {
    // runs if condition1 is true
} else if (condition2) {
    // runs if condition2 is true
} else {
    // runs if all conditions are false
}

// switch-case
switch (expression) {
    case value1:
        // code
        break;
    case value2:
        // code
        break;
    default:
        // code if no cases match
}

// Ternary operator
let result = condition ? valueIfTrue : valueIfFalse;

// Nested ternary (use sparingly!)
let result = cond1 ? val1 : cond2 ? val2 : val3;
```

### Comparison: When to Use What

| Scenario | Use This | Why |
|----------|----------|-----|
| Testing if age >= 18 | `if-else` | Simple boolean condition |
| Assigning value based on one condition | Ternary `? :` | Concise, readable for simple cases |
| Checking ranges (score 0-100 for grades) | `if-else if` | Need comparison operators |
| Matching day number to day name | `switch-case` | Single value, multiple exact matches |
| Complex conditions with && or \|\| | `if-else` | switch can't handle complex logic |
| 10+ similar equality checks | `switch-case` | More readable and efficient |

### Decision Guide: if-else vs switch-case

```
START: Need to make a decision?
   ↓
   Is it ONE variable being compared?
   ↓                           ↓
   YES                         NO
   ↓                           ↓
   Testing for EXACT           Use if-else
   equality only?              (multiple conditions)
   ↓           ↓
   YES         NO
   ↓           ↓
   Use         Use if-else
   switch      (need >, <, etc.)
```

### Important Terminology

**Control Flow**: The order in which individual statements, instructions, or function calls are executed in a program.

**Truthy/Falsy**: Values that JavaScript considers true or false in boolean contexts. Falsy values: `false`, `0`, `""`, `null`, `undefined`, `NaN`. Everything else is truthy.

**Short-Circuit Evaluation**: JavaScript stops evaluating once the result is determined (e.g., in `if (false && anything)`, the second part never runs).

**Fall-through**: In switch statements, when you omit `break`, execution continues to the next case regardless of matching.

**Guard Clause**: An early return or check that handles edge cases first (defensive programming).

---

## 3. PREDICT THE OUTPUT

Test your understanding before writing code! For each challenge, try to predict the output before checking the answer.

### Challenge 1: Basic if-else (Easy)
```javascript
let temperature = 25;

if (temperature > 30) {
    console.log("It's hot!");
} else {
    console.log("It's pleasant!");
}
```

**🤔 Think**: Which condition is true?

**💡 Hint**: Is 25 greater than 30?

**✅ Answer**: 
```
Output: It's pleasant!
```
**Explanation**: Since `25 > 30` is false, the else block executes. The condition must be true for the if block to run.

---

### Challenge 2: String Comparison Gotcha (Medium)
```javascript
let day = "Monday";

switch (day) {
    case "monday":
        console.log("Start of week");
        break;
    default:
        console.log("Normal day");
}
```

**🤔 Think**: Will "Start of week" print?

**💡 Hint**: JavaScript is case-sensitive!

**✅ Answer**:
```
Output: Normal day
```
**Explanation**: `"Monday"` !== `"monday"`. switch uses strict equality (===), so case matters. The string values must match exactly, including capitalization.

---

### Challenge 3: Missing Break Statement (Medium)
```javascript
let grade = 'B';

switch (grade) {
    case 'A':
        console.log("Excellent!");
    case 'B':
        console.log("Good job!");
    case 'C':
        console.log("Keep trying!");
    default:
        console.log("Done");
}
```

**🤔 Think**: How many messages will print?

**💡 Hint**: What happens without `break`?

**✅ Answer**:
```
Output:
Good job!
Keep trying!
Done
```
**Explanation**: This is **fall-through** behavior. Once case 'B' matches, all subsequent code runs until a break or the end is reached. Always use `break` unless you specifically want fall-through.

---

### Challenge 4: Ternary Operator Chain (Medium)
```javascript
let score = 75;
let grade = score >= 90 ? 'A' : score >= 80 ? 'B' : score >= 70 ? 'C' : 'F';
console.log(grade);
```

**🤔 Think**: Trace through each condition.

**💡 Hint**: Evaluates left to right, first true wins.

**✅ Answer**:
```
Output: C
```
**Explanation**: 
- `75 >= 90`? No → check next
- `75 >= 80`? No → check next  
- `75 >= 70`? Yes → return 'C'

Nested ternaries evaluate from left to right, stopping at the first true condition.

---

### Challenge 5: Nested if-else (Medium)
```javascript
let age = 20;
let hasLicense = false;

if (age >= 18) {
    if (hasLicense) {
        console.log("Can drive");
    } else {
        console.log("Need license");
    }
} else {
    console.log("Too young");
}
```

**🤔 Think**: Which path does execution take?

**💡 Hint**: Check outer condition first.

**✅ Answer**:
```
Output: Need license
```
**Explanation**: Outer condition `age >= 18` is true (20 >= 18), so we enter the first block. Inner condition `hasLicense` is false, so the inner else executes.

---

### Challenge 6: Truthy/Falsy Trap (Hard)
```javascript
let userInput = "";

if (userInput) {
    console.log("Has input");
} else {
    console.log("No input");
}
```

**🤔 Think**: Is an empty string truthy or falsy?

**💡 Hint**: Remember the 6 falsy values!

**✅ Answer**:
```
Output: No input
```
**Explanation**: Empty string `""` is one of JavaScript's 6 falsy values. Even though it's a string, it evaluates to false in a boolean context. The falsy values are: `false`, `0`, `""`, `null`, `undefined`, `NaN`.

---

### Challenge 7: Operator Precedence with Conditions (Hard)
```javascript
let a = 5;
let b = 10;

if (a < b && a > 0 || b < 0) {
    console.log("Condition met");
} else {
    console.log("Condition not met");
}
```

**🤔 Think**: How do && and || interact?

**💡 Hint**: && has higher precedence than ||

**✅ Answer**:
```
Output: Condition met
```
**Explanation**: 
1. `a < b && a > 0` evaluates first: `5 < 10 && 5 > 0` → `true && true` → `true`
2. `true || b < 0` → `true || false` → `true`

Since || short-circuits, the second part after || doesn't even need to be true.

---

### Challenge 8: Multiple Cases in Switch (Medium)
```javascript
let month = 4;
let season;

switch (month) {
    case 12:
    case 1:
    case 2:
        season = "Winter";
        break;
    case 3:
    case 4:
    case 5:
        season = "Spring";
        break;
    default:
        season = "Unknown";
}

console.log(season);
```

**🤔 Think**: Which case matches month 4?

**💡 Hint**: Fall-through is intentional here!

**✅ Answer**:
```
Output: Spring
```
**Explanation**: Cases 3, 4, and 5 all lead to the same code block (intentional fall-through). When month is 4, it matches case 4 and falls through to execute `season = "Spring"`, then hits the break.

---

### Challenge 9: Ternary with Side Effects (Hard)
```javascript
let count = 0;
let result = count++ > 0 ? "Positive" : "Zero or negative";
console.log(result);
console.log(count);
```

**🤔 Think**: When does count++ happen?

**💡 Hint**: Post-increment returns old value.

**✅ Answer**:
```
Output:
Zero or negative
1
```
**Explanation**: `count++` uses the current value (0) for comparison, then increments. So `0 > 0` is false, returning "Zero or negative". But count becomes 1 after the operation.

---

### Challenge 10: Default Case Position (Easy)
```javascript
let color = "purple";

switch (color) {
    default:
        console.log("Unknown color");
        break;
    case "red":
        console.log("Red");
        break;
    case "blue":
        console.log("Blue");
        break;
}
```

**🤔 Think**: Does default position matter?

**💡 Hint**: Cases are checked first!

**✅ Answer**:
```
Output: Unknown color
```
**Explanation**: Even though default is written first, it's still the fallback. JavaScript checks all cases first. Since "purple" doesn't match any case, default executes. Position of default doesn't matter functionally, but convention puts it last for readability.

---

## 4. GUIDED PRACTICE PROBLEMS

### Problem 1: Voting Eligibility Checker (Easy)

**Task**: Create a program that checks if someone can vote based on their age and citizenship status.

**Requirements Checklist**:
- [ ] Accept age and isCitizen as inputs
- [ ] Check if age >= 18 AND isCitizen is true
- [ ] Print appropriate message for each scenario
- [ ] Handle edge case: exactly 18 years old

**Test Cases**:
```javascript
// Test 1
age = 20, isCitizen = true
Expected: "You are eligible to vote!"

// Test 2
age = 17, isCitizen = true
Expected: "You are not old enough to vote yet."

// Test 3
age = 25, isCitizen = false
Expected: "You must be a citizen to vote."

// Test 4
age = 18, isCitizen = true
Expected: "You are eligible to vote!"
```

**Solution**:
```javascript
let age = 20;
let isCitizen = true;

if (age >= 18 && isCitizen) {
    console.log("You are eligible to vote!");
} else if (age < 18) {
    console.log("You are not old enough to vote yet.");
} else {
    console.log("You must be a citizen to vote.");
}
```

**Why This Solution Works**:
- Uses `&&` to check BOTH conditions must be true
- Orders conditions logically: check age first (more specific error)
- Handles all three scenarios clearly

**Common Mistakes**:
❌ **Mistake**: Using `||` instead of `&&`
```javascript
if (age >= 18 || isCitizen) // WRONG - allows non-citizens!
```
**Why it's wrong**: This allows voting if EITHER condition is true, not both.

❌ **Mistake**: Not handling the citizenship case
```javascript
if (age >= 18) {
    console.log("Eligible");
} else {
    console.log("Not eligible");
}
// Doesn't check citizenship!
```

**What You Learned**:
- Using logical AND (&&) for multiple requirements
- Ordering conditions from specific to general
- Importance of testing edge cases

**Extension Challenges**:
1. Add a check for voter registration status
2. Include a residency requirement (must live in state for 30+ days)
3. Create a function that returns eligibility status as a boolean

---

### Problem 2: ATM Withdrawal Validator (Easy)

**Task**: Build an ATM system that validates withdrawal amounts. ATM only dispenses multiples of 100.

**Requirements Checklist**:
- [ ] Check if amount is a positive number
- [ ] Check if amount is a multiple of 100
- [ ] Handle zero or negative amounts
- [ ] Print success or error message

**Test Cases**:
```javascript
// Test 1
amount = 500
Expected: "Withdrawal successful! Please take your cash."

// Test 2
amount = 350
Expected: "Invalid amount. Please enter a multiple of 100."

// Test 3
amount = 0
Expected: "Invalid amount. Please enter a positive value."

// Test 4
amount = -200
Expected: "Invalid amount. Please enter a positive value."
```

**Solution**:
```javascript
let amount = 500;

if (amount <= 0) {
    console.log("Invalid amount. Please enter a positive value.");
} else if (amount % 100 !== 0) {
    console.log("Invalid amount. Please enter a multiple of 100.");
} else {
    console.log("Withdrawal successful! Please take your cash.");
}
```

**Why This Solution Works**:
- Checks invalid cases first (guard clauses)
- Uses modulo operator (%) to check divisibility
- Clear, specific error messages guide the user

**Common Mistakes**:
❌ **Mistake**: Checking conditions in wrong order
```javascript
if (amount % 100 !== 0) {
    // This runs for -200, giving wrong error!
}
```
**Why it's wrong**: `-200 % 100 = 0`, so this passes! Always validate positive first.

❌ **Mistake**: Using `amount % 100 === 0` alone
```javascript
if (amount % 100 === 0) {
    console.log("Success");
} else {
    console.log("Failed");
}
// Doesn't catch zero or negative!
```

**What You Learned**:
- Importance of condition ordering (guard clauses first)
- Using modulo (%) operator for divisibility checks
- Defensive programming: validate inputs before processing

**Extension Challenges**:
1. Add a maximum withdrawal limit ($1000)
2. Check if balance is sufficient
3. Limit to 5 notes maximum (max $500 per transaction)

---

### Problem 3: Simple Calculator (Medium)

**Task**: Create a calculator that performs basic operations based on an operator input.

**Requirements Checklist**:
- [ ] Accept two numbers and an operator (+, -, *, /, %)
- [ ] Use switch-case for operator selection
- [ ] Handle division by zero
- [ ] Print result with proper formatting

**Test Cases**:
```javascript
// Test 1
num1 = 10, num2 = 5, operator = '+'
Expected: "Result: 10 + 5 = 15"

// Test 2
num1 = 20, num2 = 4, operator = '/'
Expected: "Result: 20 / 4 = 5"

// Test 3
num1 = 10, num2 = 0, operator = '/'
Expected: "Error: Cannot divide by zero"

// Test 4
num1 = 7, num2 = 3, operator = '%'
Expected: "Result: 7 % 3 = 1"

// Test 5
num1 = 5, num2 = 3, operator = '^'
Expected: "Error: Invalid operator"
```

**Solution**:
```javascript
let num1 = 10;
let num2 = 5;
let operator = '+';
let result;

// Guard clause for division by zero
if (operator === '/' && num2 === 0) {
    console.log("Error: Cannot divide by zero");
} else {
    switch (operator) {
        case '+':
            result = num1 + num2;
            console.log(`Result: ${num1} + ${num2} = ${result}`);
            break;
        case '-':
            result = num1 - num2;
            console.log(`Result: ${num1} - ${num2} = ${result}`);
            break;
        case '*':
            result = num1 * num2;
            console.log(`Result: ${num1} * ${num2} = ${result}`);
            break;
        case '/':
            result = num1 / num2;
            console.log(`Result: ${num1} / ${num2} = ${result}`);
            break;
        case '%':
            result = num1 % num2;
            console.log(`Result: ${num1} % ${num2} = ${result}`);
            break;
        default:
            console.log("Error: Invalid operator");
    }
}
```

**Why This Solution Works**:
- Guard clause prevents division by zero before switch
- Each case is independent and clear
- Template literals make output readable
- Default case handles invalid operators

**Common Mistakes**:
❌ **Mistake**: Forgetting break statements
```javascript
case '+':
    result = num1 + num2;
    // Missing break causes fall-through!
case '-':
    result = num1 - num2; // This also runs!
```

❌ **Mistake**: Not handling division by zero
```javascript
case '/':
    result = num1 / num2; // Returns Infinity if num2 is 0!
```

❌ **Mistake**: Checking division by zero inside switch
```javascript
case '/':
    if (num2 === 0) {
        console.log("Error");
    } else {
        result = num1 / num2;
    }
    break;
// Works, but guard clause before switch is cleaner
```

**What You Learned**:
- When switch-case is cleaner than if-else chains
- Guard clauses for edge cases
- Template literals for formatted output

**Extension Challenges**:
1. Add power operation (**)
2. Add square root (only for positive numbers)
3. Store calculation history in an array

---

### Problem 4: Movie Ticket Pricing (Medium)

**Task**: Calculate movie ticket price based on age categories.

**Requirements Checklist**:
- [ ] Children (<18): $3
- [ ] Adults (18-60): $10
- [ ] Seniors (>60): $8
- [ ] Handle invalid ages (negative, unrealistic)

**Test Cases**:
```javascript
// Test 1
age = 15
Expected: "Ticket price: $3 (Child)"

// Test 2
age = 25
Expected: "Ticket price: $10 (Adult)"

// Test 3
age = 65
Expected: "Ticket price: $8 (Senior)"

// Test 4
age = -5
Expected: "Invalid age"

// Test 5
age = 150
Expected: "Invalid age"
```

**Solution**:
```javascript
let age = 25;
let price;
let category;

if (age < 0 || age > 120) {
    console.log("Invalid age");
} else if (age < 18) {
    price = 3;
    category = "Child";
    console.log(`Ticket price: $${price} (${category})`);
} else if (age <= 60) {
    price = 10;
    category = "Adult";
    console.log(`Ticket price: $${price} (${category})`);
} else {
    price = 8;
    category = "Senior";
    console.log(`Ticket price: $${price} (${category})`);
}
```

**Why This Solution Works**:
- Validates input first (defensive programming)
- Uses clear age ranges with appropriate comparisons
- Provides informative output with both price and category
- Logical flow from youngest to oldest

**Common Mistakes**:
❌ **Mistake**: Overlapping conditions
```javascript
if (age < 18) {
    price = 3;
} else if (age >= 18 && age < 60) { // Redundant check
    price = 10;
}
```
**Why it's wasteful**: If first condition is false, `age >= 18` is automatically true!

❌ **Mistake**: Wrong boundary conditions
```javascript
else if (age <= 18) { // 18-year-olds get child price!
    price = 3;
}
```

**What You Learned**:
- Range checking with comparison operators
- Input validation importance
- Avoiding redundant conditions in else-if chains

**Extension Challenges**:
1. Add matinee discount (before 5 PM)
2. Add student discount with ID
3. Add group discounts (4+ people)

---

### Problem 5: Zodiac Sign Checker (Medium)

**Task**: Determine zodiac sign based on birth month (simplified month-based system).

**Requirements Checklist**:
- [ ] Use switch-case (no if-else)
- [ ] Handle all 12 months
- [ ] Use fall-through for adjacent months sharing signs
- [ ] Handle invalid month input

**Zodiac System**:
- Jan-Feb: Aquarius
- Mar-Apr: Aries
- May-Jun: Gemini
- Jul-Aug: Leo
- Sep-Oct: Libra
- Nov-Dec: Sagittarius

**Test Cases**:
```javascript
// Test 1
month = 3
Expected: "Your zodiac sign is Aries ♈"

// Test 2
month = 7
Expected: "Your zodiac sign is Leo ♌"

// Test 3
month = 12
Expected: "Your zodiac sign is Sagittarius ♐"

// Test 4
month = 13
Expected: "Invalid month"
```

**Solution**:
```javascript
let month = 3;
let zodiacSign;

switch (month) {
    case 1:
    case 2:
        zodiacSign = "Aquarius ♒";
        break;
    case 3:
    case 4:
        zodiacSign = "Aries ♈";
        break;
    case 5:
    case 6:
        zodiacSign = "Gemini ♊";
        break;
    case 7:
    case 8:
        zodiacSign = "Leo ♌";
        break;
    case 9:
    case 10:
        zodiacSign = "Libra ♎";
        break;
    case 11:
    case 12:
        zodiacSign = "Sagittarius ♐";
        break;
    default:
        zodiacSign = null;
        console.log("Invalid month");
}

if (zodiacSign) {
    console.log(`Your zodiac sign is ${zodiacSign}`);
}
```

**Why This Solution Works**:
- Perfect use case for switch with intentional fall-through
- Groups related months together
- Separates validation from output
- Default case catches invalid input

**Common Mistakes**:
❌ **Mistake**: Using break between paired months
```javascript
case 3:
    zodiacSign = "Aries";
    break; // This breaks fall-through!
case 4:
    zodiacSign = "Aries"; // Redundant code
    break;
```

❌ **Mistake**: Trying to use if-else (task requirement)
```javascript
if (month === 3 || month === 4) { // Task said NO if-else!
    zodiacSign = "Aries";
}
```

**What You Learned**:
- Intentional fall-through in switch statements
- When switch is superior to if-else
- Handling related values together
- Separating validation from business logic

**Extension Challenges**:
1. Include day for accurate zodiac calculation
2. Add personality traits for each sign
3. Add compatibility checker (two zodiac inputs)

---

### Problem 6: Triangle Type Classifier (Medium)

**Task**: Classify a triangle based on its three sides.

**Requirements Checklist**:
- [ ] Accept three side lengths
- [ ] All equal: Equilateral
- [ ] Two equal: Isosceles
- [ ] All different: Scalene
- [ ] Validate triangle inequality (a+b > c for all combinations)

**Test Cases**:
```javascript
// Test 1
side1 = 5, side2 = 5, side3 = 5
Expected: "Equilateral Triangle"

// Test 2
side1 = 5, side2 = 5, side3 = 7
Expected: "Isosceles Triangle"

// Test 3
side1 = 3, side2 = 4, side3 = 5
Expected: "Scalene Triangle"

// Test 4
side1 = 1, side2 = 2, side3 = 10
Expected: "Not a valid triangle"

// Test 5
side1 = 0, side2 = 5, side3 = 5
Expected: "Not a valid triangle"
```

**Solution**:
```javascript
let side1 = 5;
let side2 = 5;
let side3 = 7;

// Validate triangle inequality theorem
if (side1 <= 0 || side2 <= 0 || side3 <= 0) {
    console.log("Not a valid triangle - sides must be positive");
} else if (side1 + side2 <= side3 || side1 + side3 <= side2 || side2 + side3 <= side1) {
    console.log("Not a valid triangle - doesn't satisfy triangle inequality");
} else {
    // Classify triangle type
    if (side1 === side2 && side2 === side3) {
        console.log("Equilateral Triangle");
    } else if (side1 === side2 || side2 === side3 || side1 === side3) {
        console.log("Isosceles Triangle");
    } else {
        console.log("Scalene Triangle");
    }
}
```

**Why This Solution Works**:
- Validates input before classification (guard clauses)
- Checks triangle inequality theorem correctly
- Uses logical operators efficiently
- Clear classification logic from most specific to least

**Common Mistakes**:
❌ **Mistake**: Not validating triangle inequality
```javascript
if (side1 === side2 === side3) { // Also wrong syntax!
    console.log("Equilateral");
}
// Sides 1, 2, 10 would be classified, but can't form a triangle!
```

❌ **Mistake**: Wrong equality chain
```javascript
if (side1 === side2 === side3) { // WRONG!
// This evaluates as: (side1 === side2) === side3
// Which compares a boolean to a number!
```
**Correct way**: `side1 === side2 && side2 === side3`

❌ **Mistake**: Incomplete isosceles check
```javascript
else if (side1 === side2) { // Only checks one pair!
    console.log("Isosceles");
}
// Misses when side2 === side3 or side1 === side3
```

**What You Learned**:
- Mathematical validation before classification
- Chaining equality comparisons correctly
- Using || for "any of these" logic
- Guard clauses for input validation

**Extension Challenges**:
1. Calculate triangle area using Heron's formula
2. Classify by angles (acute, obtuse, right)
3. Check if triangle is a right triangle (Pythagorean theorem)

---

### Problem 7: Grade Calculator with Extra Credit (Hard)

**Task**: Calculate final grade including extra credit and letter grade assignment.

**Requirements Checklist**:
- [ ] Base score 0-100
- [ ] Extra credit can push above 100
- [ ] Cap final score at 100 for grade calculation
- [ ] Assign letter grades: A(90-100), B(80-89), C(70-79), D(60-69), F(<60)
- [ ] Show both numerical and letter grades

**Test Cases**:
```javascript
// Test 1
baseScore = 85, extraCredit = 10
Expected: "Score: 95/100, Grade: A"

// Test 2
baseScore = 95, extraCredit = 10
Expected: "Score: 100/100, Grade: A (Capped)"

// Test 3
baseScore = 72, extraCredit = 0
Expected: "Score: 72/100, Grade: C"

// Test 4
baseScore = 55, extraCredit = 8
Expected: "Score: 63/100, Grade: D"
```

**Solution**:
```javascript
let baseScore = 85;
let extraCredit = 10;

// Calculate total score
let totalScore = baseScore + extraCredit;
let cappedScore = totalScore > 100 ? 100 : totalScore;
let letterGrade;
let note = totalScore > 100 ? " (Capped)" : "";

// Determine letter grade using if-else if
if (cappedScore >= 90) {
    letterGrade = 'A';
} else if (cappedScore >= 80) {
    letterGrade = 'B';
} else if (cappedScore >= 70) {
    letterGrade = 'C';
} else if (cappedScore >= 60) {
    letterGrade = 'D';
} else {
    letterGrade = 'F';
}

console.log(`Score: ${cappedScore}/100, Grade: ${letterGrade}${note}`);

```javascript

// Alternative: Using ternary (less readable but concise)
let letterGradeAlt = cappedScore >= 90 ? 'A' : 
                     cappedScore >= 80 ? 'B' : 
                     cappedScore >= 70 ? 'C' : 
                     cappedScore >= 60 ? 'D' : 'F';

```

**Why This Solution Works**:
- Separates calculation from classification
- Uses ternary for simple capping logic
- Clear if-else chain for grade ranges
- Provides informative output with note about capping
- Handles edge case where extra credit exceeds 100

**Common Mistakes**:
❌ **Mistake**: Not capping the score for grade calculation
```javascript
let totalScore = 105;
if (totalScore >= 90) { // Would give A for any score >= 90, even 150!
    letterGrade = 'A';
}
```

❌ **Mistake**: Using wrong comparison direction
```javascript
if (cappedScore <= 90) { // BACKWARDS!
    letterGrade = 'A';
}
// This gives everyone A except perfect scores!
```

❌ **Mistake**: Overlapping ranges
```javascript
if (cappedScore >= 90 && cappedScore <= 100) {
    letterGrade = 'A';
} else if (cappedScore >= 80 && cappedScore <= 100) { // Overlaps with A!
    letterGrade = 'B';
}
```

**What You Learned**:
- Data transformation before classification (capping)
- When ternary is appropriate vs if-else
- Range boundaries in sequential conditions
- Providing contextual feedback to users

**Extension Challenges**:
1. Add +/- modifiers (A+, A, A-)
2. Calculate GPA (4.0 scale)
3. Handle multiple assignments with weights

---

### Problem 8: Traffic Light System (Hard)

**Task**: Simulate a traffic light system with pedestrian crossing logic.

**Requirements Checklist**:
- [ ] Three states: Red, Yellow, Green
- [ ] Pedestrian button can be pressed
- [ ] Red: Always allow pedestrian crossing
- [ ] Green: Allow pedestrian crossing if button pressed
- [ ] Yellow: Never allow pedestrian crossing (transition state)
- [ ] Show appropriate messages for drivers and pedestrians

**Test Cases**:
```javascript
// Test 1
lightColor = "Red", pedestrianButton = false
Expected: "Stop! Pedestrians may cross."

// Test 2
lightColor = "Green", pedestrianButton = true
Expected: "Go! Watch for pedestrians - button pressed."

// Test 3
lightColor = "Green", pedestrianButton = false
Expected: "Go! Drive safely."

// Test 4
lightColor = "Yellow", pedestrianButton = true
Expected: "Caution! Pedestrians must wait."
```

**Solution**:
```javascript
let lightColor = "Green";
let pedestrianButton = true;
let driverMessage;
let pedestrianMessage;

switch (lightColor) {
    case "Red":
        driverMessage = "Stop!";
        pedestrianMessage = "Pedestrians may cross.";
        break;
    
    case "Yellow":
        driverMessage = "Caution!";
        pedestrianMessage = "Pedestrians must wait.";
        break;
    
    case "Green":
        if (pedestrianButton) {
            driverMessage = "Go! Watch for pedestrians - button pressed.";
            pedestrianMessage = "Wait for safe crossing signal.";
        } else {
            driverMessage = "Go! Drive safely.";
            pedestrianMessage = "Press button to cross.";
        }
        break;
    
    default:
        driverMessage = "Error: Invalid light state";
        pedestrianMessage = "Wait for valid signal";
}

console.log(`${driverMessage} ${pedestrianMessage}`);
```

**Why This Solution Works**:
- Switch handles the primary state (light color)
- Nested if handles secondary condition (button) only when relevant
- Separates messages for different actors (driver/pedestrian)
- Default case handles invalid states

**Common Mistakes**:
❌ **Mistake**: Checking button state outside switch
```javascript
if (pedestrianButton) {
    // Logic here
}
switch (lightColor) { ... }
// Button state matters differently for each light color!
```

❌ **Mistake**: Overcomplicating with nested switches
```javascript
switch (lightColor) {
    case "Green":
        switch (pedestrianButton) { // Unnecessary nesting
            case true:
            case false:
        }
}
```

❌ **Mistake**: Not handling all state combinations
```javascript
case "Green":
    driverMessage = "Go!";
    // Forgot to check pedestrian button!
```

**What You Learned**:
- When to nest if-else inside switch
- Handling multiple actors in one system
- State machines with primary and secondary conditions
- Default case for error handling

**Extension Challenges**:
1. Add timer countdown for each light
2. Add emergency vehicle override
3. Implement complete light cycle automation

---

### Problem 9: Restaurant Bill Splitter with Tip (Hard)

**Task**: Split a restaurant bill with tip calculation based on service quality.

**Requirements Checklist**:
- [ ] Calculate tip: Excellent (20%), Good (15%), Fair (10%), Poor (5%)
- [ ] Split total among specified number of people
- [ ] Round to 2 decimal places
- [ ] Show breakdown: subtotal, tip amount, total, per person

**Test Cases**:
```javascript
// Test 1
billAmount = 120, service = "Excellent", numPeople = 4
Expected: 
  "Subtotal: $120.00
   Tip (20%): $24.00
   Total: $144.00
   Per person: $36.00"

// Test 2
billAmount = 80, service = "Good", numPeople = 2
Expected:
  "Subtotal: $80.00
   Tip (15%): $12.00
   Total: $92.00
   Per person: $46.00"
```

**Solution**:
```javascript
let billAmount = 120;
let serviceQuality = "Excellent";
let numPeople = 4;
let tipPercentage;

// Determine tip percentage based on service
switch (serviceQuality.toLowerCase()) {
    case "excellent":
        tipPercentage = 0.20;
        break;
    case "good":
        tipPercentage = 0.15;
        break;
    case "fair":
        tipPercentage = 0.10;
        break;
    case "poor":
        tipPercentage = 0.05;
        break;
    default:
        console.log("Invalid service quality. Using 15% default.");
        tipPercentage = 0.15;
}

// Calculate amounts
let tipAmount = billAmount * tipPercentage;
let totalAmount = billAmount + tipAmount;
let perPersonAmount = totalAmount / numPeople;

// Format output
console.log(`Subtotal: $${billAmount.toFixed(2)}`);
console.log(`Tip (${(tipPercentage * 100)}%): $${tipAmount.toFixed(2)}`);
console.log(`Total: $${totalAmount.toFixed(2)}`);
console.log(`Per person: $${perPersonAmount.toFixed(2)}`);
```

**Why This Solution Works**:
- Uses `.toLowerCase()` to handle case-insensitive input
- Switch for clear mapping of service → tip percentage
- Separates calculation from formatting
- `.toFixed(2)` ensures proper currency formatting
- Default case provides graceful fallback

**Common Mistakes**:
❌ **Mistake**: Case-sensitive comparison
```javascript
case "Excellent": // Won't match "excellent" or "EXCELLENT"
```

❌ **Mistake**: Not formatting currency
```javascript
console.log(`Total: $${totalAmount}`);
// Outputs: Total: $144.0000000001 (floating point issues)
```

❌ **Mistake**: Integer division issues
```javascript
let perPersonAmount = totalAmount / numPeople;
console.log(perPersonAmount); // Might be 36.00000000000001
// Always use .toFixed() for currency!
```

❌ **Mistake**: Not validating numPeople
```javascript
// What if numPeople = 0? Division by zero!
if (numPeople <= 0) {
    console.log("Invalid number of people");
    return;
}
```

**What You Learned**:
- Case-insensitive string comparison
- Financial calculations and formatting
- Default values as fallbacks
- Input sanitization importance

**Extension Challenges**:
1. Add tax calculation (before tip)
2. Handle unequal splits (one person pays more)
3. Add discount codes

---

### Problem 10: Password Strength Validator (Hard)

**Task**: Evaluate password strength based on multiple criteria.

**Requirements Checklist**:
- [ ] Minimum 8 characters: +1 point
- [ ] Contains uppercase: +1 point
- [ ] Contains lowercase: +1 point
- [ ] Contains number: +1 point
- [ ] Contains special character: +1 point
- [ ] Strength: 0-2 (Weak), 3 (Medium), 4-5 (Strong)

**Test Cases**:
```javascript
// Test 1
password = "abc123"
Expected: "Weak password (Score: 2/5) - Add uppercase and special characters"

// Test 2
password = "Password123"
Expected: "Medium password (Score: 3/5) - Add special characters"

// Test 3
password = "Pass@123"
Expected: "Strong password (Score: 5/5)"

// Test 4
password = "short"
Expected: "Weak password (Score: 1/5) - Too short, add uppercase, numbers, and special characters"
```

**Solution**:
```javascript
let password = "Pass@123";
let score = 0;
let suggestions = [];

// Check each criterion
if (password.length >= 8) {
    score++;
} else {
    suggestions.push("Too short");
}

if (/[A-Z]/.test(password)) {
    score++;
} else {
    suggestions.push("add uppercase");
}

if (/[a-z]/.test(password)) {
    score++;
} else {
    suggestions.push("add lowercase");
}

if (/[0-9]/.test(password)) {
    score++;
} else {
    suggestions.push("add numbers");
}

if (/[!@#$%^&*]/.test(password)) {
    score++;
} else {
    suggestions.push("add special characters");
}

// Determine strength
let strength;
if (score <= 2) {
    strength = "Weak";
} else if (score === 3) {
    strength = "Medium";
} else {
    strength = "Strong";
}

// Build message
let message = `${strength} password (Score: ${score}/5)`;
if (suggestions.length > 0) {
    message += ` - ${suggestions.join(", ")}`;
}

console.log(message);
```

**Why This Solution Works**:
- Each criterion checked independently
- Builds suggestions array dynamically
- Uses regex for pattern matching (advanced but powerful)
- Provides actionable feedback
- Clear strength classification

**Common Mistakes**:
❌ **Mistake**: Using nested if-else for independent checks
```javascript
if (hasUpper) {
    score++;
} else if (hasLower) { // WRONG! These should be independent!
    score++;
}
// Should use separate if statements, not else-if
```

❌ **Mistake**: Not providing helpful feedback
```javascript
if (score < 3) {
    console.log("Weak password");
}
// User doesn't know HOW to improve!
```

❌ **Mistake**: Hardcoding suggestions
```javascript
if (score === 2) {
    console.log("Add uppercase and special characters");
}
// What if it's missing different things?
```

**What You Learned**:
- Independent vs dependent conditions
- Building dynamic messages
- Using arrays to collect data
- Basic regex for pattern matching
- User-friendly error messages

**Extension Challenges**:
1. Check against common passwords list
2. Check for sequential characters (abc, 123)
3. Add minimum requirements mode (must have all 5)

---

## 5. DEBUG CHALLENGES

### Debug Challenge 1: Switch Statement Missing Break

**Problem Context**: A student grade system isn't working correctly. It's printing multiple grades for a single score.

**Broken Code**:
```javascript
let score = 85;
let grade;

switch (true) {
    case score >= 90:
        grade = "A";
    case score >= 80:
        grade = "B";
    case score >= 70:
        grade = "C";
    case score >= 60:
        grade = "D";
    default:
        grade = "F";
}

console.log(`Your grade is: ${grade}`);
// Expected: "Your grade is: B"
// Actual: "Your grade is: F"
```

**Your Task**: 
1. Identify why the output is wrong
2. Fix the code
3. Explain what was happening

**Solution**:
```javascript
let score = 85;
let grade;

switch (true) {
    case score >= 90:
        grade = "A";
        break;  // Added break
    case score >= 80:
        grade = "B";
        break;  // Added break
    case score >= 70:
        grade = "C";
        break;  // Added break
    case score >= 60:
        grade = "D";
        break;  // Added break
    default:
        grade = "F";
}

console.log(`Your grade is: ${grade}`);
// Output: "Your grade is: B"
```

**Explanation**: 
Without `break` statements, JavaScript exhibits "fall-through" behavior. Here's what happened:
1. `score >= 90` (85 >= 90) → false, skip
2. `score >= 80` (85 >= 80) → true, set `grade = "B"`, but no break!
3. Execution continues to next case: `grade = "C"`
4. Continues again: `grade = "D"`
5. Continues to default: `grade = "F"`
6. Finally exits with the last assigned value: "F"

**How to Prevent**:
- Always add `break` after each case (unless fall-through is intentional)
- Use a linter like ESLint with `no-fallthrough` rule
- Comment intentional fall-throughs: `// falls through`

---

### Debug Challenge 2: Wrong Comparison Operator

**Problem Context**: A login system that should accept both username and email isn't working.

**Broken Code**:
```javascript
let usernameInput = "john_doe";
let emailInput = "";
let storedUsername = "john_doe";
let storedEmail = "john@example.com";

if (usernameInput === storedUsername && emailInput === storedEmail) {
    console.log("Login successful!");
} else {
    console.log("Login failed!");
}

// Expected: "Login successful!" (username matches)
// Actual: "Login failed!"
```

**Your Task**:
1. Why does the login fail even though the username is correct?
2. Fix the logic
3. What operator should be used?

**Solution**:
```javascript
let usernameInput = "john_doe";
let emailInput = "";
let storedUsername = "john_doe";
let storedEmail = "john@example.com";

// Changed && to || - user can login with EITHER username OR email
if (usernameInput === storedUsername || emailInput === storedEmail) {
    console.log("Login successful!");
} else {
    console.log("Login failed!");
}

// Output: "Login successful!"
```

**Explanation**:
The original code used `&&` (AND), requiring BOTH conditions to be true:
- `usernameInput === storedUsername` → true ✓
- `emailInput === storedEmail` → false (empty string !== stored email) ✗
- `true && false` → false

The fix uses `||` (OR), requiring EITHER condition to be true:
- `usernameInput === storedUsername` → true ✓
- `true || false` → true (second condition not even evaluated due to short-circuit)

**How to Prevent**:
- Clearly define requirements: "AND" vs "OR" logic
- Write test cases for all input combinations
- Ask: "Should ALL conditions be met, or just ONE?"

---

### Debug Challenge 3: Truthy/Falsy Confusion

**Problem Context**: A form validator that checks if required fields are filled.

**Broken Code**:
```javascript
let age = 0;
let name = "John";

if (age) {
    console.log("Age is valid");
} else {
    console.log("Please enter your age");
}

// Expected: "Age is valid" (0 is a valid age)
// Actual: "Please enter your age"
```

**Your Task**:
1. Why does 0 fail the validation?
2. Fix the code
3. What's the proper way to check for user input?

**Solution**:
```javascript
let age = 0;
let name = "John";

// Method 1: Check for undefined/null specifically
if (age !== undefined && age !== null) {
    console.log("Age is valid");
} else {
    console.log("Please enter your age");
}

// Method 2: Check against empty string (if age is user input)
if (age !== "") {
    console.log("Age is valid");
} else {
    console.log("Please enter your age");
}

// Method 3: Check typeof (if expecting number)
if (typeof age === "number") {
    console.log("Age is valid");
} else {
    console.log("Please enter your age");
}

// Output: "Age is valid"
```

**Explanation**:
JavaScript has 6 falsy values: `false`, `0`, `""`, `null`, `undefined`, `NaN`. 

In the broken code:
- `if (age)` evaluates as `if (0)`
- 0 is falsy, so condition fails
- But 0 is a valid age!

The fix explicitly checks what you really mean:
- "Has the user provided input?" → check for `undefined` or `null`
- "Is the input not empty?" → check against `""`
- "Is it the expected type?" → use `typeof`

**How to Prevent**:
- Never rely on truthy/falsy for numeric validations
- Be explicit: check for `null`, `undefined`, or specific values
- Use strict equality (`===`) not loose (`==`)
- Document what "empty" means for each field

---

### Debug Challenge 4: Operator Precedence Issue

**Problem Context**: An eligibility checker for a scholarship program.

**Broken Code**:
```javascript
let gpa = 3.8;
let hasFinancialNeed = true;
let isMinority = false;

// Eligible if: GPA >= 3.5 AND (has financial need OR is minority)
if (gpa >= 3.5 && hasFinancialNeed || isMinority) {
    console.log("Eligible for scholarship");
} else {
    console.log("Not eligible");
}

// Test case: gpa=2.0, hasFinancialNeed=false, isMinority=true
// Expected: "Not eligible" (GPA too low)
// Actual: "Eligible for scholarship"
```

**Your Task**:
1. Why does low GPA with minority status pass?
2. Add the missing element
3. Test with multiple scenarios

**Solution**:
```javascript
let gpa = 2.0;
let hasFinancialNeed = false;
let isMinority = true;

// Added parentheses to enforce correct precedence
if (gpa >= 3.5 && (hasFinancialNeed || isMinority)) {
    console.log("Eligible for scholarship");
} else {
    console.log("Not eligible");
}

// Output: "Not eligible"
```

**Explanation**:
Operator precedence: `&&` has higher precedence than `||`

**Broken code evaluation** (gpa=2.0, hasFinancialNeed=false, isMinority=true):
```
gpa >= 3.5 && hasFinancialNeed || isMinority
= false && false || true
= (false && false) || true    // && evaluated first
= false || true
= true  ✗ WRONG!
```

**Fixed code evaluation**:
```
gpa >= 3.5 && (hasFinancialNeed || isMinority)
= false && (false || true)
= false && true
= false  ✓ CORRECT!
```

**How to Prevent**:
- Use parentheses to make intent explicit
- Don't rely on operator precedence memory
- Break complex conditions into multiple if statements
- Write conditions in natural language first, then translate

---

### Debug Challenge 5: Type Coercion Trap

**Problem Context**: An inventory system checking if stock is sufficient.

**Broken Code**:
```javascript
let requestedQuantity = "10";  // Comes from user input as string
let availableStock = 5;

if (requestedQuantity < availableStock) {
    console.log("Not enough stock");
} else {
    console.log("Processing order");
}

// Expected: "Processing order" (10 > 5)
// Actual: "Not enough stock"
```

**Your Task**:
1. Why does comparison give wrong result?
2. Fix the type issue
3. Add validation

**Solution**:
```javascript
let requestedQuantity = "10";  // User input
let availableStock = 5;

// Method 1: Convert to number first
requestedQuantity = Number(requestedQuantity);

// Add validation
if (isNaN(requestedQuantity) || requestedQuantity <= 0) {
    console.log("Invalid quantity");
} else if (requestedQuantity > availableStock) {
    console.log("Not enough stock");
} else {
    console.log("Processing order");
}

// Method 2: Use parseInt/parseFloat
requestedQuantity = parseInt(requestedQuantity, 10);

// Method 3: Use unary plus operator
requestedQuantity = +requestedQuantity;

// Output: "Not enough stock"
```

**Explanation**:
String comparison uses lexicographic (dictionary) order:
- `"10" < "5"` compares character by character
- First character: `"1"` vs `"5"` → "1" comes before "5"
- Result: `true` (even though numerically 10 > 5!)

JavaScript performs type coercion with `<`, `>` but comparison  rules can be surprising with strings.

**How to Prevent**:
- Always validate and convert user input types
- Use `Number()`, `parseInt()`, or `parseFloat()` explicitly
- Check for `NaN` after conversion
- Use TypeScript for compile-time type checking
- Never trust user input to be the expected type

---

### Debug Challenge 6: Incorrect Nested Logic

**Problem Context**: A movie rating system with age restrictions.

**Broken Code**:
```javascript
let age = 17;
let hasParent = true;
let movieRating = "R";  // R-rated: 17+ or 13+ with parent

if (age >= 17) {
    console.log("Can watch R-rated movie");
} else if (hasParent) {
    console.log("Can watch with parent");
} else {
    console.log("Cannot watch R-rated movie");
}

// Expected: "Can watch with parent" (17-year-old with parent)
// Actual: "Can watch R-rated movie"
// But what about 16-year-old without parent?
```

**Your Task**:
1. Identify the logical flaw
2. What case is not handled correctly?
3. Fix the logic structure

**Solution**:
```javascript
let age = 16;
let hasParent = false;
let movieRating = "R";

// Fixed logic: properly nest the conditions
if (movieRating === "R") {
    if (age >= 17) {
        console.log("Can watch R-rated movie");
    } else if (age >= 13 && hasParent) {
        console.log("Can watch with parent (13-16 with guardian)");
    } else {
        console.log("Cannot watch R-rated movie");
    }
} else {
    console.log("Movie is not R-rated");
}

// Test cases:
// age=17, hasParent=true  → "Can watch R-rated movie"
// age=16, hasParent=true  → "Can watch with parent"
// age=16, hasParent=false → "Cannot watch R-rated movie"
// age=12, hasParent=true  → "Cannot watch R-rated movie"
```

**Explanation**:
The broken code has a logical flaw in the else-if chain:
- If `age >= 17` is true, it never checks `hasParent`
- The `else if (hasParent)` only runs when age < 17
- This doesn't properly encode the requirement: "17+ OR (13+ with parent)"

The fix:
- First case handles 17+ (doesn't need parent)
- Second case handles 13-16 with parent (needs BOTH conditions)
- Third case is the catch-all rejection

**How to Prevent**:
- Draw a decision tree before coding
- Write out all combinations in a truth table:
  ```
  Age 17+ | Parent | Result
  Yes     | Yes    | Allow
  Yes     | No     | Allow
  No      | Yes    | Allow (if 13+)
  No      | No     | Deny
  ```
- Test every logical path with specific examples
- Use De Morgan's Laws to verify logic inversions

---

### Debug Challenge 7: Switch with Wrong Expression Type

**Problem Context**: A grading system using switch statement incorrectly.

**Broken Code**:
```javascript
let score = 85;
let grade;

switch (score) {
    case score >= 90:
        grade = "A";
        break;
    case score >= 80:
        grade = "B";
        break;
    default:
        grade = "F";
}

console.log(`Grade: ${grade}`);
// Expected: "Grade: B"
// Actual: "Grade: F"
```

**Your Task**:
1. Why does it always output "F"?
2. What's the fundamental problem with this switch usage?
3. Provide two solutions

**Solution**:

**Method 1: Use switch(true) pattern**
```javascript
let score = 85;
let grade;

switch (true) {  // Changed from switch(score)
    case score >= 90:
        grade = "A";
        break;
    case score >= 80:
        grade = "B";
        break;
    case score >= 70:
        grade = "C";
        break;
    default:
        grade = "F";
}

console.log(`Grade: ${grade}`);
// Output: "Grade: B"
```

**Method 2: Use if-else instead (better choice)**
```javascript
let score = 85;
let grade;

if (score >= 90) {
    grade = "A";
} else if (score >= 80) {
    grade = "B";
} else if (score >= 70) {
    grade = "C";
} else {
    grade = "F";
}

console.log(`Grade: ${grade}`);
// Output: "Grade: B"
```

**Explanation**:
The broken code uses `switch(score)` which checks for **exact equality**:
- `switch(85)` asks: "Does 85 === (score >= 90)?"
- `85 === false`? No
- `85 === (score >= 80)`? No (85 ≠ true)
- Falls to default: "F"

The switch statement compares `score` (85) against the case values, which are booleans (true/false). This never matches!

**Method 1 fix**: `switch(true)` compares `true === (score >= 80)`, which works.
**Method 2 fix**: Use if-else, which is designed for range checking.

**How to Prevent**:
- Use switch only for exact value matching
- Use if-else for ranges and comparisons
- Remember: switch uses strict equality (===)
- If you need `switch(true)`, consider if-else might be clearer

---

### Debug Challenge 8: Off-by-One Boundary Error

**Problem Context**: A subscription tier system with pricing boundaries.

**Broken Code**:
```javascript
let usage = 100;  // GB of data used
let tier;

if (usage < 50) {
    tier = "Basic - $10/month";
} else if (usage < 100) {
    tier = "Standard - $20/month";
} else if (usage <= 200) {
    tier = "Premium - $30/month";
}

console.log(tier);
// Expected: "Premium - $30/month" (100 GB is Premium tier)
// Actual: "Premium - $30/month"
// BUT: What about usage = 50? Let's test!
```

**Your Task**:
1. Test with usage = 50 and usage = 100
2. Find the boundary inconsistency
3. Fix to match requirements: 0-49 (Basic), 50-99 (Standard), 100-200 (Premium)

**Solution**:
```javascript
let usage = 50;
let tier;

// Fixed boundaries: consistent use of < or <=
if (usage < 50) {
    tier = "Basic - $10/month";
} else if (usage < 100) {
    tier = "Standard - $20/month";
} else if (usage <= 200) {
    tier = "Premium - $30/month";
} else {
    tier = "Enterprise - Contact sales";
}

console.log(tier);

// Test cases:
// usage = 49  → "Basic"
// usage = 50  → "Standard" ✓
// usage = 99  → "Standard" ✓
// usage = 100 → "Premium" ✓
// usage = 200 → "Premium" ✓
// usage = 201 → "Enterprise" ✓
```

**Explanation**:
Boundary errors happen when < and <= are mixed inconsistently:
- Original: `< 50`, `< 100`, `<= 200`
- Creates confusion: is 100 included in Standard or Premium?
- The code worked for 100 but created ambiguity

**Best practice patterns**:
```javascript
// Pattern 1: Exclusive upper bounds (< everywhere)
if (usage < 50) { }
else if (usage < 100) { }
else if (usage < 200) { }

// Pattern 2: Inclusive lower bounds (>= everywhere)
if (usage >= 100) { }
else if (usage >= 50) { }
else { }

// Pattern 3: Explicit ranges (most clear but verbose)
if (usage >= 0 && usage < 50) { }
else if (usage >= 50 && usage < 100) { }
else if (usage >= 100 && usage <= 200) { }
```

**How to Prevent**:
- Be consistent with < vs <=
- Write out boundary test cases
- Use a table to visualize ranges:
  ```
  Range      | Tier
  0-49       | Basic
  50-99      | Standard
  100-200    | Premium
  201+       | Enterprise
  ```
- Test exact boundary values (49, 50, 99, 100, etc.)

---

## 6. COMMON PITFALLS & ANTI-PATTERNS

### Pitfall 1: Using == Instead of ===

**❌ What Beginners Do**:
```javascript
let userInput = "5";
let targetNumber = 5;

if (userInput == targetNumber) {
    console.log("Match!");
}
// Output: "Match!" (but types are different!)
```

**⚠️ Why It's Problematic**:
- `==` performs type coercion (converts types before comparing)
- `"5" == 5` → true (string converted to number)
- Leads to unexpected bugs: `"0" == false` → true, `null == undefined` → true
- Hard to predict behavior with different types

**✅ The Right Way**:
```javascript
let userInput = "5";
let targetNumber = 5;

if (userInput === targetNumber) {
    console.log("Match!");
} else {
    console.log("No match - different types");
}
// Output: "No match - different types"

// If you need to compare after conversion, be explicit:
if (Number(userInput) === targetNumber) {
    console.log("Match!");
}
// Output: "Match!"
```

**🔍 How to Recognize**:
- Look for `==` or `!=` in code reviews
- ESLint rule: `eqeqeq` will flag these
- If you see unexpected type coercion bugs, check for `==`

---

### Pitfall 2: Forgetting Break in Switch Statements

**❌ What Beginners Do**:
```javascript
let day = 2;

switch (day) {
    case 1:
        console.log("Monday");
    case 2:
        console.log("Tuesday");
    case 3:
        console.log("Wednesday");
}
// Output: Tuesday, Wednesday (both print!)
```

**⚠️ Why It's Problematic**:
- Causes "fall-through" behavior where multiple cases execute
- Creates hard-to-debug logic errors
- User sees multiple outputs when only one is expected
- Performance issue: unnecessary code execution

**✅ The Right Way**:
```javascript
let day = 2;

switch (day) {
    case 1:
        console.log("Monday");
        break;  // Prevents fall-through
    case 2:
        console.log("Tuesday");
        break;
    case 3:
        console.log("Wednesday");
        break;
    default:
        console.log("Invalid day");
}
// Output: Tuesday (only)

// Intentional fall-through (with comment):
switch (day) {
    case 6:
    case 7:
        console.log("Weekend!");
        break;  // Intentional fall-through for 6 and 7
    default:
        console.log("Weekday");
}
```

**🔍 How to Recognize**:
- Multiple case blocks executing when only one should
- Look for missing `break;` statements at end of each case
- ESLint rule: `no-fallthrough` catches this
- Comment `// falls through` for intentional cases

---

### Pitfall 3: Overcomplicated Nested Ternaries

**❌ What Beginners Do**:
```javascript
let score = 85;
let attendance = 90;
let grade = score >= 90 ? attendance >= 80 ? "A+" : "A" : score >= 80 ? attendance >= 80 ? "B+" : "B" : score >= 70 ? "C" : "F";

console.log(grade);
// Unreadable and error-prone!
```

**⚠️ Why It's Problematic**:
- Extremely difficult to read and understand
- Easy to make logic errors
- Hard to debug when something goes wrong
- Nightmare to maintain or modify
- Violates the principle of "code is read more than written"

**✅ The Right Way**:
```javascript
let score = 85;
let attendance = 90;
let grade;

// Method 1: Clear if-else chain
if (score >= 90) {
    grade = attendance >= 80 ? "A+" : "A";
} else if (score >= 80) {
    grade = attendance >= 80 ? "B+" : "B";
} else if (score >= 70) {
    grade = "C";
} else {
    grade = "F";
}

// Method 2: Function for complex logic
function calculateGrade(score, attendance) {
    let baseGrade;
    
    if (score >= 90) baseGrade = "A";
    else if (score >= 80) baseGrade = "B";
    else if (score >= 70) baseGrade = "C";
    else return "F";
    
    // Add plus for good attendance
    return attendance >= 80 ? baseGrade + "+" : baseGrade;
}

let grade = calculateGrade(score, attendance);
console.log(grade); // "B+"
```

**🔍 How to Recognize**:
- Ternary operators nested more than 2 levels deep
- Lines of code extending past screen width
- Having to trace logic with your finger
- Rule of thumb: If a ternary has another ternary, refactor to if-else

---

### Pitfall 4: Not Handling All Cases

**❌ What Beginners Do**:
```javascript
let trafficLight = "yellow";

if (trafficLight === "red") {
    console.log("Stop");
} else if (trafficLight === "green") {
    console.log("Go");
}
// Yellow light does nothing - no output!
// No error thrown, just silent failure
```

**⚠️ Why It's Problematic**:
- Silent failures are hard to debug
- Users get no feedback when unexpected input occurs
- Production bugs that only appear with specific edge cases
- Violates defensive programming principles

**✅ The Right Way**:
```javascript
let trafficLight = "yellow";

if (trafficLight === "red") {
    console.log("Stop");
} else if (trafficLight === "green") {
    console.log("Go");
} else if (trafficLight === "yellow") {
    console.log("Caution - prepare to stop");
} else {
    console.log("Error: Invalid traffic light state");
    // In production: log error, use default safe behavior
}

// Alternative with switch (good for exhaustive cases):
switch (trafficLight) {
    case "red":
        console.log("Stop");
        break;
    case "yellow":
        console.log("Caution");
        break;
    case "green":
        console.log("Go");
        break;
    default:
        console.log("Error: Invalid state");
        // Could also throw an error in strict mode
        // throw new Error(`Invalid traffic light: ${trafficLight}`);
}
```

**🔍 How to Recognize**:
- Missing `else` clause in if-else chains
- Missing `default` case in switch statements
- Variables that might remain undefined
- Look for: "What happens if none of these conditions are true?"

---

### Pitfall 5: Confusing Assignment (=) with Comparison (===)

**❌ What Beginners Do**:
```javascript
let isLoggedIn = false;

if (isLoggedIn = true) {  // BUG: Assignment instead of comparison!
    console.log("Welcome back!");
}
// Output: "Welcome back!" (always, even when should be false)
```

**⚠️ Why It's Problematic**:
- `=` assigns value and returns it (true in this case)
- Condition always evaluates to the assigned value
- Extremely subtle bug that's hard to spot
- Completely changes program behavior
- One of the most common typos in programming

**✅ The Right Way**:
```javascript
let isLoggedIn = false;

// Correct comparison
if (isLoggedIn === true) {
    console.log("Welcome back!");
}

// Even better: direct boolean check
if (isLoggedIn) {
    console.log("Welcome back!");
}

// Most defensive: use Yoda conditions (value first)
if (true === isLoggedIn) {  // If you type =, causes error
    console.log("Welcome back!");
}
// Though Yoda conditions are less readable
```

**🔍 How to Recognize**:
- Look for `=` inside if/while conditions
- ESLint rule: `no-cond-assign` prevents this
- Modern editors highlight this differently
- If code always takes one branch regardless of variable value, check for this

---

### Pitfall 6: Not Considering Operator Precedence

**❌ What Beginners Do**:
```javascript
let age = 25;
let hasLicense = true;
let hasInsurance = false;

// Intended: age >= 18 AND (hasLicense OR hasInsurance)
if (age >= 18 && hasLicense || hasInsurance) {
    console.log("Can rent car");
}

// Test: age=15, hasLicense=false, hasInsurance=true
// Expected: Cannot rent (age too low)
// Actual: "Can rent car" (WRONG!)
```

**⚠️ Why It's Problematic**:
- `&&` has higher precedence than `||`
- Expression evaluates as: `(age >= 18 && hasLicense) || hasInsurance`
- This allows insurance alone to pass, ignoring age requirement
- Creates security vulnerabilities in authentication/authorization logic

**✅ The Right Way**:
```javascript
let age = 15;
let hasLicense = false;
let hasInsurance = true;

// Use parentheses to make intent explicit
if (age >= 18 && (hasLicense || hasInsurance)) {
    console.log("Can rent car");
} else {
    console.log("Cannot rent car");
}
// Output: "Cannot rent car" (correct!)

// Alternative: break into clear steps
let isAdult = age >= 18;
let hasValidDocuments = hasLicense || hasInsurance;

if (isAdult && hasValidDocuments) {
    console.log("Can rent car");
}
```

**🔍 How to Recognize**:
- Complex conditions with mixed `&&` and `||`
- Logic bugs that only appear with specific input combinations
- When in doubt, add parentheses for clarity
- Use truth tables to verify all combinations

---

### Pitfall 7: Unnecessary Nesting (Arrow Anti-pattern)

**❌ What Beginners Do**:
```javascript
function processOrder(user, order) {
    if (user) {
        if (user.isActive) {
            if (order) {
                if (order.items.length > 0) {
                    if (order.total > 0) {
                        console.log("Processing order...");
                        // Actual logic here, deeply nested
                    }
                }
            }
        }
    }
}
// The "Arrow" or "Pyramid of Doom"
```

**⚠️ Why It's Problematic**:
- Hard to read and follow
- Difficult to add new conditions
- More prone to errors
- Wastes horizontal space
- Makes code less maintainable

**✅ The Right Way**:
```javascript
function processOrder(user, order) {
    // Guard clauses: fail fast, return early
    if (!user) {
        console.log("Error: No user provided");
        return;
    }
    
    if (!user.isActive) {
        console.log("Error: User account is inactive");
        return;
    }
    
    if (!order) {
        console.log("Error: No order provided");
        return;
    }
    
    if (order.items.length === 0) {
        console.log("Error: Order has no items");
        return;
    }
    
    if (order.total <= 0) {
        console.log("Error: Invalid order total");
        return;
    }
    
    // Happy path at the end, not nested
    console.log("Processing order...");
    // Actual logic here, at base indentation level
}
```

**🔍 How to Recognize**:
- Code that slopes to the right (arrow shape)
- More than 3 levels of nesting
- if statements without else branches just wrapping code
- Look for opportunities to invert conditions and return early

---

## 7. KNOWLEDGE CHECK MCQs

### Question 1
What will be the output of the following code?
```javascript
let x = 10;
if (x > 5) 
    console.log("A");
    console.log("B");
console.log("C");
```

**A)** A B C  
**B)** A C  
**C)** B C  
**D)** A B

**✅ Correct Answer: A)** A B C

**Explanation**: Without curly braces, only the first statement after `if` is conditional. The second `console.log("B")` is outside the if block and always executes. This is why braces are recommended even for single statements.

**💼 Interview Tip**: Interviewers use this to test understanding of block scope. Always use curly braces for clarity.

**🎯 Key Concept**: Without `{}`, only the immediately following statement is part of the if block.

---

### Question 2
Which statement is TRUE about the switch statement?

**A)** It can handle range comparisons like `score >= 90`  
**B)** It uses loose equality (==) for comparison  
**C)** It uses strict equality (===) for comparison  
**D)** Fall-through behavior never happens

**✅ Correct Answer: C)** It uses strict equality (===) for comparison

**Explanation**: Switch always uses `===` for comparison. It cannot handle ranges directly (use `switch(true)` pattern or if-else instead). Fall-through happens when `break` is omitted.

**💼 Interview Tip**: "Switch uses strict equality" is a common gotcha question. Also be ready to explain when switch vs if-else is appropriate.

**🎯 Key Concept**: switch uses `===`, making it suitable only for exact value matching.

---

### Question 3
What is the output?
```javascript
let result = false || true && false;
console.log(result);
```

**A)** true  
**B)** false  
**C)** undefined  
**D)** Error

**✅ Correct Answer: B)** false

**Explanation**: `&&` has higher precedence than `||`. 
- First: `true && false` → false
- Then: `false || false` → false

**💼 Interview Tip**: Precedence questions are common. Remember: `&&` before `||`, or use parentheses.

**🎯 Key Concept**: Operator precedence affects evaluation order. When in doubt, use parentheses.

---

### Question 4
Which value is NOT falsy in JavaScript?

**A)** 0  
**B)** ""  
**C)** "0"  
**D)** null

**✅ Correct Answer: C)** "0"

**Explanation**: The string `"0"` is truthy (any non-empty string is truthy). The 6 falsy values are: `false`, `0`, `""`, `null`, `undefined`, `NaN`.

**💼 Interview Tip**: This trips up many developers. Empty string vs string with "0" is a classic interview question.

**🎯 Key Concept**: Only 6 values are falsy. Everything else is truthy, including "0", "false", [], {}.

---

### Question 5
What happens in this code?
```javascript
switch (2) {
    case 1:
        console.log("One");
    case 2:
        console.log("Two");
    case 3:
        console.log("Three");
}
```

**A)** Output: Two  
**B)** Output: Two Three  
**C)** Output: One Two Three  
**D)** Error

**✅ Correct Answer: B)** Output: Two Three

**Explanation**: Case 2 matches, but missing `break` causes fall-through to case 3. Always add `break` unless intentional fall-through is needed.

**💼 Interview Tip**: Explain that this is fall-through behavior and that you'd add `break` statements in real code.

**🎯 Key Concept**: Omitting `break` causes execution to continue to subsequent cases.

---

### Question 6
What is the value of `grade`?
```javascript
let score = 85;
let grade = score >= 90 ? "A" : score >= 80 ? "B" : "C";
```

**A)** A  
**B)** B  
**C)** C  
**D)** undefined

**✅ Correct Answer: B)** B

**Explanation**: Ternary evaluates left to right:
- `85 >= 90` → false, check next
- `85 >= 80` → true, return "B"

**💼 Interview Tip**: Be able to trace nested ternaries, but also mention that deep nesting reduces readability.

**🎯 Key Concept**: Nested ternaries evaluate left to right, stopping at first true condition.

---

### Question 7
Which control structure is most efficient for comparing one variable against 10+ different values?

**A)** if-else chain  
**B)** switch statement  
**C)** Nested if-else  
**D)** Multiple separate if statements

**✅ Correct Answer: B)** switch statement

**Explanation**: Switch statements are optimized for multiple equality checks against the same variable. Modern JavaScript engines use jump tables for switches, making them faster than if-else chains for many comparisons.

**💼 Interview Tip**: Discuss both readability and performance. Switch is clearer AND faster for many equality checks.

**🎯 Key Concept**: Use switch for multiple exact matches, if-else for ranges and complex conditions.

---

### Question 8
What will this output?
```javascript
let age = "18";
if (age === 18) {
    console.log("Adult");
} else {
    console.log("Not adult");
}
```

**A)** Adult  
**B)** Not adult  
**C)** Error  
**D)** undefined

**✅ Correct Answer: B)** Not adult

**Explanation**: `===` checks both value AND type. `"18"` (string) !== `18` (number). This is why input validation and type conversion are crucial.

**💼 Interview Tip**: This tests understanding of strict equality. Mention you'd convert user input: `Number(age) === 18`.

**🎯 Key Concept**: Always use `===` and explicitly convert types when needed.

---

### Question 9
What is wrong with this code?
```javascript
if (temperature > 30)
    console.log("Hot");
    console.log("Stay hydrated");
```

**A)** Nothing wrong  
**B)** Missing semicolon  
**C)** "Stay hydrated" always prints (not in if block)  
**D)** Syntax error

**✅ Correct Answer: C)** "Stay hydrated" always prints

**Explanation**: Without braces, only the first line is conditional. The second line is outside the if block. This is why curly braces are always recommended.

**💼 Interview Tip**: Explain that this is a common bug and that team coding standards should require braces.

**🎯 Key Concept**: Always use `{}` for if statements, even for single lines.

---

### Question 10
Which is the correct way to check if a variable is defined?

**A)** `if (variable) {}`  
**B)** `if (variable === undefined) {}`  
**C)** `if (typeof variable !== "undefined") {}`  
**D)** `if (variable !== null) {}`

**✅ Correct Answer: C)** `if (typeof variable !== "undefined") {}`

**Explanation**: Using `typeof` is the safest way because it doesn't throw an error if the variable doesn't exist at all. Options A, B, and D would throw ReferenceError for undeclared variables.

**💼 Interview Tip**: Distinguish between undefined (declared but no value) vs undeclared (never declared). typeof handles both.

**🎯 Key Concept**: `typeof` is the safe way to check for undefined without risking ReferenceError.

---

### Question 11
What value does this expression return?
```javascript
let result = 5 > 3 ? (3 > 1 ? "A" : "B") : "C";
```

**A)** A  
**B)** B  
**C)** C  
**D)** true

**✅ Correct Answer: A)** A

**Explanation**: 
- Outer ternary: `5 > 3` → true, so evaluate left side
- Inner ternary: `3 > 1` → true, return "A"

**💼 Interview Tip**: Walk through nested ternaries step-by-step. Mention you'd refactor for readability in production code.

**🎯 Key Concept**: Ternaries can be nested but should be kept simple for maintainability.

---

### Question 12
What is the best practice for switch statement structure?

**A)** Always put default case first  
**B)** Never use break statements  
**C)** Always include a default case  
**D)** Use switch for range comparisons

**✅ Correct Answer: C)** Always include a default case

**Explanation**: Default handles unexpected values, preventing silent failures. It should be last (by convention), break is needed (except for fall-through), and switch is for exact matches, not ranges.

**💼 Interview Tip**: Mention defensive programming - always handle the unexpected case.

**🎯 Key Concept**: Default case is your safety net for unexpected input.

---

### Question 13
How many times will "Hello" be printed?
```javascript
let count = 0;
if (count++) {
    console.log("Hello");
}
console.log(count);
```

**A)** 0 times, count is 0  
**B)** 0 times, count is 1  
**C)** 1 time, count is 1  
**D)** Error

**✅ Correct Answer: B)** 0 times, count is 1

**Explanation**: Post-increment (`count++`) returns the current value (0), then increments. `if (0)` is false, so "Hello" doesn't print. But count becomes 1.

**💼 Interview Tip**: Explain pre-increment (`++count`) vs post-increment (`count++`) differences.

**🎯 Key Concept**: Post-increment returns old value before incrementing.

---

### Question 14
Which condition checks if a number is in the range 10-20 (inclusive)?

**A)** `if (num >= 10 && num <= 20)`  
**B)** `if (num > 10 && num < 20)`  
**C)** `if (num >= 10 || num <= 20)`  
**D)** `if (num > 10 || num < 20)`

**✅ Correct Answer: A)** `if (num >= 10 && num <= 20)`

**Explanation**: "Inclusive" means including the boundaries. Use `>=` and `<=` with `&&` (both conditions must be true).

**💼 Interview Tip**: Clarify "inclusive" vs "exclusive" ranges in requirements before coding.

**🎯 Key Concept**: Range checking requires `&&` (AND), not `||` (OR).

---

### Question 15
What is logged when `status` is undefined?
```javascript
let status;
switch (status) {
    case undefined:
        console.log("Undefined");
        break;
    case null:
        console.log("Null");
        break;
    default:
        console.log("Other");
}
```

**A)** Undefined  
**B)** Null  
**C)** Other  
**D)** Error

**✅ Correct Answer: A)** Undefined

**Explanation**: Switch uses strict equality (`===`). `undefined === undefined` is true, so the first case matches.

**💼 Interview Tip**: Show you understand that undefined and null are distinct values in JavaScript.

**🎯 Key Concept**: undefined and null are different; switch can match either explicitly.

---

### Question 16
Which code correctly implements "if age is NOT between 18 and 65"?

**A)** `if (age < 18 && age > 65)`  
**B)** `if (age < 18 || age > 65)`  
**C)** `if (!(age >= 18 && age <= 65))`  
**D)** Both B and C

**✅ Correct Answer: D)** Both B and C

**Explanation**: 
- Option B: De Morgan's Law - "less than 18 OR greater than 65"
- Option C: Negation of "between 18 and 65"
- Both are logically equivalent
- Option A is wrong (can't be both < 18 AND > 65 simultaneously)

**💼 Interview Tip**: Be comfortable with De Morgan's Laws: `!(A && B)` = `!A || !B`

**🎯 Key Concept**: Multiple ways to express the same logic; choose the most readable.

---

### Question 17
What makes this code problematic?
```javascript
if (user.isAdmin = true) {
    deleteAllData();
}
```

**A)** Missing semicolon  
**B)** Assignment instead of comparison  
**C)** Should use == not =  
**D)** Nothing wrong

**✅ Correct Answer: B)** Assignment instead of comparison

**Explanation**: `=` assigns true to `user.isAdmin` (making everyone an admin!) and the condition always evaluates to true. Should be `===` for comparison.

**💼 Interview Tip**: Mention this is a dangerous bug, especially in authentication/authorization code.

**🎯 Key Concept**: `=` is assignment, `===` is comparison. This typo can create security vulnerabilities.

---

### Question 18
When should you use if-else instead of switch?

**A)** When comparing against 10+ values  
**B)** When testing ranges (>, <, >=, <=)  
**C)** When values are strings  
**D)** When values are numbers

**✅ Correct Answer: B)** When testing ranges

**Explanation**: Switch only handles exact equality (===). For ranges, complex conditions, or logical operators (&&, ||), use if-else.

**💼 Interview Tip**: Provide examples of each use case to show practical understanding.

**🎯 Key Concept**: Choose the right tool: switch for exact matches, if-else for everything else.

---

### Question 19
What is the output?
```javascript
let x = 10;
let result = x > 5 && console.log("Yes") || console.log("No");
```

**A)** Yes (logged), result is undefined  
**B)** Yes No (both logged), result is undefined  
**C)** Yes (logged), result is true  
**D)** No (logged), result is false

**✅ Correct Answer: A)** Yes (logged), result is undefined

**Explanation**: 
- `x > 5` → true
- `true && console.log("Yes")` → logs "Yes", returns undefined
- `undefined || console.log("No")` → undefined is falsy, but short-circuit already logged "Yes"
- Wait, actually: `undefined || console.log("No")` → logs "No" too!

**Correct Answer is actually B)** - both log, result is undefined from final console.log.

**💼 Interview Tip**: console.log returns undefined. Explain short-circuit evaluation carefully.

**🎯 Key Concept**: Be careful with side effects in conditional expressions.

---

### Question 20
What is the most readable way to check multiple conditions?

**A)** One long if statement with && and ||  
**B)** Nested if-else statements  
**C)** Early returns with guard clauses  
**D)** Switch statement for all conditions

**✅ Correct Answer: C)** Early returns with guard clauses

**Explanation**: Guard clauses (fail fast, return early) keep code flat and readable. They handle edge cases first, leaving the happy path at the base indentation level.

**💼 Interview Tip**: Mention the "arrow anti-pattern" and why flat code is more maintainable.

**🎯 Key Concept**: Code is read more than written; prioritize readability.

---

## 8. REAL INTERVIEW QUESTIONS

### Question 1: FizzBuzz (Entry Level)
**⏱️ Expected Time**: 5-10 minutes  
**🎯 Difficulty**: Entry

**Question**: Write a program that prints numbers from 1 to 100. But for multiples of 3, print "Fizz" instead of the number, for multiples of 5 print "Buzz", and for multiples of both 3 and 5 print "FizzBuzz".

**💡 What They're Testing**:
- Understanding of modulo operator
- Control flow logic
- Condition ordering (avoiding redundant checks)
- Attention to detail

**✅ Strong Answer**:
```javascript
for (let i = 1; i <= 100; i++) {
    if (i % 15 === 0) {
        console.log("FizzBuzz");
    } else if (i % 3 === 0) {
        console.log("Fizz");
    } else if (i % 5 === 0) {
        console.log("Buzz");
    } else {
        console.log(i);
    }
}

// Alternative: String concatenation approach
for (let i = 1; i <= 100; i++) {
    let output = "";
    if (i % 3 === 0) output += "Fizz";
    if (i % 5 === 0) output += "Buzz";
    console.log(output || i);
}
```

**🔄 Follow-up Questions**:
1. "Why check `i % 15` first?" → Because it's more specific; checking % 3 first would match 15 and never reach FizzBuzz.
2. "How would you make this extensible?" → Use a configuration object mapping divisors to words.
3. "What if we add more rules (7 → 'Bazz')?" → Show the configuration approach:

```javascript
const rules = [[3, "Fizz"], [5, "Buzz"], [7, "Bazz"]];

for (let i = 1; i <= 100; i++) {
    let output = "";
    for (let [divisor, word] of rules) {
        if (i % divisor === 0) output += word;
    }
    console.log(output || i);
}
```

**⚠️ Red Flags to Avoid**:
- Checking % 3 before % 15 (wrong logic)
- Using nested ternaries (unreadable)
- Hardcoding output instead of using control flow
- Not handling the "both" case

**💼 Pro Tips**:
- Mention time complexity: O(n)
- Explain choice between approaches
- Ask about requirements before coding (upper limit, additional rules)

---

### Question 2: Grade Calculation System (Entry Level)
**⏱️ Expected Time**: 10-15 minutes  
**🎯 Difficulty**: Entry

**Question**: Create a function that takes a score (0-100) and returns a letter grade. Also handle invalid inputs. Grades: A (90-100), B (80-89), C (70-79), D (60-69), F (0-59).

**💡 What They're Testing**:
- Input validation
- Range checking logic
- Error handling
- Function structure

**✅ Strong Answer**:
```javascript
function calculateGrade(score) {
    // Input validation
    if (typeof score !== "number") {
        return "Error: Score must be a number";
    }
    
    if (score < 0 || score > 100) {
        return "Error: Score must be between 0 and 100";
    }
    
    // Grade calculation
    if (score >= 90) return "A";
    if (score >= 80) return "B";
    if (score >= 70) return "C";
    if (score >= 60) return "D";
    return "F";
}

// Test cases
console.log(calculateGrade(95));   // "A"
console.log(calculateGrade(85));   // "B"
console.log(calculateGrade(55));   // "F"
console.log(calculateGrade(101));  // "Error: Score must be between 0 and 100"
console.log(calculateGrade("90")); // "Error: Score must be a number"
```

**🔄 Follow-up Questions**:
1. "How would you add +/- grades?" → 
```javascript
function calculateDetailedGrade(score) {
    if (typeof score !== "number" || score < 0 || score > 100) {
        return "Error: Invalid score";
    }
    
    if (score >= 97) return "A+";
    if (score >= 93) return "A";
    if (score >= 90) return "A-";
    // ... continue pattern
}
```

2. "What about weighted grades?" → Discuss taking an array of {score, weight} objects

3. "How would you handle multiple students?" → Show array processing with map/filter

**⚠️ Red Flags to Avoid**:
- Not validating inputs
- Using `==` instead of `===`
- Overlapping range conditions
- Not testing edge cases (0, 100, boundaries)

**💼 Pro Tips**:
- Ask about edge case handling (59.5?)
- Mention you'd document the function
- Discuss potential for unit tests
- Show awareness of IEEE 754 float precision for edge cases

---

### Question 3: Traffic Light Simulator (Mid Level)
**⏱️ Expected Time**: 15-20 minutes  
**🎯 Difficulty**: Mid

**Question**: Implement a traffic light system that cycles through Red → Green → Yellow → Red. Include a method to get the current state, advance to the next state, and determine if pedestrians can cross.

**💡 What They're Testing**:
- State management
- Object-oriented thinking
- Method design
- Edge case handling

**✅ Strong Answer**:
```javascript
class TrafficLight {
    constructor() {
        this.states = ['Red', 'Green', 'Yellow'];
        this.currentIndex = 0;
    }
    
    getCurrentState() {
        return this.states[this.currentIndex];
    }
    
    nextState() {
        this.currentIndex = (this.currentIndex + 1) % this.states.length;
        return this.getCurrentState();
    }
    
    canPedestriansCross() {
        return this.getCurrentState() === 'Red';
    }
    
    getDriverInstruction() {
        const state = this.getCurrentState();
        switch (state) {
            case 'Red':
                return 'Stop';
            case 'Yellow':
                return 'Prepare to stop';
            case 'Green':
                return 'Go';
            default:
                return 'Error: Invalid state';
        }
    }
}

// Usage
const light = new TrafficLight();
console.log(light.getCurrentState());        // "Red"
console.log(light.canPedestriansCross());    // true
console.log(light.getDriverInstruction());   // "Stop"

light.nextState();
console.log(light.getCurrentState());        // "Green"
console.log(light.canPedestriansCross());    // false

light.nextState();
console.log(light.getCurrentState());        // "Yellow"

light.nextState();
console.log(light.getCurrentState());        // "Red" (cycles back)
```

**🔄 Follow-up Questions**:
1. "How would you add timing to auto-advance states?"
```javascript
class TimedTrafficLight extends TrafficLight {
    constructor() {
        super();
        this.timings = { Red: 30, Green: 45, Yellow: 5 }; // seconds
        this.timer = null;
    }
    
    startAutomatic() {
        this.advance();
    }
    
    advance() {
        const currentState = this.getCurrentState();
        const duration = this.timings[currentState] * 1000;
        
        this.timer = setTimeout(() => {
            this.nextState();
            this.advance();
        }, duration);
    }
    
    stop() {
        clearTimeout(this.timer);
    }
}
```

2. "How would you handle emergency vehicle override?"
```javascript
class EmergencyTrafficLight extends TrafficLight {
    setEmergencyMode(direction) {
        this.emergencyMode = true;
        this.emergencyDirection = direction;
        // Force green for emergency direction
        // Implementation details...
    }
    
    clearEmergencyMode() {
        this.emergencyMode = false;
        // Resume normal cycle
    }
}
```

3. "How would you coordinate multiple lights at an intersection?"
```javascript
class Intersection {
    constructor() {
        this.lights = {
            north: new TrafficLight(),
            south: new TrafficLight(),
            east: new TrafficLight(),
            west: new TrafficLight()
        };
    }
    
    syncLights() {
        // North-South synchronized
        // East-West synchronized but opposite
    }
}
```

**⚠️ Red Flags to Avoid**:
- Hardcoding state transitions without cycle logic
- Not handling wrap-around (Yellow → Red)
- Using strings everywhere instead of constants/enums
- No validation in state changes

**💼 Pro Tips**:
- Mention state machine pattern
- Discuss immutability (return new state vs mutate)
- Consider using enums or constants for states
- Think about testability

---

### Question 4: User Permission System (Mid Level)
**⏱️ Expected Time**: 20 minutes  
**🎯 Difficulty**: Mid

**Question**: Create a function that checks if a user has permission to perform an action. Users can be: 'guest', 'user', 'moderator', or 'admin'. Actions include: 'read', 'write', 'delete', 'ban'. Implement proper permission hierarchy.

**💡 What They're Testing**:
- Complex conditional logic
- Permission/authorization concepts
- Code organization
- Scalability thinking

**✅ Strong Answer**:
```javascript
// Permission configuration
const PERMISSIONS = {
    guest: ['read'],
    user: ['read', 'write'],
    moderator: ['read', 'write', 'delete'],
    admin: ['read', 'write', 'delete', 'ban']
};

// Method 1: Direct lookup
function hasPermission(userRole, action) {
    // Validate inputs
    if (!PERMISSIONS[userRole]) {
        throw new Error(`Invalid role: ${userRole}`);
    }
    
    return PERMISSIONS[userRole].includes(action);
}

// Method 2: Hierarchical with inheritance
const ROLE_HIERARCHY = {
    guest: 0,
    user: 1,
    moderator: 2,
    admin: 3
};

const ACTION_REQUIREMENTS = {
    read: 0,      // guest level
    write: 1,     // user level
    delete: 2,    // moderator level
    ban: 3        // admin level
};

function hasPermissionHierarchical(userRole, action) {
    const userLevel = ROLE_HIERARCHY[userRole];
    const requiredLevel = ACTION_REQUIREMENTS[action];
    
    if (userLevel === undefined) {
        throw new Error(`Invalid role: ${userRole}`);
    }
    
    if (requiredLevel === undefined) {
        throw new Error(`Invalid action: ${action}`);
    }
    
    return userLevel >= requiredLevel;
}

// Test cases
console.log(hasPermission('guest', 'read'));     // true
console.log(hasPermission('guest', 'write'));    // false
console.log(hasPermission('admin', 'ban'));      // true
console.log(hasPermission('user', 'delete'));    // false

// Advanced: Handle special cases
function checkPermission(user, action, resource) {
    const role = user.role;
    
    // Owner can always edit their own content
    if (action === 'write' && resource.ownerId === user.id) {
        return true;
    }
    
    // Check role-based permission
    return hasPermissionHierarchical(role, action);
}
```

**🔄 Follow-up Questions**:
1. "How would you handle custom permissions per user?"
```javascript
function hasPermissionWithCustom(user, action) {
    // Check custom permissions first
    if (user.customPermissions?.includes(action)) {
        return true;
    }
    
    // Check custom denials
    if (user.deniedPermissions?.includes(action)) {
        return false;
    }
    
    // Fall back to role-based
    return hasPermission(user.role, action);
}
```

2. "What about resource-level permissions (can edit only own posts)?"
```javascript
function canPerformAction(user, action, resource) {
    // Global admin override
    if (user.role === 'admin') return true;
    
    // Resource ownership check
    if (resource.ownerId === user.id) {
        return ['read', 'write', 'delete'].includes(action);
    }
    
    // Public read access
    if (action === 'read' && resource.isPublic) {
        return true;
    }
    
    // Fall back to role permissions
    return hasPermission(user.role, action);
}
```

3. "How would you optimize this for performance?"
```javascript
// Caching strategy
const permissionCache = new Map();

function getCacheKey(userRole, action) {
    return `${userRole}:${action}`;
}

function hasPermissionCached(userRole, action) {
    const key = getCacheKey(userRole, action);
    
    if (permissionCache.has(key)) {
        return permissionCache.get(key);
    }
    
    const result = hasPermission(userRole, action);
    permissionCache.set(key, result);
    return result;
}
```

**⚠️ Red Flags to Avoid**:
- Giant if-else chain checking every combination
- Not validating inputs
- Hardcoding permissions in multiple places
- No consideration for scalability

**💼 Pro Tips**:
- Mention RBAC (Role-Based Access Control) pattern
- Discuss separation of concerns
- Consider database storage for permissions
- Think about audit logging for security

---

### Question 5: Leap Year Calculator (Entry Level)
**⏱️ Expected Time**: 10 minutes  
**🎯 Difficulty**: Entry

**Question**: Write a function to determine if a year is a leap year. Rules: divisible by 4 is leap year, EXCEPT divisible by 100 is NOT, EXCEPT divisible by 400 IS.

**💡 What They're Testing**:
- Complex boolean logic
- Understanding of nested conditions
- Edge case thinking

**✅ Strong Answer**:
```javascript
function isLeapYear(year) {
    // Validate input
    if (typeof year !== 'number' || year < 0 || !Number.isInteger(year)) {
        throw new Error('Year must be a positive integer');
    }
    
    // Method 1: Nested conditions (most readable)
    if (year % 400 === 0) {
        return true;  // Divisible by 400
    }
    if (year % 100 === 0) {
        return false; // Divisible by 100 but not 400
    }
    if (year % 4 === 0) {
        return true;  // Divisible by 4 but not 100
    }
    return false;     // Not divisible by 4
    
    // Method 2: Single expression (more concise)
    // return (year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0);
}

// Test cases
console.log(isLeapYear(2000));  // true (divisible by 400)
console.log(isLeapYear(1900));  // false (divisible by 100 but not 400)
console.log(isLeapYear(2020));  // true (divisible by 4 but not 100)
console.log(isLeapYear(2021));  // false (not divisible by 4)
```

**🔄 Follow-up Questions**:
1. "Which approach is better and why?"
   - Method 1: More readable, easier to debug, self-documenting
   - Method 2: More concise, demonstrates boolean logic understanding
   - Answer: Depends on team standards, but readability usually wins

2. "How would you calculate the number of leap years in a range?"
```javascript
function countLeapYears(startYear, endYear) {
    let count = 0;
    for (let year = startYear; year <= endYear; year++) {
        if (isLeapYear(year)) {
            count++;
        }
    }
    return count;
}

// Optimized mathematical approach (advanced)
function countLeapYearsMath(startYear, endYear) {
    const years = endYear - startYear + 1;
    const by4 = Math.floor(endYear / 4) - Math.floor((startYear - 1) / 4);
    const by100 = Math.floor(endYear / 100) - Math.floor((startYear - 1) / 100);
    const by400 = Math.floor(endYear / 400) - Math.floor((startYear - 1) / 400);
    return by4 - by100 + by400;
}
```

**⚠️ Red Flags to Avoid**:
- Checking conditions in wrong order (% 4 before % 400)
- Not validating input type
- Using floating point years
- Forgetting the century rule exception

**💼 Pro Tips**:
- Explain the historical reason for leap year rules
- Mention edge cases (negative years, year 0)
- Show multiple approaches and discuss tradeoffs

---

### Question 6: String to Number Grade Converter (Mid Level)
**⏱️ Expected Time**: 15 minutes  
**🎯 Difficulty**: Mid

**Question**: Create a function that converts letter grades (with +/-) to GPA on a 4.0 scale. Handle: A+ (4.0), A (4.0), A- (3.7), B+ (3.3), B (3.0), B- (2.7), etc. F is 0.0.

**💡 What They're Testing**:
- String parsing
- Complex mapping logic
- Error handling
- Code organization

**✅ Strong Answer**:
```javascript
function letterToGPA(grade) {
    // Validate and normalize input
    if (typeof grade !== 'string') {
        throw new Error('Grade must be a string');
    }
    
    grade = grade.trim().toUpperCase();
    
    // GPA mapping
    const gradeMap = {
        'A+': 4.0, 'A': 4.0, 'A-': 3.7,
        'B+': 3.3, 'B': 3.0, 'B-': 2.7,
        'C+': 2.3, 'C': 2.0, 'C-': 1.7,
        'D+': 1.3, 'D': 1.0, 'D-': 0.7,
        'F': 0.0
    };
    
    if (gradeMap.hasOwnProperty(grade)) {
        return gradeMap[grade];
    }
    
    throw new Error(`Invalid grade: ${grade}`);
}

// Advanced: Calculate cumulative GPA
function calculateGPA(grades) {
    if (!Array.isArray(grades) || grades.length === 0) {
        throw new Error('Grades must be a non-empty array');
    }
    
    const gpaValues = grades.map(grade => {
        if (typeof grade === 'object') {
            // Weighted: {grade: 'A', credits: 3}
            return {
                points: letterToGPA(grade.grade) * grade.credits,
                credits: grade.credits
            };
        }
        // Unweighted: just 'A'
        return { points: letterToGPA(grade), credits: 1 };
    });
    
    const totalPoints = gpaValues.reduce((sum, g) => sum + g.points, 0);
    const totalCredits = gpaValues.reduce((sum, g) => sum + g.credits, 0);
    
    return (totalPoints / totalCredits).toFixed(2);
}

// Test cases
console.log(letterToGPA('A'));      // 4.0
console.log(letterToGPA('b+'));     // 3.3 (case insensitive)
console.log(letterToGPA(' C- '));   // 1.7 (handles whitespace)

console.log(calculateGPA(['A', 'B', 'A-']));  // "3.57"
console.log(calculateGPA([
    { grade: 'A', credits: 3 },
    { grade: 'B+', credits: 4 },
    { grade: 'A-', credits: 3 }
])); // "3.69"
```

**🔄 Follow-up Questions**:
1. "How would you handle percentage grades (85% → B)?"
```javascript
function percentageToLetter(percentage) {
    if (percentage >= 97) return 'A+';
    if (percentage >= 93) return 'A';
    if (percentage >= 90) return 'A-';
    if (percentage >= 87) return 'B+';
    if (percentage >= 83) return 'B';
    if (percentage >= 80) return 'B-';
    // ... continue pattern
    return 'F';
}

function percentageToGPA(percentage) {
    return letterToGPA(percentageToLetter(percentage));
}
```

2. "What about pass/fail courses?"
```javascript
function calculateGPAWithPF(grades) {
    const validGrades = grades.filter(g => {
        const grade = typeof g === 'object' ? g.grade : g;
        return grade !== 'P' && grade !== 'F';  // Exclude Pass/Fail
    });
    
    return calculateGPA(validGrades);
}
```

**⚠️ Red Flags to Avoid**:
- Not handling case sensitivity
- Not trimming whitespace
- Giant if-else chain instead of lookup object
- Not validating input

**💼 Pro Tips**:
- Mention different GPA scales (4.0, 5.0 weighted)
- Discuss rounding precision
- Consider international grading systems
- Think about data persistence

---

### Question 7: Multi-Currency Converter (Senior Level)
**⏱️ Expected Time**: 25-30 minutes  
**🎯 Difficulty**: Senior

**Question**: Build a currency converter that handles multiple currencies with exchange rates. Include rate caching, error handling, and support for chained conversions (USD → EUR → GBP).

**💡 What They're Testing**:
- System design thinking
- Error handling strategy
- Performance optimization
- API design

**✅ Strong Answer**:
```javascript
class CurrencyConverter {
    constructor() {
        // Base rates relative to USD
        this.rates = {
            USD: 1.0,
            EUR: 0.85,
            GBP: 0.73,
            JPY: 110.0,
            INR: 74.5
        };
        
        this.cache = new Map();
        this.cacheExpiry = 3600000; // 1 hour in milliseconds
    }
    
    // Get exchange rate from cache or calculate
    getRate(from, to) {
        if (from === to) return 1.0;
        
        const cacheKey = `${from}-${to}`;
        const cached = this.cache.get(cacheKey);
        
        // Check cache validity
        if (cached && Date.now() - cached.timestamp < this.cacheExpiry) {
            return cached.rate;
        }
        
        // Validate currencies
        if (!this.rates[from] || !this.rates[to]) {
            throw new Error(`Invalid currency: ${from} or ${to}`);
        }
        
        // Calculate rate: convert to USD, then to target
        const rate = this.rates[to] / this.rates[from];
        
        // Cache the result
        this.cache.set(cacheKey, {
            rate,
            timestamp: Date.now()
        });
        
        return rate;
    }
    
    // Convert amount between currencies
    convert(amount, from, to) {
        // Input validation
        if (typeof amount !== 'number' || amount < 0) {
            throw new Error('Amount must be a positive number');
        }
        
        if (!Number.isFinite(amount)) {
            throw new Error('Amount must be finite');
        }
        
        const rate = this.getRate(from, to);
        const result = amount * rate;
        
        return {
            original: {
                amount: amount.toFixed(2),
                currency: from
            },
            converted: {
                amount: result.toFixed(2),
                currency: to
            },
            rate: rate.toFixed(4),
            timestamp: new Date().toISOString()
        };
    }
    
    // Chain conversions: USD → EUR → GBP
    convertChain(amount, currencies) {
        if (!Array.isArray(currencies) || currencies.length < 2) {
            throw new Error('Need at least 2 currencies for chain conversion');
        }
        
        let currentAmount = amount;
        const conversions = [];
        
        for (let i = 0; i < currencies.length - 1; i++) {
            const from = currencies[i];
            const to = currencies[i + 1];
            const result = this.convert(currentAmount, from, to);
            
            conversions.push(result);
            currentAmount = parseFloat(result.converted.amount);
        }
        
        return {
            original: {
                amount: amount.toFixed(2),
                currency: currencies[0]
            },
            final: {
                amount: currentAmount.toFixed(2),
                currency: currencies[currencies.length - 1]
            },
            steps: conversions
        };
    }
    
    // Update exchange rates (simulate API call)
    updateRates(newRates) {
        // Validate new rates
        for (const [currency, rate] of Object.entries(newRates)) {
            if (typeof rate !== 'number' || rate <= 0) {
                throw new Error(`Invalid rate for ${currency}`);
            }
        }
        
        // Update rates and clear cache
        this.rates = { ...this.rates, ...newRates };
        this.cache.clear();
        
        return { success: true, timestamp: new Date().toISOString() };
    }
    
    // Get all supported currencies
    getSupportedCurrencies() {
        return Object.keys(this.rates);
    }
}

// Usage
const converter = new CurrencyConverter();

// Simple conversion
console.log(converter.convert(100, 'USD', 'EUR'));
// Output: { original: {amount: '100.00', currency: 'USD'}, 
//           converted: {amount: '85.00', currency: 'EUR'}, ... }

// Chain conversion
console.log(converter.convertChain(100, ['USD', 'EUR', 'GBP']));

// Update rates
converter.updateRates({ EUR: 0.87, GBP: 0.75 });
```

**🔄 Follow-up Questions**:
1. "How would you handle real-time rate updates from an API?"
```javascript
class LiveCurrencyConverter extends CurrencyConverter {
    constructor(apiEndpoint) {
        super();
        this.apiEndpoint = apiEndpoint;
        this.refreshInterval = null;
    }
    
    async fetchRates() {
        try {
            const response = await fetch(this.apiEndpoint);
            const data = await response.json();
            this.updateRates(data.rates);
        } catch (error) {
            console.error('Failed to fetch rates:', error);
            // Fall back to cached/default rates
        }
    }
    
    startAutoRefresh(intervalMs = 3600000) {
        this.refreshInterval = setInterval(() => {
            this.fetchRates();
        }, intervalMs);
    }
    
    stopAutoRefresh() {
        clearInterval(this.refreshInterval);
    }
}
```

2. "How would you handle offline mode?"
```javascript
class OfflineConverter extends CurrencyConverter {
    constructor() {
        super();
        this.loadFromStorage();
    }
    
    loadFromStorage() {
        const stored = localStorage.getItem('exchangeRates');
        if (stored) {
            const { rates, timestamp } = JSON.parse(stored);
            // Check if rates are not too old
            if (Date.now() - timestamp < 86400000) { // 24 hours
                this.rates = rates;
            }
        }
    }
    
    saveToStorage() {
        localStorage.setItem('exchangeRates', JSON.stringify({
            rates: this.rates,
            timestamp: Date.now()
        }));
    }
    
    updateRates(newRates) {
        super.updateRates(newRates);
        this.saveToStorage();
    }
}
```

3. "How would you optimize for high-volume conversions?"
```javascript
// Use Map for O(1) lookups
// Batch updates
// Implement rate limiting
// Use web workers for calculations
// Implement request coalescing
```

**⚠️ Red Flags to Avoid**:
- Hardcoded rates everywhere
- No error handling
- Precision loss with floating-point arithmetic
- Not considering currency code validity
- No caching strategy

**💼 Pro Tips**:
- Mention decimal.js for precise calculations
- Discuss distributed caching (Redis)
- Consider regulatory compliance (financial data)
- Mention monitoring and logging
- Discuss testing strategy for financial calculations

---


## 9. CHAPTER SUMMARY & NEXT STEPS

### 🎯 Key Takeaways

1. **Control flow determines execution order** - Your code doesn't always run top to bottom; conditions create branches

2. **Choose the right tool**:
   - **if-else**: Ranges, complex conditions, different tests
   - **switch**: Single value against many options
   - **ternary**: Simple inline conditionals
   - **Guard clauses**: Early returns for edge cases

3. **Always use strict equality (===)** - Avoid unexpected type coercion bugs

4. **Validate inputs first** - Check for null, undefined, wrong types before processing

5. **Break statements are crucial in switch** - Unless you intentionally want fall-through

6. **Truthy/Falsy can be tricky** - Remember the 6 falsy values: `false`, `0`, `""`, `null`, `undefined`, `NaN`

7. **Readability trumps cleverness** - Nested ternaries and complex conditions should be refactored



### 🔮 Tomorrow's Preview: Day 5 - Loops & Iteration

Get ready to learn how to repeat code efficiently:
- **for loops**: When you know how many times to repeat
- **while loops**: When you repeat based on a condition
- **do-while loops**: When you need to run at least once
- **Loop control**: break, continue, and nested loops
- **Common patterns**: Counting, accumulating, searching

**Sneak peek problem**: "Write a program that prints a multiplication table from 1 to 10."

You'll see how control flow (today) combines with loops (tomorrow) to create powerful programs!


### 🤔 Still Struggling?

**If you're confused about when to use what**:
- Draw a decision flowchart on paper first
- Write pseudocode before actual code
- Start with simple if-else, optimize later

**If your conditions aren't working**:
- Add console.log before each condition
- Check data types with `typeof`
- Test with simple, known values first

**If your switch statements aren't working**:
- Verify you're using `===` comparison (exact match)
- Check for missing `break` statements
- Ensure your expression type matches case types

**If nested conditions confuse you**:
- Flatten with guard clauses (early returns)
- Break into separate functions
- Draw the logic as a tree diagram

**Common mental blocks**:
- **"I don't know which to choose"** → Start with if-else, it works for everything
- **"My logic seems backward"** → Write it in plain English first
- **"Too many edge cases"** → Handle the normal case first, then add edge cases
- **"Code too nested"** → Invert conditions and return early

**Get help**:
- Re-read specific sections that confused you
- Work through problems with pencil and paper
- Check the debug challenges section
- Review the MCQs - they test core concepts
- Join the Discord community (mentioned in the PDF)


### Remember this:

```javascript
// ========== IF-ELSE ==========
if (condition) {
    // true path
} else if (anotherCondition) {
    // alternative path
} else {
    // default path
}

// ========== SWITCH ==========
switch (expression) {
    case value1:
        // code
        break;
    case value2:
        // code
        break;
    default:
        // fallback
}

// ========== TERNARY ==========
let result = condition ? valueIfTrue : valueIfFalse;

// ========== COMMON PATTERNS ==========

// Guard clause
if (!validInput) return;

// Range check
if (value >= min && value <= max) { }

// Multiple conditions (OR)
if (a === x || a === y || a === z) { }

// Multiple conditions (AND)
if (isValid && hasPermission && isActive) { }

// Null/undefined check
if (value !== null && value !== undefined) { }
// or
if (value != null) { } // catches both

// Type check
if (typeof value === 'string') { }

// ========== FALSY VALUES ==========
// false, 0, "", null, undefined, NaN

// ========== REMEMBER ==========
// Use === not ==
// Add break in switch
// Validate inputs first
// Use guard clauses for edge cases
// Keep conditions simple and readable
```

**Print this reference card and keep it handy!**

---

### 🌅 **Next — Day 5: Mastering loops and iteration🔥**

Tomorrow, you'll master the art of **repetition and automation** using:

* `for` loops for counted iterations
* `while` and `do-while` for condition-based repetition
* `break` and `continue` for precise loop control
* Nested loops for multi-dimensional problems
* Loop patterns for real-world scenarios

**Sneak peek**: By tomorrow's end, you'll build a multiplication table generator, pattern printers, and solve problems that would take 100+ lines with just 5 lines of loop magic! 🎯
---


## 💬 **Join the Community**

💡 Share your Daily task, learning, discuss and document your journey on our **Discord**.

Discord: [Click to Join](https://discord.gg/vRycXDkYm)

Linkedin: [Click to join](https://www.linkedin.com/in/tapasadhikary?utm_source=share&utm_campaign=share_via&utm_content=profile&utm_medium=android_app)

YouTube: [Click to join](https://youtube.com/playlist?list=PLIJrr73KDmRw2Fwwjt6cPC_tk5vcSICCu)

<div align="center">

### 📘 Tapascript by Tapas Adhikary

_Day 4 complete ✅ — Control Flow Conquered!_  
_Tomorrow we loop! Get ready to automate everything 🔁._

**"First, solve the problem. Then, write the code." — John Johnson**

</div>



