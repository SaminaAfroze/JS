# The Complete JavaScript Guide for Test Automation
### Zero to Framework-Ready — Concepts, Code, Practice, and Interview Prep

---

## How to Use This Document

Every concept below follows the same pattern: **explanation → example code →
what's happening line by line**. Type the examples yourself instead of just
reading them — muscle memory matters far more than reading for programming.
At the end of major sections you'll find **practice exercises**, and the
final section is a bank of **interview questions** (with answers) aimed
specifically at QA/SDET roles.

---

## Table of Contents

**Part A — JavaScript Fundamentals**
1. What Is JavaScript and How Do You Run It
2. Variables and Data Types
3. Operators
4. Control Flow (if/else, switch, loops)
5. Functions (all forms)
6. Scope, Hoisting, and Closures
7. Arrays and Array Methods
8. Objects and Object Methods
9. Strings and String Methods
10. Destructuring, Spread, and Rest
11. `this`, `call`, `apply`, `bind`
12. Classes and Prototypes (OOP in JS)
13. Error Handling
14. Regular Expressions

**Part B — Asynchronous JavaScript**
15. The Event Loop (Why JS Behaves the Way It Does)
16. Callbacks
17. Promises
18. Async/Await
19. Making HTTP Requests (fetch/axios) — the backbone of API testing

**Part C — Node.js & Tooling**
20. Node.js Basics
21. npm, package.json, and Modules (CommonJS vs ES Modules)
22. Working with Files (fs module)
23. Environment Variables & Config

**Part D — Building Test Frameworks**
24. Testing Concepts (Assertions, Test Runners, Fixtures)
25. Jest — Unit & API Testing
26. Playwright — End-to-End / UI Testing
27. Cypress — End-to-End / UI Testing
28. Designing a Real Test Framework (Page Object Model, Structure, CI)

**Part E — Practice & Interview Prep**
29. Practice Coding Exercises (with Solutions)
30. JavaScript Interview Questions (Core Concepts)
31. Testing/Framework Interview Questions
32. Final Cheat Sheet

---

# PART A — JAVASCRIPT FUNDAMENTALS

## 1. What Is JavaScript and How Do You Run It

JavaScript (JS) is a programming language originally built to make web
pages interactive. Today it also runs **outside** the browser via
**Node.js**, which is why it's used heavily for test automation, backend
APIs, and command-line tools — you write one language and use it everywhere.

### Ways to run JavaScript

**1. In a browser** — Open any browser, press F12 (DevTools), go to the
"Console" tab, type `console.log("Hello")`, hit Enter.

**2. Using Node.js (what you'll use for test automation)**
1. Install Node.js from nodejs.org (choose the LTS version).
2. Verify installation in a terminal:
   ```
   node -v
   npm -v
   ```
3. Create a file `hello.js`:
   ```javascript
   console.log("Hello, JavaScript!");
   ```
4. Run it:
   ```
   node hello.js
   ```
   Output: `Hello, JavaScript!`

That's the loop you'll use for the rest of this guide: write a `.js` file,
run it with `node filename.js`, read the output.

---

## 2. Variables and Data Types

A variable is a named box that stores a value.

```javascript
let age = 25;          // can be reassigned later
const name = "Sara";   // cannot be reassigned
var oldWay = "avoid";  // old syntax, avoid in modern code
```

**Why avoid `var`?** `var` doesn't respect block scope (explained in
Section 6) and causes subtle bugs. Use `const` by default, and `let` only
when you know the value will change.

### Primitive data types

```javascript
let a = 10;              // Number
let b = "hello";         // String
let c = true;            // Boolean
let d = null;             // Null - intentional "no value"
let e;                    // Undefined - declared but not assigned
let f = Symbol("id");     // Symbol - unique identifier (rarely needed early on)
let g = 123n;             // BigInt - for numbers bigger than Number can hold
```

### Checking a type
```javascript
console.log(typeof 10);        // "number"
console.log(typeof "hello");   // "string"
console.log(typeof true);      // "boolean"
console.log(typeof undefined); // "undefined"
console.log(typeof null);      // "object"  <- famous JS quirk, memorize this
```

### Reference types
```javascript
let arr = [1, 2, 3];               // Array
let obj = { name: "Sara", age: 25 }; // Object
function greet() { }               // Function (technically an object too)
```

**Practice:** Declare a `const` holding your name, a `let` holding your age,
and print both using `console.log`.

---

## 3. Operators

```javascript
// Arithmetic
console.log(10 + 5);   // 15
console.log(10 % 3);   // 1  (remainder/modulus — very useful for test data)
console.log(2 ** 3);   // 8  (exponent)

// Comparison — ALWAYS prefer === and !== over == and !=
console.log(5 == "5");    // true  (loose equality converts types — avoid)
console.log(5 === "5");   // false (strict equality — checks type too)
console.log(5 !== "5");   // true

// Logical
console.log(true && false); // false (AND)
console.log(true || false); // true  (OR)
console.log(!true);         // false (NOT)

// Ternary (shorthand if/else)
let age = 20;
let canVote = age >= 18 ? "Yes" : "No";
console.log(canVote); // "Yes"

// Nullish coalescing — use a default ONLY if value is null/undefined
let username = null;
console.log(username ?? "Guest"); // "Guest"

// Optional chaining — safely access nested properties that might not exist
let user = { profile: null };
console.log(user.profile?.email); // undefined (doesn't throw an error)
```

**Rule to remember forever:** always use `===` and `!==`, never `==` and
`!=`, unless you have a very specific reason. This alone prevents a large
class of bugs.

---

## 4. Control Flow

### if / else
```javascript
let score = 75;

if (score >= 90) {
  console.log("Grade A");
} else if (score >= 70) {
  console.log("Grade B");
} else {
  console.log("Grade C");
}
// Output: Grade B
```

