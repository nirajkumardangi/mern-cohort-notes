# React Complete Notes – Interview Ready ⚛️

---

# 7. Useful Hooks in React 🪝

---

## Understanding React Hooks

**Hooks** are special functions that let you **use React features** (like state and lifecycle) inside **functional components**.

Before Hooks (React < 16.8), you could only use state and lifecycle in **class components**. Hooks changed everything.

### Simple Interview Answer:

> Hooks are functions introduced in React 16.8 that allow functional components to use state, lifecycle methods, and other React features without writing class components. They start with the word "use" like useState, useEffect, useRef, etc.

### Why Hooks?

```
Before Hooks (Class Components):
❌ Complex class syntax (this keyword confusion)
❌ Lifecycle methods mixed unrelated logic
❌ Hard to reuse stateful logic between components
❌ Wrapper hell with HOCs and Render Props

After Hooks (Functional Components):
✅ Simple functions, no 'this' keyword
✅ Related logic stays together
✅ Easy to extract and reuse logic (Custom Hooks)
✅ Cleaner, shorter code
```

---

## Rules of Hooks

React has **two strict rules** for using hooks. Breaking them causes bugs.

### Rule 1: Only Call Hooks at the Top Level

```jsx
// ❌ WRONG - Hooks inside conditions, loops, or nested functions
function MyComponent({ isLoggedIn }) {
  if (isLoggedIn) {
    const [name, setName] = useState("Rahul"); // ❌ Inside if block!
  }

  for (let i = 0; i < 3; i++) {
    const [count, setCount] = useState(0); // ❌ Inside loop!
  }

  function handleClick() {
    const [show, setShow] = useState(false); // ❌ Inside function!
  }
}

// ✅ CORRECT - Always at the top level of the component
function MyComponent({ isLoggedIn }) {
  const [name, setName] = useState("Rahul"); // ✅ Top level
  const [count, setCount] = useState(0); // ✅ Top level
  const [show, setShow] = useState(false); // ✅ Top level

  if (isLoggedIn) {
    // Use the state here, but DON'T declare hooks here
  }
}
```

### Rule 2: Only Call Hooks from React Functions

```jsx
// ❌ WRONG - Calling hook from regular JS function
function regularFunction() {
  const [count, setCount] = useState(0); // ❌ Not a React component or custom hook!
}

// ✅ CORRECT - Call from React component
function MyComponent() {
  const [count, setCount] = useState(0); // ✅ React component
}

// ✅ CORRECT - Call from Custom Hook
function useMyHook() {
  const [count, setCount] = useState(0); // ✅ Custom hook (starts with "use")
}
```

### Why These Rules?

> React relies on the **order of hook calls** to match each hook with its state. If hooks are inside conditions or loops, the order can change between renders, causing React to mix up which state belongs to which hook.

---

## Commonly Used Hooks

---

### 1. useState – Managing Local State

```jsx
import { useState } from "react";

function Counter() {
  // useState returns [currentValue, updateFunction]
  const [count, setCount] = useState(0); // Number
  const [name, setName] = useState(""); // String
  const [isOpen, setIsOpen] = useState(false); // Boolean
  const [items, setItems] = useState([]); // Array
  const [user, setUser] = useState({
    // Object
    name: "",
    age: 0,
  });

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(count - 1)}>-</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
```

### Key Points:

```
✅ State updates are asynchronous (not instant)
✅ State updates trigger re-render
✅ Use callback for updates based on previous state:
   setCount(prev => prev + 1)
✅ For objects/arrays, always create a NEW copy:
   setUser(prev => ({ ...prev, name: "Amit" }))
```

---

### 2. useEffect – Side Effects

```jsx
import { useState, useEffect } from "react";

function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    // Side effect: start timer
    const intervalId = setInterval(() => {
      setSeconds((prev) => prev + 1);
    }, 1000);

    // Cleanup: stop timer when component unmounts
    return () => clearInterval(intervalId);
  }, []); // Empty array = run once on mount

  return <h1>Seconds: {seconds}</h1>;
}
```

### Dependency Array Cheat Sheet:

```js
useEffect(() => { ... })           // Runs on EVERY render
useEffect(() => { ... }, [])       // Runs ONCE on mount
useEffect(() => { ... }, [count])  // Runs when 'count' changes
useEffect(() => {
  return () => { ... }             // Cleanup runs on unmount
}, [])
```

---

### 3. useContext – Sharing Data Without Prop Drilling

**Context** allows you to share data across **many levels** of components without passing props manually.

```jsx
import { createContext, useContext, useState } from "react";

// Step 1: Create Context
const ThemeContext = createContext();

// Step 2: Create Provider (wraps components that need the data)
function App() {
  const [theme, setTheme] = useState("light");

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Header />
      <MainContent />
    </ThemeContext.Provider>
  );
}

// Step 3: Consume Context in any child component (no props needed!)
function Header() {
  const { theme, setTheme } = useContext(ThemeContext);

  return (
    <header style={{ background: theme === "dark" ? "#333" : "#fff" }}>
      <h1>Current Theme: {theme}</h1>
      <button onClick={() => setTheme(theme === "light" ? "dark" : "light")}>
        Toggle Theme
      </button>
    </header>
  );
}

function MainContent() {
  const { theme } = useContext(ThemeContext); // Access same data!

  return (
    <div style={{ background: theme === "dark" ? "#555" : "#f0f0f0" }}>
      <p>Content adapts to {theme} theme!</p>
    </div>
  );
}
```

### When to Use Context:

```
✅ Theme (dark/light mode)
✅ Authentication (logged-in user info)
✅ Language/Locale settings
✅ Shopping cart data

❌ Don't use for frequently changing data (use state management instead)
❌ Don't use for data needed by only 1-2 components (use props)
```

