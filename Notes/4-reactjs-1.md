# React Complete Notes – Interview Ready ⚛️

---

# 1. Introduction to React – Why Modern UI Needs It

---

## Why React Exists – Problems with Manual DOM Manipulation

In **plain JavaScript**, when you want to update the UI, you manually find DOM elements and change them.

### Problems with Manual DOM in Large Apps:

```
❌ Code becomes messy and hard to maintain
❌ Manually tracking which data changed and which UI to update
❌ Performance issues – updating entire DOM is slow
❌ Bugs increase as app grows
❌ No structure – everything is scattered
```

### Example – Manual DOM (Old Way)

```js
// Updating a counter manually
let count = 0;
let display = document.getElementById("count");
let btn = document.getElementById("btn");

btn.addEventListener("click", function () {
  count++;
  display.textContent = count; // Manually updating DOM
});
```

### React Solution

```jsx
// Same counter in React – clean & automatic
function Counter() {
  const [count, setCount] = useState(0);

  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
}
```

### Simple Interview Answer:

> React exists because manually manipulating the DOM in large applications becomes slow, buggy, and hard to maintain. React provides a **component-based, declarative** approach where you describe **what the UI should look like** based on data, and React handles the DOM updates automatically and efficiently.

---

## Declarative vs Imperative UI

### Imperative (How to do it – Step by Step)

```js
// You tell the browser EXACTLY how to update the DOM
let list = document.getElementById("list");
let item = document.createElement("li");
item.textContent = "Apple";
item.style.color = "red";
list.appendChild(item);
```

### Declarative (What to show – Describe the result)

```jsx
// You tell React WHAT you want, React figures out HOW
function FruitList() {
  const fruits = ["Apple", "Banana", "Mango"];

  return (
    <ul>
      {fruits.map((fruit) => (
        <li key={fruit} style={{ color: "red" }}>
          {fruit}
        </li>
      ))}
    </ul>
  );
}
```

### Comparison

| Feature     | Imperative         | Declarative        |
| ----------- | ------------------ | ------------------ |
| Focus       | HOW to do it       | WHAT to show       |
| Code        | Long, step-by-step | Short, descriptive |
| DOM Updates | Manual             | Automatic          |
| Debugging   | Hard               | Easy               |
| Example     | Plain JS DOM       | React, Vue         |

> **Interview Tip:** React is **declarative**. You describe the UI based on current state, and React takes care of updating the DOM.

---

## What is SPA (Single Page Application)?

**SPA** is a web application that loads **one single HTML page** and dynamically updates content **without reloading the entire page**.

### How It Works:

```
Traditional Website (MPA - Multi Page):
Click Link → Server sends NEW HTML → Full page reload → Slow ❌

Single Page App (SPA):
Click Link → JS fetches data → React updates only changed part → Fast ✅
```

### Simple Interview Answer:

> SPA is a web application that loads a single HTML page initially. After that, all navigation and content changes happen dynamically using JavaScript without full page reloads. This makes the app feel faster and smoother, like a mobile app. React, Angular, and Vue are used to build SPAs.

### Benefits of SPA:

```
✅ Faster navigation (no full reload)
✅ Better user experience (feels like app)
✅ Less server load (only data is fetched, not HTML)
✅ Smooth transitions and animations
```

---

## Real DOM vs Virtual DOM

### Real DOM

- The **actual HTML structure** in the browser
- Updating it is **slow and expensive**
- Every change causes the browser to **recalculate layout, paint, etc.**

### Virtual DOM

- A **lightweight JavaScript copy** of the Real DOM
- React creates a Virtual DOM tree **in memory**
- When state changes, React:
  1. Creates a **new Virtual DOM** tree
  2. **Compares** it with the old Virtual DOM (this process is called **Diffing**)
  3. Finds the **minimum changes** needed
  4. Updates **only those parts** in the Real DOM (this is called **Reconciliation**)

### Visual Flow:

```
State Changes
    ↓
New Virtual DOM created
    ↓
Compare Old Virtual DOM vs New Virtual DOM (Diffing)
    ↓
Find minimum changes
    ↓
Update ONLY changed parts in Real DOM (Reconciliation)
    ↓
UI Updated! ✅
```

### Simple Interview Answer:

> Virtual DOM is a lightweight JavaScript representation of the Real DOM. When state changes, React creates a new Virtual DOM, compares it with the previous one using a diffing algorithm, and then updates only the changed elements in the Real DOM. This process is called Reconciliation and it makes React very fast because it avoids unnecessary Real DOM updates.

---

## Setting Up React with Vite

### Why Vite?

```
Vite vs Create React App (CRA):
├── Vite is MUCH faster (uses native ES modules)
├── Instant dev server startup
├── Faster hot module replacement (HMR)
├── Smaller bundle size
└── CRA is outdated and no longer recommended
```

### Installation Steps:

```bash
# Step 1 - Create React project with Vite
npm create vite@latest my-react-app -- --template react

# Step 2 - Go into project folder
cd my-react-app

# Step 3 - Install dependencies
npm install

# Step 4 - Start development server
npm run dev

# Output: Local server running at http://localhost:5173
```

---

## Project Structure Breakdown

