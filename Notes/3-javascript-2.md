# JavaScript Complete Notes – Interview Ready 📘

---

# 17. Asynchronous Programming in JavaScript

---

## Introduction to Asynchrony in JavaScript

JavaScript is **Single Threaded** – it can do **only one thing at a time**.

### Synchronous vs Asynchronous

```js
// Synchronous - runs line by line, waits for each line
console.log("Step 1");
console.log("Step 2");
console.log("Step 3");
// Output: Step 1 → Step 2 → Step 3 (in order)

// Asynchronous - doesn't wait, moves to next line
console.log("Step 1");
setTimeout(() => {
  console.log("Step 2 - delayed");
}, 2000);
console.log("Step 3");
// Output: Step 1 → Step 3 → Step 2 - delayed
```

### Simple Interview Answer:
> JavaScript is single-threaded, meaning it executes one task at a time. Asynchronous programming allows JS to **start a task, move on to next tasks, and come back** when the first task is done. This prevents the page from freezing while waiting for slow operations like API calls or file reading.

### Why Asynchronous?
```
Slow Operations that need Async:
├── API calls (fetching data from server)
├── File reading/writing
├── Database queries
├── setTimeout / setInterval
└── User events
```

### How JS Handles Async – Event Loop
```
Call Stack        →  Runs synchronous code
Web APIs          →  Handles async tasks (setTimeout, fetch)
Callback Queue    →  Waits for stack to be empty
Event Loop        →  Moves tasks from queue to stack when stack is empty
```

---

## Introduction to Callbacks

A **Callback** is a **function passed as argument to another function**, which is called later when a task is done.

```js
// Simple Callback Example
function greet(name, callbackFn) {
  console.log("Hello, " + name);
  callbackFn();   // Call the passed function
}

function sayBye() {
  console.log("Goodbye!");
}

greet("Rahul", sayBye);
// Output:
// Hello, Rahul
// Goodbye!
```

### Real World Callback Example
```js
// Simulating data fetching with callback
function fetchUserData(userId, callback) {
  console.log("Fetching user data...");

  // Simulating delay (like real API call)
  setTimeout(function() {
    let user = { id: userId, name: "Rahul", age: 25 };
    callback(user);   // Call callback with data when ready
  }, 2000);
}

fetchUserData(1, function(user) {
  console.log("Got user:", user.name);   // Got user: Rahul
});

console.log("This runs while data is being fetched");
```

---

## Problems with Callbacks – Callback Hell

When multiple async operations depend on each other, callbacks get **deeply nested** – this is called **Callback Hell** or **Pyramid of Doom**.

```js
// Callback Hell Example - Real World Scenario
// Step 1: Get user → Step 2: Get orders → Step 3: Get payment → Step 4: Send receipt

getUser(1, function(user) {
  console.log("Got user:", user.name);

  getOrders(user.id, function(orders) {
    console.log("Got orders:", orders);

    getPayment(orders[0].id, function(payment) {
      console.log("Got payment:", payment);

      sendReceipt(payment.id, function(receipt) {
        console.log("Sent receipt:", receipt);

        // Keeps going deeper and deeper... ❌
      });
    });
  });
});
```

### Problems with Callback Hell:
```
❌ Hard to read and understand
❌ Hard to debug
❌ Hard to maintain
❌ Error handling becomes messy
❌ Code looks like a pyramid
```

---

## Understanding Promises

A **Promise** is an object that represents the **eventual completion or failure** of an async operation.

> Think of Promise like ordering food at a restaurant. You get a **token (promise)**. The food is either **ready (resolved)** or **not available (rejected)**. You don't wait standing – you do other things meanwhile.

### Promise States:
```
pending   → Initial state, operation is in progress
resolved  → Operation completed successfully (then)
rejected  → Operation failed (catch)
```

### Creating a Promise
```js
// Creating a Promise
let myPromise = new Promise(function(resolve, reject) {
  let success = true;

  if (success) {
    resolve("Data fetched successfully!");   // ✅ Success
  } else {
    reject("Something went wrong!");         // ❌ Failure
  }
});

// Using the Promise
myPromise
  .then(function(result) {
    console.log("Success:", result);    // Runs if resolved
  })
  .catch(function(error) {
    console.log("Error:", error);       // Runs if rejected
  })
  .finally(function() {
    console.log("Always runs!");        // Runs always (cleanup)
  });
```