---

### 4. useRef – Persistent Values Without Re-render

`useRef` creates a **mutable reference** that persists across renders **without causing re-renders**.

```jsx
import { useRef, useState } from "react";

function RefExample() {
  const inputRef = useRef(null); // Reference to DOM element
  const renderCount = useRef(0); // Persistent value (like instance variable)
  const [name, setName] = useState("");

  renderCount.current += 1; // Tracks renders without causing re-render

  const focusInput = () => {
    inputRef.current.focus(); // Focus the input field
    inputRef.current.style.border = "2px solid blue";
  };

  return (
    <div>
      <input
        ref={inputRef}
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <button onClick={focusInput}>Focus Input</button>
      <p>Render count: {renderCount.current}</p>
    </div>
  );
}
```

### useRef vs useState

| Feature          | useRef                            | useState           |
| ---------------- | --------------------------------- | ------------------ |
| Causes re-render | ❌ No                             | ✅ Yes             |
| Value persists   | ✅ Across renders                 | ✅ Across renders  |
| Mutable          | ✅ `.current` can change          | ❌ Only via setter |
| Use for          | DOM refs, timers, previous values | UI-related data    |

---

### 5. useCallback – Memoizing Functions

`useCallback` **caches a function** so it's not recreated on every render. Useful when passing functions to child components.

```jsx
import { useState, useCallback } from "react";

function Parent() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState("");

  // ❌ Without useCallback - new function created every render
  // const handleClick = () => console.log("Clicked");

  // ✅ With useCallback - same function reference unless deps change
  const handleClick = useCallback(() => {
    console.log("Button clicked! Count:", count);
  }, [count]); // Only recreate when count changes

  return (
    <div>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <ExpensiveChild onClick={handleClick} />
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
    </div>
  );
}

// Child component wrapped in React.memo for optimization
const ExpensiveChild = React.memo(function ({ onClick }) {
  console.log("Child rendered!");
  return <button onClick={onClick}>Click Me</button>;
});
```

### When to Use useCallback:

```
✅ Passing callbacks to optimized child components (React.memo)
✅ Function used as dependency in useEffect
❌ Don't use for every function (adds unnecessary complexity)
```

---

### 6. useMemo – Memoizing Expensive Calculations

`useMemo` **caches the result** of an expensive calculation so it's not recomputed on every render.

```jsx
import { useState, useMemo } from "react";

function ProductList({ products, filterText }) {
  const [count, setCount] = useState(0);

  // ❌ Without useMemo - filters run on EVERY render (even when count changes)
  // const filtered = products.filter(p => p.name.includes(filterText));

  // ✅ With useMemo - only re-filters when products or filterText changes
  const filteredProducts = useMemo(() => {
    console.log("Filtering products..."); // Only logs when deps change
    return products.filter((p) =>
      p.name.toLowerCase().includes(filterText.toLowerCase()),
    );
  }, [products, filterText]); // Dependencies

  return (
    <div>
      <h2>Filtered Products: {filteredProducts.length}</h2>
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      {/* Clicking count button does NOT re-filter! ✅ */}
      <ul>
        {filteredProducts.map((p) => (
          <li key={p.id}>{p.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

### useCallback vs useMemo

| Feature | useCallback                     | useMemo                                |
| ------- | ------------------------------- | -------------------------------------- |
| Caches  | Function                        | Computed value                         |
| Returns | Memoized function               | Memoized result                        |
| Use for | Event handlers, callbacks       | Expensive calculations, filtered lists |
| Example | `useCallback(() => fn, [deps])` | `useMemo(() => compute(), [deps])`     |

---

## All Hooks Quick Reference

```
useState      → Manage local state
useEffect     → Side effects (API, timers, subscriptions)
useContext     → Access shared data (avoid prop drilling)
useRef        → DOM references & persistent values (no re-render)
useCallback   → Cache functions (prevent unnecessary child re-renders)
useMemo       → Cache expensive calculations
```

---

---

# 8. useEffect Deep Dive

---

## What useEffect Really Does – Synchronizing with External Systems

`useEffect` lets your component **synchronize with something outside of React**.

### What are "External Systems"?

```
External Systems = Anything NOT managed by React
├── Browser APIs (document.title, localStorage)
├── Server APIs (fetch data from backend)
├── Timers (setInterval, setTimeout)
├── Event Listeners (window resize, scroll)
├── Subscriptions (WebSocket, Firebase)
└── Third-party libraries (charts, maps)
```

### Simple Interview Answer:

> useEffect is used to synchronize a React component with external systems like APIs, browser APIs, timers, and event listeners. It runs after the component renders and can optionally clean up when the component unmounts or before the effect re-runs.

```jsx
// Example: Sync document title with component state
function ChatRoom({ roomName, unreadCount }) {
  // Sync with browser tab title (external system)
  useEffect(() => {
    document.title = `${roomName} (${unreadCount} new messages)`;
  }, [roomName, unreadCount]);

  return <h1>Welcome to {roomName}</h1>;
}
```

---

## Dependency Array Behavior – Why Infinite Loops Happen

### How Dependency Array Works:

```js
useEffect(() => {
  // Effect code
}, [dep1, dep2]);
// React compares dep1 and dep2 with previous render values
// If ANY dependency changed → effect runs again
// If NO dependency changed → effect is skipped
```

### Common Infinite Loop Mistakes:

```jsx
// ❌ Mistake 1: Setting state inside useEffect without dependency array
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    setCount(count + 1); // State changes → re-render → effect runs → state changes → ...
  }); // ❌ No dependency array = runs every render = INFINITE LOOP!
}