```
my-react-app/
├── node_modules/        # All installed packages (don't touch!)
├── public/              # Static files (favicon, images that don't change)
│   └── vite.svg
├── src/                 # Main source code (where you write React)
│   ├── assets/          # Images, fonts, CSS files
│   ├── components/      # Reusable React components (you create this)
│   │   ├── Header.jsx
│   │   ├── Card.jsx
│   │   └── Footer.jsx
│   ├── App.jsx          # Root component (main layout)
│   ├── App.css          # Styles for App component
│   ├── main.jsx         # Entry point (renders App to DOM)
│   ├── index.css        # Global styles
│   └── ...
├── index.html           # Single HTML file (SPA entry point)
├── package.json         # Project info, scripts, dependencies
├── vite.config.js       # Vite configuration
└── README.md
```

### Key Files Explained:

```js
// main.jsx - Entry point of React app
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App /> {/* Root component rendered here */}
  </React.StrictMode>,
);
```

```jsx
// App.jsx - Root Component
function App() {
  return (
    <div>
      <h1>Hello React!</h1>
    </div>
  );
}
export default App;
```

```html
<!-- index.html - Only HTML file in SPA -->
<!DOCTYPE html>
<html>
  <body>
    <div id="root"></div>
    <!-- React renders everything inside this -->
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

---

## JSX Syntax Rules

**JSX** stands for **JavaScript XML**. It allows writing **HTML-like code inside JavaScript**.

### Rules of JSX:

```jsx
// Rule 1: Return ONE parent element (wrap everything)
// ❌ Wrong - two sibling elements
function App() {
  return (
    <h1>Hello</h1>
    <p>World</p>
  );
}

// ✅ Correct - wrap in div
function App() {
  return (
    <div>
      <h1>Hello</h1>
      <p>World</p>
    </div>
  );
}

// ✅ Correct - use Fragment (no extra div in DOM)
function App() {
  return (
    <>
      <h1>Hello</h1>
      <p>World</p>
    </>
  );
}
```

```jsx
// Rule 2: Close ALL tags (even self-closing)
// ❌ Wrong
<img src="photo.jpg">
<br>
<input type="text">

// ✅ Correct
<img src="photo.jpg" />
<br />
<input type="text" />
```

```jsx
// Rule 3: Use camelCase for HTML attributes
// ❌ Wrong
<div class="container" onclick="handleClick()">

// ✅ Correct
<div className="container" onClick={handleClick}>
```

```jsx
// Rule 4: Embed JavaScript using curly braces {}
function App() {
  const name = "Rahul";
  const age = 25;
  const isLoggedIn = true;

  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>Age: {age + 1}</p>
      <p>{isLoggedIn ? "Welcome back!" : "Please login"}</p>
      <img src={"photo.jpg"} alt={name} />
    </div>
  );
}
```

```jsx
// Rule 5: Inline styles use double curly braces (object syntax)
function App() {
  return (
    <h1 style={{ color: "red", fontSize: "24px", backgroundColor: "yellow" }}>
      Styled Text
    </h1>
  );
}
```

---

## Component-Based Architecture

**Component-Based Architecture** means breaking the entire UI into **small, reusable, independent pieces** called components.

### Visual Example:

```
Full Page (App)
├── Header Component
│   ├── Logo Component
│   └── Navbar Component
├── Main Content
│   ├── HeroSection Component
│   ├── ProductCard Component (reused many times!)
│   └── ProductCard Component
└── Footer Component
```

### Why Component-Based?

```
✅ Reusability – Write once, use everywhere
✅ Maintainability – Fix one component, fixes everywhere
✅ Separation of Concerns – Each component handles its own logic
✅ Testability – Test components individually
✅ Scalability – Easy to add new features
```

### Simple Interview Answer:

> Component-based architecture means breaking the UI into small, independent, reusable pieces called components. Each component manages its own structure, style, and behavior. This makes the code easier to maintain, test, and scale. Think of it like building with LEGO blocks – each block is a component, and you combine them to build the full UI.

---

---

# 2. React Components & Props

---

## Functional Components

A **Functional Component** is simply a **JavaScript function that returns JSX** (UI).

```jsx
// Basic Functional Component
function Welcome() {
  return <h1>Hello, Welcome to React!</h1>;
}

// Arrow Function Component (also common)
const Welcome = () => {
  return <h1>Hello, Welcome to React!</h1>;
};

// Shortest form (implicit return)
const Welcome = () => <h1>Hello, Welcome to React!</h1>;

// Using the component
function App() {
  return (
    <div>
      <Welcome /> {/* Use component like HTML tag */}
      <Welcome /> {/* Reuse as many times as you want */}
    </div>
  );
}
```

### Rules for Components:

```
1. Component name MUST start with Capital Letter (Welcome, not welcome)
2. Must return JSX (HTML-like code)
3. One file = One component (best practice)
4. Export the component to use in other files
```

---

## Understanding Props

**Props** (short for **Properties**) are used to **pass data from parent component to child component**.

> Props are like **function arguments** but for components. They are **read-only** (cannot be changed by child).

### Simple Interview Answer:

> Props are a way to pass data from a parent component to a child component in React. They are read-only, meaning the child component cannot modify them. Props make components reusable because you can pass different data to the same component.

```jsx
// Child Component - receives props
function Greeting(props) {
  return (
    <h1>
      Hello, {props.name}! You are {props.age} years old.
    </h1>
  );
}