### Promise with Async Operation
```js
function fetchUser(userId) {
  return new Promise(function(resolve, reject) {
    setTimeout(function() {
      if (userId > 0) {
        let user = { id: userId, name: "Rahul" };
        resolve(user);                    // ✅ Success after 2 seconds
      } else {
        reject("Invalid user ID!");       // ❌ Failure
      }
    }, 2000);
  });
}

// Using Promise
fetchUser(1)
  .then(function(user) {
    console.log("User:", user.name);     // User: Rahul
    return user.id;                      // Pass to next .then()
  })
  .then(function(id) {
    console.log("User ID:", id);         // User ID: 1
  })
  .catch(function(error) {
    console.log("Error:", error);
  });
```

### Promise Chaining (Solving Callback Hell)
```js
// Same scenario as callback hell - but clean with promises
getUser(1)
  .then(function(user) {
    console.log("Got user:", user.name);
    return getOrders(user.id);            // Return next promise
  })
  .then(function(orders) {
    console.log("Got orders:", orders);
    return getPayment(orders[0].id);
  })
  .then(function(payment) {
    console.log("Got payment:", payment);
    return sendReceipt(payment.id);
  })
  .then(function(receipt) {
    console.log("Receipt sent!", receipt);
  })
  .catch(function(error) {
    console.log("Error at any step:", error);  // One catch for all!
  });
```

---

## async & await – Preventing Callback Hell

`async/await` is **syntactic sugar** over Promises. Makes async code look like **synchronous code**.

```js
// async keyword makes function return a Promise
// await keyword pauses execution until Promise resolves

async function getUserData() {
  try {
    let user = await fetchUser(1);          // Wait for user
    console.log("User:", user.name);

    let orders = await getOrders(user.id);  // Wait for orders
    console.log("Orders:", orders);

    let payment = await getPayment(orders[0].id);
    console.log("Payment:", payment);

    let receipt = await sendReceipt(payment.id);
    console.log("Receipt:", receipt);

  } catch (error) {
    console.log("Error:", error);           // Catch any error
  }
}

getUserData();
```

### Real Fetch API Example with async/await
```js
async function getUsers() {
  try {
    console.log("Fetching...");

    let response = await fetch("https://jsonplaceholder.typicode.com/users");

    if (!response.ok) {
      throw new Error("Failed to fetch users");
    }

    let users = await response.json();     // Wait for JSON parsing
    console.log("Users:", users);

    return users;

  } catch (error) {
    console.log("Error:", error.message);
  }
}

getUsers();
```

### Comparison – Callback vs Promise vs Async/Await
```js
// Same task - 3 ways

// ❌ Callback (messy)
getData(function(data) {
  processData(data, function(result) {
    saveData(result, function(saved) {
      console.log(saved);
    });
  });
});

// ✅ Promise (better)
getData()
  .then(data => processData(data))
  .then(result => saveData(result))
  .then(saved => console.log(saved))
  .catch(err => console.log(err));

// ✅✅ Async/Await (best - clean & readable)
async function run() {
  try {
    let data = await getData();
    let result = await processData(data);
    let saved = await saveData(result);
    console.log(saved);
  } catch (err) {
    console.log(err);
  }
}
```

---

## setTimeout & setInterval

### setTimeout – Run code ONCE after a delay
```js
// Syntax: setTimeout(function, delayInMilliseconds)

console.log("Start");

setTimeout(function() {
  console.log("Runs after 3 seconds");
}, 3000);

console.log("End");

// Output:
// Start
// End
// Runs after 3 seconds (after 3s delay)
```

```js
// Store timeout to cancel it later
let timeoutId = setTimeout(function() {
  console.log("This will be cancelled");
}, 5000);

// Cancel the timeout before it runs
clearTimeout(timeoutId);
console.log("Timeout cancelled!");
```

### setInterval – Run code REPEATEDLY at intervals
```js
// Syntax: setInterval(function, intervalInMilliseconds)

let count = 0;

let intervalId = setInterval(function() {
  count++;
  console.log("Count:", count);

  if (count === 5) {
    clearInterval(intervalId);   // Stop after 5 times
    console.log("Interval stopped!");
  }
}, 1000);  // Runs every 1 second
```