// ✅ Fix: Add dependency array
useEffect(() => {
  setCount(count + 1);
}, []); // Runs only once
```

```jsx
// ❌ Mistake 2: Object/Array as dependency (new reference every render)
function SearchResults() {
  const [results, setResults] = useState([]);
  const filters = { category: "tech", sort: "date" }; // New object every render!

  useEffect(() => {
    fetchResults(filters);
  }, [filters]); // ❌ filters is a NEW object every render → infinite loop!

  return <div>...</div>;
}

// ✅ Fix: Use primitive values or useMemo
const [category, setCategory] = useState("tech");
const [sort, setSort] = useState("date");

useEffect(() => {
  fetchResults({ category, sort });
}, [category, sort]); // ✅ Primitives compared by value
```

```jsx
// ❌ Mistake 3: Function as dependency
function DataFetcher() {
  const [data, setData] = useState([]);

  const fetchData = () => {
    // New function every render!
    fetch("/api/data")
      .then((res) => res.json())
      .then(setData);
  };

  useEffect(() => {
    fetchData();
  }, [fetchData]); // ❌ fetchData is new every render → infinite loop!

  return <div>...</div>;
}

// ✅ Fix: Move function inside useEffect or use useCallback
useEffect(() => {
  const fetchData = async () => {
    const res = await fetch("/api/data");
    const data = await res.json();
    setData(data);
  };
  fetchData();
}, []); // ✅ Function is inside effect, not a dependency
```

---

## Cleanup Functions – Preventing Memory Leaks

A **cleanup function** is returned from useEffect. It runs:

1. **Before the effect re-runs** (when dependencies change)
2. **When the component unmounts**

### Why Cleanup is Important:

```
Without cleanup:
❌ Timers keep running after component is gone → Memory leak
❌ Event listeners pile up → Performance issues
❌ API responses update unmounted components → React warning
❌ WebSocket connections stay open → Resource waste
```

### Cleanup Examples:

```jsx
// 1. Timer Cleanup
function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const id = setInterval(() => setSeconds((s) => s + 1), 1000);

    return () => {
      clearInterval(id); // ✅ Cleanup: stop timer
      console.log("Timer cleared!");
    };
  }, []);

  return <h1>{seconds}s</h1>;
}
```

```jsx
// 2. Event Listener Cleanup
function WindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);
    window.addEventListener("resize", handleResize);

    return () => {
      window.removeEventListener("resize", handleResize); // ✅ Cleanup
    };
  }, []);

  return <p>Width: {width}px</p>;
}
```

```jsx
// 3. Subscription Cleanup
function ChatRoom({ roomId }) {
  const [messages, setMessages] = useState([]);

  useEffect(() => {
    const connection = createWebSocketConnection(roomId);

    connection.onMessage((msg) => {
      setMessages((prev) => [...prev, msg]);
    });

    return () => {
      connection.disconnect(); // ✅ Cleanup: disconnect when roomId changes or unmount
    };
  }, [roomId]); // Re-connects when roomId changes

  return (
    <div>
      {messages.map((m) => (
        <p key={m.id}>{m.text}</p>
      ))}
    </div>
  );
}
```

```jsx
// 4. Abort Fetch Request Cleanup
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    const controller = new AbortController(); // Create abort controller

    fetch(`https://api.example.com/users/${userId}`, {
      signal: controller.signal, // Attach signal to fetch
    })
      .then((res) => res.json())
      .then((data) => setUser(data))
      .catch((err) => {
        if (err.name !== "AbortError") {
          console.error(err);
        }
      });

    return () => {
      controller.abort(); // ✅ Cleanup: cancel fetch if userId changes
    };
  }, [userId]);

  return user ? <h1>{user.name}</h1> : <p>Loading...</p>;
}
```

---

## Data Fetching Pattern with useEffect

### Complete Pattern with Loading, Error, and Data States:

```jsx
import { useState, useEffect } from "react";

function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    let isMounted = true; // Flag to prevent setting state on unmounted component

    const fetchUsers = async () => {
      try {
        setLoading(true);
        setError(null);

        const response = await fetch(
          "https://jsonplaceholder.typicode.com/users",
        );

        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }

        const data = await response.json();

        if (isMounted) {
          setUsers(data);
        }
      } catch (err) {
        if (isMounted) {
          setError(err.message);
        }
      } finally {
        if (isMounted) {
          setLoading(false);
        }
      }
    };

    fetchUsers();

    return () => {
      isMounted = false; // ✅ Cleanup: component unmounted
    };
  }, []);

  if (loading) return <h2>⏳ Loading users...</h2>;
  if (error) return <h2>❌ Error: {error}</h2>;

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

---

## Practical Example – Fetching API Data on Mount

```jsx
import { useState, useEffect } from "react";

function ProductPage() {
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  const [searchTerm, setSearchTerm] = useState("");

  // Fetch products on mount
  useEffect(() => {
    const controller = new AbortController();

    async function loadProducts() {
      try {
        setLoading(true);
        const res = await fetch("https://fakestoreapi.com/products", {
          signal: controller.signal,
        });
        const data = await res.json();
        setProducts(data);
      } catch (err) {
        if (err.name !== "AbortError") setError(err.message);
      } finally {
        setLoading(false);
      }
    }

    loadProducts();
    return () => controller.abort();
  }, []);

  // Derived state - filtered products
  const filteredProducts = products.filter((p) =>
    p.title.toLowerCase().includes(searchTerm.toLowerCase()),
  );

  if (loading) return <h2>Loading products...</h2>;
  if (error) return <h2>Error: {error}</h2>;

  return (
    <div>
      <h1>Products ({filteredProducts.length})</h1>
      <input
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search products..."
      />
      <div
        style={{
          display: "grid",
          gridTemplateColumns: "repeat(3, 1fr)",
          gap: "16px",
        }}
      >
        {filteredProducts.map((product) => (
          <div
            key={product.id}
            style={{ border: "1px solid #ccc", padding: "16px" }}
          >
            <img
              src={product.image}
              alt={product.title}
              style={{ width: "100px" }}
            />
            <h3>{product.title}</h3>
            <p>₹{product.price}</p>
          </div>
        ))}
      </div>
    </div>
  );
}

export default ProductPage;
```

