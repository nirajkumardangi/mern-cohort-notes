# JavaScript Complete Notes – Interview Ready 📘

---

# 21. Arrays

---

## What are Arrays & When to Use Them?

An **Array** is a **collection of multiple values stored in a single variable**, in an **ordered list**.

### Simple Interview Answer:
> Array is a data structure that stores multiple values in a single variable in an ordered way. Each value has an index starting from 0.

### When to Use Arrays?
```
Use Arrays when:
├── You have a LIST of similar items (students, products, scores)
├── Order matters (first, second, third...)
├── You need to loop through items
└── You need to add/remove items dynamically
```

### Creating Arrays
```js
// Method 1 - Array Literal (Most Common ✅)
let fruits = ["Apple", "Banana", "Mango"];
let numbers = [1, 2, 3, 4, 5];
let mixed = [1, "hello", true, null];   // Can hold different types

// Method 2 - Array Constructor
let arr = new Array(3);           // Creates array with 3 empty slots
let arr2 = new Array(1, 2, 3);   // [1, 2, 3]

// Empty Array
let emptyArr = [];

console.log(fruits);         // ["Apple", "Banana", "Mango"]
console.log(typeof fruits);  // "object" (arrays are objects in JS!)
console.log(Array.isArray(fruits));  // true ✅ Better way to check
```

---

## Accessing Elements – Indexing Basics

```js
let fruits = ["Apple", "Banana", "Mango", "Orange", "Grapes"];
//  Index:      0         1          2        3          4

// Access by index (starts from 0)
console.log(fruits[0]);   // Apple
console.log(fruits[2]);   // Mango
console.log(fruits[4]);   // Grapes

// Last element
console.log(fruits[fruits.length - 1]);   // Grapes

// Using at() method (ES2022) - supports negative index
console.log(fruits.at(0));    // Apple   (first)
console.log(fruits.at(-1));   // Grapes  (last)
console.log(fruits.at(-2));   // Orange  (second last)

// Access non-existent index
console.log(fruits[10]);  // undefined (no error)

// Modify element by index
fruits[1] = "Blueberry";
console.log(fruits);  // ["Apple", "Blueberry", "Mango", "Orange", "Grapes"]
```

---

## Array Operations – push, pop, shift, unshift

```js
let fruits = ["Apple", "Banana", "Mango"];

// push() - Add item to END of array
fruits.push("Orange");
console.log(fruits);  // ["Apple", "Banana", "Mango", "Orange"]

fruits.push("Grapes", "Kiwi");  // Add multiple items
console.log(fruits);  // ["Apple", "Banana", "Mango", "Orange", "Grapes", "Kiwi"]

// pop() - Remove item from END of array (returns removed item)
let removed = fruits.pop();
console.log(removed);  // "Kiwi"
console.log(fruits);   // ["Apple", "Banana", "Mango", "Orange", "Grapes"]

// unshift() - Add item to BEGINNING of array
fruits.unshift("Strawberry");
console.log(fruits);  // ["Strawberry", "Apple", "Banana", "Mango", "Orange", "Grapes"]

// shift() - Remove item from BEGINNING of array (returns removed item)
let first = fruits.shift();
console.log(first);   // "Strawberry"
console.log(fruits);  // ["Apple", "Banana", "Mango", "Orange", "Grapes"]
```

### Quick Memory Trick
```
push    → Add to END      ➡️ [array →  item]
pop     → Remove from END ➡️ [array → ❌item]
unshift → Add to START    ➡️ [item  →  array]
shift   → Remove START    ➡️ [❌item →  array]
```

---

## length Property

```js
let fruits = ["Apple", "Banana", "Mango"];

console.log(fruits.length);   // 3

// length updates automatically
fruits.push("Orange");
console.log(fruits.length);   // 4

fruits.pop();
console.log(fruits.length);   // 3

// Use length for last index
let lastIndex = fruits.length - 1;
console.log(fruits[lastIndex]);  // "Mango"

// Truncate array using length
fruits.length = 2;
console.log(fruits);  // ["Apple", "Banana"] (rest deleted!)

// Check if array is empty
let emptyArr = [];
if (emptyArr.length === 0) {
  console.log("Array is empty");
}
```

---

## Basic Iteration – Looping Through Arrays

```js
let numbers = [10, 20, 30, 40, 50];

// Method 1 - for loop (classic)
for (let i = 0; i < numbers.length; i++) {
  console.log(`Index ${i}: ${numbers[i]}`);
}

// Method 2 - for...of (clean & modern ✅)
for (let num of numbers) {
  console.log(num);
}

// Method 3 - while loop
let i = 0;
while (i < numbers.length) {
  console.log(numbers[i]);
  i++;
}

// Method 4 - forEach (functional way)
numbers.forEach(function(num, index) {
  console.log(`Index ${index}: ${num}`);
});
```

---

## Practice Programs

### 1. Todo List Using Array
```js
let todos = [];

// Add todo
function addTodo(task) {
  todos.push(task);
  console.log(`Added: "${task}"`);
}

// Remove todo
function removeTodo(task) {
  let index = todos.indexOf(task);
  if (index !== -1) {
    todos.splice(index, 1);
    console.log(`Removed: "${task}"`);
  } else {
    console.log("Task not found!");
  }
}

// Show all todos
function showTodos() {
  console.log("--- Your Todos ---");
  if (todos.length === 0) {
    console.log("No todos yet!");
    return;
  }
  todos.forEach(function(todo, index) {
    console.log(`${index + 1}. ${todo}`);
  });
}

// Usage
addTodo("Learn JavaScript");
addTodo("Practice Arrays");
addTodo("Build a project");
showTodos();
removeTodo("Practice Arrays");
showTodos();
```