### Real World Example – Countdown Timer
```js
let seconds = 10;

let countdown = setInterval(function() {
  console.log("Time left:", seconds, "seconds");
  seconds--;

  if (seconds < 0) {
    clearInterval(countdown);
    console.log("Time's up!");
  }
}, 1000);
```

### setTimeout vs setInterval

| Feature | setTimeout | setInterval |
|---|---|---|
| Runs | Once after delay | Repeatedly at interval |
| Cancel with | clearTimeout() | clearInterval() |
| Use case | Delay a task | Repeat a task |

---
---

# 18. Operators & Type System

---

## Arithmetic Operators

```js
let a = 10;
let b = 3;

console.log(a + b);   // 13  - Addition
console.log(a - b);   // 7   - Subtraction
console.log(a * b);   // 30  - Multiplication
console.log(a / b);   // 3.33 - Division
console.log(a % b);   // 1   - Modulus (remainder)
console.log(a ** b);  // 1000 - Exponentiation (10^3)

// Increment
let x = 5;
console.log(x++);   // 5  (post-increment: returns THEN increments)
console.log(x);     // 6

let y = 5;
console.log(++y);   // 6  (pre-increment: increments THEN returns)
console.log(y);     // 6

// Decrement
let z = 5;
console.log(z--);   // 5  (post-decrement)
console.log(z);     // 4
```

---

## Comparison Operators – == vs ===

```js
// == (Loose Equality) - Compares VALUES only, ignores type
console.log(5 == "5");     // true  (converts string to number)
console.log(0 == false);   // true  (converts to same type)
console.log(null == undefined); // true

// === (Strict Equality) - Compares VALUE and TYPE both
console.log(5 === "5");    // false (different types)
console.log(0 === false);  // false (different types)
console.log(5 === 5);      // true  (same value AND type)

// != and !==
console.log(5 != "5");     // false (loose - same value after conversion)
console.log(5 !== "5");    // true  (strict - different types)

// Other comparisons
console.log(10 > 5);       // true
console.log(10 < 5);       // false
console.log(10 >= 10);     // true
console.log(10 <= 9);      // false
```

> **Interview Tip:** Always use `===` (strict equality) in real code. `==` can cause unexpected bugs due to type coercion.

---

## Logical Operators

### AND (&&) – Both must be true
```js
let age = 25;
let hasID = true;

console.log(age >= 18 && hasID);    // true  (both true)
console.log(age >= 18 && !hasID);   // false (one is false)

// Short Circuit - if first is false, second is NOT evaluated
let result = false && someFunction(); // someFunction never called
```

### OR (||) – At least one must be true
```js
let isAdmin = false;
let isModerator = true;

console.log(isAdmin || isModerator);  // true  (one is true)
console.log(false || false);          // false (both false)

// Short Circuit - if first is true, second is NOT evaluated
let name = "" || "Default Name";
console.log(name);  // "Default Name" (common pattern!)

let user = null;
let userName = user || "Guest";
console.log(userName);  // "Guest"
```

### NOT (!) – Reverses boolean value
```js
console.log(!true);    // false
console.log(!false);   // true
console.log(!0);       // true  (0 is falsy)
console.log(!"");      // true  (empty string is falsy)
console.log(!"hello"); // false (non-empty string is truthy)

// Double NOT - convert to boolean
console.log(!!0);       // false
console.log(!!1);       // true
console.log(!!"hello"); // true
```

---

## Assignment Operators

```js
let x = 10;

x += 5;    // x = x + 5  → 15
x -= 3;    // x = x - 3  → 12
x *= 2;    // x = x * 2  → 24
x /= 4;    // x = x / 4  → 6
x %= 4;    // x = x % 4  → 2
x **= 3;   // x = x ** 3 → 8

// Nullish Assignment Operators (ES2021)
let name = null;
name ??= "Default";    // Assign only if null or undefined
console.log(name);     // "Default"

let count = 0;
count ||= 10;          // Assign if falsy
console.log(count);    // 10

let value = 5;
value &&= 20;          // Assign if truthy
console.log(value);    // 20
```

---

## typeof Operator