// Parent Component - passes props
function App() {
  return (
    <div>
      <Greeting name="Rahul" age={25} />
      <Greeting name="Amit" age={30} />
      <Greeting name="Priya" age={22} />
    </div>
  );
}
```

### Destructuring Props (Cleaner Way ✅)

```jsx
// Instead of props.name, props.age → destructure directly
function Greeting({ name, age, city }) {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>
        Age: {age}, City: {city}
      </p>
    </div>
  );
}

function App() {
  return <Greeting name="Rahul" age={25} city="Delhi" />;
}
```

### Passing Different Types of Props

```jsx
function UserProfile({ name, age, isOnline, hobbies, address }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <p>Status: {isOnline ? "🟢 Online" : "🔴 Offline"}</p>
      <p>Hobbies: {hobbies.join(", ")}</p>
      <p>City: {address.city}</p>
    </div>
  );
}

function App() {
  return (
    <UserProfile
      name="Rahul"
      age={25}
      isOnline={true}
      hobbies={["Coding", "Gaming", "Reading"]}
      address={{ city: "Delhi", country: "India" }}
    />
  );
}
```

### Default Props

```jsx
function Button({ text = "Click Me", color = "blue" }) {
  return <button style={{ backgroundColor: color }}>{text}</button>;
}

function App() {
  return (
    <div>
      <Button /> {/* Uses defaults: "Click Me", "blue" */}
      <Button text="Submit" color="green" /> {/* Overrides defaults */}
    </div>
  );
}
```

---

## Dynamic Rendering

Rendering **different UI** based on **props data**.

```jsx
function StatusBadge({ status }) {
  let color;

  if (status === "active") color = "green";
  else if (status === "inactive") color = "red";
  else color = "gray";

  return (
    <span style={{ color: color, fontWeight: "bold" }}>
      ● {status.toUpperCase()}
    </span>
  );
}

function App() {
  return (
    <div>
      <StatusBadge status="active" /> {/* 🟢 ACTIVE */}
      <StatusBadge status="inactive" /> {/* 🔴 INACTIVE */}
      <StatusBadge status="pending" /> {/* ⚪ PENDING */}
    </div>
  );
}
```

---

## Rendering Lists – Using map()

```jsx
function FruitList() {
  const fruits = ["Apple", "Banana", "Mango", "Orange"];

  return (
    <ul>
      {fruits.map((fruit, index) => (
        <li key={index}>{fruit}</li>
      ))}
    </ul>
  );
}
```

### Rendering List of Objects (Real World)

```jsx
function StudentList() {
  const students = [
    { id: 1, name: "Rahul", marks: 85 },
    { id: 2, name: "Amit", marks: 92 },
    { id: 3, name: "Priya", marks: 78 },
  ];

  return (
    <div>
      <h2>Student List</h2>
      {students.map((student) => (
        <div key={student.id} className="student-card">
          <h3>{student.name}</h3>
          <p>Marks: {student.marks}</p>
          <p>Grade: {student.marks >= 80 ? "A" : "B"}</p>
        </div>
      ))}
    </div>
  );
}
```

---

## Keys in React

**Keys** are special string attributes that help React **identify which items have changed, been added, or removed**.

### Why Keys are Important:

```
Without keys:
❌ React re-renders ALL items even if only one changed
❌ Performance issues in large lists
❌ State bugs (wrong item gets updated)

With keys:
✅ React knows exactly which item changed
✅ Only changed item re-renders
✅ Better performance
✅ No state bugs
```

```jsx
// ❌ Bad - using index as key (causes bugs when list changes)
{
  items.map((item, index) => <li key={index}>{item.name}</li>);
}

// ✅ Good - using unique ID as key
{
  items.map((item) => <li key={item.id}>{item.name}</li>);
}
```

### Simple Interview Answer:

> Keys help React identify which items in a list have changed, been added, or removed. They should be unique and stable. Using array index as key is not recommended because if the list order changes, React may incorrectly reuse or destroy components, leading to bugs. Always use a unique identifier like a database ID.

---

## Creating Reusable Card Component – Mini Practical

```jsx
// Card.jsx - Reusable Card Component
function Card({ title, description, image, price, rating }) {
  return (
    <div
      className="card"
      style={{
        border: "1px solid #ccc",
        padding: "16px",
        borderRadius: "8px",
        width: "250px",
      }}
    >
      <img
        src={image}
        alt={title}
        style={{ width: "100%", borderRadius: "8px" }}
      />
      <h3>{title}</h3>
      <p>{description}</p>
      <p>⭐ {rating}/5</p>
      <h2 style={{ color: "green" }}>₹{price}</h2>
      <button
        style={{
          padding: "8px 16px",
          backgroundColor: "blue",
          color: "white",
          border: "none",
          borderRadius: "4px",
        }}
      >
        Add to Cart
      </button>
    </div>
  );
}

// App.jsx - Using Card Component
function App() {
  const products = [
    {
      id: 1,
      title: "Laptop",
      description: "High performance laptop",
      image: "laptop.jpg",
      price: 50000,
      rating: 4.5,
    },
    {
      id: 2,
      title: "Phone",
      description: "Latest smartphone",
      image: "phone.jpg",
      price: 20000,
      rating: 4.2,
    },
    {
      id: 3,
      title: "Headphones",
      description: "Noise cancelling",
      image: "headphones.jpg",
      price: 5000,
      rating: 4.8,
    },
  ];

  return (
    <div style={{ display: "flex", gap: "20px", padding: "20px" }}>
      {products.map((product) => (
        <Card
          key={product.id}
          title={product.title}
          description={product.description}
          image={product.image}
          price={product.price}
          rating={product.rating}
        />
      ))}
    </div>
  );
}