### switch
```javascript
let browser = "chrome";

switch (browser) {
  case "chrome":
    console.log("Testing on Chrome");
    break;
  case "firefox":
    console.log("Testing on Firefox");
    break;
  default:
    console.log("Unknown browser");
}
// Output: Testing on Chrome
```

### Loops
```javascript
// for loop — when you know how many times to repeat
for (let i = 0; i < 5; i++) {
  console.log(`Iteration ${i}`);
}

// while loop — repeat while a condition is true
let count = 0;
while (count < 3) {
  console.log("count is", count);
  count++;
}

// for...of — loop over VALUES of an array (most common in test scripts)
let browsers = ["chrome", "firefox", "webkit"];
for (const b of browsers) {
  console.log(`Running tests on ${b}`);
}

// for...in — loop over KEYS of an object (less common, use carefully)
let user = { name: "Sara", role: "QA" };
for (const key in user) {
  console.log(key, "=", user[key]);
}
```

**Practice:** Write a loop that prints all even numbers from 1 to 20.

---

## 5. Functions (All Forms)

Functions are reusable blocks of code — the foundation of every test
script and framework you'll build.

```javascript
// 1. Function declaration
function add(a, b) {
  return a + b;
}
console.log(add(2, 3)); // 5

// 2. Function expression
const subtract = function (a, b) {
  return a - b;
};
console.log(subtract(5, 2)); // 3

// 3. Arrow function (modern, most common in test code)
const multiply = (a, b) => a * b;
console.log(multiply(3, 4)); // 12

// Arrow function with a body (needs return + braces)
const divide = (a, b) => {
  if (b === 0) return "Cannot divide by zero";
  return a / b;
};
console.log(divide(10, 2)); // 5

// 4. Default parameters
function greet(name = "Guest") {
  return `Hello, ${name}`;
}
console.log(greet());        // "Hello, Guest"
console.log(greet("Sara"));  // "Hello, Sara"

// 5. Rest parameters — collect any number of arguments into an array
function sumAll(...numbers) {
  return numbers.reduce((total, n) => total + n, 0);
}
console.log(sumAll(1, 2, 3, 4)); // 10
```

**Why arrow functions matter for testers:** almost every modern test
framework (Jest, Playwright, Cypress) is written using arrow functions
inside `test()`, `it()`, `describe()` blocks. You'll see this pattern
constantly:

```javascript
test("homepage loads correctly", async ({ page }) => {
  await page.goto("https://example.com");
});
```

That `async ({ page }) => { ... }` is an arrow function being passed as an
argument — understanding arrow functions is what makes that syntax click.

**Practice:** Write an arrow function `isEven(n)` that returns `true` if a
number is even, `false` otherwise.

---

## 6. Scope, Hoisting, and Closures

### Scope — where a variable is accessible
```javascript
let globalVar = "I am global";

function testScope() {
  let localVar = "I am local";
  console.log(globalVar); // accessible
  console.log(localVar);  // accessible
}
testScope();
console.log(globalVar);   // accessible
// console.log(localVar); // ERROR - localVar doesn't exist out here

// Block scope with let/const
if (true) {
  let blockVar = "only exists inside this block";
}
// console.log(blockVar); // ERROR
```

### Hoisting — JS "moves declarations to the top" (sort of)
```javascript
console.log(hoistedVar); // undefined (NOT an error — declaration is hoisted)
var hoistedVar = "value";

// console.log(notHoisted); // ERROR - let/const are NOT accessible before declaration
let notHoisted = "value";
```
This is another reason to avoid `var` — its hoisting behavior causes
confusing bugs. `let`/`const` throw a clear error instead of silently
giving `undefined`.

### Closures — a function "remembers" variables from where it was created
This is one of the most important (and most interview-tested) concepts.

```javascript
function counterFactory() {
  let count = 0; // this variable is "enclosed" by the returned function
  return function () {
    count++;
    return count;
  };
}

const counter = counterFactory();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```

**Why this matters:** `count` should technically disappear after
`counterFactory()` finishes running — but the returned function "closes
over" it and keeps it alive. This pattern is used constantly for things
like generating unique test data, retry counters, or private state in test
helpers.

**Practice:** Write a closure-based function `createIdGenerator()` that
returns a function which produces `"ID-1"`, `"ID-2"`, `"ID-3"` each time
it's called.

---

## 7. Arrays and Array Methods

Arrays are everywhere in test automation — lists of test data, lists of
elements found on a page, lists of API responses.

```javascript
let fruits = ["apple", "banana", "mango"];

// Basics
console.log(fruits.length);     // 3
console.log(fruits[0]);         // "apple"
fruits.push("orange");          // add to end -> ["apple","banana","mango","orange"]
fruits.pop();                   // remove from end
fruits.unshift("kiwi");         // add to start
fruits.shift();                 // remove from start

// includes / indexOf
console.log(fruits.includes("banana")); // true
console.log(fruits.indexOf("mango"));   // 1
```

### The methods you will use CONSTANTLY in test code
```javascript
let users = [
  { name: "Sara", age: 25, active: true },
  { name: "Tom", age: 30, active: false },
  { name: "Lily", age: 22, active: true },
];

// map() — transform every item, returns a NEW array
let names = users.map(u => u.name);
console.log(names); // ["Sara", "Tom", "Lily"]

// filter() — keep only items matching a condition
let activeUsers = users.filter(u => u.active);
console.log(activeUsers); // [{Sara...}, {Lily...}]

// find() — return the FIRST item matching a condition
let tom = users.find(u => u.name === "Tom");
console.log(tom); // { name: "Tom", age: 30, active: false }

// some() — does AT LEAST ONE item match?
console.log(users.some(u => u.age > 28)); // true

// every() — do ALL items match?
console.log(users.every(u => u.active)); // false

// reduce() — combine all items into a single value
let totalAge = users.reduce((sum, u) => sum + u.age, 0);
console.log(totalAge); // 77

// forEach() — run code for each item (no return value, just side effects)
users.forEach(u => console.log(`${u.name} is ${u.age}`));

// sort() — sorts IN PLACE (mutates original array)
let numbers = [5, 2, 8, 1];
numbers.sort((a, b) => a - b); // ascending
console.log(numbers); // [1, 2, 5, 8]
```