---

---

# 9. Custom Hooks & Reusable Logic

---

## Why Custom Hooks Exist

**Custom Hooks** let you **extract reusable logic** from components into standalone functions.

### Problem Without Custom Hooks:

```jsx
// Component A - fetches users
function UserList() {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetch("/api/users")
      .then((res) => res.json())
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false));
  }, []);

  // ...render UI
}

// Component B - fetches products (SAME LOGIC, DUPLICATED!)
function ProductList() {
  const [data, setData] = useState([]); // ❌ Duplicated!
  const [loading, setLoading] = useState(true); // ❌ Duplicated!
  const [error, setError] = useState(null); // ❌ Duplicated!

  useEffect(() => {
    // ❌ Same fetching logic!
    fetch("/api/products")
      .then((res) => res.json())
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false));
  }, []);

  // ...render UI
}
```

### Solution – Custom Hook:

```jsx
// Write fetching logic ONCE
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetch(url)
      .then((res) => res.json())
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false));
  }, [url]);

  return { data, loading, error };
}

// Now use it ANYWHERE!
function UserList() {
  const { data: users, loading, error } = useFetch("/api/users");
  // ...render users
}

function ProductList() {
  const { data: products, loading, error } = useFetch("/api/products");
  // ...render products
}
```

### Simple Interview Answer:

> Custom hooks are JavaScript functions that start with "use" and can call other React hooks. They allow you to extract and reuse stateful logic across multiple components without duplicating code. For example, a useFetch hook can handle data fetching, loading, and error states in any component that needs API data.

---

## Writing a useFetch Hook – Reusable API Fetching

```jsx
import { useState, useEffect } from "react";

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    if (!url) return; // Don't fetch if no URL

    const controller = new AbortController();
    setLoading(true);
    setError(null);

    async function fetchData() {
      try {
        const response = await fetch(url, { signal: controller.signal });

        if (!response.ok) {
          throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }

        const result = await response.json();
        setData(result);
      } catch (err) {
        if (err.name !== "AbortError") {
          setError(err.message);
        }
      } finally {
        setLoading(false);
      }
    }

    fetchData();

    return () => controller.abort(); // Cleanup on unmount or URL change
  }, [url]);

  return { data, loading, error };
}

// Usage in components
function UserList() {
  const {
    data: users,
    loading,
    error,
  } = useFetch("https://jsonplaceholder.typicode.com/users");

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

### More Custom Hook Examples:

```jsx
// useLocalStorage - Sync state with localStorage
function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    const stored = localStorage.getItem(key);
    return stored ? JSON.parse(stored) : initialValue;
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);

  return [value, setValue];
}

// Usage
function Settings() {
  const [theme, setTheme] = useLocalStorage("theme", "light");
  return (
    <button onClick={() => setTheme((t) => (t === "light" ? "dark" : "light"))}>
      Theme: {theme}
    </button>
  );
}
```

```jsx
// useWindowSize - Track window dimensions
function useWindowSize() {
  const [size, setSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight,
  });

  useEffect(() => {
    const handleResize = () =>
      setSize({
        width: window.innerWidth,
        height: window.innerHeight,
      });

    window.addEventListener("resize", handleResize);
    return () => window.removeEventListener("resize", handleResize);
  }, []);

  return size;
}

// Usage
function ResponsiveLayout() {
  const { width } = useWindowSize();
  return <p>{width < 768 ? "Mobile" : "Desktop"} View</p>;
}
```

---

## Separation of Business Logic from UI

```jsx
// ❌ Bad - Logic and UI mixed together
function ShoppingCart() {
  const [items, setItems] = useState([]);

  // Business logic mixed with UI
  const addToCart = (product) => {
    const existing = items.find(i => i.id === product.id);
    if (existing) {
      setItems(items.map(i =>
        i.id === product.id ? { ...i, qty: i.qty + 1 } : i
      ));
    } else {
      setItems([...items, { ...product, qty: 1 }]);
    }
  };

  const total = items.reduce((sum, i) => sum + i.price * i.qty, 0);
  const discount = total > 1000 ? total * 0.1 : 0;
  const finalTotal = total - discount;

  return (/* JSX */);
}

// ✅ Good - Logic in custom hook, UI in component
function useCart() {
  const [items, setItems] = useState([]);

  const addToCart = (product) => {
    setItems(prev => {
      const existing = prev.find(i => i.id === product.id);
      if (existing) {
        return prev.map(i => i.id === product.id ? { ...i, qty: i.qty + 1 } : i);
      }
      return [...prev, { ...product, qty: 1 }];
    });
  };

  const removeFromCart = (productId) => {
    setItems(prev => prev.filter(i => i.id !== productId));
  };

  const total = items.reduce((sum, i) => sum + i.price * i.qty, 0);
  const discount = total > 1000 ? total * 0.1 : 0;
  const finalTotal = total - discount;

  return { items, addToCart, removeFromCart, total, discount, finalTotal };
}