export default App;
```

---

---

# 3. State & Re-rendering Logic

---

## What is State?

**State** is data that **changes over time** inside a component. When state changes, React **automatically re-renders** the component to show updated UI.

### Simple Interview Answer:

> State is a built-in object in React that stores data that can change over the component's lifetime. When state changes, React automatically re-renders the component to reflect the new data. State is local to the component and managed using the useState hook in functional components.

### Props vs State

| Feature      | Props               | State                    |
| ------------ | ------------------- | ------------------------ |
| Who controls | Parent component    | Component itself         |
| Can change?  | ❌ Read-only        | ✅ Can be updated        |
| Passed from  | Parent to child     | Created inside component |
| Purpose      | Configure component | Track changing data      |

---

## useState Hook

`useState` is a **React Hook** that lets you add state to functional components.

```jsx
import { useState } from "react";

function Counter() {
  // useState returns [currentValue, functionToUpdateValue]
  const [count, setCount] = useState(0); // 0 is initial value

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount(count - 1)}>Decrement</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
```

### useState with Different Data Types

```jsx
function FormExample() {
  const [name, setName] = useState(""); // String
  const [age, setAge] = useState(0); // Number
  const [isOnline, setIsOnline] = useState(false); // Boolean
  const [hobbies, setHobbies] = useState([]); // Array
  const [user, setUser] = useState({
    // Object
    name: "",
    email: "",
  });

  return (
    <div>
      <input
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Enter name"
      />
      <p>Name: {name}</p>
    </div>
  );
}
```

### Updating State Based on Previous State

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  // ❌ Wrong - may use stale state
  const increment = () => {
    setCount(count + 1);
    setCount(count + 1); // Still uses old count! Result: +1 not +2
  };

  // ✅ Correct - use callback function
  const incrementTwice = () => {
    setCount((prevCount) => prevCount + 1);
    setCount((prevCount) => prevCount + 1); // Uses updated count! Result: +2
  };

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={incrementTwice}>+2</button>
    </div>
  );
}
```

---

## How React Re-renders – Reconciliation Process

```
Step 1: State changes (e.g., setCount(5))
    ↓
Step 2: React calls the component function again (re-render)
    ↓
Step 3: New JSX is generated with updated state
    ↓
Step 4: React compares new Virtual DOM with old Virtual DOM (Diffing)
    ↓
Step 5: React finds minimum changes needed
    ↓
Step 6: React updates ONLY changed parts in Real DOM (Reconciliation)
    ↓
Step 7: User sees updated UI ✅
```

### Important Points:

```
✅ React re-renders the component where state changed AND its children
✅ React does NOT re-render sibling or parent components
✅ Re-rendering is FAST because of Virtual DOM diffing
✅ React batches multiple state updates into one re-render
```

---

## Batching State Updates

React **groups multiple state updates** into a **single re-render** for performance.

```jsx
function BatchingExample() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState("Rahul");

  const handleClick = () => {
    setCount(count + 1); // Update 1
    setName("Amit"); // Update 2
    setCount(count + 2); // Update 3

    // React batches all 3 updates → Only ONE re-render! ✅
    // Without batching → 3 re-renders ❌
  };

  return (
    <div>
      <p>Count: {count}</p>
      <p>Name: {name}</p>
      <button onClick={handleClick}>Update All</button>
    </div>
  );
}
```

---

## Derived State Concept

**Derived State** means calculating a value from existing state instead of storing it separately.

```jsx
// ❌ Bad - unnecessary state variable
function ShoppingCart() {
  const [items, setItems] = useState([
    { name: "Laptop", price: 50000 },
    { name: "Mouse", price: 500 },
  ]);
  const [total, setTotal] = useState(50500); // ❌ Don't store this!

  // Problem: You have to manually update total every time items change
}

// ✅ Good - derive total from items (no extra state needed)
function ShoppingCart() {
  const [items, setItems] = useState([
    { name: "Laptop", price: 50000 },
    { name: "Mouse", price: 500 },
  ]);

  // Derived state - calculated from existing state
  const total = items.reduce((sum, item) => sum + item.price, 0);
  const itemCount = items.length;
  const hasItems = items.length > 0;

  return (
    <div>
      <p>Items: {itemCount}</p>
      <p>Total: ₹{total}</p>
      <p>{hasItems ? "Cart is not empty" : "Cart is empty"}</p>
    </div>
  );
}
```

> **Interview Tip:** If a value can be calculated from existing state or props, **don't store it as separate state**. This avoids bugs where derived data gets out of sync.

---

## Practical Build – Interactive Counter & Toggle

### Counter Component

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div style={{ textAlign: "center", padding: "20px" }}>
      <h1>Counter: {count}</h1>
      <button onClick={() => setCount(count + 1)}>➕ Increment</button>
      <button onClick={() => setCount(count - 1)}>➖ Decrement</button>
      <button onClick={() => setCount(0)}>🔄 Reset</button>

      {count > 10 && <p style={{ color: "green" }}>🎉 You crossed 10!</p>}
      {count < 0 && <p style={{ color: "red" }}>⚠️ Count is negative!</p>}
    </div>
  );
}

