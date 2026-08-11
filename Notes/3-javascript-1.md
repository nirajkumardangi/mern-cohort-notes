# JavaScript Complete Notes – Interview Ready 📘

---

# 13. Introduction to JavaScript

---

## What is JavaScript & Why It Powers the Web?

**JavaScript** is a **lightweight, interpreted, high-level programming language** used to make web pages **interactive and dynamic**.

### Simple Answer for Interview:
> JavaScript is a scripting language that runs in the browser and on the server. It allows us to add interactivity like button clicks, form validation, animations, and dynamic content to websites.

### Why It Powers the Web?
- **HTML** → Structure of the page
- **CSS** → Styling/Design of the page
- **JavaScript** → Behavior/Interactivity of the page

```
HTML + CSS + JavaScript = Complete Website
```

| Feature | Description |
|---|---|
| Dynamic Content | Change content without reloading page |
| Event Handling | Respond to user actions (click, type, scroll) |
| API Communication | Fetch data from server |
| Cross Platform | Runs everywhere – browser, server, mobile |

---

## Where JavaScript Runs?

### 1. Browser (Client Side)
- JS runs inside browser using **JavaScript Engine**
- Chrome uses **V8 Engine**
- Firefox uses **SpiderMonkey**
- JS can access DOM, handle events, manipulate HTML/CSS

### 2. Node.js (Server Side)
- Node.js is a **runtime environment** that runs JS outside the browser
- Built on **V8 Engine**
- Used for backend development, file handling, APIs

```
Browser JS  → Can access DOM, window, document
Node.js JS  → Can access file system, database, server
```

---

## Linking JS with HTML using Script Tag

### Method 1 – Inline JavaScript
```html
<!DOCTYPE html>
<html>
  <head>
    <title>JS Example</title>
  </head>
  <body>
    <h1>Hello World</h1>

    <!-- Inline JS -->
    <script>
      console.log("Hello from Inline JS");
    </script>
  </body>
</html>
```

### Method 2 – External JavaScript File
```html
<!DOCTYPE html>
<html>
  <head>
    <title>JS Example</title>
  </head>
  <body>
    <h1>Hello World</h1>

    <!-- External JS File -->
    <script src="script.js"></script>
  </body>
</html>
```

```js
// script.js
console.log("Hello from External JS File");
```

### Where to place Script Tag?
```html
<!-- ❌ In Head - JS runs before HTML loads, may cause errors -->
<head>
  <script src="script.js"></script>
</head>

<!-- ✅ Before closing body - HTML loads first, then JS runs -->
<body>
  ...
  <script src="script.js"></script>
</body>
```

> **Interview Tip:** Always place `<script>` before closing `</body>` tag so HTML loads completely before JS runs.

---

## Using Console for Debugging & Testing

The **Console** is a browser developer tool used to test and debug JavaScript code.

```js
// Different Console Methods

console.log("Normal message");           // Regular output
console.warn("This is a warning");       // Yellow warning
console.error("This is an error");       // Red error
console.table([1, 2, 3]);               // Shows data in table format
console.clear();                         // Clears the console
```

### How to Open Console?
- **Windows/Linux:** `F12` → Console Tab
- **Mac:** `Cmd + Option + J`

```js
// Example - Debugging with console
let name = "Rahul";
let age = 25;
console.log("Name:", name);   // Name: Rahul
console.log("Age:", age);     // Age: 25
```

---

## Variables – var, let, const

Variables are **containers to store data/values**.

### var (Old Way – Avoid Using)
```js
var name = "Rahul";
console.log(name);  // Rahul

var name = "Amit";  // ✅ Can be re-declared
name = "Suresh";    // ✅ Can be updated
```

### let (Modern Way – Use for changeable values)
```js
let age = 25;
age = 26;           // ✅ Can be updated
// let age = 30;    // ❌ Cannot be re-declared in same scope
```

### const (Use for fixed/constant values)
```js
const PI = 3.14;
// PI = 3.15;       // ❌ Cannot be updated
// const PI = 3;    // ❌ Cannot be re-declared
```

---

## Scope Understanding

### var → Function Scoped
```js
function testVar() {
  var x = 10;
  if (true) {
    var x = 20;         // Same variable (function scoped)
    console.log(x);     // 20
  }
  console.log(x);       // 20 (changed!)
}
testVar();
```