// Clean UI component
function ShoppingCart() {
  const { items, addToCart, removeFromCart, total, discount, finalTotal } = useCart();

  return (
    <div>
      {items.map(item => (
        <div key={item.id}>
          {item.name} x{item.qty} - ₹{item.price * item.qty}
          <button onClick={() => removeFromCart(item.id)}>Remove</button>
        </div>
      ))}
      <p>Total: ₹{total}</p>
      {discount > 0 && <p>Discount: -₹{discount}</p>}
      <h2>Final: ₹{finalTotal}</h2>
    </div>
  );
}
```

---

## Improving Code Maintainability with Hooks

```
Benefits of Custom Hooks:
✅ DRY Principle - Don't Repeat Yourself
✅ Single Source of Truth - Logic in one place
✅ Easy Testing - Test hooks independently
✅ Reusability - Use across any component
✅ Readability - Components focus on UI only
✅ Maintainability - Fix bug once, fixed everywhere
```

---

---

# 10. Advanced Reusability Patterns

---

## Higher Order Components (HOCs)

An **HOC** is a **function that takes a component and returns a new enhanced component**. It's a pattern for reusing component logic.

### Simple Interview Answer:

> HOC is a function that takes a component as an argument and returns a new component with additional props or behavior. It's a pattern for reusing logic across components. Example: withAuth(Component) wraps a component and adds authentication check.

```jsx
// HOC that adds loading state to any component
function withLoading(WrappedComponent) {
  return function EnhancedComponent({ isLoading, ...props }) {
    if (isLoading) {
      return <h2>⏳ Loading...</h2>;
    }
    return <WrappedComponent {...props} />;
  };
}

// Original component
function UserList({ users }) {
  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

// Enhanced component with loading capability
const UserListWithLoading = withLoading(UserList);

// Usage
function App() {
  return (
    <div>
      <UserListWithLoading isLoading={true} users={[]} />{" "}
      {/* Shows "Loading..." */}
      <UserListWithLoading
        isLoading={false}
        users={[{ id: 1, name: "Rahul" }]}
      />{" "}
      {/* Shows list */}
    </div>
  );
}
```

### HOC for Authentication

```jsx
function withAuth(WrappedComponent) {
  return function AuthenticatedComponent(props) {
    const isLoggedIn = localStorage.getItem("token");

    if (!isLoggedIn) {
      return <h2>🔒 Please login to access this page</h2>;
    }

    return <WrappedComponent {...props} />;
  };
}

// Usage
const ProtectedDashboard = withAuth(Dashboard);
const ProtectedSettings = withAuth(Settings);
```

---

## Render Props Pattern

**Render Props** is a pattern where a component receives a **function as a prop** and calls it to render UI.

### Simple Interview Answer:

> Render Props is a pattern where a component accepts a function prop (usually called "render" or "children") that returns JSX. This allows the parent component to control what gets rendered while the child component manages the shared logic.

```jsx
// Component that manages mouse position logic
function MouseTracker({ render }) {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  useEffect(() => {
    const handleMove = (e) => setPosition({ x: e.clientX, y: e.clientY });
    window.addEventListener("mousemove", handleMove);
    return () => window.removeEventListener("mousemove", handleMove);
  }, []);

  // Call the render function and pass the data
  return render(position);
}

// Usage - Parent decides how to render
function App() {
  return (
    <MouseTracker
      render={({ x, y }) => (
        <h1>
          Mouse is at: X={x}, Y={y}
        </h1>
      )}
    />
  );
}

// Same logic, different UI!
function App2() {
  return (
    <MouseTracker
      render={({ x, y }) => (
        <div style={{ position: "fixed", left: x, top: y }}>🎯</div>
      )}
    />
  );
}
```

### Render Props with children

```jsx
function DataFetcher({ url, children }) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(url)
      .then((res) => res.json())
      .then((data) => {
        setData(data);
        setLoading(false);
      });
  }, [url]);

  return children({ data, loading }); // children is a function!
}

// Usage
function App() {
  return (
    <DataFetcher url="https://jsonplaceholder.typicode.com/users">
      {({ data, loading }) => {
        if (loading) return <p>Loading...</p>;
        return (
          <ul>
            {data.map((u) => (
              <li key={u.id}>{u.name}</li>
            ))}
          </ul>
        );
      }}
    </DataFetcher>
  );
}
```

---

## Compound Components

**Compound Components** are a set of components that **work together** and share internal state implicitly.

Think of HTML `<select>` and `<option>` – they work together.

### Simple Interview Answer:

> Compound Components is a pattern where multiple components work together as a unit, sharing internal state through Context. The parent component manages the state and child components consume it. Example: Tabs, Accordion, Dropdown menus.

```jsx
import { createContext, useContext, useState } from "react";

// Create shared context
const TabsContext = createContext();

// Parent Component - manages shared state
function Tabs({ children, defaultTab = 0 }) {
  const [activeTab, setActiveTab] = useState(defaultTab);

  return (
    <TabsContext.Provider value={{ activeTab, setActiveTab }}>
      <div className="tabs">{children}</div>
    </TabsContext.Provider>
  );
}

// Child Component - Tab List
Tabs.List = function ({ children }) {
  return (
    <div className="tab-list" style={{ display: "flex", gap: "8px" }}>
      {children}
    </div>
  );
};

// Child Component - Individual Tab Button
Tabs.Tab = function ({ index, children }) {
  const { activeTab, setActiveTab } = useContext(TabsContext);
  const isActive = activeTab === index;

  return (
    <button
      onClick={() => setActiveTab(index)}
      style={{
        padding: "8px 16px",
        backgroundColor: isActive ? "blue" : "gray",
        color: "white",
        border: "none",
        cursor: "pointer",
      }}
    >
      {children}
    </button>
  );
};

// Child Component - Tab Panel
Tabs.Panel = function ({ index, children }) {
  const { activeTab } = useContext(TabsContext);

  if (activeTab !== index) return null; // Only show active panel

  return (
    <div className="tab-panel" style={{ padding: "16px" }}>
      {children}
    </div>
  );
};