export default Counter;
```

### Toggle Switch Component

```jsx
import { useState } from "react";

function ToggleSwitch() {
  const [isOn, setIsOn] = useState(false);

  return (
    <div style={{ textAlign: "center", padding: "20px" }}>
      <h2>Dark Mode: {isOn ? "ON 🌙" : "OFF ☀️"}</h2>

      <button
        onClick={() => setIsOn(!isOn)}
        style={{
          padding: "10px 20px",
          fontSize: "16px",
          backgroundColor: isOn ? "#333" : "#fff",
          color: isOn ? "#fff" : "#333",
          border: "2px solid #333",
          borderRadius: "20px",
          cursor: "pointer",
        }}
      >
        Toggle {isOn ? "OFF" : "ON"}
      </button>

      <div
        style={{
          marginTop: "20px",
          padding: "20px",
          backgroundColor: isOn ? "#333" : "#f0f0f0",
          color: isOn ? "#fff" : "#333",
          borderRadius: "8px",
        }}
      >
        <p>This content changes based on toggle state!</p>
      </div>
    </div>
  );
}

export default ToggleSwitch;
```

---

---

# 4. React Lifecycle Methods

---

## Class Components Lifecycle

In **class components**, React provides special **lifecycle methods** that run at different stages of a component's life.

### Three Phases of Lifecycle:

```
1. Mounting    → Component is created and inserted into DOM (Birth)
2. Updating    → Component re-renders due to state/props change (Growth)
3. Unmounting  → Component is removed from DOM (Death)
```

---

## React's Lifecycle Methods (Class Components)

```jsx
import React from "react";

class MyComponent extends React.Component {
  // MOUNTING PHASE
  constructor(props) {
    super(props);
    this.state = { count: 0 };
    console.log("1. Constructor - Component is being created");
  }

  componentDidMount() {
    console.log("2. Component Did Mount - Component is now in DOM");
    // Perfect place for: API calls, subscriptions, timers
    // Example: fetch("https://api.example.com/data")
  }

  // UPDATING PHASE
  componentDidUpdate(prevProps, prevState) {
    console.log("3. Component Did Update - Component re-rendered");
    // Runs after every state or props change
    if (prevState.count !== this.state.count) {
      console.log("Count changed!");
    }
  }

  // UNMOUNTING PHASE
  componentWillUnmount() {
    console.log("4. Component Will Unmount - Component is being removed");
    // Perfect place for: cleanup timers, cancel API calls, remove listeners
  }

  render() {
    console.log("Render - JSX is being generated");
    return (
      <div>
        <h1>Count: {this.state.count}</h1>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Increment
        </button>
      </div>
    );
  }
}
```

### Lifecycle Flow:

```
Mounting:   constructor → render → componentDidMount
Updating:   render → componentDidUpdate
Unmounting: componentWillUnmount
```

---

## useEffect Hook – Side Effects in Functional Components

`useEffect` replaces **all lifecycle methods** in functional components.

### Basic Syntax:

```jsx
import { useState, useEffect } from "react";

function MyComponent() {
  const [count, setCount] = useState(0);

  // useEffect runs AFTER every render by default
  useEffect(() => {
    console.log("Component rendered! Count:", count);
  });

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### useEffect with Dependency Array

```jsx
function MyComponent() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState("Rahul");

  // Case 1: No dependency array → Runs after EVERY render
  useEffect(() => {
    console.log("Runs on every render");
  });

  // Case 2: Empty array [] → Runs ONLY ONCE (like componentDidMount)
  useEffect(() => {
    console.log("Runs only once on mount");
  }, []);

  // Case 3: With dependencies → Runs when dependencies CHANGE
  useEffect(() => {
    console.log("Runs when count changes:", count);
  }, [count]); // Only re-runs when count changes, NOT when name changes

  return (
    <div>
      <h1>Count: {count}</h1>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### useEffect with Cleanup (like componentWillUnmount)

```jsx
function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    // Setup - runs when component mounts
    const intervalId = setInterval(() => {
      setSeconds((prev) => prev + 1);
    }, 1000);

    console.log("Timer started");

    // Cleanup - runs when component unmounts
    return () => {
      clearInterval(intervalId);
      console.log("Timer stopped - cleanup!");
    };
  }, []); // Empty array = run once

  return <h1>Seconds: {seconds}</h1>;
}
```

---

## Data Fetching, Cleanup & DOM Manipulation

### Data Fetching with useEffect

```jsx
import { useState, useEffect } from "react";