**Why this matters for testing:** validating an API response almost always
looks like this:

```javascript
// Example: assert every returned product has a price greater than 0
const response = await getProducts(); // pretend API call
const allPricesValid = response.data.every(product => product.price > 0);
expect(allPricesValid).toBe(true);
```

**Practice:** Given `let scores = [45, 89, 67, 90, 32, 78];`, use array
methods to: (a) get only scores above 60, (b) get the average score,
(c) check if any score is a perfect 100.

---

## 8. Objects and Object Methods

```javascript
const user = {
  name: "Sara",
  age: 25,
  isActive: true,
  greet() {
    return `Hi, I'm ${this.name}`;
  },
};

console.log(user.name);       // dot notation
console.log(user["age"]);     // bracket notation (needed for dynamic keys)
console.log(user.greet());    // "Hi, I'm Sara"

// Adding/updating/removing properties
user.email = "sara@test.com"; // add
user.age = 26;                // update
delete user.isActive;         // remove

// Useful Object methods
console.log(Object.keys(user));    // ["name","age","greet","email"]
console.log(Object.values(user));  // ["Sara",26,function,"sara@test.com"]
console.log(Object.entries(user)); // [["name","Sara"],["age",26],...]

// Merging objects (very common when building test payloads)
const baseUser = { role: "QA", active: true };
const overrides = { active: false };
const merged = { ...baseUser, ...overrides };
console.log(merged); // { role: "QA", active: false }
```

**Why this matters:** API test payloads are objects. You'll constantly
build a "base" test data object and override just one field per test:

```javascript
const basePayload = { username: "test_user", email: "test@example.com", age: 25 };
const invalidAgePayload = { ...basePayload, age: -1 }; // test negative age validation
```

---

## 9. Strings and String Methods

```javascript
let str = "Hello, Automation World!";

console.log(str.length);              // 24
console.log(str.toUpperCase());       // "HELLO, AUTOMATION WORLD!"
console.log(str.toLowerCase());       // "hello, automation world!"
console.log(str.includes("Automation")); // true
console.log(str.startsWith("Hello"));    // true
console.log(str.endsWith("!"));          // true
console.log(str.slice(0, 5));            // "Hello"
console.log(str.split(", "));            // ["Hello", "Automation World!"]
console.log(str.trim());                 // removes leading/trailing whitespace
console.log(str.replace("World", "Testing")); // "Hello, Automation Testing!"

// Template literals — the modern way to build strings (use this over concatenation)
let name = "Sara";
let env = "staging";
console.log(`Running tests as ${name} on ${env} environment`);
```

**Practice:** Given `let email = "  Sara.Khan@TestMail.com  ";`, clean it up
so it becomes `"sara.khan@testmail.com"` (trim whitespace, lowercase it).

---

## 10. Destructuring, Spread, and Rest

```javascript
// Array destructuring
const coordinates = [10, 20];
const [x, y] = coordinates;
console.log(x, y); // 10 20

// Object destructuring (used constantly in Playwright/Jest function signatures)
const user = { name: "Sara", role: "QA", country: "Bangladesh" };
const { name, role } = user;
console.log(name, role); // Sara QA

// Renaming while destructuring
const { name: userName } = user;
console.log(userName); // Sara

// Default values while destructuring
const { name: n, age = 18 } = user;
console.log(age); // 18 (fallback since 'age' isn't in the object)

// Spread — expand an array/object
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5];
console.log(arr2); // [1,2,3,4,5]

// Rest — collect remaining items
const [first, ...rest] = [1, 2, 3, 4];
console.log(first, rest); // 1 [2,3,4]
```

**This is exactly what you're seeing in Playwright syntax:**
```javascript
test("login test", async ({ page, browser }) => {
  // { page, browser } is OBJECT DESTRUCTURING of the argument Playwright passes in
});
```

---

## 11. `this`, `call`, `apply`, `bind`

`this` refers to "whatever object is currently calling the function" — and
it's one of the trickiest parts of JS.

```javascript
const user = {
  name: "Sara",
  greet: function () {
    console.log(`Hi, I'm ${this.name}`);
  },
};
user.greet(); // "Hi, I'm Sara" — this = user, because user called it

const greetFn = user.greet;
// greetFn(); // "Hi, I'm undefined" — this is lost when detached from the object

// Arrow functions do NOT have their own `this` — they inherit it from
// where they were WRITTEN, not where they're called. This is why arrow
// functions are usually safer for callbacks.
```

`call`, `apply`, and `bind` let you manually control what `this` means:
```javascript
function introduce() {
  console.log(`I am ${this.name}`);
}
const person = { name: "Tom" };

introduce.call(person);   // "I am Tom" - calls immediately
introduce.apply(person);  // "I am Tom" - same as call, but args passed as array
const boundFn = introduce.bind(person); // returns a NEW function, doesn't call yet
boundFn();                 // "I am Tom"
```
You won't use `call`/`apply`/`bind` often in day-to-day test scripting, but
it's a very common interview question, so know it exists and what it does.

---

## 12. Classes and Prototypes (OOP in JS)

Classes are essential for building test frameworks — especially the **Page
Object Model** pattern (Section 28).

```javascript
class Animal {
  constructor(name, sound) {
    this.name = name;
    this.sound = sound;
  }