### let & const → Block Scoped
```js
function testLet() {
  let x = 10;
  if (true) {
    let x = 20;         // Different variable (block scoped)
    console.log(x);     // 20
  }
  console.log(x);       // 10 (unchanged!)
}
testLet();
```

### Hoisting
```js
// var is hoisted (moved to top) but value is undefined
console.log(a);   // undefined (not error)
var a = 5;

// let and const are NOT hoisted
// console.log(b); // ❌ ReferenceError
let b = 10;
```

---

## Variable Comparison Table

| Feature | var | let | const |
|---|---|---|---|
| Scope | Function | Block | Block |
| Re-declare | ✅ Yes | ❌ No | ❌ No |
| Update | ✅ Yes | ✅ Yes | ❌ No |
| Hoisting | ✅ Yes (undefined) | ❌ No | ❌ No |
| Use When | Avoid | Value changes | Value is fixed |

---

## Naming Conventions & Clean Code Practices

```js
// ✅ camelCase - Most common in JavaScript
let firstName = "Rahul";
let totalAmount = 500;

// ✅ PascalCase - Used for Classes/Constructors
class UserProfile {}

// ✅ UPPER_SNAKE_CASE - Used for Constants
const MAX_LIMIT = 100;
const API_URL = "https://api.example.com";

// ❌ Bad naming - Avoid this
let x = "Rahul";        // Not meaningful
let d = new Date();     // Not clear
let abc123 = 500;       // Not readable

// ✅ Good naming
let userName = "Rahul";
let currentDate = new Date();
let totalPrice = 500;
```

### Clean Code Rules:
```js
// 1. Use meaningful names
let u = "Rahul";          // ❌ Bad
let userName = "Rahul";   // ✅ Good

// 2. Use const by default, let when needed
const MAX = 100;
let count = 0;

// 3. One variable, one purpose
let data = "Rahul";       // ❌ Reusing for different things later
let data = 25;

// 4. Avoid magic numbers, use constants
if (age > 18) {}          // ❌ What is 18?
const ADULT_AGE = 18;
if (age > ADULT_AGE) {}  // ✅ Clear!
```

---

## Data Types

JavaScript has two categories of data types:

---

### Primitive Data Types
> Stored directly in memory. Immutable (cannot be changed directly).

#### 1. Number
```js
let age = 25;
let price = 99.99;
let negative = -10;
let result = 10 / 0;    // Infinity
let invalid = "abc" * 2; // NaN (Not a Number)

console.log(typeof age);    // "number"
console.log(typeof NaN);    // "number" (special case!)
```

#### 2. String
```js
let name = "Rahul";           // Double quotes
let city = 'Delhi';           // Single quotes
let greeting = `Hello ${name}`; // Template literal (backtick)

console.log(greeting);         // Hello Rahul
console.log(typeof name);      // "string"
console.log(name.length);      // 5
console.log(name.toUpperCase()); // RAHUL
```

#### 3. Boolean
```js
let isLoggedIn = true;
let isAdmin = false;

console.log(typeof isLoggedIn);  // "boolean"
console.log(5 > 3);              // true
console.log(5 < 3);              // false
```

#### 4. Null
```js
// null = intentionally empty / no value
let user = null;

console.log(user);         // null
console.log(typeof null);  // "object" (this is a known JS bug!)
```

#### 5. Undefined
```js
// undefined = variable declared but no value assigned
let score;
console.log(score);          // undefined
console.log(typeof score);   // "undefined"
```

---

### Non-Primitive Data Types
> Stored as reference in memory. Can hold multiple values.

#### Object
```js
let person = {
  name: "Rahul",
  age: 25,
  city: "Delhi"
};

console.log(person.name);     // Rahul
console.log(person["age"]);   // 25
console.log(typeof person);   // "object"
```

#### Array
```js
let fruits = ["Apple", "Banana", "Mango"];

console.log(fruits[0]);       // Apple
console.log(fruits.length);   // 3
console.log(typeof fruits);   // "object"
```

#### Function
```js
function greet() {
  return "Hello!";
}

console.log(typeof greet);    // "function"
```

---

### Primitive vs Non-Primitive