### 2. Max / Min Finder
```js
let scores = [45, 89, 23, 67, 91, 34, 78];

// Method 1 - Using Math.max/min with spread
let max = Math.max(...scores);
let min = Math.min(...scores);
console.log("Max:", max);   // 91
console.log("Min:", min);   // 23

// Method 2 - Manual loop (for interview)
function findMax(arr) {
  let max = arr[0];   // Start with first element
  for (let i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
      max = arr[i];   // Update if larger found
    }
  }
  return max;
}

function findMin(arr) {
  let min = arr[0];
  for (let i = 1; i < arr.length; i++) {
    if (arr[i] < min) {
      min = arr[i];
    }
  }
  return min;
}

console.log("Max:", findMax(scores));  // 91
console.log("Min:", findMin(scores));  // 23
```

---

## Multidimensional Arrays

An array **inside another array** – like a **table or grid**.

```js
// 2D Array (Array of Arrays)
let matrix = [
  [1, 2, 3],    // Row 0
  [4, 5, 6],    // Row 1
  [7, 8, 9]     // Row 2
];

// Access elements [row][column]
console.log(matrix[0][0]);  // 1  (row 0, col 0)
console.log(matrix[1][2]);  // 6  (row 1, col 2)
console.log(matrix[2][1]);  // 8  (row 2, col 1)

// Loop through 2D array
for (let row = 0; row < matrix.length; row++) {
  for (let col = 0; col < matrix[row].length; col++) {
    process.stdout.write(matrix[row][col] + " ");
  }
  console.log();  // New line after each row
}
// Output:
// 1 2 3
// 4 5 6
// 7 8 9
```

### Real World 2D Array – Student Marks
```js
// [student][subject marks]
let students = [
  ["Rahul",  85, 90, 78],
  ["Amit",   70, 65, 88],
  ["Priya",  95, 92, 89]
];

// Access specific student
console.log(students[0][0]);   // "Rahul"
console.log(students[0][1]);   // 85 (first subject mark)

// Calculate average marks for each student
for (let i = 0; i < students.length; i++) {
  let name = students[i][0];
  let marks = students[i].slice(1);  // Get only marks (skip name)
  let avg = marks.reduce((sum, m) => sum + m, 0) / marks.length;
  console.log(`${name}: Average = ${avg.toFixed(2)}`);
}
// Rahul: Average = 84.33
// Amit: Average = 74.33
// Priya: Average = 92.00
```

---
---

# 22. Array Methods

---

## map() – Transforming Arrays

`map()` creates a **new array** by **transforming each element** using a function.

> Original array is NOT changed.

```js
let numbers = [1, 2, 3, 4, 5];

// Double each number
let doubled = numbers.map(function(num) {
  return num * 2;
});
console.log(doubled);   // [2, 4, 6, 8, 10]
console.log(numbers);   // [1, 2, 3, 4, 5] (original unchanged!)

// Arrow function version (shorter)
let squared = numbers.map(num => num ** 2);
console.log(squared);   // [1, 4, 9, 16, 25]

// Real world - format user names
let users = ["rahul", "amit", "priya"];
let formatted = users.map(name => name.charAt(0).toUpperCase() + name.slice(1));
console.log(formatted);  // ["Rahul", "Amit", "Priya"]

// Extract specific property from objects
let products = [
  { name: "Laptop", price: 50000 },
  { name: "Phone", price: 20000 },
  { name: "Tablet", price: 30000 }
];
let prices = products.map(product => product.price);
console.log(prices);   // [50000, 20000, 30000]
```

---

## filter() – Selecting Items Based on Condition

`filter()` creates a **new array** with only elements that **pass the condition** (return true).

```js
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Filter even numbers only
let evens = numbers.filter(num => num % 2 === 0);
console.log(evens);   // [2, 4, 6, 8, 10]

// Filter numbers greater than 5
let big = numbers.filter(num => num > 5);
console.log(big);     // [6, 7, 8, 9, 10]

// Real world - filter active users
let users = [
  { name: "Rahul", active: true },
  { name: "Amit", active: false },
  { name: "Priya", active: true },
  { name: "Neha", active: false }
];

let activeUsers = users.filter(user => user.active);
console.log(activeUsers);
// [{name: "Rahul", active: true}, {name: "Priya", active: true}]

// Filter products by price
let products = [
  { name: "Laptop", price: 50000 },
  { name: "Mouse", price: 500 },
  { name: "Keyboard", price: 1500 },
  { name: "Monitor", price: 15000 }
];

let affordable = products.filter(p => p.price < 5000);
console.log(affordable);
// Mouse: 500, Keyboard: 1500
```

---

## reduce() – Accumulating Values

`reduce()` reduces an array to a **single value** by applying a function to each element.

```js
// Syntax: array.reduce(function(accumulator, currentValue), initialValue)

let numbers = [1, 2, 3, 4, 5];

// Sum all numbers
let sum = numbers.reduce(function(acc, curr) {
  return acc + curr;
}, 0);   // 0 is initial value

console.log(sum);  // 15

// Step by step:
// acc=0, curr=1 → 0+1 = 1
// acc=1, curr=2 → 1+2 = 3
// acc=3, curr=3 → 3+3 = 6
// acc=6, curr=4 → 6+4 = 10
// acc=10, curr=5 → 10+5 = 15

// Product of all numbers
let product = numbers.reduce((acc, curr) => acc * curr, 1);
console.log(product);  // 120

// Find maximum value
let max = numbers.reduce((acc, curr) => curr > acc ? curr : acc, numbers[0]);
console.log(max);  // 5

// Real world - calculate total cart price
let cart = [
  { item: "Laptop", price: 50000, qty: 1 },
  { item: "Mouse", price: 500, qty: 2 },
  { item: "Keyboard", price: 1500, qty: 1 }
];

let totalPrice = cart.reduce(function(total, product) {
  return total + (product.price * product.qty);
}, 0);

console.log("Total:", totalPrice);  // Total: 52500
```

---

## forEach() – Iteration Without New Array

`forEach()` **loops through** each element and runs a function. It does **NOT return a new array**.

```js
let fruits = ["Apple", "Banana", "Mango"];

// Basic forEach
fruits.forEach(function(fruit) {
  console.log(fruit);
});

// With index
fruits.forEach(function(fruit, index) {
  console.log(`${index + 1}. ${fruit}`);
});
// 1. Apple
// 2. Banana
// 3. Mango

// Real world - display items on page
let students = ["Rahul", "Amit", "Priya"];
students.forEach(student => {
  let li = document.createElement("li");
  li.textContent = student;
  document.querySelector("ul").appendChild(li);
});
```