function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    // Async function inside useEffect
    const fetchUsers = async () => {
      try {
        setLoading(true);
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/users",
        );

        if (!response.ok) {
          throw new Error("Failed to fetch users");
        }

        const data = await response.json();
        setUsers(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchUsers();
  }, []); // Fetch once on mount

  if (loading) return <h2>Loading...</h2>;
  if (error) return <h2>Error: {error}</h2>;

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>
          {user.name} - {user.email}
        </li>
      ))}
    </ul>
  );
}
```

### Event Listener Cleanup

```jsx
function WindowSize() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);

    window.addEventListener("resize", handleResize);

    // Cleanup - remove listener when component unmounts
    return () => {
      window.removeEventListener("resize", handleResize);
    };
  }, []);

  return <h1>Window Width: {width}px</h1>;
}
```

### useEffect Summary Table

| Dependency Array | When it Runs       | Equivalent Lifecycle |
| ---------------- | ------------------ | -------------------- |
| No array         | Every render       | render               |
| `[]` empty       | Once on mount      | componentDidMount    |
| `[count]`        | When count changes | componentDidUpdate   |
| Return function  | On unmount         | componentWillUnmount |

---

---

# 5. Event Handling & Conditional Rendering

---

## Handling Click Events in React

```jsx
function ClickExample() {
  // Method 1 - Inline arrow function
  const handleClick = () => {
    console.log("Button clicked!");
  };

  // Method 2 - Function with parameter
  const greetUser = (name) => {
    alert(`Hello, ${name}!`);
  };

  return (
    <div>
      {/* ✅ Correct - pass function reference */}
      <button onClick={handleClick}>Click Me</button>

      {/* ✅ Correct - arrow function with parameter */}
      <button onClick={() => greetUser("Rahul")}>Greet</button>

      {/* ❌ Wrong - this CALLS the function immediately! */}
      {/* <button onClick={handleClick()}>Wrong!</button> */}
    </div>
  );
}
```

### Event Object

```jsx
function EventExample() {
  const handleClick = (event) => {
    console.log("Event type:", event.type); // "click"
    console.log("Target:", event.target); // Button element
    console.log("Mouse X:", event.clientX); // Mouse position
  };

  const handleKeyDown = (event) => {
    if (event.key === "Enter") {
      console.log("Enter key pressed!");
    }
  };

  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
      <input onKeyDown={handleKeyDown} placeholder="Press Enter" />
    </div>
  );
}
```

---

## Controlled Inputs – Managing Form Inputs via State

**Controlled Input** means the input value is **controlled by React state**, not by the DOM.

```jsx
import { useState } from "react";

function ControlledForm() {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");

  const handleSubmit = (event) => {
    event.preventDefault(); // Stop page reload
    console.log("Name:", name, "Email:", email);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={name} // Value from state
        onChange={(e) => setName(e.target.value)} // Update state on change
        placeholder="Enter name"
      />
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Enter email"
      />
      <button type="submit">Submit</button>

      <p>
        Preview: {name} ({email})
      </p>
    </form>
  );
}
```

### Why Controlled?

```
Uncontrolled: DOM manages the value (like plain HTML)
Controlled:   React state manages the value ✅

Benefits of Controlled:
✅ Instant validation
✅ Format input as user types
✅ Enable/disable submit button based on input
✅ Single source of truth (state)
```

---

## Conditional Rendering Patterns

### 1. Ternary Operator (if-else)

```jsx
function Welcome({ isLoggedIn }) {
  return (
    <div>
      {isLoggedIn ? <h1>Welcome back, Rahul! 👋</h1> : <h1>Please Login 🔒</h1>}
    </div>
  );
}
```

### 2. && Operator (if only, no else)

```jsx
function Notifications({ count }) {
  return (
    <div>
      <h1>Dashboard</h1>
      {count > 0 && <p>🔔 You have {count} new notifications!</p>}
      {count === 0 && <p>✅ No new notifications</p>}
    </div>
  );
}
```

### 3. Early Return

```jsx
function UserProfile({ user, loading }) {
  if (loading) {
    return <h2>Loading...</h2>; // Stop here if loading
  }

  if (!user) {
    return <h2>No user found!</h2>; // Stop here if no user
  }

  // Only reaches here if loading is false AND user exists
  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  );
}
```

### 4. Variable Assignment

```jsx
function StatusMessage({ status }) {
  let message;

  if (status === "success") {
    message = <p style={{ color: "green" }}>✅ Operation successful!</p>;
  } else if (status === "error") {
    message = <p style={{ color: "red" }}>❌ Something went wrong!</p>;
  } else {
    message = <p style={{ color: "gray" }}>⏳ Processing...</p>;
  }

  return <div>{message}</div>;
}
```

---

## Dynamic UI Rendering – Show/Hide Components

```jsx
import { useState } from "react";

function Accordion() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div style={{ border: "1px solid #ccc", padding: "10px", margin: "10px" }}>
      <button onClick={() => setIsOpen(!isOpen)}>
        {isOpen ? "▼ Hide" : "▶ Show"} Details
      </button>

      {isOpen && (
        <div
          style={{
            marginTop: "10px",
            padding: "10px",
            backgroundColor: "#f9f9f9",
          }}
        >
          <p>
            This is the hidden content that appears when you click the button!
          </p>
          <p>You can put any component here.</p>
        </div>
      )}
    </div>
  );
}
```

---

## Practical Build – Dynamic Form with Validation

```jsx
import { useState } from "react";