```js
// typeof returns a string describing the data type

console.log(typeof 42);           // "number"
console.log(typeof 3.14);         // "number"
console.log(typeof NaN);          // "number"  ← Special case!
console.log(typeof "Hello");      // "string"
console.log(typeof true);         // "boolean"
console.log(typeof undefined);    // "undefined"
console.log(typeof null);         // "object"  ← JS Bug! (historical)
console.log(typeof {});           // "object"
console.log(typeof []);           // "object"  ← Arrays are objects!
console.log(typeof function(){}); // "function"

// Practical use - check type before using
function add(a, b) {
  if (typeof a !== "number" || typeof b !== "number") {
    return "Both arguments must be numbers!";
  }
  return a + b;
}

console.log(add(5, 10));       // 15
console.log(add(5, "hello")); // Both arguments must be numbers!
```

---

## Truthy & Falsy Values

### Falsy Values (Only 6 in JavaScript)
```js
// These 6 values are FALSY (treated as false in conditions)
if (false)     console.log("false");          // ❌
if (0)         console.log("zero");            // ❌
if (-0)        console.log("negative zero");   // ❌
if ("")        console.log("empty string");    // ❌
if (null)      console.log("null");            // ❌
if (undefined) console.log("undefined");       // ❌
if (NaN)       console.log("NaN");            // ❌

// Everything else is TRUTHY
if (1)         console.log("1 is truthy");     // ✅
if ("hello")   console.log("string truthy");   // ✅
if ([])        console.log("array truthy");    // ✅ (even empty array!)
if ({})        console.log("object truthy");   // ✅ (even empty object!)
if (-1)        console.log("-1 is truthy");    // ✅
```

### Real World Conditional Traps
```js
// ❌ Trap 1 - Empty array is truthy!
let items = [];
if (items) {
  console.log("Has items");   // This RUNS even though array is empty!
}
// ✅ Fix
if (items.length > 0) {
  console.log("Has items");
}

// ❌ Trap 2 - 0 is falsy!
let score = 0;
if (score) {
  console.log("Has score");  // This does NOT run even though score is 0
}
// ✅ Fix
if (score !== undefined && score !== null) {
  console.log("Has score:", score);
}

// ❌ Trap 3 - Using || for default values with 0
let count = 0;
let display = count || "No count";
console.log(display);  // "No count" - wrong! count is 0 not missing

// ✅ Fix - Use ?? (Nullish Coalescing) - only checks null/undefined
let display2 = count ?? "No count";
console.log(display2);  // 0 - correct!
```

---

## Type Coercion – Implicit vs Explicit

### Implicit Coercion (JavaScript does it automatically)
```js
// String + Number = String (concatenation)
console.log("5" + 3);      // "53"  (number becomes string)
console.log("5" + true);   // "5true"

// Other operators convert to number
console.log("5" - 3);      // 2     (string becomes number)
console.log("5" * 2);      // 10
console.log("10" / 2);     // 5
console.log(true + 1);     // 2     (true = 1)
console.log(false + 1);    // 1     (false = 0)
console.log(null + 1);     // 1     (null = 0)
console.log(undefined + 1);// NaN  (undefined = NaN)

// Comparison coercion
console.log("5" == 5);     // true  (implicit coercion)
console.log("5" === 5);    // false (no coercion with ===)
```

### Explicit Coercion (We do it manually)
```js
// Convert to Number
let str = "42";
console.log(Number(str));       // 42
console.log(parseInt("42px"));  // 42 (stops at non-number)
console.log(parseFloat("3.14abc")); // 3.14
console.log(+"42");             // 42 (Unary + operator)
console.log(Number(true));      // 1
console.log(Number(false));     // 0
console.log(Number(null));      // 0
console.log(Number("abc"));     // NaN

// Convert to String
let num = 42;
console.log(String(num));       // "42"
console.log(num.toString());    // "42"
console.log(`${num}`);          // "42" (template literal)

// Convert to Boolean
console.log(Boolean(1));        // true
console.log(Boolean(0));        // false
console.log(Boolean("hello"));  // true
console.log(Boolean(""));       // false
console.log(Boolean(null));     // false
console.log(!!42);              // true (double NOT)
```

---
---

# 19. Conditionals & Loops

---

## if, else-if, else

```js
// Basic if-else
let age = 20;

if (age >= 18) {
  console.log("You are an adult");
} else {
  console.log("You are a minor");
}

// if - else if - else chain
let score = 75;
let grade;

if (score >= 90) {
  grade = "A";
} else if (score >= 80) {
  grade = "B";
} else if (score >= 70) {
  grade = "C";
} else if (score >= 60) {
  grade = "D";
} else {
  grade = "F";
}

console.log("Grade:", grade);  // Grade: C

// Ternary Operator (shorthand if-else)
let isAdult = age >= 18 ? "Adult" : "Minor";
console.log(isAdult);  // Adult

// Nested ternary (use carefully)
let category = age < 13 ? "Child" : age < 18 ? "Teen" : "Adult";
console.log(category);  // Adult
```