  makeSound() {
    console.log(`${this.name} says ${this.sound}`);
  }
}

const dog = new Animal("Dog", "Woof");
dog.makeSound(); // "Dog says Woof"

// Inheritance
class Robot extends Animal {
  constructor(name) {
    super(name, "Beep"); // call the parent constructor
  }

  makeSound() {
    super.makeSound();       // call parent's method too
    console.log("...but I am a robot.");
  }
}

const r2d2 = new Robot("R2D2");
r2d2.makeSound();
// "R2D2 says Beep"
// "...but I am a robot."
```

**A realistic testing example — a Page Object:**
```javascript
class LoginPage {
  constructor(page) {
    this.page = page; // the Playwright/Puppeteer "page" object
    this.usernameInput = "#username";
    this.passwordInput = "#password";
    this.loginButton = "#login-btn";
  }

  async login(username, password) {
    await this.page.fill(this.usernameInput, username);
    await this.page.fill(this.passwordInput, password);
    await this.page.click(this.loginButton);
  }
}

// Usage in a test:
// const loginPage = new LoginPage(page);
// await loginPage.login("testuser", "password123");
```
This is exactly the pattern used in real automation frameworks — a class
represents a page, its properties are locators, and its methods are
actions a user can take on that page.

---

## 13. Error Handling

```javascript
function divide(a, b) {
  if (b === 0) {
    throw new Error("Cannot divide by zero");
  }
  return a / b;
}

try {
  console.log(divide(10, 0));
} catch (error) {
  console.log("Caught an error:", error.message);
} finally {
  console.log("This always runs, error or not.");
}
```

### Custom error classes (useful for framework-building)
```javascript
class ApiError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.name = "ApiError";
    this.statusCode = statusCode;
  }
}

function checkResponse(status) {
  if (status >= 400) {
    throw new ApiError(`Request failed`, status);
  }
}

try {
  checkResponse(404);
} catch (err) {
  console.log(err.name, err.message, err.statusCode);
  // "ApiError" "Request failed" 404
}
```

**Why this matters:** in real test frameworks, custom errors make failure
reports far more readable — instead of a generic error, your team sees
`ApiError: Request failed (status 404)`.

---

## 14. Regular Expressions

Used constantly for validating text (emails, phone numbers, log parsing).

```javascript
const emailPattern = /^[\w.-]+@[\w.-]+\.\w+$/;

console.log(emailPattern.test("sara@test.com"));   // true
console.log(emailPattern.test("not-an-email"));    // false

const text = "Order ID: 45231, Status: Shipped";
const match = text.match(/Order ID: (\d+)/);
console.log(match[1]); // "45231"

// Replace using regex
console.log("2026-09-11".replace(/-/g, "/")); // "2026/09/11"
```
You don't need to master every regex symbol immediately — learn `\d`
(digit), `\w` (word character), `+` (one or more), `*` (zero or more),
`^`/`$` (start/end of string), and look the rest up as you need it.

---

# PART B — ASYNCHRONOUS JAVASCRIPT

This is the single most important part of JS for a tester to understand,
because every UI test (waiting for a page/element) and every API test
(waiting for a network response) is asynchronous.

## 15. The Event Loop (Why JS Behaves the Way It Does)

JavaScript runs on a **single thread** — it can only do one thing at a
time. But it doesn't want slow operations (network calls, timers, file
reads) to freeze everything. So it hands those operations off, keeps
running other code, and comes back to handle the result later, via the
**event loop**.

```javascript
console.log("1. Start");

setTimeout(() => {
  console.log("2. Inside timeout (runs later)");
}, 0);

console.log("3. End");

// Output order:
// 1. Start
// 3. End
// 2. Inside timeout (runs later)
```
Even with a `0` millisecond delay, the `setTimeout` callback runs **after**
all synchronous code finishes. This is the core idea behind every async
pattern below — async code always waits its turn behind the currently
running code.

---

## 16. Callbacks

The original way to handle async code — pass a function to be called
"later."

```javascript
function fetchUserCallback(id, callback) {
  setTimeout(() => {
    callback({ id, name: "Sara" }); // simulate a network delay
  }, 1000);
}

fetchUserCallback(1, (user) => {
  console.log("Got user:", user);
});
console.log("This prints BEFORE the user data, because fetch is async");
```

**The problem — "callback hell"** when you need several async steps in a
row:
```javascript
fetchUserCallback(1, (user) => {
  fetchOrdersCallback(user.id, (orders) => {
    fetchOrderDetailsCallback(orders[0].id, (details) => {
      console.log(details); // deeply nested, hard to read/maintain
    });
  });
});
```
This is exactly the problem Promises were created to solve.

---

## 17. Promises

A Promise represents a value that will exist **eventually** — it's either
`pending`, `fulfilled` (success), or `rejected` (failure).

```javascript
function fetchUser(id) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (id > 0) {
        resolve({ id, name: "Sara" });
      } else {
        reject(new Error("Invalid ID"));
      }
    }, 1000);
  });
}

fetchUser(1)
  .then((user) => console.log("Got user:", user))
  .catch((error) => console.log("Error:", error.message))
  .finally(() => console.log("Done, success or fail"));
```

### Chaining promises (fixes callback hell)
```javascript
fetchUser(1)
  .then((user) => fetchOrders(user.id))     // returns another promise
  .then((orders) => fetchOrderDetails(orders[0].id))
  .then((details) => console.log(details))
  .catch((error) => console.log("Something failed:", error.message));
```

### Running promises in parallel
```javascript
Promise.all([fetchUser(1), fetchUser(2), fetchUser(3)])
  .then((users) => console.log("All users:", users))
  .catch((error) => console.log("At least one failed:", error.message));