function RegistrationForm() {
  const [formData, setFormData] = useState({
    name: "",
    email: "",
    password: "",
  });

  const [errors, setErrors] = useState({});
  const [isSubmitted, setIsSubmitted] = useState(false);

  // Handle input change
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData((prev) => ({
      ...prev,
      [name]: value,
    }));

    // Clear error when user starts typing
    if (errors[name]) {
      setErrors((prev) => ({ ...prev, [name]: "" }));
    }
  };

  // Validation logic
  const validate = () => {
    let newErrors = {};

    if (formData.name.trim().length < 3) {
      newErrors.name = "Name must be at least 3 characters";
    }

    if (!formData.email.includes("@")) {
      newErrors.email = "Enter a valid email address";
    }

    if (formData.password.length < 6) {
      newErrors.password = "Password must be at least 6 characters";
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0; // true if no errors
  };

  // Handle submit
  const handleSubmit = (e) => {
    e.preventDefault();

    if (validate()) {
      setIsSubmitted(true);
      console.log("Form submitted:", formData);
    }
  };

  if (isSubmitted) {
    return (
      <div style={{ color: "green", textAlign: "center" }}>
        <h2>✅ Registration Successful!</h2>
        <p>Welcome, {formData.name}!</p>
      </div>
    );
  }

  return (
    <form onSubmit={handleSubmit} style={{ maxWidth: "400px", margin: "auto" }}>
      <h2>Register</h2>

      {/* Name Field */}
      <div style={{ marginBottom: "10px" }}>
        <label>Name:</label>
        <input
          type="text"
          name="name"
          value={formData.name}
          onChange={handleChange}
          style={{ width: "100%", padding: "8px" }}
        />
        {errors.name && <span style={{ color: "red" }}>{errors.name}</span>}
      </div>

      {/* Email Field */}
      <div style={{ marginBottom: "10px" }}>
        <label>Email:</label>
        <input
          type="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
          style={{ width: "100%", padding: "8px" }}
        />
        {errors.email && <span style={{ color: "red" }}>{errors.email}</span>}
      </div>

      {/* Password Field */}
      <div style={{ marginBottom: "10px" }}>
        <label>Password:</label>
        <input
          type="password"
          name="password"
          value={formData.password}
          onChange={handleChange}
          style={{ width: "100%", padding: "8px" }}
        />
        {errors.password && (
          <span style={{ color: "red" }}>{errors.password}</span>
        )}
      </div>

      <button
        type="submit"
        style={{
          padding: "10px 20px",
          backgroundColor: "blue",
          color: "white",
        }}
      >
        Register
      </button>
    </form>
  );
}

export default RegistrationForm;
```

---

---

# 6. Component Architecture Principles

---

## Single Responsibility Principle

**One component should do ONE thing only.**

```jsx
// ❌ Bad - One component doing everything
function Dashboard() {
  // Fetching data
  // Calculating stats
  // Rendering charts
  // Handling forms
  // Managing auth
  // 500 lines of code!
}

// ✅ Good - Each component has one responsibility
function Dashboard() {
  return (
    <div>
      <Header />
      <StatsCards />
      <SalesChart />
      <RecentOrders />
    </div>
  );
}
```

### Simple Interview Answer:

> Single Responsibility Principle means each component should have one clear purpose. A component should either fetch data, display data, or handle user interaction – not all three. This makes components easier to understand, test, and reuse.

---

## Smart vs Dumb Components

### Smart Component (Container) – Has Logic & State

```jsx
// Smart Component - manages state and logic
function UserListContainer() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch("https://api.example.com/users")
      .then((res) => res.json())
      .then((data) => {
        setUsers(data);
        setLoading(false);
      });
  }, []);

  const handleDelete = (id) => {
    setUsers(users.filter((u) => u.id !== id));
  };

  // Passes data and functions to dumb component
  return <UserList users={users} loading={loading} onDelete={handleDelete} />;
}
```

### Dumb Component (Presentational) – Only Displays UI

```jsx
// Dumb Component - only renders UI, no logic
function UserList({ users, loading, onDelete }) {
  if (loading) return <p>Loading...</p>;

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>
          {user.name}
          <button onClick={() => onDelete(user.id)}>Delete</button>
        </li>
      ))}
    </ul>
  );
}
```

### Comparison

| Feature     | Smart (Container)     | Dumb (Presentational)      |
| ----------- | --------------------- | -------------------------- |
| State       | ✅ Has state          | ❌ No state                |
| Logic       | ✅ Has business logic | ❌ No logic                |
| API calls   | ✅ Fetches data       | ❌ Receives data via props |
| Reusability | Less reusable         | Highly reusable            |
| Example     | UserListContainer     | UserList, Button, Card     |

---

## Lifting State Up

When **sibling components** need to share data, move the state to their **common parent**.

```jsx
// ❌ Problem - Two siblings need same data
// SiblingA has state → SiblingB also needs it → How to share?

// ✅ Solution - Lift state to parent
function Parent() {
  const [count, setCount] = useState(0); // State lives in parent

  return (
    <div>
      <DisplayCount count={count} /> {/* Pass data down */}
      <IncrementButton onIncrement={() => setCount(count + 1)} />{" "}
      {/* Pass function down */}
    </div>
  );
}

// Sibling 1 - Displays count
function DisplayCount({ count }) {
  return <h1>Count: {count}</h1>;
}

// Sibling 2 - Changes count
function IncrementButton({ onIncrement }) {
  return <button onClick={onIncrement}>Increment</button>;
}
```

### Simple Interview Answer:

> Lifting state up means moving the state from a child component to the nearest common parent when multiple sibling components need to share the same data. The parent passes the state down as props and passes callback functions to allow children to update the state.

---

## Prop Drilling Problem

**Prop Drilling** happens when you pass props through **many levels** of components that don't need the data themselves.

```jsx
// ❌ Prop Drilling - passing user through 3 levels
function App() {
  const [user, setUser] = useState({ name: "Rahul" });

  return <Layout user={user} />; // Level 1 - doesn't need user
}