| Feature | Primitive | Non-Primitive |
|---|---|---|
| Examples | number, string, boolean, null, undefined | object, array, function |
| Stored as | Value (copy) | Reference (address) |
| Mutable | No | Yes |
| Memory | Stack | Heap |

```js
// Primitive - Copy by Value
let a = 10;
let b = a;
b = 20;
console.log(a);  // 10 (unchanged)
console.log(b);  // 20

// Non-Primitive - Copy by Reference
let obj1 = { name: "Rahul" };
let obj2 = obj1;
obj2.name = "Amit";
console.log(obj1.name);  // Amit (changed! both point to same object)
```

---

## Mini Practice Programs

### 1. Simple Calculator
```js
// Calculator using prompt
let num1 = Number(prompt("Enter first number:"));
let num2 = Number(prompt("Enter second number:"));
let operator = prompt("Enter operator (+, -, *, /):");

let result;

if (operator === "+") {
  result = num1 + num2;
} else if (operator === "-") {
  result = num1 - num2;
} else if (operator === "*") {
  result = num1 * num2;
} else if (operator === "/") {
  result = num1 / num2;
} else {
  result = "Invalid operator";
}

console.log(`Result: ${result}`);
alert(`Result: ${result}`);
```

### 2. Greeting System
```js
// Greeting based on time
let userName = prompt("Enter your name:");
let currentHour = new Date().getHours();
let greeting;

if (currentHour < 12) {
  greeting = "Good Morning";
} else if (currentHour < 17) {
  greeting = "Good Afternoon";
} else {
  greeting = "Good Evening";
}

console.log(`${greeting}, ${userName}!`);
alert(`${greeting}, ${userName}!`);
```

### 3. Input Handling via Prompt
```js
// Basic input handling
let name = prompt("What is your name?");
let age = Number(prompt("What is your age?"));

if (age >= 18) {
  console.log(`Hello ${name}, you are an adult.`);
} else {
  console.log(`Hello ${name}, you are a minor.`);
}
```

---
---

# 14. Document Object Model (DOM) Manipulation

---

## Introduction to DOM

**DOM (Document Object Model)** is a **programming interface** for HTML documents.

> When a browser loads an HTML page, it creates a **tree-like structure** called the DOM. JavaScript can use this tree to **read, update, delete, or add** HTML elements.

### Simple Interview Answer:
> DOM is like a map of the entire HTML page that JavaScript uses to find and change any element on the page.

```
Browser loads HTML → Creates DOM Tree → JS uses DOM to manipulate page
```

---

## DOM Structure and Tree

```
document
└── html
    ├── head
    │   └── title
    │       └── "My Page"
    └── body
        ├── h1
        │   └── "Hello World"
        ├── p
        │   └── "This is a paragraph"
        └── div
            └── a
                └── "Click Here"
```

### Key Terms:

| Term | Meaning |
|---|---|
| **Document** | Root of the DOM tree (the entire page) |
| **Node** | Everything in DOM is a node (elements, text, comments) |
| **Element** | HTML tags like `<div>`, `<p>`, `<h1>` are element nodes |
| **Text Node** | Text inside elements is a text node |

```js
// document is the entry point to DOM
console.log(document);           // Entire HTML page
console.log(document.body);      // Body element
console.log(document.title);     // Page title
```

---

## Fetching / Selecting Elements in DOM

### 1. getElementById
```js
// Select element by its ID (returns single element)
let heading = document.getElementById("main-heading");
console.log(heading);         // <h1 id="main-heading">...</h1>
```

```html
<h1 id="main-heading">Hello World</h1>
```

### 2. getElementsByTagName
```js
// Select all elements by tag name (returns HTMLCollection)
let allParas = document.getElementsByTagName("p");
console.log(allParas);        // HTMLCollection of all <p> tags
console.log(allParas[0]);     // First <p> element
```

### 3. getElementsByClassName
```js
// Select all elements by class name (returns HTMLCollection)
let items = document.getElementsByClassName("list-item");
console.log(items);           // All elements with class "list-item"
console.log(items[0]);        // First element
```

### 4. querySelector
```js
// Select FIRST matching element using CSS selector
let heading = document.querySelector("h1");           // By tag
let title = document.querySelector("#main-heading");  // By ID
let box = document.querySelector(".box");             // By Class
let special = document.querySelector("div.card p");   // Complex selector
```