// Promise.all fails fast — if ANY promise rejects, the whole thing rejects

Promise.allSettled([fetchUser(1), fetchUser(-1)])
  .then((results) => console.log(results));
// allSettled waits for ALL, and tells you which succeeded/failed individually
// — very useful for running independent test data setup steps
```

---

## 18. Async/Await

Syntactic sugar over Promises that makes async code **look synchronous** —
this is what you'll use 95% of the time in real test scripts.

```javascript
async function getUserData(id) {
  try {
    const user = await fetchUser(id);       // "pause" here until resolved
    console.log("Got user:", user);
    return user;
  } catch (error) {
    console.log("Error:", error.message);
  }
}

getUserData(1);
```

**Key rules:**
- `await` can only be used inside a function marked `async`.
- An `async` function ALWAYS returns a Promise, even if you write a plain
  `return` inside it.
- Wrap `await` calls in `try/catch` to handle failures gracefully.

**This is exactly the pattern in every modern test framework:**
```javascript
test("user can log in", async ({ page }) => {
  await page.goto("https://example.com/login");
  await page.fill("#username", "testuser");
  await page.fill("#password", "password123");
  await page.click("#login-btn");
  await expect(page.locator("#welcome-message")).toBeVisible();
});
```
Every `await` here is pausing the test until that specific action (page
load, typing, click, becoming visible) actually finishes — without
`async/await`, the test would try to click a login button before the page
had even loaded.

**Practice:** Write an `async` function `getSquareAfterDelay(n)` that
waits 1 second (using a Promise-wrapped `setTimeout`) and then returns
`n * n`. Call it with `await` and log the result.

---

## 19. Making HTTP Requests (fetch/axios)

This is the backbone of API testing — sending a request and asserting on
the response.

### Using built-in `fetch` (available in modern Node.js and all browsers)
```javascript
async function getPost(id) {
  const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`);
  const data = await response.json(); // parse JSON body
  console.log("Status:", response.status);
  console.log("Data:", data);
  return data;
}

getPost(1);
```

### POST request with a body
```javascript
async function createPost(payload) {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(payload),
  });
  const data = await response.json();
  return { status: response.status, data };
}

createPost({ title: "My Test Post", body: "Hello", userId: 1 })
  .then((result) => console.log(result));
```

### Using `axios` (a popular library — install with `npm install axios`)
```javascript
const axios = require("axios");

async function getPostAxios(id) {
  const response = await axios.get(`https://jsonplaceholder.typicode.com/posts/${id}`);
  console.log(response.status);  // axios auto-parses JSON, no .json() needed
  console.log(response.data);
  return response.data;
}
```

**A realistic API test using plain `fetch` + Node's built-in `assert`:**
```javascript
const assert = require("assert");

async function testGetPostReturns200() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
  assert.strictEqual(response.status, 200, "Expected status 200");
  const data = await response.json();
  assert.ok(data.id === 1, "Expected post id to be 1");
  console.log("Test passed!");
}

testGetPostReturns200();
```
This — request, await response, assert on it — is the entire skeleton of
API test automation. Everything Jest/Mocha add on top is just better
structure and reporting around this same idea.

---

# PART C — NODE.JS & TOOLING

## 20. Node.js Basics

Node.js lets JS run outside a browser — on your machine or a CI server.
You already installed it in Section 1. A few essentials:

```javascript
// Reading command-line arguments
console.log(process.argv); // array of arguments passed when running the script

// Reading environment variables
console.log(process.env.NODE_ENV);

// Exiting with a specific code (useful for CI — non-zero = failure)
process.exit(1);
```

---

## 21. npm, package.json, and Modules

### package.json — the "identity card" of a Node project
```
npm init -y
```
This generates a `package.json` file:
```json
{
  "name": "my-test-framework",
  "version": "1.0.0",
  "scripts": {
    "test": "jest"
  },
  "dependencies": {},
  "devDependencies": {}
}
```

### Installing packages
```
npm install axios            # adds to "dependencies"
npm install --save-dev jest  # adds to "devDependencies" (test-only tools)
```

### CommonJS modules (older, still common in Node)
```javascript
// math.js
function add(a, b) { return a + b; }
module.exports = { add };

// app.js
const { add } = require("./math");
console.log(add(2, 3));
```

### ES Modules (modern standard — needs `"type": "module"` in package.json)
```javascript
// math.js
export function add(a, b) { return a + b; }

// app.js
import { add } from "./math.js";
console.log(add(2, 3));
```
**Rule of thumb:** new projects and most modern frameworks (Playwright,
Vite-based tools) default to ES Modules; older Node/Jest setups often still
use CommonJS. Check your project's `package.json` to know which one you're
in.

---

## 22. Working with Files (fs module)

Common in test frameworks for reading test data or writing reports.

```javascript
const fs = require("fs");

// Read a file synchronously
const data = fs.readFileSync("testdata.json", "utf-8");
const testData = JSON.parse(data);
console.log(testData);

// Write a file
fs.writeFileSync("report.txt", "All tests passed!");

// Async version (preferred in real apps to avoid blocking)
fs.readFile("testdata.json", "utf-8", (err, data) => {
  if (err) throw err;
  console.log(JSON.parse(data));
});

// Modern Promise-based version
const fsPromises = require("fs/promises");
async function loadData() {
  const data = await fsPromises.readFile("testdata.json", "utf-8");
  return JSON.parse(data);
}
```

---

## 23. Environment Variables & Config

Test frameworks need to run against different environments (dev, staging,
prod) without code changes.

```javascript
// Install: npm install dotenv
// Create a .env file:
// BASE_URL=https://staging.example.com
// API_KEY=abc123