---

## Switch Statement

```js
// switch is good for checking one variable against many values
let day = "Monday";

switch (day) {
  case "Monday":
    console.log("Start of work week");
    break;   // ← IMPORTANT: stops execution from falling to next case

  case "Tuesday":
  case "Wednesday":
  case "Thursday":
    console.log("Mid week");
    break;

  case "Friday":
    console.log("End of work week!");
    break;

  case "Saturday":
  case "Sunday":
    console.log("Weekend!");
    break;

  default:
    console.log("Invalid day");  // Runs if no case matches
}
```

### if-else vs switch
```js
// ✅ Use if-else for: ranges, complex conditions
if (score >= 90) { ... }
if (age > 18 && hasID) { ... }

// ✅ Use switch for: exact value matching
switch (color) {
  case "red": ...
  case "blue": ...
}
```

---

## for Loop

```js
// Basic for loop
for (let i = 0; i < 5; i++) {
  console.log("Count:", i);
}
// Output: 0, 1, 2, 3, 4

// Loop through array
let fruits = ["Apple", "Banana", "Mango"];
for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]);
}

// Reverse loop
for (let i = 5; i >= 1; i--) {
  console.log(i);
}
// Output: 5, 4, 3, 2, 1

// for...of (loop through values of array/string)
for (let fruit of fruits) {
  console.log(fruit);   // Apple, Banana, Mango
}

// for...in (loop through keys of object)
let person = { name: "Rahul", age: 25, city: "Delhi" };
for (let key in person) {
  console.log(key, ":", person[key]);
}
// name : Rahul
// age : 25
// city : Delhi
```

---

## while & do-while

### while Loop
```js
// Runs WHILE condition is true
// Condition checked BEFORE each run
let count = 1;

while (count <= 5) {
  console.log("Count:", count);
  count++;
}

// User Input Example
let userInput = "";
while (userInput !== "quit") {
  userInput = prompt("Type something (or 'quit' to exit):");
  console.log("You typed:", userInput);
}
```

### do-while Loop
```js
// Runs AT LEAST ONCE, then checks condition
// Condition checked AFTER each run
let num = 10;

do {
  console.log("Num:", num);
  num++;
} while (num < 5);

// Output: Num: 10 (runs once even though 10 < 5 is false)
```

### while vs do-while
```js
// while - may never run if condition is false from start
let x = 10;
while (x < 5) {
  console.log("Never runs");    // ❌ Skipped
}

// do-while - always runs at least once
do {
  console.log("Runs once!");    // ✅ Runs once
} while (x < 5);
```

---

## break & continue

### break – Exit loop immediately
```js
// Stop loop when condition is met
for (let i = 0; i < 10; i++) {
  if (i === 5) {
    break;   // Exit loop when i is 5
  }
  console.log(i);
}
// Output: 0, 1, 2, 3, 4
```

### continue – Skip current iteration, go to next
```js
// Skip even numbers
for (let i = 0; i < 10; i++) {
  if (i % 2 === 0) {
    continue;   // Skip even numbers
  }
  console.log(i);
}
// Output: 1, 3, 5, 7, 9
```

---

## Practice Programs

### 1. Prime Number Check
```js
function isPrime(num) {
  // Numbers less than 2 are not prime
  if (num < 2) return false;

  // Check if divisible by any number from 2 to sqrt(num)
  for (let i = 2; i <= Math.sqrt(num); i++) {
    if (num % i === 0) {
      return false;  // Found a divisor, not prime
    }
  }

  return true;  // No divisor found, is prime
}

console.log(isPrime(2));   // true
console.log(isPrime(7));   // true
console.log(isPrime(10));  // false
console.log(isPrime(1));   // false

// Print all primes from 1 to 50
for (let i = 1; i <= 50; i++) {
  if (isPrime(i)) {
    process.stdout.write(i + " ");
  }
}
// Output: 2 3 5 7 11 13 17 19 23 29 31 37 41 43 47
```