### 5. querySelectorAll
```js
// Select ALL matching elements using CSS selector (returns NodeList)
let allButtons = document.querySelectorAll("button");
let allCards = document.querySelectorAll(".card");

// Loop through NodeList
allCards.forEach(function(card) {
  console.log(card);
});
```

---

### Comparison Table

| Method | Returns | Select By |
|---|---|---|
| `getElementById` | Single Element | ID |
| `getElementsByTagName` | HTMLCollection | Tag name |
| `getElementsByClassName` | HTMLCollection | Class name |
| `querySelector` | Single Element (first) | Any CSS selector |
| `querySelectorAll` | NodeList (all) | Any CSS selector |

> **Interview Tip:** `querySelector` and `querySelectorAll` are most powerful and commonly used. They accept any CSS selector.

---

## DOM Tree Traversal

Navigating through the DOM tree using parent, child, and sibling relationships.

```html
<div id="parent">
  <p id="child1">First</p>
  <p id="child2">Second</p>
  <p id="child3">Third</p>
</div>
```

```js
let parent = document.getElementById("parent");
let child1 = document.getElementById("child1");

// Parent Node
console.log(child1.parentNode);        // <div id="parent">

// Child Nodes (includes text nodes - whitespace)
console.log(parent.childNodes);        // NodeList [text, p, text, p, text, p, text]

// Children (only element nodes, no text nodes)
console.log(parent.children);          // HTMLCollection [p, p, p]

// First Child (includes text node)
console.log(parent.firstChild);        // text node (whitespace)

// First Element Child (only elements)
console.log(parent.firstElementChild); // <p id="child1">First</p>

// Last Child
console.log(parent.lastChild);         // text node
console.log(parent.lastElementChild);  // <p id="child3">Third</p>

// Next Sibling (includes text nodes)
console.log(child1.nextSibling);        // text node

// Next Element Sibling (only elements)
console.log(child1.nextElementSibling); // <p id="child2">Second</p>

// Previous Element Sibling
console.log(child1.previousElementSibling); // null (no element before)
```

---

## Manipulating DOM Elements

### 1. innerHTML
```js
// Gets or sets HTML content inside an element
let box = document.querySelector(".box");

// Get
console.log(box.innerHTML);   // <p>Hello</p>

// Set (can include HTML tags)
box.innerHTML = "<h2>New Content</h2>";
box.innerHTML += "<p>Added paragraph</p>";  // Append
```

### 2. textContent
```js
// Gets or sets plain TEXT content (no HTML parsing)
let para = document.querySelector("p");

// Get
console.log(para.textContent);   // Plain text

// Set
para.textContent = "New Text";   // Only text, not HTML
```

### innerHTML vs textContent
```js
let div = document.querySelector("div");

div.innerHTML = "<strong>Bold Text</strong>";  // ✅ Renders as Bold Text
div.textContent = "<strong>Bold Text</strong>"; // Shows literally as text
```

### 3. setAttribute & getAttribute
```js
// Set attribute on element
let img = document.querySelector("img");
img.setAttribute("src", "photo.jpg");     // Set src
img.setAttribute("alt", "My Photo");     // Set alt
img.setAttribute("class", "profile");    // Set class

// Get attribute value
let src = img.getAttribute("src");       // "photo.jpg"
let alt = img.getAttribute("alt");       // "My Photo"
console.log(src, alt);
```

### 4. Style Property
```js
// Change inline CSS using style property
let heading = document.querySelector("h1");

heading.style.color = "red";
heading.style.fontSize = "32px";
heading.style.backgroundColor = "yellow";
heading.style.display = "none";     // Hide element
heading.style.display = "block";    // Show element
```

### 5. classList
```js
let box = document.querySelector(".box");

// Add a class
box.classList.add("active");

// Remove a class
box.classList.remove("hidden");

// Toggle a class (add if absent, remove if present)
box.classList.toggle("dark-mode");

// Check if class exists
console.log(box.classList.contains("active"));  // true or false

// Replace one class with another
box.classList.replace("old-class", "new-class");
```

---

## Creating and Removing DOM Elements