require("dotenv").config();
console.log(process.env.BASE_URL); // "https://staging.example.com"
```
Running with an inline override:
```
BASE_URL=https://prod.example.com node runTests.js
```

---

# PART D — BUILDING TEST FRAMEWORKS

## 24. Testing Concepts (Assertions, Test Runners, Fixtures)

- **Assertion** — a statement that must be true, or the test fails
  (`expect(x).toBe(y)`).
- **Test runner** — the tool that finds, executes, and reports on your
  tests (Jest, Mocha, Playwright Test).
- **Test suite** — a group of related tests, usually declared with
  `describe()`.
- **Fixture / Setup-Teardown** — code that runs before/after tests to
  prepare and clean up state (`beforeEach`, `afterEach`, `beforeAll`,
  `afterAll`).
- **Mocking** — replacing a real dependency (like a network call) with a
  fake, controlled version, so a unit test doesn't depend on a real server.

---

## 25. Jest — Unit & API Testing

Install:
```
npm install --save-dev jest
```
Add to `package.json`:
```json
"scripts": { "test": "jest" }
```

### Basic test structure
```javascript
// math.js
function add(a, b) { return a + b; }
module.exports = { add };

// math.test.js
const { add } = require("./math");

describe("add function", () => {
  test("adds two positive numbers", () => {
    expect(add(2, 3)).toBe(5);
  });

  test("adds a negative and a positive number", () => {
    expect(add(-2, 3)).toBe(1);
  });
});
```
Run with:
```
npm test
```

### Common matchers
```javascript
expect(5).toBe(5);                       // strict equality
expect({ a: 1 }).toEqual({ a: 1 });      // deep equality for objects/arrays
expect([1, 2, 3]).toContain(2);
expect("hello world").toMatch(/world/);
expect(true).toBeTruthy();
expect(null).toBeFalsy();
expect(() => { throw new Error("fail"); }).toThrow();
```

### Setup/teardown hooks
```javascript
describe("User API tests", () => {
  beforeAll(() => {
    console.log("Runs once before all tests in this block");
  });

  beforeEach(() => {
    console.log("Runs before EVERY test in this block");
  });

  afterEach(() => {
    console.log("Runs after EVERY test — good for cleanup");
  });

  test("example test", () => {
    expect(true).toBe(true);
  });
});
```

### A real async API test in Jest
```javascript
test("GET /posts/1 returns correct post", async () => {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
  expect(response.status).toBe(200);

  const data = await response.json();
  expect(data.id).toBe(1);
  expect(data).toHaveProperty("title");
});
```

### Mocking a function
```javascript
const fetchData = jest.fn(() => Promise.resolve({ status: 200 }));

test("mocked fetch returns 200", async () => {
  const result = await fetchData();
  expect(result.status).toBe(200);
  expect(fetchData).toHaveBeenCalledTimes(1);
});
```

---

## 26. Playwright — End-to-End / UI Testing

Install:
```
npm init playwright@latest
```

### A basic UI test
```javascript
const { test, expect } = require("@playwright/test");

test("homepage has correct title", async ({ page }) => {
  await page.goto("https://example.com");
  await expect(page).toHaveTitle(/Example Domain/);
});

test("search returns results", async ({ page }) => {
  await page.goto("https://example.com/search");
  await page.fill("#search-input", "JavaScript");
  await page.click("#search-button");
  const results = page.locator(".result-item");
  await expect(results.first()).toBeVisible();
  expect(await results.count()).toBeGreaterThan(0);
});
```

### Common Playwright actions
```javascript
await page.goto(url);
await page.click(selector);
await page.fill(selector, text);
await page.check(selector);         // check a checkbox
await page.selectOption(selector, value);
await page.waitForSelector(selector);
const text = await page.textContent(selector);
await page.screenshot({ path: "screenshot.png" });
```
Run tests:
```
npx playwright test
```

---

## 27. Cypress — End-to-End / UI Testing

Install:
```
npm install --save-dev cypress
npx cypress open
```

### A basic Cypress test
```javascript
describe("Login flow", () => {
  it("logs in successfully with valid credentials", () => {
    cy.visit("https://example.com/login");
    cy.get("#username").type("testuser");
    cy.get("#password").type("password123");
    cy.get("#login-btn").click();
    cy.get("#welcome-message").should("be.visible");
    cy.url().should("include", "/dashboard");
  });
});
```
Note Cypress commands are **chained and queued** rather than using
`async/await` directly — Cypress handles the waiting internally, which is
a key difference from Playwright's explicit `await` style.

---

## 28. Designing a Real Test Framework (Page Object Model, Structure, CI)

### Typical folder structure
```
test-framework/
├── pages/
│   ├── LoginPage.js
│   └── DashboardPage.js
├── tests/
│   ├── login.spec.js
│   └── dashboard.spec.js
├── utils/
│   └── apiClient.js
├── testdata/
│   └── users.json
├── package.json
└── playwright.config.js
```

### Page Object Model (POM) — full example
```javascript
// pages/LoginPage.js
class LoginPage {
  constructor(page) {
    this.page = page;
    this.username = page.locator("#username");
    this.password = page.locator("#password");
    this.loginBtn = page.locator("#login-btn");
    this.errorMsg = page.locator("#error-message");
  }

  async goto() {
    await this.page.goto("https://example.com/login");
  }

  async login(username, password) {
    await this.username.fill(username);
    await this.password.fill(password);
    await this.loginBtn.click();
  }
}
module.exports = { LoginPage };
```

```javascript
// tests/login.spec.js
const { test, expect } = require("@playwright/test");
const { LoginPage } = require("../pages/LoginPage");