function Layout({ user }) {
  return <Sidebar user={user} />; // Level 2 - doesn't need user
}

function Sidebar({ user }) {
  return <ProfileCard user={user} />; // Level 3 - doesn't need user
}

function ProfileCard({ user }) {
  return <h1>Hello, {user.name}!</h1>; // Level 4 - ACTUALLY needs user!
}
```

### Problems with Prop Drilling:

```
❌ Code is messy and hard to read
❌ Middle components receive props they don't use
❌ Hard to maintain when app grows
❌ Adding new levels means updating all middle components
```

### Solutions:

```
1. Context API (built-in React solution)
2. State management libraries (Redux, Zustand)
3. Component Composition (pass components as children)
```

---

## Component Composition

**Composition** means building complex UI by **combining smaller components** together, like nesting HTML elements.

### Using children Prop

```jsx
// Layout component that wraps any content
function Card({ children, title }) {
  return (
    <div
      style={{ border: "1px solid #ccc", padding: "16px", borderRadius: "8px" }}
    >
      {title && <h2>{title}</h2>}
      {children} {/* Renders whatever is placed inside <Card> */}
    </div>
  );
}

// Use composition - no prop drilling needed!
function App() {
  return (
    <Card title="User Profile">
      <p>Name: Rahul</p>
      <p>Age: 25</p>
      <button>Edit Profile</button>
    </Card>
  );
}
```

### Building Scalable Layout with Composition

```jsx
// Reusable Layout Components
function Page({ children }) {
  return <div className="page">{children}</div>;
}

function Header({ children }) {
  return (
    <header
      style={{ backgroundColor: "#333", color: "white", padding: "16px" }}
    >
      {children}
    </header>
  );
}

function Main({ children }) {
  return <main style={{ padding: "20px" }}>{children}</main>;
}

function Footer({ children }) {
  return (
    <footer style={{ textAlign: "center", padding: "16px" }}>{children}</footer>
  );
}

// Compose them together
function App() {
  return (
    <Page>
      <Header>
        <h1>My Website</h1>
        <nav>Home | About | Contact</nav>
      </Header>

      <Main>
        <h2>Welcome!</h2>
        <p>This is the main content area.</p>
      </Main>

      <Footer>
        <p>© 2025 My Website</p>
      </Footer>
    </Page>
  );
}
```

### Why Composition Solves Prop Drilling:

```jsx
// ❌ Prop Drilling
<App user={user}>
  <Layout user={user}>
    <Sidebar user={user}>
      <Profile user={user} />
    </Sidebar>
  </Layout>
</App>

// ✅ Composition - no drilling!
<App>
  <Layout>
    <Sidebar>
      <Profile user={user} />   {/* Only the component that needs user gets it */}
    </Sidebar>
  </Layout>
</App>
```

---

## Final Quick Summary

```
1. React Introduction
   ├── React = Declarative, component-based UI library
   ├── SPA = Single HTML page, no full reload
   ├── Virtual DOM = Fast diffing + minimal Real DOM updates
   ├── Setup: npm create vite@latest my-app -- --template react
   ├── JSX = HTML inside JS (className, {}, one parent)
   └── Components = Reusable UI building blocks

2. Components & Props
   ├── Functional Component = JS function returning JSX
   ├── Props = Data passed parent → child (read-only)
   ├── Destructure props: ({ name, age })
   ├── Lists: items.map(item => <li key={item.id}>{item.name}</li>)
   └── Keys = Unique IDs for list items (not index!)

3. State & Re-rendering
   ├── State = Data that changes over time
   ├── useState(initialValue) → [value, setValue]
   ├── State change → Re-render → Virtual DOM diff → Real DOM update
   ├── Batching = Multiple updates → One re-render
   └── Derived State = Calculate from existing state, don't store

4. Lifecycle Methods
   ├── Mounting → constructor → render → componentDidMount
   ├── Updating → render → componentDidUpdate
   ├── Unmounting → componentWillUnmount
   └── useEffect(fn, [deps]) replaces all lifecycle methods

5. Events & Conditional Rendering
   ├── onClick={handleClick} (pass reference, not call!)
   ├── Controlled inputs: value={state} onChange={setState}
   ├── Conditional: ternary (? :), && operator, early return
   └── Form validation: validate on submit, show errors

6. Architecture Principles
   ├── Single Responsibility = One component, one job
   ├── Smart (logic) vs Dumb (UI) components
   ├── Lifting State Up = Move state to common parent
   ├── Prop Drilling = Passing props through many levels ❌
   └── Composition = Combine components using children prop ✅
```

---

> 💡 **Interview Tips:**
>
> - Always say **"React uses Virtual DOM with diffing algorithm"** when asked about performance
> - **Props are read-only**, **State is mutable** – fundamental difference
> - `useEffect` with `[]` = runs once, with `[dep]` = runs when dep changes
> - **Keys should be unique and stable** – never use array index for dynamic lists
> - **Lifting state up** is the React way to share data between siblings
> - **Prop drilling** is solved by Context API or Composition