### 1. createElement()
```js
// Create a new HTML element
let newDiv = document.createElement("div");
let newPara = document.createElement("p");
let newBtn = document.createElement("button");

newPara.textContent = "This is a new paragraph";
newPara.classList.add("my-para");
```

### 2. appendChild()
```js
// Add new element as LAST child of parent
let container = document.querySelector(".container");
let newPara = document.createElement("p");
newPara.textContent = "I am a new paragraph";

container.appendChild(newPara);   // Added at end of container
```

### 3. insertBefore()
```js
// Insert new element BEFORE a specific child
let container = document.querySelector(".container");
let newPara = document.createElement("p");
newPara.textContent = "I am inserted before";

let firstChild = container.firstElementChild;
container.insertBefore(newPara, firstChild);  // Insert before first child
```

### 4. removeChild()
```js
// Remove a child element from parent
let container = document.querySelector(".container");
let firstChild = container.firstElementChild;

container.removeChild(firstChild);   // Remove first child

// Modern way - remove element directly
firstChild.remove();   // ✅ Simpler
```

### Complete Example – Create & Remove Elements
```html
<!DOCTYPE html>
<html>
<body>
  <div id="list-container">
    <p>Item 1</p>
  </div>
  <button id="add-btn">Add Item</button>
  <button id="remove-btn">Remove Item</button>

  <script>
    let container = document.getElementById("list-container");
    let addBtn = document.getElementById("add-btn");
    let removeBtn = document.getElementById("remove-btn");

    // Add new element
    addBtn.addEventListener("click", function() {
      let newItem = document.createElement("p");
      newItem.textContent = "New Item";
      container.appendChild(newItem);
    });

    // Remove last element
    removeBtn.addEventListener("click", function() {
      let lastChild = container.lastElementChild;
      if (lastChild) {
        container.removeChild(lastChild);
      }
    });
  </script>
</body>
</html>
```

---
---

# 15. Event Handling in JavaScript

---

## Event Handling in JavaScript

An **event** is something that happens on the page – like a **click, keypress, scroll, hover, submit**, etc.

**Event Handling** means writing code that runs when an event happens.

---

### addEventListener()
```js
// Syntax
element.addEventListener("event-type", callbackFunction);
```

```js
let btn = document.querySelector("button");

// Method 1 - Named function
function handleClick() {
  console.log("Button was clicked!");
}
btn.addEventListener("click", handleClick);

// Method 2 - Anonymous function
btn.addEventListener("click", function() {
  console.log("Button clicked!");
});

// Method 3 - Arrow function
btn.addEventListener("click", () => {
  console.log("Button clicked!");
});
```

---

### Event Bubbling
> When an event fires on an element, it **bubbles up** to its parent elements.

```html
<div id="parent">
  <button id="child">Click Me</button>
</div>
```

```js
let parent = document.getElementById("parent");
let child = document.getElementById("child");

child.addEventListener("click", function() {
  console.log("Child clicked");    // Fires first
});

parent.addEventListener("click", function() {
  console.log("Parent clicked");   // Fires second (bubbling up!)
});

// Output when button is clicked:
// "Child clicked"
// "Parent clicked"
```

### Stopping Event Bubbling
```js
child.addEventListener("click", function(event) {
  event.stopPropagation();   // Stops bubbling to parent
  console.log("Child clicked");
});
```

---

### event.target
```js
// event.target = the element that triggered the event
document.addEventListener("click", function(event) {
  console.log(event.target);         // Exact element clicked
  console.log(event.target.id);      // ID of clicked element
  console.log(event.target.tagName); // Tag name of clicked element
});
```

### Event Delegation using event.target
```js
// Instead of adding listener to each button, add to parent
let container = document.getElementById("btn-container");

container.addEventListener("click", function(event) {
  if (event.target.tagName === "BUTTON") {
    console.log("Button clicked:", event.target.textContent);
  }
});
```

---

## Scroll Events, Mouse Events, Key Events

### Mouse Events
```js
let box = document.querySelector(".box");

box.addEventListener("click", () => console.log("Clicked!"));
box.addEventListener("dblclick", () => console.log("Double Clicked!"));
box.addEventListener("mouseover", () => console.log("Mouse entered!"));
box.addEventListener("mouseout", () => console.log("Mouse left!"));
box.addEventListener("mousemove", (e) => {
  console.log(`Mouse at X:${e.clientX}, Y:${e.clientY}`);
});
```