### forEach vs map
```js
let numbers = [1, 2, 3];

// forEach - no return value, just loops
let result1 = numbers.forEach(n => n * 2);
console.log(result1);   // undefined

// map - returns new array
let result2 = numbers.map(n => n * 2);
console.log(result2);   // [2, 4, 6]
```

---

## some() / every()

### some() – Returns true if AT LEAST ONE element passes condition
```js
let numbers = [1, 2, 3, 4, 5];

let hasEven = numbers.some(num => num % 2 === 0);
console.log(hasEven);   // true (2 and 4 are even)

let hasNegative = numbers.some(num => num < 0);
console.log(hasNegative);  // false (no negative numbers)

// Real world
let users = [
  { name: "Rahul", admin: false },
  { name: "Amit", admin: true }
];
let hasAdmin = users.some(user => user.admin);
console.log(hasAdmin);   // true
```

### every() – Returns true if ALL elements pass condition
```js
let numbers = [2, 4, 6, 8, 10];

let allEven = numbers.every(num => num % 2 === 0);
console.log(allEven);   // true (all are even)

let allPositive = numbers.every(num => num > 0);
console.log(allPositive);  // true

let allBig = numbers.every(num => num > 5);
console.log(allBig);    // false (2 and 4 fail)

// Real world - check if all students passed
let scores = [65, 72, 80, 55, 90];
let allPassed = scores.every(score => score >= 50);
console.log(allPassed);   // true
```

---

## slice() / splice()

### slice() – Copy part of array (NON-destructive)
```js
// slice(start, end) - end is NOT included, original unchanged
let fruits = ["Apple", "Banana", "Mango", "Orange", "Grapes"];

let portion = fruits.slice(1, 4);
console.log(portion);  // ["Banana", "Mango", "Orange"]
console.log(fruits);   // Original unchanged!

// From index 2 to end
let fromMiddle = fruits.slice(2);
console.log(fromMiddle);  // ["Mango", "Orange", "Grapes"]

// Negative index (from end)
let lastTwo = fruits.slice(-2);
console.log(lastTwo);  // ["Orange", "Grapes"]

// Copy entire array
let copy = fruits.slice();
console.log(copy);   // ["Apple", "Banana", "Mango", "Orange", "Grapes"]
```

### splice() – Modify array (destructive)
```js
// splice(start, deleteCount, ...itemsToAdd)
// MODIFIES original array!

let fruits = ["Apple", "Banana", "Mango", "Orange"];

// Remove 1 item at index 1
let removed = fruits.splice(1, 1);
console.log(removed);  // ["Banana"]
console.log(fruits);   // ["Apple", "Mango", "Orange"]

// Remove 2 items starting at index 0
fruits.splice(0, 2);
console.log(fruits);   // ["Orange"]

// Reset
fruits = ["Apple", "Banana", "Mango", "Orange"];

// Insert items (0 deletions, just add)
fruits.splice(2, 0, "Kiwi", "Grapes");
console.log(fruits);   // ["Apple", "Banana", "Kiwi", "Grapes", "Mango", "Orange"]

// Replace items
fruits.splice(1, 1, "Blueberry");
console.log(fruits);   // ["Apple", "Blueberry", "Kiwi", "Grapes", "Mango", "Orange"]
```

### slice vs splice
| Feature | slice | splice |
|---|---|---|
| Modifies original | ❌ No | ✅ Yes |
| Returns | New array (copy) | Removed elements |
| Use for | Copying/extracting | Adding/removing/replacing |

---

## sort() – Sort the Array

```js
// Sort strings alphabetically (default behavior)
let fruits = ["Banana", "Apple", "Mango", "Orange"];
fruits.sort();
console.log(fruits);  // ["Apple", "Banana", "Mango", "Orange"]

// ⚠️ WARNING - sort() converts to strings by default!
let numbers = [10, 2, 100, 21, 3];
numbers.sort();
console.log(numbers);   // [10, 100, 2, 21, 3] ❌ Wrong!

// ✅ Correct way - use compare function
numbers.sort((a, b) => a - b);   // Ascending
console.log(numbers);  // [2, 3, 10, 21, 100] ✅

numbers.sort((a, b) => b - a);   // Descending
console.log(numbers);  // [100, 21, 10, 3, 2] ✅

// Sort objects by property
let students = [
  { name: "Priya", marks: 92 },
  { name: "Rahul", marks: 85 },
  { name: "Amit", marks: 78 }
];

// Sort by marks (ascending)
students.sort((a, b) => a.marks - b.marks);
console.log(students);
// Amit: 78, Rahul: 85, Priya: 92

// Sort by name (alphabetically)
students.sort((a, b) => a.name.localeCompare(b.name));
console.log(students);
// Amit, Priya, Rahul
```

---

## flat() – Flatten Nested Arrays

```js
// flat() flattens nested arrays by one level by default
let nested = [1, 2, [3, 4], [5, 6]];
let flat = nested.flat();
console.log(flat);  // [1, 2, 3, 4, 5, 6]

// Deeply nested - specify depth
let deepNested = [1, [2, [3, [4, [5]]]]];
console.log(deepNested.flat());     // [1, 2, [3, [4, [5]]]] - 1 level
console.log(deepNested.flat(2));    // [1, 2, 3, [4, [5]]]   - 2 levels
console.log(deepNested.flat(Infinity)); // [1, 2, 3, 4, 5] - all levels ✅

// flatMap() = map() + flat() combined
let sentences = ["Hello World", "Foo Bar"];
let words = sentences.flatMap(sentence => sentence.split(" "));
console.log(words);  // ["Hello", "World", "Foo", "Bar"]
```

---

## Functional Thinking

> **Functional thinking** means treating operations as **transformations** using pure functions, avoiding mutations.