test("invalid login shows error message", async ({ page }) => {
  const loginPage = new LoginPage(page);
  await loginPage.goto();
  await loginPage.login("wronguser", "wrongpass");
  await expect(loginPage.errorMsg).toContainText("Invalid credentials");
});
```

**Why POM matters:** if the login button's HTML ID changes tomorrow, you
fix it in **one place** (`LoginPage.js`) instead of in every test file
that clicks a login button.

### A reusable API client utility
```javascript
// utils/apiClient.js
const axios = require("axios");

class ApiClient {
  constructor(baseURL) {
    this.client = axios.create({ baseURL });
  }

  async get(endpoint) {
    return this.client.get(endpoint);
  }

  async post(endpoint, payload) {
    return this.client.post(endpoint, payload);
  }
}
module.exports = { ApiClient };
```

```javascript
// tests/api.spec.js
const { ApiClient } = require("../utils/apiClient");

test("create post returns 201", async () => {
  const api = new ApiClient("https://jsonplaceholder.typicode.com");
  const response = await api.post("/posts", { title: "Test", body: "Hi", userId: 1 });
  expect(response.status).toBe(201);
});
```

### Running tests in CI (conceptual GitHub Actions snippet)
```yaml
name: Run Tests
on: [push]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 18 }
      - run: npm install
      - run: npx playwright install --with-deps
      - run: npx playwright test
```

---

# PART E — PRACTICE & INTERVIEW PREP

## 29. Practice Coding Exercises (with Solutions)

Try each one yourself first, then check the solution.

**Exercise 1:** Write a function `isPalindrome(str)` that returns `true` if
a string reads the same forwards and backwards.
```javascript
function isPalindrome(str) {
  const cleaned = str.toLowerCase().replace(/[^a-z0-9]/g, "");
  return cleaned === cleaned.split("").reverse().join("");
}
console.log(isPalindrome("A man a plan a canal Panama")); // true
```

**Exercise 2:** Given an array of test results, count how many passed and
failed.
```javascript
const results = ["pass", "fail", "pass", "pass", "fail"];
const summary = results.reduce(
  (acc, r) => {
    acc[r]++;
    return acc;
  },
  { pass: 0, fail: 0 }
);
console.log(summary); // { pass: 3, fail: 2 }
```

**Exercise 3:** Write an async function `retry(fn, times)` that retries a
failing async function up to `times` attempts before giving up (common
pattern for flaky UI checks).
```javascript
async function retry(fn, times) {
  for (let attempt = 1; attempt <= times; attempt++) {
    try {
      return await fn();
    } catch (error) {
      console.log(`Attempt ${attempt} failed: ${error.message}`);
      if (attempt === times) throw error;
    }
  }
}