### Key Events
```js
document.addEventListener("keydown", function(event) {
  console.log("Key pressed:", event.key);   // e.g., "Enter", "a"
  console.log("Key code:", event.keyCode);  // e.g., 13, 65

  if (event.key === "Enter") {
    console.log("Enter key was pressed!");
  }
});

document.addEventListener("keyup", function(event) {
  console.log("Key released:", event.key);
});
```

### Scroll Events
```js
window.addEventListener("scroll", function() {
  let scrollY = window.scrollY;   // How much scrolled vertically
  console.log("Scrolled:", scrollY, "px");

  if (scrollY > 200) {
    document.querySelector(".navbar").classList.add("sticky");
  }
});
```

### Strict Mode
```js
// 'use strict' - Makes JS run in strict mode
// Catches common coding mistakes and throws errors

"use strict";

// x = 10;        // ❌ Error - variable not declared
let x = 10;      // ✅ Must declare variables
console.log(x);
```

---

## Working with Forms and Input Elements

### Accessing Form Data
```html
<form id="user-form">
  <input type="text" id="name-input" placeholder="Enter name">
  <input type="email" id="email-input" placeholder="Enter email">
  <button type="submit">Submit</button>
</form>
```

```js
let form = document.getElementById("user-form");
let nameInput = document.getElementById("name-input");
let emailInput = document.getElementById("email-input");

// Access input value
console.log(nameInput.value);    // Current value of input
```

### Validating Forms + preventDefault
```js
form.addEventListener("submit", function(event) {
  event.preventDefault();   // Stops page from reloading on submit

  let name = nameInput.value;
  let email = emailInput.value;

  // Validation
  if (name === "") {
    alert("Name is required!");
    return;
  }

  if (email === "") {
    alert("Email is required!");
    return;
  }

  console.log("Form submitted:", name, email);
});
```

### onsubmit
```js
// onsubmit - fires when form is submitted
form.onsubmit = function(event) {
  event.preventDefault();
  console.log("Form submitted!");
};
```

### onchange
```js
// onchange - fires when input value changes and loses focus
nameInput.addEventListener("change", function() {
  console.log("Name changed to:", nameInput.value);
});

// oninput - fires on every keystroke (real-time)
nameInput.addEventListener("input", function() {
  console.log("Current value:", nameInput.value);
});
```

### Complete Form Validation Example
```js
form.addEventListener("submit", function(event) {
  event.preventDefault();

  let name = document.getElementById("name-input").value.trim();
  let email = document.getElementById("email-input").value.trim();

  if (name.length < 3) {
    alert("Name must be at least 3 characters");
    return;
  }

  if (!email.includes("@")) {
    alert("Enter a valid email address");
    return;
  }

  alert(`Welcome, ${name}! Form submitted successfully.`);
});
```

---

## Working with Classes – classList Methods

```js
let element = document.querySelector(".card");

// Add class
element.classList.add("active");
element.classList.add("highlight", "bold");  // Add multiple

// Remove class
element.classList.remove("active");

// Toggle class (add if not present, remove if present)
element.classList.toggle("dark-mode");

// Check if class exists
let hasActive = element.classList.contains("active");
console.log(hasActive);   // true or false

// Replace class
element.classList.replace("old-class", "new-class");
```

### Dark Mode Toggle Example
```html
<button id="toggle-btn">Toggle Dark Mode</button>
```

```js
let toggleBtn = document.getElementById("toggle-btn");

toggleBtn.addEventListener("click", function() {
  document.body.classList.toggle("dark-mode");
});
```

---

## Browser Events

### DOMContentLoaded
```js
// Fires when HTML is fully loaded and parsed (before images/CSS)
document.addEventListener("DOMContentLoaded", function() {
  console.log("DOM is fully loaded!");
  // Safe to manipulate DOM here
});
```

### load
```js
// Fires when entire page is loaded (including images, CSS, JS)
window.addEventListener("load", function() {
  console.log("Full page loaded including images!");
});
```

### resize
```js
// Fires when browser window is resized
window.addEventListener("resize", function() {
  let width = window.innerWidth;
  let height = window.innerHeight;
  console.log(`Window size: ${width} x ${height}`);
});
```