```js
let students = [
  { name: "Rahul", marks: 85 },
  { name: "Amit", marks: 45 },
  { name: "Priya", marks: 92 },
  { name: "Neha", marks: 38 }
];

// Chaining methods - functional style ✅
let topStudents = students
  .filter(s => s.marks >= 50)          // Step 1: Only passed
  .sort((a, b) => b.marks - a.marks)   // Step 2: Sort by marks
  .map(s => `${s.name}: ${s.marks}`);  // Step 3: Format output

console.log(topStudents);
// ["Priya: 92", "Rahul: 85"]
```

---

## Practice Programs

### 1. Filter Students by Marks
```js
let students = [
  { name: "Rahul", marks: 85, subject: "Math" },
  { name: "Amit", marks: 45, subject: "Math" },
  { name: "Priya", marks: 92, subject: "Science" },
  { name: "Neha", marks: 38, subject: "Math" },
  { name: "Rohan", marks: 75, subject: "Science" }
];

// Students who passed (marks >= 50)
let passed = students.filter(s => s.marks >= 50);
console.log("Passed:", passed.map(s => s.name));  // Rahul, Priya, Rohan

// Top scorers (marks > 80)
let toppers = students.filter(s => s.marks > 80);
console.log("Toppers:", toppers.map(s => s.name)); // Rahul, Priya

// Math students only
let mathStudents = students.filter(s => s.subject === "Math");
console.log("Math:", mathStudents.map(s => s.name));  // Rahul, Amit, Neha
```

### 2. Transform User List
```js
let rawUsers = [
  { first: "rahul", last: "sharma", age: 25 },
  { first: "amit", last: "verma", age: 17 },
  { first: "priya", last: "gupta", age: 30 }
];

// Transform: full name, capitalize, adults only
let processedUsers = rawUsers
  .filter(user => user.age >= 18)    // Only adults
  .map(user => ({
    fullName: `${user.first} ${user.last}`.toUpperCase(),
    age: user.age
  }));

console.log(processedUsers);
// [{fullName: "RAHUL SHARMA", age: 25}, {fullName: "PRIYA GUPTA", age: 30}]
```

### 3. Calculate Total Price
```js
let cart = [
  { item: "Laptop", price: 50000, qty: 1 },
  { item: "Mouse", price: 500, qty: 2 },
  { item: "Keyboard", price: 1500, qty: 1 },
  { item: "Monitor", price: 15000, qty: 2 }
];

let totalPrice = cart.reduce((total, product) => {
  return total + (product.price * product.qty);
}, 0);

console.log("Total Price: ₹" + totalPrice);  // Total Price: ₹82000

// With discount on items > 10000
let discountedTotal = cart.reduce((total, product) => {
  let itemPrice = product.price * product.qty;
  if (product.price > 10000) {
    itemPrice = itemPrice * 0.9;  // 10% discount
  }
  return total + itemPrice;
}, 0);

console.log("Discounted Total: ₹" + discountedTotal);
```

---
---

# 23. Objects

---

## What is an Object?

An **Object** is a collection of **key-value pairs** used to represent a real-world entity.

### Simple Interview Answer:
> Object is a data structure that stores data in key-value pairs. Keys are strings (properties) and values can be anything. Objects model real-world things like a user, product, or car.

```js
// Real world modeling
let person = {
  name: "Rahul",        // key: "name", value: "Rahul"
  age: 25,              // key: "age", value: 25
  isStudent: false,     // key: "isStudent", value: false
  city: "Delhi"         // key: "city", value: "Delhi"
};
```

---

## Key-Value Pairs – Properties and Values

```js
let car = {
  brand: "Toyota",       // String value
  year: 2022,            // Number value
  isElectric: false,     // Boolean value
  colors: ["Red", "Blue", "White"],   // Array value
  engine: {              // Nested object value
    type: "V6",
    horsepower: 300
  },
  start: function() {    // Function value (method)
    console.log("Car started!");
  }
};

console.log(car);
```

---

## Accessing Properties – Dot vs Bracket Notation

```js
let person = {
  name: "Rahul",
  age: 25,
  "home city": "Delhi",   // Key with space must use bracket notation
  1stPrize: "Gold"        // ⚠️ Invalid key name
};

// Dot Notation (most common)
console.log(person.name);   // "Rahul"
console.log(person.age);    // 25

// Bracket Notation (when key has special chars or is dynamic)
console.log(person["name"]);        // "Rahul"
console.log(person["home city"]);   // "Delhi" (key with space)

// Dynamic key access
let key = "age";
console.log(person[key]);           // 25 ✅ Dynamic!
// console.log(person.key);         // undefined ❌ Looks for key "key"
```

---

## Adding / Deleting Properties

```js
let user = {
  name: "Rahul",
  age: 25
};

// Add new property
user.email = "rahul@gmail.com";
user["phone"] = "9876543210";
console.log(user);
// { name: "Rahul", age: 25, email: "...", phone: "..." }

// Update existing property
user.age = 26;
console.log(user.age);   // 26

// Delete property
delete user.phone;
console.log(user.phone);  // undefined

// Check if property exists
console.log("name" in user);    // true
console.log("phone" in user);   // false
console.log(user.hasOwnProperty("email"));  // true
```

---

## Nested Objects

```js
let user = {
  name: "Rahul",
  age: 25,
  address: {
    street: "MG Road",
    city: "Delhi",
    pincode: "110001",
    country: {
      name: "India",
      code: "IN"
    }
  },
  skills: ["JavaScript", "React", "Node.js"]
};

// Access nested properties
console.log(user.address.city);              // "Delhi"
console.log(user.address.country.name);     // "India"
console.log(user.skills[0]);                 // "JavaScript"

// Modify nested property
user.address.city = "Mumbai";
console.log(user.address.city);   // "Mumbai"

// Optional Chaining (?.) - safe access (ES2020)
// Prevents error if property doesn't exist
console.log(user?.address?.city);         // "Mumbai"
console.log(user?.contact?.phone);        // undefined (no error!)
// console.log(user.contact.phone);       // ❌ TypeError!
```

---

## Looping Through Keys

### for...in
```js
let person = {
  name: "Rahul",
  age: 25,
  city: "Delhi"
};

// Loop through all keys
for (let key in person) {
  console.log(key, ":", person[key]);
}
// name : Rahul
// age : 25
// city : Delhi
```