### 2. Multiplication Table
```js
function multiplicationTable(num) {
  console.log(`--- Multiplication Table of ${num} ---`);

  for (let i = 1; i <= 10; i++) {
    console.log(`${num} x ${i} = ${num * i}`);
  }
}

multiplicationTable(5);
// 5 x 1 = 5
// 5 x 2 = 10
// ...
// 5 x 10 = 50
```

### 3. Pattern Printing
```js
// Pattern 1 - Right Triangle
function rightTriangle(rows) {
  for (let i = 1; i <= rows; i++) {
    let pattern = "";
    for (let j = 1; j <= i; j++) {
      pattern += "* ";
    }
    console.log(pattern);
  }
}

rightTriangle(5);
// *
// * *
// * * *
// * * * *
// * * * * *


// Pattern 2 - Number Triangle
function numberTriangle(rows) {
  for (let i = 1; i <= rows; i++) {
    let row = "";
    for (let j = 1; j <= i; j++) {
      row += j + " ";
    }
    console.log(row);
  }
}

numberTriangle(5);
// 1
// 1 2
// 1 2 3
// 1 2 3 4
// 1 2 3 4 5


// Pattern 3 - Inverted Triangle
function invertedTriangle(rows) {
  for (let i = rows; i >= 1; i--) {
    let pattern = "";
    for (let j = 1; j <= i; j++) {
      pattern += "* ";
    }
    console.log(pattern);
  }
}

invertedTriangle(5);
// * * * * *
// * * * *
// * * *
// * *
// *
```

---
---

# 20. Functions

---

## Function Declaration vs Function Expression

### Function Declaration
```js
// Can be called BEFORE it is defined (Hoisted)
sayHello();   // ✅ Works! (hoisted)

function sayHello() {
  console.log("Hello!");
}

sayHello();   // ✅ Works!
```

### Function Expression
```js
// Cannot be called before it is defined (NOT hoisted)
// greet();   // ❌ Error: greet is not a function

let greet = function() {
  console.log("Hello from expression!");
};

greet();   // ✅ Works (called after definition)
```

### Comparison Table

| Feature | Function Declaration | Function Expression |
|---|---|---|
| Syntax | `function name() {}` | `let name = function() {}` |
| Hoisting | ✅ Yes (can call before define) | ❌ No |
| Named | Always named | Can be anonymous |
| When to use | General functions | Callbacks, assign to variable |

---

## Parameters & Arguments

```js
// Parameters = placeholders in function definition
// Arguments = actual values passed when calling

function greetUser(name, age) {   // name, age = PARAMETERS
  console.log(`Hello ${name}, you are ${age} years old`);
}

greetUser("Rahul", 25);   // "Rahul", 25 = ARGUMENTS
greetUser("Amit", 30);    // Different arguments, same function
```

### Default Parameters
```js
// If argument not passed, use default value
function greet(name = "Guest", message = "Welcome!") {
  console.log(`${message} ${name}`);
}

greet("Rahul", "Hello");   // Hello Rahul
greet("Amit");              // Welcome! Amit
greet();                    // Welcome! Guest
```

### Rest Parameters (…args)
```js
// Collect all extra arguments into an array
function sum(...numbers) {
  let total = 0;
  for (let num of numbers) {
    total += num;
  }
  return total;
}

console.log(sum(1, 2, 3));        // 6
console.log(sum(1, 2, 3, 4, 5)); // 15
console.log(sum(10, 20));         // 30
```

---

## Return Values & Scope

### Return Values
```js
// return sends a value back to where function was called
function add(a, b) {
  return a + b;   // Returns the result
}

let result = add(5, 3);   // result = 8
console.log(result);       // 8

// Function without return = returns undefined
function sayHi() {
  console.log("Hi!");
  // no return statement
}
let val = sayHi();
console.log(val);   // undefined
```

### Scope
```js
// Global Scope - accessible everywhere
let globalVar = "I am global";

function testScope() {
  // Local Scope - only inside this function
  let localVar = "I am local";

  console.log(globalVar);   // ✅ Can access global
  console.log(localVar);    // ✅ Can access local
}

testScope();
console.log(globalVar);   // ✅ Works
// console.log(localVar); // ❌ Error - localVar not accessible here

// Block Scope (let & const)
{
  let blockVar = "block scoped";
  console.log(blockVar);   // ✅ Works
}
// console.log(blockVar);  // ❌ Error - not accessible outside block
```