### scroll
```js
// Fires when user scrolls the page
window.addEventListener("scroll", function() {
  console.log("Page scrolled! Y position:", window.scrollY);
});
```

### Comparison Table

| Event | When it Fires |
|---|---|
| `DOMContentLoaded` | HTML parsed, DOM ready (before images load) |
| `load` | Full page loaded including all assets |
| `resize` | Browser window is resized |
| `scroll` | Page is scrolled |

---
---

# 16. Using Browser Functionalities in JavaScript

---

## Browser Object Model (BOM)

**BOM (Browser Object Model)** allows JavaScript to interact with the **browser** (not just the HTML page).

### Main BOM Objects:

| Object | Description |
|---|---|
| `window` | Global object – represents browser window |
| `navigator` | Information about the browser |
| `history` | Browser history (back, forward) |
| `location` | Current page URL info |
| `document` | The HTML page (DOM) |

---

## Window Object

`window` is the **global object** in browsers. All global variables and functions are part of `window`.

```js
// window is the global object
console.log(window.innerWidth);    // Browser window width
console.log(window.innerHeight);   // Browser window height

// Alert, prompt, confirm are window methods
window.alert("Hello!");            // Same as alert("Hello!")
window.prompt("Enter name:");      // Same as prompt()
window.confirm("Are you sure?");   // Returns true/false

// Timers
window.setTimeout(function() {
  console.log("Runs after 2 seconds");
}, 2000);

window.setInterval(function() {
  console.log("Runs every 1 second");
}, 1000);
```

---

### window.location
```js
// Current page URL information
console.log(window.location.href);      // Full URL: "https://example.com/about"
console.log(window.location.hostname);  // "example.com"
console.log(window.location.pathname);  // "/about"
console.log(window.location.protocol); // "https:"
console.log(window.location.port);     // "3000" or ""
console.log(window.location.search);   // "?id=5&name=Rahul" (query string)

// Redirect to another page
window.location.href = "https://google.com";   // Redirect

// Reload current page
window.location.reload();
```

### window.history
```js
// Browser navigation history
window.history.back();      // Go to previous page (like browser back button)
window.history.forward();   // Go to next page (like browser forward button)
window.history.go(-1);      // Go back 1 page
window.history.go(-2);      // Go back 2 pages
window.history.go(1);       // Go forward 1 page

console.log(window.history.length);  // Number of pages in history
```

---

## Navigator Object
```js
// Information about browser and device
console.log(navigator.userAgent);    // Browser info string
console.log(navigator.language);     // Language: "en-US"
console.log(navigator.onLine);       // true if connected to internet
console.log(navigator.platform);     // OS: "Win32", "MacIntel"

// Geolocation
navigator.geolocation.getCurrentPosition(function(position) {
  console.log("Latitude:", position.coords.latitude);
  console.log("Longitude:", position.coords.longitude);
});
```

---

## Working with Storage

### 1. Local Storage
> Stores data **permanently** in browser (until manually cleared). No expiry.

```js
// Set item
localStorage.setItem("userName", "Rahul");
localStorage.setItem("age", "25");

// Get item
let name = localStorage.getItem("userName");
console.log(name);   // "Rahul"

// Remove specific item
localStorage.removeItem("age");

// Clear all localStorage
localStorage.clear();

// Store objects (must convert to JSON string)
let user = { name: "Rahul", age: 25 };
localStorage.setItem("user", JSON.stringify(user));

// Get object back
let storedUser = JSON.parse(localStorage.getItem("user"));
console.log(storedUser.name);   // "Rahul"
```

---

### 2. Session Storage
> Stores data **temporarily** – clears when browser tab is closed.

```js
// Same methods as localStorage
sessionStorage.setItem("sessionToken", "abc123");

let token = sessionStorage.getItem("sessionToken");
console.log(token);   // "abc123"

sessionStorage.removeItem("sessionToken");
sessionStorage.clear();
```

---

### Local Storage vs Session Storage

| Feature | Local Storage | Session Storage |
|---|---|---|
| Lifetime | Permanent (until cleared) | Until tab/browser closes |
| Size | ~5-10MB | ~5MB |
| Scope | All tabs same origin | Only same tab |
| Use Case | Remember login, preferences | Temporary form data |