### Object.keys() / Object.values() / Object.entries()
```js
let person = { name: "Rahul", age: 25, city: "Delhi" };

// Object.keys() - array of keys
let keys = Object.keys(person);
console.log(keys);   // ["name", "age", "city"]

// Object.values() - array of values
let values = Object.values(person);
console.log(values);  // ["Rahul", 25, "Delhi"]

// Object.entries() - array of [key, value] pairs
let entries = Object.entries(person);
console.log(entries);
// [["name", "Rahul"], ["age", 25], ["city", "Delhi"]]

// Loop with entries
for (let [key, value] of Object.entries(person)) {
  console.log(`${key}: ${value}`);
}
```

---

## Practice Programs

### 1. User Profile System
```js
let userProfile = {
  id: 1,
  name: "Rahul Sharma",
  email: "rahul@gmail.com",
  age: 25,
  skills: ["JavaScript", "React"],
  address: {
    city: "Delhi",
    country: "India"
  },

  // Methods inside object
  getInfo: function() {
    return `${this.name} (${this.age}) from ${this.address.city}`;
  },

  addSkill: function(skill) {
    this.skills.push(skill);
  }
};

console.log(userProfile.getInfo());
// "Rahul Sharma (25) from Delhi"

userProfile.addSkill("Node.js");
console.log(userProfile.skills);
// ["JavaScript", "React", "Node.js"]
```

### 2. Cart Object
```js
let cart = {
  items: [],
  totalItems: 0,
  totalPrice: 0,

  addItem: function(product, qty) {
    this.items.push({ product, qty });
    this.totalItems += qty;
    this.totalPrice += product.price * qty;
    console.log(`Added ${product.name} x${qty}`);
  },

  removeItem: function(productName) {
    let index = this.items.findIndex(i => i.product.name === productName);
    if (index !== -1) {
      let item = this.items[index];
      this.totalPrice -= item.product.price * item.qty;
      this.totalItems -= item.qty;
      this.items.splice(index, 1);
    }
  },

  showCart: function() {
    console.log("--- Cart ---");
    this.items.forEach(i => {
      console.log(`${i.product.name} x${i.qty} = ₹${i.product.price * i.qty}`);
    });
    console.log(`Total: ₹${this.totalPrice}`);
  }
};

cart.addItem({ name: "Laptop", price: 50000 }, 1);
cart.addItem({ name: "Mouse", price: 500 }, 2);
cart.showCart();
```

---
---

# 24. Object Methods

---

## Object.entries()

Converts object to array of **[key, value]** pairs.

```js
let person = { name: "Rahul", age: 25, city: "Delhi" };

let entries = Object.entries(person);
console.log(entries);
// [["name", "Rahul"], ["age", 25], ["city", "Delhi"]]

// Use case - loop with destructuring
for (let [key, value] of Object.entries(person)) {
  console.log(`${key} → ${value}`);
}

// Use case - transform object values
let prices = { apple: 50, banana: 20, mango: 80 };
let discounted = Object.entries(prices).map(([item, price]) => [item, price * 0.9]);
console.log(discounted);
// [["apple", 45], ["banana", 18], ["mango", 72]]
```

---

## Object.assign()

**Copies properties** from one or more source objects into a target object.

```js
let target = { name: "Rahul" };
let source = { age: 25, city: "Delhi" };

Object.assign(target, source);
console.log(target);  // { name: "Rahul", age: 25, city: "Delhi" }

// Merge multiple objects
let obj1 = { a: 1 };
let obj2 = { b: 2 };
let obj3 = { c: 3 };

let merged = Object.assign({}, obj1, obj2, obj3);
console.log(merged);  // { a: 1, b: 2, c: 3 }

// ⚠️ If keys overlap, later source OVERWRITES earlier
let base = { theme: "light", lang: "en" };
let override = { theme: "dark" };

let config = Object.assign({}, base, override);
console.log(config);  // { theme: "dark", lang: "en" }

// ⚠️ Shallow copy only (nested objects are still referenced)
let user = { name: "Rahul", address: { city: "Delhi" } };
let copy = Object.assign({}, user);
copy.address.city = "Mumbai";
console.log(user.address.city);  // "Mumbai" ← Original also changed!
```

---

## Object.freeze()

Makes object **completely immutable** – no add, delete, or update.

```js
let config = {
  apiUrl: "https://api.example.com",
  maxRetries: 3,
  timeout: 5000
};

Object.freeze(config);

// All these are silently ignored (no error in non-strict mode)
config.apiUrl = "https://new.com";   // ❌ Won't change
config.newProp = "test";             // ❌ Won't add
delete config.timeout;               // ❌ Won't delete

console.log(config.apiUrl);    // "https://api.example.com" (unchanged!)
console.log(Object.isFrozen(config));  // true
```

---

## Object.fromEntries()

Converts array of **[key, value]** pairs back into an object.

```js
// Array of pairs → Object
let entries = [["name", "Rahul"], ["age", 25], ["city", "Delhi"]];
let person = Object.fromEntries(entries);
console.log(person);  // { name: "Rahul", age: 25, city: "Delhi" }

// Use case - transform object via entries → modify → back to object
let prices = { apple: 50, banana: 20, mango: 80 };

// Apply 10% discount to all prices
let discounted = Object.fromEntries(
  Object.entries(prices).map(([item, price]) => [item, price * 0.9])
);
console.log(discounted);  // { apple: 45, banana: 18, mango: 72 }

// Convert Map to Object
let map = new Map([["a", 1], ["b", 2]]);
let obj = Object.fromEntries(map);
console.log(obj);  // { a: 1, b: 2 }
```

---

## Object.is()

**Safe equality comparison** – handles edge cases that `===` doesn't.