// Usage - Clean, declarative API!
function App() {
  return (
    <Tabs defaultTab={0}>
      <Tabs.List>
        <Tabs.Tab index={0}>Home</Tabs.Tab>
        <Tabs.Tab index={1}>Profile</Tabs.Tab>
        <Tabs.Tab index={2}>Settings</Tabs.Tab>
      </Tabs.List>

      <Tabs.Panel index={0}>
        <h2>Welcome Home!</h2>
      </Tabs.Panel>
      <Tabs.Panel index={1}>
        <h2>User Profile</h2>
      </Tabs.Panel>
      <Tabs.Panel index={2}>
        <h2>App Settings</h2>
      </Tabs.Panel>
    </Tabs>
  );
}
```

---

## Pattern Comparison

| Pattern      | How it Works                       | Best For                      | Modern Alternative    |
| ------------ | ---------------------------------- | ----------------------------- | --------------------- |
| HOC          | Wraps component, adds props        | Auth, loading, logging        | Custom Hooks          |
| Render Props | Function as prop                   | Shared logic with flexible UI | Custom Hooks          |
| Compound     | Components share state via Context | Complex UI (Tabs, Accordion)  | Still widely used     |
| Custom Hooks | Extract logic to function          | Reusable stateful logic       | ✅ Preferred approach |

---

---

# 11. Routing & Application Structure

---

## Introduction to React Router

**React Router** is the standard library for **client-side routing** in React. It enables navigation between pages **without full page reloads** (SPA behavior).

### Installation:

```bash
npm install react-router-dom
```

### Simple Interview Answer:

> React Router is a library that enables client-side routing in React SPAs. It allows users to navigate between different views/pages without full page reloads. It uses the browser's History API to change the URL and render the appropriate component.

---

## Defining Routes & Nested Routes

### Basic Routing Setup:

```jsx
import { BrowserRouter, Routes, Route, Link } from "react-router-dom";

function App() {
  return (
    <BrowserRouter>
      {/* Navigation Links */}
      <nav style={{ display: "flex", gap: "16px", padding: "16px" }}>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/contact">Contact</Link>
      </nav>

      {/* Route Definitions */}
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
        <Route path="*" element={<NotFound />} /> {/* 404 Catch-all */}
      </Routes>
    </BrowserRouter>
  );
}

function Home() {
  return <h1>🏠 Home Page</h1>;
}
function About() {
  return <h1>ℹ️ About Page</h1>;
}
function Contact() {
  return <h1>📧 Contact Page</h1>;
}
function NotFound() {
  return <h1>❌ 404 - Page Not Found</h1>;
}
```

### Nested Routes:

```jsx
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Layout />}>
          {/* Index route - shows when path is exactly "/" */}
          <Route index element={<Home />} />

          <Route path="about" element={<About />} />

          {/* Nested routes under /products */}
          <Route path="products" element={<Products />}>
            <Route index element={<ProductList />} />
            <Route path=":id" element={<ProductDetail />} />
          </Route>

          <Route path="*" element={<NotFound />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}

// Layout uses Outlet to render child routes
function Layout() {
  return (
    <div>
      <Header />
      <main>
        <Outlet /> {/* Child route renders here */}
      </main>
      <Footer />
    </div>
  );
}
```

---

## Dynamic Routing & URL Parameters

```jsx
import { useParams, useSearchParams } from "react-router-dom";

// Route: /users/:id
function UserProfile() {
  const { id } = useParams(); // Get URL parameter

  return <h1>User Profile: ID = {id}</h1>;
}

// Route: /products/:category/:id
function ProductDetail() {
  const { category, id } = useParams();

  return (
    <div>
      <h1>Product in {category}</h1>
      <p>Product ID: {id}</p>
    </div>
  );
}

// Query Parameters: /search?q=laptop&sort=price
function SearchPage() {
  const [searchParams] = useSearchParams();
  const query = searchParams.get("q");
  const sort = searchParams.get("sort");

  return (
    <div>
      <h1>Search Results for: "{query}"</h1>
      <p>Sorted by: {sort}</p>
    </div>
  );
}

// Routes definition
<Routes>
  <Route path="/users/:id" element={<UserProfile />} />
  <Route path="/products/:category/:id" element={<ProductDetail />} />
  <Route path="/search" element={<SearchPage />} />
</Routes>;
```

---

## useNavigate Hook – Programmatic Navigation

```jsx
import { useNavigate } from "react-router-dom";

function LoginForm() {
  const navigate = useNavigate();

  const handleLogin = () => {
    // Simulate login
    const isAuthenticated = true;

    if (isAuthenticated) {
      navigate("/dashboard"); // Go to dashboard
    } else {
      navigate("/login?error=true"); // Stay on login with error
    }
  };

  const goBack = () => {
    navigate(-1); // Go back one page (like browser back button)
  };

  const goForward = () => {
    navigate(1); // Go forward one page
  };

  const replacePage = () => {
    navigate("/home", { replace: true }); // Replace current history entry
  };

  return (
    <div>
      <button onClick={handleLogin}>Login</button>
      <button onClick={goBack}>Go Back</button>
    </div>
  );
}
```

---

## Layout Components – Shared Header/Sidebar

```jsx
import { Outlet, NavLink } from "react-router-dom";

function DashboardLayout() {
  return (
    <div style={{ display: "flex", minHeight: "100vh" }}>
      {/* Sidebar */}
      <aside
        style={{
          width: "250px",
          backgroundColor: "#333",
          color: "white",
          padding: "20px",
        }}
      >
        <h2>Dashboard</h2>
        <nav style={{ display: "flex", flexDirection: "column", gap: "10px" }}>
          <NavLink
            to="/dashboard"
            end
            style={({ isActive }) => ({
              color: isActive ? "yellow" : "white",
            })}
          >
            Overview
          </NavLink>
          <NavLink
            to="/dashboard/analytics"
            style={({ isActive }) => ({
              color: isActive ? "yellow" : "white",
            })}
          >
            Analytics
          </NavLink>
          <NavLink
            to="/dashboard/settings"
            style={({ isActive }) => ({
              color: isActive ? "yellow" : "white",
            })}
          >
            Settings
          </NavLink>
        </nav>
      </aside>

      {/* Main Content Area */}
      <main style={{ flex: 1, padding: "20px" }}>
        <Outlet /> {/* Nested route content renders here */}
      </main>
    </div>
  );
}