// usage:
// await retry(() => checkElementVisible(), 3);
```

**Exercise 4:** Remove duplicate values from an array.
```javascript
const nums = [1, 2, 2, 3, 4, 4, 5];
const unique = [...new Set(nums)];
console.log(unique); // [1,2,3,4,5]
```

**Exercise 5:** Given test data as an array of objects, generate a
CSV-style string.
```javascript
const data = [
  { name: "Sara", role: "QA" },
  { name: "Tom", role: "Dev" },
];
const csv = data.map(row => `${row.name},${row.role}`).join("\n");
console.log(csv);
// Sara,QA
// Tom,Dev
```

**Exercise 6:** Write a function `flatten(arr)` that flattens a nested
array one level deep, without using `Array.prototype.flat()`.
```javascript
function flatten(arr) {
  return arr.reduce((flat, item) => flat.concat(item), []);
}
console.log(flatten([1, [2, 3], [4, [5, 6]]])); // [1,2,3,4,[5,6]]
```

**Exercise 7 (async):** Write a function `waitFor(conditionFn, timeoutMs)`
that repeatedly checks a condition every 100ms until it's true or the
timeout is hit (the core idea behind every UI framework's built-in
"wait" logic).
```javascript
async function waitFor(conditionFn, timeoutMs = 5000) {
  const start = Date.now();
  while (Date.now() - start < timeoutMs) {
    if (await conditionFn()) return true;
    await new Promise(resolve => setTimeout(resolve, 100));
  }
  throw new Error("Timed out waiting for condition");
}
```

---

## 30. JavaScript Interview Questions (Core Concepts)

**Q1: What's the difference between `var`, `let`, and `const`?**
`var` is function-scoped and hoisted with an initial value of `undefined`,
which can cause bugs. `let` and `const` are block-scoped and are not
accessible before their declaration line (the "temporal dead zone").
`const` cannot be reassigned after declaration (though objects/arrays it
points to can still be mutated internally).

**Q2: What's the difference between `==` and `===`?**
`==` performs type coercion before comparing (`5 == "5"` is `true`). `===`
compares both value and type without coercion (`5 === "5"` is `false`).
Always prefer `===`.

**Q3: What is a closure?**
A closure is a function that "remembers" the variables from the scope it
was created in, even after that outer scope has finished executing. Used
for private state, counters, and generating configured helper functions.

**Q4: Explain the event loop in simple terms.**
JavaScript is single-threaded. Synchronous code runs first, completely.
Async operations (timers, network calls, promises) are handed off, and
their callback/`.then()` code is placed in a queue. The event loop checks
if the call stack is empty, and if so, pulls the next item from that
queue to run. This is why `setTimeout(fn, 0)` still runs after all
synchronous code.

**Q5: What's the difference between `null` and `undefined`?**
`undefined` means a variable has been declared but not assigned a value
(or a function didn't return anything). `null` is an intentional
assignment meaning "no value here." `typeof undefined` is `"undefined"`;
`typeof null` is (famously, a long-standing bug in JS) `"object"`.

**Q6: What is the difference between `Promise.all` and `Promise.allSettled`?**
`Promise.all` rejects immediately if any promise in the array rejects.
`Promise.allSettled` waits for every promise to finish (success or
failure) and returns an array describing each outcome individually —
useful when you want results from all operations even if some fail.

**Q7: What does `this` refer to inside an arrow function?**
Arrow functions don't have their own `this`. They inherit `this` from the
enclosing (lexical) scope at the time they were defined — this is why
arrow functions are often safer to use as callbacks than regular
functions.

**Q8: What is the difference between `map()` and `forEach()`?**
`map()` returns a new array built from the return value of the callback
on each item. `forEach()` returns `undefined` and is used purely for side
effects (like logging), not for building a new array.

**Q9: What is hoisting?**
JavaScript "moves" variable and function declarations to the top of their
scope before executing code. `var` declarations are hoisted and
initialized as `undefined`. Function declarations are hoisted completely
(you can call them before their line in the file). `let`/`const` are
hoisted but not initialized, so accessing them before their declaration
throws a `ReferenceError`.

**Q10: What is a Promise and what are its three states?**
A Promise represents the eventual result of an asynchronous operation. Its
three states are `pending` (not resolved yet), `fulfilled` (completed
successfully, has a value), and `rejected` (failed, has a reason/error).

**Q11: What's the difference between synchronous and asynchronous code?**
Synchronous code runs top-to-bottom, each line blocking the next until it
finishes. Asynchronous code lets long-running operations (network calls,
timers) run in the background, without blocking the rest of the program,
and their results are handled later via callbacks/promises/async-await.

**Q12: What is destructuring and why is it useful?**
Destructuring lets you unpack values from arrays or properties from
objects into individual variables in one line, making code shorter and
more readable — especially common when accessing specific fields from an
API response or a function's arguments object.

---

## 31. Testing/Framework Interview Questions

**Q1: How would you handle flaky tests in an automation suite?**
Identify the root cause first (timing issues, unstable test data, shared
state between tests, environment issues) rather than just adding retries.
Use explicit waits (`waitForSelector`, `expect().toBeVisible()`) instead
of hard-coded `sleep()` calls, isolate test data per test, and only add a
controlled retry mechanism as a safety net — not as the primary fix.

**Q2: What's the difference between unit, integration, and end-to-end
(E2E) tests?**
Unit tests check a single function/module in isolation (fast, no external
dependencies, often using mocks). Integration tests check how multiple
components work together (e.g., service + database). E2E tests simulate
a real user's full journey through the actual application (slowest, but
catches issues the other levels miss).

**Q3: Why use the Page Object Model?**
It separates "how to interact with a page" (locators + actions, in one
class) from "what the test verifies" (assertions, in the test file). This
means a UI change only requires updating one Page Object, not every test
that touches that page — reducing maintenance cost significantly.

**Q4: How do you handle test data in automation?**
Options include: hardcoded data (fine for small/simple tests), external
files like JSON/CSV (`testdata/users.json`), a dedicated test-data
factory/builder function that generates fresh data per test (best for
avoiding collisions), or calling an API/database directly to set up and
tear down data around each test.

**Q5: What's the difference between a mock and a stub?**
A **stub** provides a pre-set canned response for a dependency so the
test can run without needing the real thing (e.g., a fake API response).
A **mock** goes further — it also records/verifies *how* it was
called (how many times, with what arguments), letting you assert on the
interaction itself, not just the returned data.

**Q6: How would you test an API endpoint thoroughly?**
Check the happy path (valid input returns correct status/data), invalid
input (missing fields, wrong types, boundary values), authentication/
authorization behavior, response schema/structure, status codes for edge
cases (404, 400, 500), and performance characteristics if relevant
(response time under load).

**Q7: What's the difference between `await page.click()` in Playwright and
`cy.click()` in Cypress in terms of how they handle waiting?**
Playwright's `await` explicitly pauses test execution until the action's
promise resolves — you control the flow directly with `async/await`.
Cypress queues commands and automatically retries/waits for elements
behind the scenes without you writing `await` at all — the waiting logic
is baked into the command queue rather than being explicit in your code.

**Q8: How do you decide what NOT to automate?**
Tests that are one-off/rarely run, highly exploratory/subjective (visual
polish, UX feel), extremely unstable UI still in active design flux, or
where the cost of automating and maintaining the test exceeds the value it
returns, are generally better left as manual checks.

---

## 32. Final Cheat Sheet

```javascript
// Declaring variables
const x = 5;       let y = 10;

// Arrow function
const add = (a, b) => a + b;

// Array methods
arr.map(x => x * 2);
arr.filter(x => x > 5);
arr.find(x => x === 3);
arr.reduce((acc, x) => acc + x, 0);

// Destructuring
const { name, age } = obj;
const [first, ...rest] = arr;

// Template literal
`Hello, ${name}`;

// Async/await
async function run() {
  try {
    const result = await someAsyncCall();
  } catch (e) {
    console.log(e.message);
  }
}

// Class
class MyPage {
  constructor(page) { this.page = page; }
  async doThing() { await this.page.click("#btn"); }
}

// Fetch API
const res = await fetch(url);
const data = await res.json();

// Jest test
test("description", () => {
  expect(actual).toBe(expected);
});

// Playwright test
test("description", async ({ page }) => {
  await page.goto(url);
  await expect(page.locator("selector")).toBeVisible();
});
```

---

*You now have every core JavaScript concept needed to read, write, and
design real test automation frameworks — from raw language fundamentals,
through async programming, through Jest/Playwright/Cypress, to framework
architecture (Page Object Model) and the interview questions that get
asked about all of it. The fastest way to lock this in: pick one small
real website, write 5 UI tests with Playwright and 5 API tests with Jest
against a public test API (like jsonplaceholder.typicode.com), using
everything in this guide.*