```js
// Regular === issues
console.log(NaN === NaN);    // false ❌ (NaN is never equal to itself with ===)
console.log(+0 === -0);      // true  ❌ (they are actually different)

// Object.is() fixes this
console.log(Object.is(NaN, NaN));   // true  ✅
console.log(Object.is(+0, -0));     // false ✅

// For regular values, same as ===
console.log(Object.is(5, 5));       // true
console.log(Object.is("hi", "hi")); // true
console.log(Object.is(5, "5"));     // false

// Use case - safe NaN check
function safeCheck(val) {
  if (Object.is(val, NaN)) {
    return "Value is NaN";
  }
  return val;
}
```

---

## Object.keys()

Returns array of **all keys** of an object.

```js
let person = { name: "Rahul", age: 25, city: "Delhi" };

let keys = Object.keys(person);
console.log(keys);   // ["name", "age", "city"]

// Count properties
console.log(Object.keys(person).length);  // 3

// Check if object is empty
let emptyObj = {};
console.log(Object.keys(emptyObj).length === 0);  // true

// Loop through keys
Object.keys(person).forEach(key => {
  console.log(`${key}: ${person[key]}`);
});
```

---

## Object.seal()

**Prevents adding or deleting** properties. Values of existing properties **can still be changed**.

```js
let user = {
  name: "Rahul",
  age: 25
};

Object.seal(user);

user.age = 26;            // ✅ Can UPDATE existing
// user.email = "...";   // ❌ Cannot ADD new property
// delete user.name;     // ❌ Cannot DELETE property

console.log(user);  // { name: "Rahul", age: 26 }
console.log(Object.isSealed(user));  // true
```

### Object.seal() vs Object.freeze()

| Feature | Object.seal() | Object.freeze() |
|---|---|---|
| Add properties | ❌ No | ❌ No |
| Delete properties | ❌ No | ❌ No |
| Update values | ✅ Yes | ❌ No |
| Use case | Protect structure | Full immutability |

---

## Object.values()

Returns array of **all values** of an object.

```js
let person = { name: "Rahul", age: 25, city: "Delhi" };

let values = Object.values(person);
console.log(values);  // ["Rahul", 25, "Delhi"]

// Sum all values (if numeric)
let scores = { math: 85, science: 90, english: 78 };
let total = Object.values(scores).reduce((sum, score) => sum + score, 0);
console.log("Total:", total);   // 253

let avg = total / Object.values(scores).length;
console.log("Average:", avg);  // 84.33
```

---

## All Object Methods – Quick Reference

```js
let obj = { name: "Rahul", age: 25 };

Object.keys(obj)           // → ["name", "age"]
Object.values(obj)         // → ["Rahul", 25]
Object.entries(obj)        // → [["name","Rahul"],["age",25]]
Object.fromEntries(entries)// → { name: "Rahul", age: 25 }
Object.assign({}, obj)     // → Shallow copy
Object.freeze(obj)         // → No add/delete/update
Object.seal(obj)           // → No add/delete (update OK)
Object.is(NaN, NaN)        // → true (safe equality)
```

---
---

# 25. TypeScript Essentials

---

## What is TypeScript & Why It Exists?

**TypeScript** is a **superset of JavaScript** that adds **static type checking**. TypeScript code compiles down to regular JavaScript.

### Simple Interview Answer:
> TypeScript is JavaScript with types. It helps catch errors during development (before running code) by letting us define what type of data a variable, parameter, or function should hold. It makes large codebases more maintainable and predictable.

```
JavaScript → Dynamic types (checked at runtime)
TypeScript → Static types (checked at compile time) ✅
```

### Why TypeScript?
```
Problems in JavaScript:
let age = 25;
age = "twenty five";  // No error! But causes bugs later

TypeScript Solution:
let age: number = 25;
age = "twenty five";  // ❌ Error caught immediately!
```

---

## Installing and Setting Up TypeScript

```bash
# Step 1 - Install TypeScript globally
npm install -g typescript

# Verify installation
tsc --version   # Shows version like "Version 5.0.0"

# Step 2 - Create a TypeScript file
# Create file: index.ts

# Step 3 - Compile TypeScript to JavaScript
tsc index.ts    # Creates index.js

# Step 4 - Run compiled JS
node index.js

# OR - Initialize TypeScript project
tsc --init      # Creates tsconfig.json
```

---

## tsconfig.json Explained

```json
{
  "compilerOptions": {
    "target": "ES6",          // Compile to ES6 JavaScript
    "module": "commonjs",     // Module system (commonjs for Node)
    "outDir": "./dist",       // Output compiled JS here
    "rootDir": "./src",       // TS source files are here
    "strict": true,           // Enable all strict type checks ✅
    "noImplicitAny": true,    // Error when 'any' type is implicit
    "esModuleInterop": true   // Better module compatibility
  },
  "include": ["src/**/*"],    // Compile these files
  "exclude": ["node_modules"] // Ignore these
}
```

### TypeScript Compiler (tsc)
```bash
# Compile single file
tsc index.ts

# Compile with watch mode (auto-recompile on save)
tsc --watch

# Compile entire project (uses tsconfig.json)
tsc

# Compile and watch project
tsc -w
```

---

## Basic Types

```ts
// String
let name: string = "Rahul";
let greeting: string = `Hello ${name}`;

// Number
let age: number = 25;
let price: number = 99.99;

// Boolean
let isLoggedIn: boolean = true;
let isAdmin: boolean = false;

// Array
let fruits: string[] = ["Apple", "Banana"];      // String array
let scores: number[] = [85, 90, 78];             // Number array
let flags: boolean[] = [true, false, true];
let mixed: Array<string | number> = ["hi", 1];  // Generic syntax

// Tuple - Fixed length, fixed types
let person: [string, number] = ["Rahul", 25];
console.log(person[0]);  // "Rahul"
console.log(person[1]);  // 25

// any - Opt out of type checking (avoid using!)
let data: any = "hello";
data = 42;     // ✅ No error (defeats TypeScript purpose)
data = true;   // ✅ No error

// unknown - Safer than any (must check type before using)
let value: unknown = "hello";
// value.toUpperCase();  // ❌ Error! Must check type first
if (typeof value === "string") {
  console.log(value.toUpperCase());  // ✅ Safe!
}

// void - Function returns nothing
function logMessage(msg: string): void {
  console.log(msg);
  // no return statement
}

// never - Function never returns (throws or infinite loop)
function throwError(message: string): never {
  throw new Error(message);
}

// null and undefined
let nothing: null = null;
let notDefined: undefined = undefined;
```