---

### 3. Cookies
```js
// Set cookie
document.cookie = "userName=Rahul";
document.cookie = "age=25; expires=Fri, 31 Dec 2025 12:00:00 UTC; path=/";

// Read all cookies
console.log(document.cookie);   // "userName=Rahul; age=25"

// Delete cookie (set expiry to past date)
document.cookie = "userName=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;";
```

### All Three Storage Comparison

| Feature | Local Storage | Session Storage | Cookies |
|---|---|---|---|
| Capacity | 5-10MB | 5MB | 4KB |
| Expiry | Never | Tab close | Set manually |
| Sent to Server | ❌ No | ❌ No | ✅ Yes (with every request) |
| Access via JS | ✅ Yes | ✅ Yes | ✅ Yes |

---

## Fetch API

**Fetch API** is used to make **HTTP requests** to a server and get data (usually JSON).

### Basic Fetch Request
```js
// fetch() returns a Promise
fetch("https://jsonplaceholder.typicode.com/users")
  .then(function(response) {
    return response.json();   // Convert response to JSON
  })
  .then(function(data) {
    console.log(data);        // Array of users
  })
  .catch(function(error) {
    console.log("Error:", error);
  });
```

### Fetch with async/await (Cleaner Way)
```js
async function getUsers() {
  try {
    let response = await fetch("https://jsonplaceholder.typicode.com/users");
    
    if (!response.ok) {
      throw new Error("Network response was not ok");
    }
    
    let data = await response.json();
    console.log(data);
    return data;
  } catch (error) {
    console.log("Error:", error);
  }
}

getUsers();
```

### Fetch – POST Request (Sending Data)
```js
async function createUser() {
  let newUser = {
    name: "Rahul",
    email: "rahul@example.com"
  };

  let response = await fetch("https://jsonplaceholder.typicode.com/users", {
    method: "POST",                              // HTTP method
    headers: {
      "Content-Type": "application/json"        // Tell server we're sending JSON
    },
    body: JSON.stringify(newUser)                // Convert object to JSON string
  });

  let data = await response.json();
  console.log("Created:", data);
}

createUser();
```

### Fetch & Display Data in DOM
```js
async function displayUsers() {
  let response = await fetch("https://jsonplaceholder.typicode.com/users");
  let users = await response.json();

  let container = document.getElementById("users-container");

  users.forEach(function(user) {
    let card = document.createElement("div");
    card.classList.add("user-card");
    card.innerHTML = `
      <h3>${user.name}</h3>
      <p>Email: ${user.email}</p>
      <p>City: ${user.address.city}</p>
    `;
    container.appendChild(card);
  });
}

displayUsers();
```

---

## Quick Summary – All Topics

```
13. JavaScript Intro
    ├── JS runs in Browser (V8) and Node.js
    ├── Link using <script src="file.js">
    ├── var (function scope) | let (block) | const (block, no update)
    ├── Data Types: number, string, boolean, null, undefined (Primitive)
    └── Objects, Arrays (Non-Primitive - passed by reference)

14. DOM Manipulation
    ├── DOM = Tree structure of HTML page
    ├── Select: getElementById, querySelector, querySelectorAll
    ├── Traverse: parentNode, children, nextElementSibling
    ├── Modify: innerHTML, textContent, setAttribute, classList
    └── Create/Remove: createElement, appendChild, removeChild

15. Event Handling
    ├── addEventListener("click", function)
    ├── Event Bubbling = event travels from child to parent
    ├── event.target = element that triggered event
    ├── preventDefault() = stops default browser behavior
    └── Browser Events: DOMContentLoaded, load, resize, scroll

16. Browser Functionalities
    ├── BOM: window, navigator, history, location
    ├── localStorage = permanent | sessionStorage = temporary
    ├── Cookies = sent to server, max 4KB
    └── Fetch API = make HTTP requests, handle with .then() or async/await
```

---

> 💡 **Interview Tips:**
> - Always say **"DOM is a tree-like structure"** when explaining DOM
> - Explain `var` vs `let` vs `const` with **scope and hoisting**
> - For Fetch API, mention it **returns a Promise**
> - `localStorage` data is **never deleted** unless manually cleared
> - `event.preventDefault()` is used to **stop form from reloading the page**