---

## Arrow Functions (ES6+)

```js
// Regular function
function add(a, b) {
  return a + b;
}

// Arrow function - same thing, shorter syntax
let add = (a, b) => {
  return a + b;
};

// Even shorter - if only one return statement
let add = (a, b) => a + b;

// Single parameter - no need for parentheses
let double = x => x * 2;
let greet = name => `Hello, ${name}!`;

// No parameters - empty parentheses required
let sayHello = () => "Hello!";

// Multiple lines - need curly braces and return
let calculate = (a, b) => {
  let sum = a + b;
  let product = a * b;
  return { sum, product };
};
```

### Regular vs Arrow Function – Key Difference
```js
// Arrow functions DO NOT have their own 'this'
// They use 'this' from surrounding context

const person = {
  name: "Rahul",

  // Regular function - has its own 'this'
  regularGreet: function() {
    console.log("Hello", this.name);   // ✅ "Hello Rahul"
  },

  // Arrow function - uses 'this' from outer scope
  arrowGreet: () => {
    console.log("Hello", this.name);   // ❌ undefined
  }
};

person.regularGreet();   // Hello Rahul
person.arrowGreet();     // Hello undefined
```

---

## Writing Reusable Logic Blocks

```js
// ✅ Good reusable functions - single responsibility
function validateEmail(email) {
  return email.includes("@") && email.includes(".");
}

function validateAge(age) {
  return typeof age === "number" && age >= 0 && age <= 120;
}

function formatCurrency(amount, currency = "USD") {
  return `${currency} ${amount.toFixed(2)}`;
}

function capitalizeFirst(str) {
  return str.charAt(0).toUpperCase() + str.slice(1).toLowerCase();
}

// Use everywhere
console.log(validateEmail("rahul@gmail.com")); // true
console.log(validateEmail("rahulgmail.com"));  // false
console.log(formatCurrency(99.5));              // USD 99.50
console.log(capitalizeFirst("hELLO"));          // Hello
```

---

## Practice Programs

### 1. Grade Calculator
```js
function calculateGrade(score) {
  if (typeof score !== "number" || score < 0 || score > 100) {
    return "Invalid score!";
  }

  if (score >= 90) return "A - Excellent";
  if (score >= 80) return "B - Good";
  if (score >= 70) return "C - Average";
  if (score >= 60) return "D - Below Average";
  return "F - Fail";
}

console.log(calculateGrade(95));   // A - Excellent
console.log(calculateGrade(72));   // C - Average
console.log(calculateGrade(45));   // F - Fail
console.log(calculateGrade(105));  // Invalid score!
```

### 2. Even/Odd Checker
```js
// Simple function
function checkEvenOdd(num) {
  if (num % 2 === 0) {
    return `${num} is Even`;
  } else {
    return `${num} is Odd`;
  }
}

// Arrow function version
let isEven = num => num % 2 === 0;
let isOdd = num => num % 2 !== 0;

console.log(checkEvenOdd(4));    // 4 is Even
console.log(checkEvenOdd(7));    // 7 is Odd
console.log(isEven(10));         // true
console.log(isOdd(9));           // true

// Check a range
for (let i = 1; i <= 10; i++) {
  console.log(checkEvenOdd(i));
}
```

### 3. Reusable Utility Functions
```js
// Utility functions library

// Check if number is in range
function inRange(num, min, max) {
  return num >= min && num <= max;
}

// Get random number between min and max
function randomBetween(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

// Remove duplicates from array
function removeDuplicates(arr) {
  return [...new Set(arr)];
}

// Truncate long string
function truncate(str, maxLength) {
  if (str.length > maxLength) {
    return str.slice(0, maxLength) + "...";
  }
  return str;
}

// Usage
console.log(inRange(5, 1, 10));                    // true
console.log(randomBetween(1, 100));                 // random number
console.log(removeDuplicates([1, 2, 2, 3, 3, 4])); // [1, 2, 3, 4]
console.log(truncate("Hello World long text", 8));  // "Hello Wo..."
```

---

## Recursion

**Recursion** is when a **function calls itself** until a base condition is met.

```
Recursion needs:
1. Base Case  → When to STOP (prevents infinite loop)
2. Recursive Case → When to CALL ITSELF again
```