---

## Interfaces vs Type Aliases

### Interface
```ts
// Interface - defines shape of an object
interface User {
  name: string;
  age: number;
  email?: string;   // Optional property (?)
  readonly id: number;  // Cannot be changed after creation
}

let user: User = {
  name: "Rahul",
  age: 25,
  id: 1
};

// user.id = 2;  // ❌ Error! readonly

// Interface for function
interface GreetFn {
  (name: string, age: number): string;
}

let greet: GreetFn = (name, age) => `Hello ${name}, age ${age}`;

// Extending Interface
interface Admin extends User {
  role: string;
  permissions: string[];
}

let admin: Admin = {
  name: "Amit",
  age: 30,
  id: 2,
  role: "superadmin",
  permissions: ["read", "write", "delete"]
};
```

### Type Alias
```ts
// Type alias - can define any type, not just objects
type ID = string | number;   // Union type
type Status = "active" | "inactive" | "pending";  // Literal type

type Point = {
  x: number;
  y: number;
};

let point: Point = { x: 10, y: 20 };

// Function type alias
type AddFn = (a: number, b: number) => number;
let add: AddFn = (a, b) => a + b;
```

### Interface vs Type Alias

| Feature | Interface | Type Alias |
|---|---|---|
| Objects | ✅ Yes | ✅ Yes |
| Primitives/Unions | ❌ No | ✅ Yes |
| Extend/Merge | ✅ Yes (extends) | ✅ Yes (& intersection) |
| Declaration Merging | ✅ Yes | ❌ No |
| Use when | Defining object shapes | Unions, primitives, complex types |

> **Interview Tip:** Use `interface` for object shapes (especially in OOP). Use `type` for unions, primitives, or complex types.

---

## Union & Intersection Types

### Union Type (|) – Either this OR that
```ts
// Variable can be one of multiple types
let id: string | number;
id = "user_123";   // ✅ String
id = 456;          // ✅ Number
// id = true;      // ❌ Error

// Function accepting multiple types
function formatId(id: string | number): string {
  if (typeof id === "string") {
    return id.toUpperCase();
  }
  return id.toString();
}

console.log(formatId("abc"));   // "ABC"
console.log(formatId(123));     // "123"

// Literal union (restrict to specific values)
type Direction = "left" | "right" | "up" | "down";
let move: Direction = "left";   // ✅
// move = "diagonal";           // ❌ Error!
```

### Intersection Type (&) – Both types combined
```ts
// Combines multiple types into one
type Employee = {
  name: string;
  company: string;
};

type Developer = {
  language: string;
  experience: number;
};

// DevEmployee must have ALL properties from both
type DevEmployee = Employee & Developer;

let dev: DevEmployee = {
  name: "Rahul",
  company: "TechCorp",
  language: "TypeScript",
  experience: 3
};
```

---

## Generic Functions

**Generics** allow writing **reusable functions** that work with **any type** while maintaining type safety.

```ts
// Without generics - only works for one type
function getFirstNumber(arr: number[]): number {
  return arr[0];
}

// With generics - works for any type ✅
function getFirst<T>(arr: T[]): T {
  return arr[0];
}

console.log(getFirst([1, 2, 3]));          // 1 (type: number)
console.log(getFirst(["a", "b", "c"]));    // "a" (type: string)
console.log(getFirst([true, false]));       // true (type: boolean)

// Generic function with multiple types
function pair<T, U>(first: T, second: U): [T, U] {
  return [first, second];
}

let result = pair("hello", 42);    // ["hello", 42]
let result2 = pair(true, "yes");   // [true, "yes"]
```

---

## Generic Interfaces & Constraints

### Generic Interface
```ts
// Interface with generic type
interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}

// Use with specific types
interface User {
  name: string;
  age: number;
}

let userResponse: ApiResponse<User> = {
  data: { name: "Rahul", age: 25 },
  status: 200,
  message: "Success"
};

let listResponse: ApiResponse<string[]> = {
  data: ["Apple", "Banana"],
  status: 200,
  message: "Success"
};
```

### Generic Constraints (extends)
```ts
// Constraint: T must have a 'length' property
function logLength<T extends { length: number }>(item: T): void {
  console.log("Length:", item.length);
}

logLength("hello");       // ✅ String has length → 5
logLength([1, 2, 3]);     // ✅ Array has length → 3
// logLength(42);         // ❌ Error! Number has no length

// Constraint: Key must exist in object
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

let user = { name: "Rahul", age: 25 };
console.log(getProperty(user, "name"));   // "Rahul" ✅
// getProperty(user, "email");            // ❌ Error! "email" not in user
```

---
---

# Additional Topics

---

## Throttling & Debouncing

---

### Debouncing

**Debouncing** delays function execution until the user **stops doing something** for a set time.

> Real World: Search bar – don't search on every keystroke, wait until user stops typing.

```js
// Without debouncing - fires on every keystroke!
searchInput.addEventListener("input", function() {
  fetchResults(this.value);  // ❌ API call on EVERY keystroke
});

// Debounce function
function debounce(fn, delay) {
  let timeoutId;

  return function(...args) {
    // Clear previous timer
    clearTimeout(timeoutId);

    // Set new timer
    timeoutId = setTimeout(() => {
      fn(...args);   // Call original function after delay
    }, delay);
  };
}

// Usage
function fetchResults(query) {
  console.log("Fetching:", query);   // Only called when user stops typing
}

let debouncedFetch = debounce(fetchResults, 500);   // Wait 500ms

searchInput.addEventListener("input", function() {
  debouncedFetch(this.value);   // ✅ Waits 500ms after last keystroke
});
```

---

### Throttling

**Throttling** ensures a function is called **at most once** in a given time period, no matter how many times the event fires.

> Real World: Scroll event – don't run heavy code on every scroll pixel, limit to once per 200ms.