// Routes
<Routes>
  <Route path="/dashboard" element={<DashboardLayout />}>
    <Route index element={<Overview />} />
    <Route path="analytics" element={<Analytics />} />
    <Route path="settings" element={<Settings />} />
  </Route>
</Routes>;
```

---

## Folder Structure for Scalable Projects

```
src/
├── assets/              # Images, fonts, global CSS
│   ├── images/
│   └── styles/
│       └── global.css
│
├── components/          # Reusable UI components
│   ├── common/          # Generic components (Button, Input, Modal)
│   │   ├── Button.jsx
│   │   ├── Input.jsx
│   │   └── Modal.jsx
│   ├── layout/          # Layout components (Header, Sidebar, Footer)
│   │   ├── Header.jsx
│   │   ├── Sidebar.jsx
│   │   └── Footer.jsx
│   └── features/        # Feature-specific components
│       ├── UserCard.jsx
│       └── ProductCard.jsx
│
├── hooks/               # Custom hooks
│   ├── useFetch.js
│   ├── useAuth.js
│   └── useLocalStorage.js
│
├── pages/               # Page components (one per route)
│   ├── Home.jsx
│   ├── About.jsx
│   ├── Login.jsx
│   ├── Dashboard.jsx
│   └── NotFound.jsx
│
├── services/            # API calls and external services
│   ├── api.js           # Axios/Fetch configuration
│   ├── userService.js   # User-related API calls
│   └── productService.js
│
├── utils/               # Utility/helper functions
│   ├── formatDate.js
│   ├── validateEmail.js
│   └── constants.js
│
├── context/             # React Context providers
│   ├── AuthContext.jsx
│   └── ThemeContext.jsx
│
├── App.jsx              # Root component with routes
├── main.jsx             # Entry point
└── index.css            # Global styles
```

---

---

# 12. Server State & API Integration

---

## Fetch API Integration in React

```jsx
import { useState, useEffect } from "react";

function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchUsers = async () => {
      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/users",
        );
        if (!response.ok) throw new Error("Failed to fetch");
        const data = await response.json();
        setUsers(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchUsers();
  }, []);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

---

## Handling Loading & Error States Gracefully

```jsx
function DataDisplay({ url }) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    let isMounted = true;

    const fetchData = async () => {
      setLoading(true);
      setError(null);

      try {
        const res = await fetch(url);
        if (!res.ok) throw new Error(`Error: ${res.status}`);
        const result = await res.json();
        if (isMounted) setData(result);
      } catch (err) {
        if (isMounted) setError(err.message);
      } finally {
        if (isMounted) setLoading(false);
      }
    };

    fetchData();
    return () => {
      isMounted = false;
    };
  }, [url]);

  // Graceful UI for each state
  if (loading) {
    return (
      <div style={{ textAlign: "center", padding: "40px" }}>
        <div className="spinner">⏳</div>
        <p>Loading data, please wait...</p>
      </div>
    );
  }

  if (error) {
    return (
      <div style={{ color: "red", textAlign: "center", padding: "20px" }}>
        <h2>❌ Something went wrong</h2>
        <p>{error}</p>
        <button onClick={() => window.location.reload()}>Try Again</button>
      </div>
    );
  }

  if (!data || data.length === 0) {
    return <p>No data available.</p>;
  }

  return (
    <div>
      {data.map((item) => (
        <div key={item.id}>{item.name || item.title}</div>
      ))}
    </div>
  );
}
```

---

## Separation of Server State vs Client State

### What's the Difference?

| Feature    | Client State                              | Server State                          |
| ---------- | ----------------------------------------- | ------------------------------------- |
| Source     | Created in browser                        | Comes from server/database            |
| Examples   | Form inputs, UI toggles, modal open/close | User data, products, posts            |
| Ownership  | App owns it                               | Server owns it (app has a copy)       |
| Syncing    | No sync needed                            | Can become stale, needs refetching    |
| Managed by | useState                                  | useEffect + fetch (or TanStack Query) |

```jsx
function Dashboard() {
  // CLIENT STATE - UI concerns
  const [isSidebarOpen, setIsSidebarOpen] = useState(true);
  const [searchTerm, setSearchTerm] = useState("");
  const [selectedTab, setSelectedTab] = useState("overview");

  // SERVER STATE - Data from API
  const [users, setUsers] = useState([]);
  const [orders, setOrders] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Fetch server state
    fetch("/api/users").then(r => r.json()).then(setUsers);
    fetch("/api/orders").then(r => r.json()).then(setOrders);
  }, []);

  return (/* UI */);
}
```

### Why Separate Them?

```
Server State problems when managed with useState:
❌ No caching (refetches on every mount)
❌ No automatic refetching when data changes
❌ No background updates
❌ No deduplication (same API called multiple times)
❌ Manual loading/error handling everywhere

Solution: Use TanStack Query (React Query) ✅
```

---

## Introduction to TanStack Query (React Query)

**TanStack Query** is a library that manages **server state** with smart caching, automatic refetching, and background updates.

### Installation:

```bash
npm install @tanstack/react-query
```

### Setup:

```jsx
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

const queryClient = new QueryClient();

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <YourApp />
    </QueryClientProvider>
  );
}
```

### Simple Interview Answer:

> TanStack Query (formerly React Query) is a server state management library that handles data fetching, caching, synchronization, and background updates automatically. Instead of manually managing loading, error, and data states with useState and useEffect, TanStack Query provides the useQuery hook that handles all of this out of the box with built-in caching and refetching.

---

## useQuery Basics – Data Fetching Pattern

```jsx
import { useQuery } from "@tanstack/react-query";

function UserList() {
  const {
    data: users, // Fetched data
    isLoading, // First time loading
    isError, // Error occurred
    error, // Error object
    isFetching, // Fetching in background (refetch)
    refetch, // Manual refetch function
  } = useQuery({
    queryKey: ["users"], // Unique key for caching
    queryFn: async () => {
      const res = await fetch("https://jsonplaceholder.typicode.com/users");
      if (!res.ok) throw new Error("Failed to fetch");
      return res.json();
    },
    staleTime: 5 * 60 * 1000, // Data is fresh for 5 minutes
    refetchOnWindowFocus: true, // Refetch when user comes back to tab
  });

  if (isLoading) return <p>Loading users...</p>;
  if (isError) return <p>Error: {error.message}</p>;

  return (
    <div>
      <button onClick={() => refetch()}>🔄 Refresh</button>
      {isFetching && <span> (updating...)</span>}
      <ul>
        {users.map((user) => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

### useQuery with Dynamic Parameters:

```jsx
function UserProfile({ userId }) {
  const { data: user, isLoading } = useQuery({
    queryKey: ["user", userId], // Key includes userId → refetches when userId changes
    queryFn: async () => {
      const res = await fetch(
        `https://jsonplaceholder.typicode.com/users/${userId}`,
      );
      return res.json();
    },
    enabled: !!userId, // Only fetch when userId exists
  });

  if (isLoading) return <p>Loading...</p>;

  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  );
}
```

---

## Mutations – Updating Server Data

**Mutations** are used to **create, update, or delete** data on the server.

```jsx
import { useMutation, useQueryClient } from "@tanstack/react-query";

function AddUserForm() {
  const queryClient = useQueryClient();

  const mutation = useMutation({
    mutationFn: async (newUser) => {
      const res = await fetch("https://jsonplaceholder.typicode.com/users", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(newUser),
      });
      return res.json();
    },

    onSuccess: () => {
      // Invalidate and refetch users list after adding new user
      queryClient.invalidateQueries({ queryKey: ["users"] });
      alert("User added successfully!");
    },

    onError: (error) => {
      alert("Failed to add user: " + error.message);
    },
  });

  const handleSubmit = (e) => {
    e.preventDefault();
    const formData = new FormData(e.target);

    mutation.mutate({
      name: formData.get("name"),
      email: formData.get("email"),
    });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="name" placeholder="Name" required />
      <input name="email" placeholder="Email" required />
      <button type="submit" disabled={mutation.isPending}>
        {mutation.isPending ? "Adding..." : "Add User"}
      </button>
      {mutation.isError && (
        <p style={{ color: "red" }}>Error: {mutation.error.message}</p>
      )}
      {mutation.isSuccess && <p style={{ color: "green" }}>User added!</p>}
    </form>
  );
}
```

### useQuery vs useMutation

| Feature    | useQuery                 | useMutation                    |
| ---------- | ------------------------ | ------------------------------ |
| Purpose    | READ data (GET)          | WRITE data (POST, PUT, DELETE) |
| Runs       | Automatically on mount   | Manually via `.mutate()`       |
| Caching    | ✅ Automatic             | ❌ No caching                  |
| Key needed | ✅ queryKey              | ❌ No key                      |
| Returns    | data, isLoading, isError | mutate, isPending, isSuccess   |

---

## Final Quick Summary

```
7. Hooks
   ├── Rules: Top level only, React functions only
   ├── useState → Local state
   ├── useEffect → Side effects
   ├── useContext → Shared data (no prop drilling)
   ├── useRef → DOM refs, no re-render
   ├── useCallback → Cache functions
   └── useMemo → Cache calculations

8. useEffect Deep Dive
   ├── Syncs component with external systems
   ├── [] = once | [dep] = on change | no array = every render
   ├── Cleanup prevents memory leaks (timers, listeners)
   └── Infinite loops: objects/arrays as deps, setting state without deps

9. Custom Hooks
   ├── Functions starting with "use" that call other hooks
   ├── Extract shared logic (useFetch, useLocalStorage)
   └── Separate business logic from UI

10. Advanced Patterns
    ├── HOC → Function wraps component (withAuth)
    ├── Render Props → Function as prop for flexible UI
    └── Compound → Components share state via Context (Tabs)

11. Routing
    ├── React Router = client-side navigation
    ├── <Route path="/users/:id" element={<User />} />
    ├── useParams() → URL params | useNavigate() → programmatic
    └── Layout with <Outlet /> for shared structure

12. Server State
    ├── Client state (UI) vs Server state (API data)
    ├── Fetch + useState + useEffect = manual way
    ├── TanStack Query = smart caching + auto refetch
    ├── useQuery → Read data | useMutation → Write data
    └── queryClient.invalidateQueries() → Refetch after mutation
```

---

> 💡 **Interview Tips:**
>
> - **Rules of Hooks**: Top level only, React functions only – always mention this first
> - `useEffect` cleanup is **mandatory** for timers, listeners, and subscriptions
> - **Custom hooks** must start with "use" – this is how React recognizes them
> - **React Router** uses `<BrowserRouter>`, `<Routes>`, `<Route>`, and `<Link>`
> - **TanStack Query** replaces manual `useState + useEffect` for server data
> - `useQuery` for **reading**, `useMutation` for **writing** server data
> - Always mention **queryKey** for caching and **invalidateQueries** for refetching after mutations