### Simple Countdown Example
```js
function countdown(n) {
  // Base Case - stop condition
  if (n <= 0) {
    console.log("Done!");
    return;
  }

  console.log(n);
  countdown(n - 1);   // Recursive call with smaller value
}

countdown(5);
// 5, 4, 3, 2, 1, Done!
```

### Factorial using Recursion
```js
// Factorial: 5! = 5 × 4 × 3 × 2 × 1 = 120

function factorial(n) {
  // Base Case
  if (n === 0 || n === 1) return 1;

  // Recursive Case
  return n * factorial(n - 1);
}

console.log(factorial(5));   // 120
console.log(factorial(3));   // 6

// How it works:
// factorial(5) = 5 * factorial(4)
//              = 5 * 4 * factorial(3)
//              = 5 * 4 * 3 * factorial(2)
//              = 5 * 4 * 3 * 2 * factorial(1)
//              = 5 * 4 * 3 * 2 * 1 = 120
```

### Fibonacci using Recursion
```js
// Fibonacci: 0, 1, 1, 2, 3, 5, 8, 13, 21...
// Each number = sum of previous two numbers

function fibonacci(n) {
  // Base Cases
  if (n === 0) return 0;
  if (n === 1) return 1;

  // Recursive Case
  return fibonacci(n - 1) + fibonacci(n - 2);
}

console.log(fibonacci(0));   // 0
console.log(fibonacci(1));   // 1
console.log(fibonacci(6));   // 8
console.log(fibonacci(10));  // 55
```

### Sum of Array using Recursion
```js
function sumArray(arr) {
  // Base Case - empty array
  if (arr.length === 0) return 0;

  // Recursive Case - first element + sum of rest
  return arr[0] + sumArray(arr.slice(1));
}

console.log(sumArray([1, 2, 3, 4, 5]));   // 15

// How it works:
// sumArray([1,2,3,4,5])
// = 1 + sumArray([2,3,4,5])
// = 1 + 2 + sumArray([3,4,5])
// = 1 + 2 + 3 + sumArray([4,5])
// = 1 + 2 + 3 + 4 + sumArray([5])
// = 1 + 2 + 3 + 4 + 5 + sumArray([])
// = 1 + 2 + 3 + 4 + 5 + 0 = 15
```

### Recursion vs Loop
```js
// Same task - sum from 1 to n

// With Loop
function sumLoop(n) {
  let total = 0;
  for (let i = 1; i <= n; i++) {
    total += i;
  }
  return total;
}

// With Recursion
function sumRecursion(n) {
  if (n <= 0) return 0;
  return n + sumRecursion(n - 1);
}

console.log(sumLoop(5));       // 15
console.log(sumRecursion(5));  // 15
```

> **Interview Tip:** Recursion is elegant but can be slow for large inputs due to **stack overflow risk**. Always mention the **base case** when explaining recursion. Loops are generally more efficient.

---

## Quick Summary

```
17. Async JavaScript
    ├── JS is single-threaded, async allows non-blocking code
    ├── Callback = function passed to another function
    ├── Callback Hell = deeply nested callbacks ❌
    ├── Promise = pending → resolved/rejected
    ├── async/await = cleaner way to write promises
    └── setTimeout (once) | setInterval (repeat)

18. Operators & Types
    ├── == checks value only | === checks value AND type
    ├── && (both true) | || (one true) | ! (reverse)
    ├── typeof returns type as string
    ├── Falsy: false, 0, "", null, undefined, NaN
    └── Coercion: implicit (auto) | explicit (manual)

19. Conditionals & Loops
    ├── if-else → range/complex conditions
    ├── switch → exact value matching (need break!)
    ├── for → known iterations
    ├── while → unknown iterations (check first)
    ├── do-while → runs at least once
    └── break → exit | continue → skip iteration

20. Functions
    ├── Declaration = hoisted | Expression = not hoisted
    ├── Parameters (definition) vs Arguments (calling)
    ├── Arrow function → shorter, no own 'this'
    ├── Default params, Rest params (...args)
    └── Recursion = function calls itself + base case needed
```

---

> 💡 **Interview Tips:**
> - For async: say **"JS is single-threaded and uses Event Loop"**
> - Always use `===` instead of `==` in real code
> - Mention **6 falsy values** by memory
> - For recursion: always say **"we need a base case to prevent infinite loop"**
> - Arrow functions **don't have their own `this`** – important interview point!