```js
// Throttle function
function throttle(fn, limit) {
  let lastCall = 0;

  return function(...args) {
    let now = Date.now();

    // Only call if enough time has passed
    if (now - lastCall >= limit) {
      lastCall = now;
      fn(...args);   // Call original function
    }
  };
}

// Usage
function handleScroll() {
  console.log("Scroll handled at:", Date.now());
}

let throttledScroll = throttle(handleScroll, 200);   // Max once per 200ms

window.addEventListener("scroll", throttledScroll);  // ✅ Limited calls
```

### Debounce vs Throttle

| Feature | Debounce | Throttle |
|---|---|---|
| When it runs | After user STOPS | During action at regular intervals |
| Good for | Search input, form validation | Scroll, resize, mousemove |
| Calls function | Once after delay | At most once per interval |

---

## JSON Handling in JavaScript

**JSON (JavaScript Object Notation)** is a lightweight format for **storing and exchanging data** between server and browser.

---

### JSON.stringify() – Convert Object to JSON String

```js
// Convert JavaScript Object → JSON String (for sending to server)

let user = {
  name: "Rahul",
  age: 25,
  skills: ["JavaScript", "React"],
  address: { city: "Delhi" }
};

let jsonString = JSON.stringify(user);
console.log(jsonString);
// '{"name":"Rahul","age":25,"skills":["JavaScript","React"],"address":{"city":"Delhi"}}'

console.log(typeof jsonString);  // "string"

// Pretty print with indentation
let pretty = JSON.stringify(user, null, 2);
console.log(pretty);
// {
//   "name": "Rahul",
//   "age": 25,
//   ...
// }

// Store in localStorage (localStorage only accepts strings)
localStorage.setItem("user", JSON.stringify(user));

// What JSON.stringify ignores:
let data = {
  name: "Rahul",
  fn: function() {},     // ❌ Functions are ignored
  sym: Symbol("id"),     // ❌ Symbols are ignored
  undef: undefined       // ❌ Undefined values are ignored
};
console.log(JSON.stringify(data));
// '{"name":"Rahul"}' - only name remains!
```

---

### JSON.parse() – Convert JSON String to Object

```js
// Convert JSON String → JavaScript Object (data received from server)

let jsonString = '{"name":"Rahul","age":25,"skills":["JavaScript","React"]}';

let user = JSON.parse(jsonString);
console.log(user);         // JavaScript Object
console.log(user.name);    // "Rahul"
console.log(user.skills);  // ["JavaScript", "React"]

console.log(typeof user);  // "object"

// Get from localStorage and parse
let storedUser = JSON.parse(localStorage.getItem("user"));
console.log(storedUser.name);  // "Rahul"

// Error handling - invalid JSON
try {
  let bad = JSON.parse("invalid json {{{");
} catch (error) {
  console.log("Invalid JSON:", error.message);  // Catches parse error
}
```

### Real World – Fetch API with JSON
```js
async function getUser() {
  let response = await fetch("https://jsonplaceholder.typicode.com/users/1");

  // response.json() internally calls JSON.parse() for us
  let user = await response.json();
  console.log(user.name);

  // Sending data to server
  let newUser = { name: "Rahul", email: "rahul@gmail.com" };

  let postResponse = await fetch("https://api.example.com/users", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(newUser)   // Convert to JSON string before sending
  });

  let result = await postResponse.json();
  console.log(result);
}
```

### JSON Rules to Remember
```js
// ✅ Valid JSON
// - Keys must be in DOUBLE quotes
// - String values must be in DOUBLE quotes
// - No trailing commas
// - No comments
// - No functions or undefined

// ❌ Invalid JSON
let bad = '{ name: "Rahul" }';        // Key not in quotes
let bad2 = '{ "name": "Rahul", }';   // Trailing comma
```

---

## Final Quick Summary

```
21. Arrays
    ├── push/pop → End | shift/unshift → Start
    ├── Access by index (starts at 0)
    ├── length → Count items
    └── 2D Array → Array inside array [row][col]

22. Array Methods
    ├── map()     → Transform → New array (same length)
    ├── filter()  → Select    → New array (shorter)
    ├── reduce()  → Accumulate → Single value
    ├── forEach() → Loop, no return
    ├── some()    → At least one passes?
    ├── every()   → All pass?
    ├── slice()   → Copy (non-destructive)
    ├── splice()  → Modify (destructive)
    ├── sort()    → Sort (use compare fn for numbers!)
    └── flat()    → Flatten nested arrays

23. Objects
    ├── Key-value pairs { key: value }
    ├── Dot notation (obj.key) vs Bracket (obj["key"])
    ├── Add: obj.newKey = val | Delete: delete obj.key
    ├── Nested: obj.nested.deep
    └── Loop: for...in | Object.keys()

24. Object Methods
    ├── Object.keys()        → Array of keys
    ├── Object.values()      → Array of values
    ├── Object.entries()     → Array of [key, value]
    ├── Object.fromEntries() → Object from [key, value]
    ├── Object.assign()      → Merge/copy objects
    ├── Object.freeze()      → Full immutability
    ├── Object.seal()        → No add/delete (update OK)
    └── Object.is()          → Safe equality (NaN safe)

25. TypeScript
    ├── Superset of JS with static types
    ├── Types: string, number, boolean, any, unknown
    ├── Interface → Object shapes | Type → Anything
    ├── Union (|) → OR | Intersection (&) → AND
    └── Generics <T> → Reusable + type-safe

Extras
    ├── Debounce  → Wait for user to stop (search input)
    ├── Throttle  → Limit calls per time (scroll)
    ├── JSON.stringify() → Object → String (send to server)
    └── JSON.parse()     → String → Object (receive from server)
```

---

> 💡 **Interview Tips:**
> - `sort()` without compare function sorts as **strings**, always pass `(a,b) => a-b` for numbers
> - `slice` doesn't modify original, `splice` does – remember: **splice = splice open = modifies**
> - TypeScript `interface` can be **extended and merged**, `type` cannot be re-declared
> - **Debounce** = waiting, **Throttle** = limiting
> - `JSON.stringify()` **ignores functions, symbols, and undefined**
> - `Object.freeze()` is **shallow** – nested objects can still be modified