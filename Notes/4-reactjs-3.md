# React Complete Notes – Interview Ready ⚛️

---

# 13. Global State Management

---

## Context API – Managing App-Wide State

**Context API** is React's **built-in solution** for sharing data across the entire component tree **without prop drilling**.

### Simple Interview Answer:

> Context API is a built-in React feature that allows you to share state globally across all components without passing props through every level. It works with a Provider that wraps the app and a Consumer (or useContext hook) that reads the data in any child component. It's ideal for data like theme, authentication, and language settings.

### The Problem Context Solves:

```
App
 └── Layout
      └── Sidebar
           └── Menu
                └── MenuItem  ← Needs "user" data
                                    ↑
                        Props passed through 4 levels! ❌ (Prop Drilling)
```

### Creating Context – Step by Step:

```jsx
import { createContext, useContext, useState } from "react";

// Step 1: Create Context
const AuthContext = createContext(null);

// Step 2: Create Provider Component
function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  const login = (userData) => {
    setUser(userData);
    setIsLoggedIn(true);
  };

  const logout = () => {
    setUser(null);
    setIsLoggedIn(false);
  };

  // Value object shared with all children
  const value = { user, isLoggedIn, login, logout };

  return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
}

// Step 3: Create Custom Hook for easy consumption
function useAuth() {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error("useAuth must be used within AuthProvider");
  }
  return context;
}

// Step 4: Wrap your app with Provider
function App() {
  return (
    <AuthProvider>
      <Header />
      <MainContent />
    </AuthProvider>
  );
}

// Step 5: Use in ANY child component (no props needed!)
function Header() {
  const { user, isLoggedIn, logout } = useAuth();

  return (
    <header>
      {isLoggedIn ? (
        <div>
          <span>Welcome, {user.name}!</span>
          <button onClick={logout}>Logout</button>
        </div>
      ) : (
        <span>Please Login</span>
      )}
    </header>
  );
}

function ProfilePage() {
  const { user, isLoggedIn, login } = useAuth();

  if (!isLoggedIn) {
    return (
      <button
        onClick={() => login({ name: "Rahul", email: "rahul@gmail.com" })}
      >
        Login
      </button>
    );
  }

  return <h1>Profile: {user.name}</h1>;
}
```

---

## Context Providers & Consumers

### Provider

- **Wraps** the part of the app that needs access to shared data
- **Provides** the value to all children

### Consumer (Two Ways)

```jsx
// Way 1: useContext Hook (Modern ✅)
function MyComponent() {
  const { theme } = useContext(ThemeContext);
  return <p>Theme: {theme}</p>;
}

// Way 2: Consumer Component (Old way, rarely used now)
function MyComponent() {
  return (
    <ThemeContext.Consumer>
      {({ theme }) => <p>Theme: {theme}</p>}
    </ThemeContext.Consumer>
  );
}
```

### Multiple Contexts Example:

```jsx
// Theme Context
const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");
  const toggleTheme = () => setTheme((t) => (t === "light" ? "dark" : "light"));

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// Language Context
const LangContext = createContext();

function LangProvider({ children }) {
  const [lang, setLang] = useState("en");
  return (
    <LangContext.Provider value={{ lang, setLang }}>
      {children}
    </LangContext.Provider>
  );
}

// Nest multiple providers
function App() {
  return (
    <ThemeProvider>
      <LangProvider>
        <AuthProvider>
          <Header />
          <MainContent />
        </AuthProvider>
      </LangProvider>
    </ThemeProvider>
  );
}
```

---

## Zustand – Lightweight Alternative to Redux

**Zustand** is a small, fast, and simple state management library. No boilerplate, no providers needed.

### Installation:

```bash
npm install zustand
```

### Simple Interview Answer:

> Zustand is a lightweight state management library for React. Unlike Redux, it doesn't need providers, actions, or reducers. You create a store with a simple function and use it directly in any component. It's much simpler than Redux and great for small to medium apps.

### Creating a Zustand Store:

```jsx
import { create } from "zustand";

// Create store - that's it! No provider, no boilerplate
const useStore = create((set) => ({
  // State
  count: 0,
  user: null,
  todos: [],

  // Actions (functions that update state)
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),

  setUser: (user) => set({ user }),
  logout: () => set({ user: null }),

  addTodo: (todo) =>
    set((state) => ({
      todos: [...state.todos, { id: Date.now(), text: todo, done: false }],
    })),

  toggleTodo: (id) =>
    set((state) => ({
      todos: state.todos.map((t) =>
        t.id === id ? { ...t, done: !t.done } : t,
      ),
    })),
}));

// Use in ANY component - no Provider needed!
function Counter() {
  const count = useStore((state) => state.count); // Select only what you need
  const increment = useStore((state) => state.increment);
  const decrement = useStore((state) => state.decrement);

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </div>
  );
}

function TodoList() {
  const todos = useStore((state) => state.todos);
  const addTodo = useStore((state) => state.addTodo);

  return (
    <div>
      <button onClick={() => addTodo("Learn Zustand")}>Add Todo</button>
      {todos.map((todo) => (
        <p key={todo.id}>
          {todo.done ? "✅" : "⬜"} {todo.text}
        </p>
      ))}
    </div>
  );
}
```

### Context API vs Zustand vs Redux

| Feature         | Context API              | Zustand                | Redux                     |
| --------------- | ------------------------ | ---------------------- | ------------------------- |
| Setup           | Built-in (no install)    | Simple (1 function)    | Complex (boilerplate)     |
| Provider needed | ✅ Yes                   | ❌ No                  | ✅ Yes                    |
| Performance     | Re-renders all consumers | Only changed selectors | Only connected components |
| DevTools        | ❌ No                    | ✅ Yes                 | ✅ Excellent              |
| Boilerplate     | Low                      | Very Low               | High                      |
| Best for        | Theme, Auth, Language    | Small-Medium apps      | Large enterprise apps     |
| Learning curve  | Easy                     | Very Easy              | Steep                     |

---

## When to Use Global State vs Local State

```
LOCAL STATE (useState) - Use when:
✅ Data is used by ONE component only
✅ UI toggles (modal open/close, dropdown)
✅ Form input values
✅ Temporary UI state (hover, active tab)

SHARED STATE (Lifting Up) - Use when:
✅ Data shared between 2-3 sibling components
✅ Parent-child communication

GLOBAL STATE (Context / Zustand / Redux) - Use when:
✅ Data needed by MANY components across the app
✅ Authentication (logged-in user)
✅ Theme (dark/light mode)
✅ Shopping cart data
✅ Language/locale settings
```

### Simple Interview Answer:

> I use local state with useState for data that belongs to a single component. If two siblings need the same data, I lift state up to the parent. I only use global state like Context or Zustand when many components across different parts of the app need the same data, like user authentication or theme. Overusing global state makes the app harder to debug and maintain.

---

## Avoiding Overengineering State Logic

```
❌ Overengineering Mistakes:
├── Using Redux for a simple counter app
├── Putting form inputs in global state
├── Storing derived data (calculate it instead!)
├── Creating context for data used by only 2 components
├── Using Zustand when useState is enough
└── Multiple layers of state management in a small app

✅ Best Practices:
├── Start with useState (local)
├── Lift state up when siblings share data
├── Use Context for truly global data (auth, theme)
├── Use Zustand/Redux only when app is complex
├── Derive data instead of storing it
└── Keep state as close to where it's used as possible
```

### Simple Interview Answer:

> I follow the principle of keeping state as local as possible. I start with useState, then lift state up if needed, and only move to global state when many components truly need the same data. Overengineering state management adds unnecessary complexity, makes debugging harder, and slows down development. The best state management is the simplest one that solves the problem.

---

---

# 14. State Management Using Redux 🏪

---

## Introduction to Redux

### What is Redux?

**Redux** is a **predictable state management library** for JavaScript applications. It stores the entire application state in a **single central location** called the **Store**.

### Simple Interview Answer:

> Redux is a state management library that stores the entire application state in a single centralized store. It follows a strict unidirectional data flow where actions describe what happened, reducers calculate the new state, and the store holds the state. It makes state changes predictable and easy to debug.

### When and Why Use Redux?

```
Use Redux When:
✅ Large application with complex state
✅ Many components need the same data
✅ State changes frequently and from many sources
✅ You need time-travel debugging
✅ Multiple developers working on the same codebase
✅ State updates need to be predictable and traceable

DON'T Use Redux When:
❌ Small app with simple state
❌ Only a few components share data (use Context)
❌ State is mostly local to components
❌ You're just learning React (master useState first)
```

---

## Principles of Redux

Redux follows **three core principles**:

### 1. Single Source of Truth

```
The entire application state is stored in ONE object tree inside a SINGLE store.

Store = {
  user: { name: "Rahul", loggedIn: true },
  cart: { items: [], total: 0 },
  theme: "dark"
}
```

### 2. State is Read-Only

```
You CANNOT directly change the state.
The only way to change state is by dispatching an ACTION.

❌ store.state.user.name = "Amit"     // NOT allowed!
✅ store.dispatch({ type: "SET_NAME", payload: "Amit" })  // Correct!
```

### 3. Changes are Made with Pure Functions (Reducers)

```
Reducers are pure functions that take the current state and an action,
and return a NEW state. They never modify the original state.

function userReducer(state, action) {
  // Return NEW state object, never mutate
  return { ...state, name: action.payload };
}
```

---

## Redux Flow

```
User clicks button
    ↓
Component dispatches an ACTION
    ↓  { type: "ADD_TO_CART", payload: { id: 1, name: "Laptop" } }
    ↓
Store sends action to REDUCER
    ↓
Reducer calculates NEW state (pure function)
    ↓
Store saves new state
    ↓
React components re-render with new state
    ↓
UI Updated! ✅
```

### Visual Flow:

```
┌─────────────┐    dispatch(action)   ┌──────────┐
│  Component  │ ──────────────────→   │  Store   │
│  (React)    │                       │ (State)  │
└─────────────┘                       └────┬─────┘
       ↑                                   │
       │ subscribe                         │ sends action
       │ (re-render)                       ↓
       │                             ┌──────────┐
       └──────────────────────────── │ Reducer  │
                                     │ (Pure Fn)│
                                     └──────────┘
```

---

## Redux Basics: Actions, Reducers, Store

### 1. Actions

**Actions** are plain JavaScript objects that describe **what happened**.

```js
// Action must have a "type" property
const addToCart = {
  type: "ADD_TO_CART",
  payload: { id: 1, name: "Laptop", price: 50000 },
};

const removeFromCart = {
  type: "REMOVE_FROM_CART",
  payload: 1, // product id
};

const increment = {
  type: "INCREMENT",
  // No payload needed
};

// Action Creator - Function that returns an action
function addToCartAction(product) {
  return {
    type: "ADD_TO_CART",
    payload: product,
  };
}
```

### 2. Reducers

**Reducers** are **pure functions** that take current state + action and return NEW state.

```js
// Initial State
const initialState = {
  items: [],
  total: 0,
};

// Cart Reducer
function cartReducer(state = initialState, action) {
  switch (action.type) {
    case "ADD_TO_CART":
      return {
        ...state, // Keep existing state
        items: [...state.items, action.payload], // Add new item
        total: state.total + action.payload.price, // Update total
      };

    case "REMOVE_FROM_CART":
      const removedItem = state.items.find((i) => i.id === action.payload);
      return {
        ...state,
        items: state.items.filter((i) => i.id !== action.payload),
        total: state.total - removedItem.price,
      };

    case "CLEAR_CART":
      return initialState; // Reset to initial state

    default:
      return state; // ALWAYS return current state for unknown actions
  }
}
```

### Why Reducers Must Be Pure Functions:

```
Pure Function Rules:
✅ Same input → Same output (always)
✅ No side effects (no API calls, no console.log, no Date.now())
✅ Don't mutate arguments (don't change state directly)
✅ Return a NEW object

Why?
✅ Predictable - same action always produces same state
✅ Testable - easy to unit test
✅ Time-travel debugging - Redux can replay actions
✅ Redux compares old vs new state by reference to detect changes

❌ Impure Example (WRONG):
function badReducer(state, action) {
  state.items.push(action.payload);  // ❌ Mutating state directly!
  state.total += action.payload.price;
  return state;
}

✅ Pure Example (CORRECT):
function goodReducer(state, action) {
  return {
    ...state,
    items: [...state.items, action.payload],  // ✅ New array
    total: state.total + action.payload.price
  };
}
```

### 3. Store

**Store** is the object that holds the entire application state.

```js
import { createStore } from "redux";

// Create store with reducer
const store = createStore(cartReducer);

// Get current state
console.log(store.getState());
// { items: [], total: 0 }

// Dispatch action to change state
store.dispatch({
  type: "ADD_TO_CART",
  payload: { id: 1, name: "Laptop", price: 50000 },
});

console.log(store.getState());
// { items: [{id:1, name:"Laptop", price:50000}], total: 50000 }

// Subscribe to state changes
store.subscribe(() => {
  console.log("State changed!", store.getState());
});
```

---

## Currying in Redux

**Currying** is a technique where a function takes arguments **one at a time**, returning a new function for each argument.

```js
// Normal function
function add(a, b) {
  return a + b;
}
add(2, 3); // 5

// Curried function
function curriedAdd(a) {
  return function (b) {
    return a + b;
  };
}
curriedAdd(2)(3); // 5

// Arrow function currying
const curriedAdd = (a) => (b) => a + b;
curriedAdd(2)(3); // 5
```

### Where Currying is Used in Redux:

```js
// Redux Middleware uses currying
const loggerMiddleware = (store) => (next) => (action) => {
  console.log("Dispatching:", action);
  let result = next(action);
  console.log("Next State:", store.getState());
  return result;
};

// connect() in react-redux uses currying
const enhance = connect(mapStateToProps, mapDispatchToProps);
const ConnectedComponent = enhance(MyComponent);
// Same as: connect(mapState, mapDispatch)(MyComponent)
```

---

## Middleware

**Middleware** sits between dispatching an action and the reducer. It can intercept, modify, or delay actions.

```js
// Logger Middleware - logs every action
const logger = (store) => (next) => (action) => {
  console.log("Previous State:", store.getState());
  console.log("Action:", action);
  const result = next(action); // Pass action to next middleware/reducer
  console.log("Next State:", store.getState());
  return result;
};

// Apply middleware
import { createStore, applyMiddleware } from "redux";

const store = createStore(rootReducer, applyMiddleware(logger));
```

---

## Async Actions: Thunk

**Redux Thunk** is middleware that allows you to write **async logic** (like API calls) in action creators.

### Why Thunk?

```
Normal Redux actions are synchronous:
dispatch({ type: "ADD_USER", payload: user })  // Instant!

But API calls are asynchronous:
fetch("/api/users")  // Takes time! How to dispatch after data arrives?

Thunk solves this by allowing action creators to return FUNCTIONS instead of objects.
```

### Setup:

```bash
npm install redux-thunk
```

### Thunk Example:

```js
// Thunk Action Creator - returns a FUNCTION instead of an object
function fetchUsers() {
  return async function (dispatch, getState) {
    // Dispatch loading action
    dispatch({ type: "FETCH_USERS_LOADING" });

    try {
      const response = await fetch(
        "https://jsonplaceholder.typicode.com/users",
      );
      const data = await response.json();

      // Dispatch success action with data
      dispatch({ type: "FETCH_USERS_SUCCESS", payload: data });
    } catch (error) {
      // Dispatch error action
      dispatch({ type: "FETCH_USERS_ERROR", payload: error.message });
    }
  };
}

// Reducer handles all three states
function usersReducer(
  state = { data: [], loading: false, error: null },
  action,
) {
  switch (action.type) {
    case "FETCH_USERS_LOADING":
      return { ...state, loading: true, error: null };
    case "FETCH_USERS_SUCCESS":
      return { ...state, loading: false, data: action.payload };
    case "FETCH_USERS_ERROR":
      return { ...state, loading: false, error: action.payload };
    default:
      return state;
  }
}
```

---

## Connecting Redux to React with react-redux

### Installation:

```bash
npm install react-redux
```

### Setup Provider:

```jsx
import { Provider } from "react-redux";
import { createStore } from "redux";

const store = createStore(rootReducer);

function App() {
  return (
    <Provider store={store}>
      {" "}
      {/* Wrap entire app */}
      <Header />
      <MainContent />
    </Provider>
  );
}
```

### Using useSelector & useDispatch (Modern Hooks Way ✅):

```jsx
import { useSelector, useDispatch } from "react-redux";

function CartComponent() {
  // Read state from store
  const items = useSelector((state) => state.cart.items);
  const total = useSelector((state) => state.cart.total);

  // Get dispatch function
  const dispatch = useDispatch();

  const handleAdd = (product) => {
    dispatch({ type: "ADD_TO_CART", payload: product });
  };

  const handleRemove = (id) => {
    dispatch({ type: "REMOVE_FROM_CART", payload: id });
  };

  return (
    <div>
      <h2>Cart ({items.length} items)</h2>
      <p>Total: ₹{total}</p>
      {items.map((item) => (
        <div key={item.id}>
          {item.name} - ₹{item.price}
          <button onClick={() => handleRemove(item.id)}>Remove</button>
        </div>
      ))}
      <button onClick={() => handleAdd({ id: 1, name: "Phone", price: 20000 })}>
        Add Phone
      </button>
    </div>
  );
}
```

---

## Introduction to Redux Toolkit (RTK)

**Redux Toolkit** is the **official, recommended** way to write Redux. It eliminates boilerplate and simplifies setup.

### Installation:

```bash
npm install @reduxjs/toolkit react-redux
```

### Simple Interview Answer:

> Redux Toolkit is the official recommended way to use Redux. It simplifies Redux by eliminating boilerplate code. Instead of writing separate action types, action creators, and reducers, RTK's createSlice combines them all into one. It also includes Redux Thunk by default and uses Immer internally so you can write "mutating" code that's actually immutable.

### Redux vs Redux Toolkit Comparison:

```
Old Redux (LOTS of boilerplate):
❌ Define action type constants
❌ Write action creator functions
❌ Write switch-case reducers
❌ Manually spread state for immutability
❌ Configure thunk middleware separately
❌ Complex store setup

Redux Toolkit (Clean & Simple):
✅ createSlice() handles actions + reducers together
✅ Immer built-in (write "mutating" code safely)
✅ Thunk included by default
✅ configureStore() simplifies setup
✅ Less code, fewer bugs
```

### Complete RTK Example:

```jsx
// Step 1: Create a Slice (store/features/counterSlice.js)
import { createSlice } from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",
  initialState: {
    value: 0,
    status: "idle",
  },
  reducers: {
    increment: (state) => {
      state.value += 1; // ✅ Immer allows "mutating" syntax!
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    },
    reset: (state) => {
      state.value = 0;
    },
  },
});

// Export actions and reducer
export const { increment, decrement, incrementByAmount, reset } =
  counterSlice.actions;
export default counterSlice.reducer;
```

```jsx
// Step 2: Async Thunk with RTK (store/features/usersSlice.js)
import { createSlice, createAsyncThunk } from "@reduxjs/toolkit";

// Async thunk for API calls
export const fetchUsers = createAsyncThunk("users/fetchUsers", async () => {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  return response.json();
});

const usersSlice = createSlice({
  name: "users",
  initialState: {
    data: [],
    loading: false,
    error: null,
  },
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(fetchUsers.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.loading = false;
        state.data = action.payload;
      })
      .addCase(fetchUsers.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message;
      });
  },
});

export default usersSlice.reducer;
```

```jsx
// Step 3: Configure Store (store/index.js)
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "./features/counterSlice";
import usersReducer from "./features/usersSlice";

const store = configureStore({
  reducer: {
    counter: counterReducer,
    users: usersReducer,
  },
});

export default store;
```

```jsx
// Step 4: Wrap App with Provider (main.jsx)
import { Provider } from "react-redux";
import store from "./store";

function App() {
  return (
    <Provider store={store}>
      <Counter />
      <UserList />
    </Provider>
  );
}
```

```jsx
// Step 5: Use in Components
import { useSelector, useDispatch } from "react-redux";
import {
  increment,
  decrement,
  incrementByAmount,
} from "./store/features/counterSlice";
import { fetchUsers } from "./store/features/usersSlice";
import { useEffect } from "react";

function Counter() {
  const count = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
      <button onClick={() => dispatch(incrementByAmount(5))}>+5</button>
    </div>
  );
}

function UserList() {
  const { data, loading, error } = useSelector((state) => state.users);
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(fetchUsers());
  }, [dispatch]);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <ul>
      {data.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

---

## Alternatives: Recoil, Zustand, MobX

| Library           | Size        | Complexity | Best For                           |
| ----------------- | ----------- | ---------- | ---------------------------------- |
| **Redux Toolkit** | Medium      | Medium     | Large enterprise apps              |
| **Zustand**       | Tiny (~1KB) | Very Low   | Small-Medium apps                  |
| **Recoil**        | Small       | Low        | Complex state with atoms (by Meta) |
| **MobX**          | Medium      | Low        | OOP-style reactive state           |
| **Context API**   | Built-in    | Low        | Simple global state (theme, auth)  |
| **Jotai**         | Tiny        | Very Low   | Atomic state management            |

### Simple Interview Answer:

> Redux Toolkit is the industry standard for large applications. For smaller apps, I prefer Zustand because it has almost zero boilerplate. Context API is fine for simple global state like theme or auth. The choice depends on app size, team experience, and complexity of state management needs.

---

---

# 15. Advanced Forms & Validation

---

## React Hook Form – Efficient Form Handling

**React Hook Form** is a library that makes form handling **fast, simple, and performant** by minimizing re-renders.

### Installation:

```bash
npm install react-hook-form
```

### Simple Interview Answer:

> React Hook Form is a performant form library that uses uncontrolled inputs and refs instead of state for each field. This means the form doesn't re-render on every keystroke, making it much faster than traditional controlled forms. It also provides built-in validation, error handling, and integrates well with validation libraries like Zod and Yup.

### Basic Usage:

```jsx
import { useForm } from "react-hook-form";

function RegistrationForm() {
  const {
    register, // Connect input to form
    handleSubmit, // Handle form submission
    formState: { errors, isSubmitting }, // Access errors and status
  } = useForm();

  const onSubmit = async (data) => {
    console.log("Form Data:", data);
    // Send to API
    await fetch("/api/register", {
      method: "POST",
      body: JSON.stringify(data),
    });
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* Name Field */}
      <input
        {...register("name", {
          required: "Name is required",
          minLength: { value: 3, message: "Min 3 characters" },
        })}
        placeholder="Full Name"
      />
      {errors.name && (
        <span style={{ color: "red" }}>{errors.name.message}</span>
      )}

      {/* Email Field */}
      <input
        {...register("email", {
          required: "Email is required",
          pattern: {
            value: /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i,
            message: "Invalid email address",
          },
        })}
        placeholder="Email"
      />
      {errors.email && (
        <span style={{ color: "red" }}>{errors.email.message}</span>
      )}

      {/* Password Field */}
      <input
        type="password"
        {...register("password", {
          required: "Password is required",
          minLength: { value: 6, message: "Min 6 characters" },
        })}
        placeholder="Password"
      />
      {errors.password && (
        <span style={{ color: "red" }}>{errors.password.message}</span>
      )}

      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? "Submitting..." : "Register"}
      </button>
    </form>
  );
}
```

---

## Controlled vs Uncontrolled Forms

### Controlled Form (React manages value)

```jsx
function ControlledForm() {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");

  // Every keystroke triggers re-render
  return (
    <form>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <input value={email} onChange={(e) => setEmail(e.target.value)} />
    </form>
  );
}
```

### Uncontrolled Form (DOM manages value)

```jsx
function UncontrolledForm() {
  const nameRef = useRef();
  const emailRef = useRef();

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(nameRef.current.value); // Read value from DOM
    console.log(emailRef.current.value);
  };

  // No re-renders on keystroke!
  return (
    <form onSubmit={handleSubmit}>
      <input ref={nameRef} />
      <input ref={emailRef} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Comparison

| Feature              | Controlled                        | Uncontrolled              |
| -------------------- | --------------------------------- | ------------------------- |
| Value managed by     | React state                       | DOM                       |
| Re-renders on input  | ✅ Yes (every keystroke)          | ❌ No                     |
| Performance          | Slower for large forms            | Faster                    |
| Validation           | Real-time                         | On submit                 |
| Reset form           | Set state to initial              | `formRef.current.reset()` |
| React Hook Form uses | ❌ No                             | ✅ Yes (under the hood)   |
| Best for             | Small forms, real-time validation | Large forms, performance  |

---

## Zod Schema Validation – Type-Safe Validation

**Zod** is a TypeScript-first schema validation library that works perfectly with React Hook Form.

### Installation:

```bash
npm install zod @hookform/resolvers
```

### Simple Interview Answer:

> Zod is a TypeScript-first validation library that lets you define schemas for your data. When combined with React Hook Form via the resolver, it provides type-safe validation where the form data type is automatically inferred from the validation schema. This eliminates the mismatch between validation rules and TypeScript types.

### Complete Example:

```jsx
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';

// Step 1: Define validation schema with Zod
const userSchema = z.object({
  name: z
    .string()
    .min(3, "Name must be at least 3 characters")
    .max(50, "Name too long"),

  email: z
    .string()
    .email("Invalid email address"),

  age: z
    .number()
    .min(18, "Must be at least 18")
    .max(120, "Invalid age"),

  password: z
    .string()
    .min(8, "Password must be at least 8 characters")
    .regex(/[A-Z]/, "Must contain uppercase letter")
    .regex(/[0-9]/, "Must contain a number"),

  confirmPassword: z.string()
}).refine((data) => data.password === data.confirmPassword, {
  message: "Passwords don't match",
  path: ["confirmPassword"]
});

// Infer TypeScript type from schema
type UserFormData = z.infer<typeof userSchema>;

// Step 2: Use with React Hook Form
function RegistrationForm() {
  const {
    register,
    handleSubmit,
    formState: { errors }
  } = useForm<UserFormData>({
    resolver: zodResolver(userSchema),   // Connect Zod to React Hook Form
    defaultValues: {
      name: "",
      email: "",
      age: 0,
      password: "",
      confirmPassword: ""
    }
  });

  const onSubmit = (data: UserFormData) => {
    console.log("Valid data:", data);
    // data is fully typed! ✅
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("name")} placeholder="Name" />
      {errors.name && <span className="error">{errors.name.message}</span>}

      <input {...register("email")} placeholder="Email" />
      {errors.email && <span className="error">{errors.email.message}</span>}

      <input type="number" {...register("age", { valueAsNumber: true })} placeholder="Age" />
      {errors.age && <span className="error">{errors.age.message}</span>}

      <input type="password" {...register("password")} placeholder="Password" />
      {errors.password && <span className="error">{errors.password.message}</span>}

      <input type="password" {...register("confirmPassword")} placeholder="Confirm Password" />
      {errors.confirmPassword && <span className="error">{errors.confirmPassword.message}</span>}

      <button type="submit">Register</button>
    </form>
  );
}
```

---

## Error Display Patterns

```jsx
// Pattern 1: Inline Errors (below each field)
{errors.email && <span style={{ color: "red", fontSize: "12px" }}>{errors.email.message}</span>}

// Pattern 2: Error Summary (top of form)
{Object.keys(errors).length > 0 && (
  <div style={{ color: "red", padding: "10px", border: "1px solid red" }}>
    <h3>Please fix the following errors:</h3>
    <ul>
      {Object.entries(errors).map(([field, error]) => (
        <li key={field}>{error.message}</li>
      ))}
    </ul>
  </div>
)}

// Pattern 3: Highlight Input Border
<input
  {...register("email")}
  style={{
    border: errors.email ? "2px solid red" : "1px solid gray"
  }}
/>

// Pattern 4: Reusable Error Component
function FieldError({ error }) {
  if (!error) return null;
  return <span style={{ color: "red", fontSize: "12px", display: "block" }}>{error.message}</span>;
}

// Usage
<input {...register("name")} />
<FieldError error={errors.name} />
```

---

---

# 16. Performance Optimization in React

---

## React.memo – Preventing Unnecessary Re-renders

`React.memo` is a **Higher Order Component** that **memoizes** a component. It prevents re-rendering if props haven't changed.

### Simple Interview Answer:

> React.memo is a higher-order component that prevents a functional component from re-rendering when its props haven't changed. It does a shallow comparison of props. It's useful for expensive components that receive the same props frequently, especially when the parent re-renders often.

```jsx
import { useState, memo } from "react";

// Child component wrapped in React.memo
const ExpensiveList = memo(function ({ items }) {
  console.log("ExpensiveList rendered!"); // Only logs when items actually change

  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
});

function Parent() {
  const [count, setCount] = useState(0);
  const [items] = useState([
    { id: 1, name: "Apple" },
    { id: 2, name: "Banana" },
  ]);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      {/* ExpensiveList does NOT re-render when count changes ✅ */}
      {/* Because items reference hasn't changed */}
      <ExpensiveList items={items} />
    </div>
  );
}
```

### When to Use React.memo:

```
✅ Use when:
├── Component renders often with same props
├── Component does expensive rendering
├── Parent re-renders frequently but child props stay same
└── Component renders large lists

❌ Don't use when:
├── Component props change every render (memo is useless)
├── Component is simple and fast to render
├── It adds complexity without measurable benefit
└── You haven't identified a performance problem yet
```

---

## useMemo – Memoizing Expensive Calculations

```jsx
import { useState, useMemo } from "react";

function ProductList({ products, filter }) {
  const [count, setCount] = useState(0);

  // ✅ Expensive calculation only runs when products or filter changes
  const filteredProducts = useMemo(() => {
    console.log("Filtering products..."); // Only logs when deps change
    return products
      .filter((p) => p.category === filter)
      .sort((a, b) => b.price - a.price)
      .slice(0, 10);
  }, [products, filter]);

  return (
    <div>
      <button onClick={() => setCount((c) => c + 1)}>Count: {count}</button>
      {/* Clicking count does NOT re-filter! ✅ */}
      <ul>
        {filteredProducts.map((p) => (
          <li key={p.id}>
            {p.name} - ₹{p.price}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

## useCallback – Stable Function References

```jsx
import { useState, useCallback, memo } from "react";

const ChildButton = memo(function ({ onClick, label }) {
  console.log(`${label} rendered!`);
  return <button onClick={onClick}>{label}</button>;
});

function Parent() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState("");

  // ❌ Without useCallback - new function every render → ChildButton re-renders
  // const handleClick = () => console.log("clicked");

  // ✅ With useCallback - same function reference → ChildButton skips re-render
  const handleClick = useCallback(() => {
    console.log("Button clicked!");
  }, []); // Empty deps = function never changes

  return (
    <div>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <button onClick={() => setCount((c) => c + 1)}>Count: {count}</button>
      <ChildButton onClick={handleClick} label="Click Me" />
      {/* ChildButton does NOT re-render when text or count changes ✅ */}
    </div>
  );
}
```

### Performance Trio Summary:

| Tool          | What it Caches     | When to Use                            |
| ------------- | ------------------ | -------------------------------------- |
| `React.memo`  | Entire component   | Component receives same props          |
| `useMemo`     | Computed value     | Expensive calculations                 |
| `useCallback` | Function reference | Passing functions to memoized children |

---

## Code Splitting with React.lazy & Suspense

**Code Splitting** means splitting your JavaScript bundle into **smaller chunks** that load **on demand** instead of loading everything at once.

### Simple Interview Answer:

> Code splitting is a technique where you split your JavaScript bundle into smaller chunks. Instead of loading the entire app at once, you load only the code needed for the current page. React.lazy and Suspense make this easy by letting you lazy-load components. This improves initial load time significantly.

### Without Code Splitting (Everything loads at once):

```jsx
import Home from "./pages/Home"; // Loaded immediately
import About from "./pages/About"; // Loaded immediately
import Dashboard from "./pages/Dashboard"; // Loaded immediately (even if user never visits!)
import Settings from "./pages/Settings"; // Loaded immediately
```

### With Code Splitting (Load on demand):

```jsx
import { lazy, Suspense } from "react";

// Lazy load - these are NOT loaded until needed
const Home = lazy(() => import("./pages/Home"));
const About = lazy(() => import("./pages/About"));
const Dashboard = lazy(() => import("./pages/Dashboard"));
const Settings = lazy(() => import("./pages/Settings"));

function App() {
  return (
    <Suspense fallback={<h2>⏳ Loading page...</h2>}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/settings" element={<Settings />} />
      </Routes>
    </Suspense>
  );
}
```

### How it Works:

```
User visits "/"
    ↓
Only Home chunk is downloaded (small!) ✅
    ↓
User clicks "Dashboard"
    ↓
Dashboard chunk is downloaded on demand ✅
    ↓
Suspense shows fallback while loading
    ↓
Dashboard renders when ready
```

### Benefits:

```
✅ Faster initial page load (less JS to download)
✅ Better Lighthouse performance score
✅ Users only download what they need
✅ Smaller bundle size per page
```

---

## Lighthouse Performance Testing

**Lighthouse** is a built-in Chrome tool that audits your website's performance.

### How to Run:

```
1. Open Chrome DevTools (F12)
2. Go to "Lighthouse" tab
3. Select "Performance"
4. Click "Analyze page load"
```

### Key Metrics:

```
FCP (First Contentful Paint)  → When first text/image appears
LCP (Largest Contentful Paint) → When main content is visible
CLS (Cumulative Layout Shift)  → Visual stability (elements jumping)
TBT (Total Blocking Time)      → How long main thread is blocked
TTI (Time to Interactive)      → When page is fully interactive
```

### React Performance Tips for Lighthouse:

```
✅ Code split with React.lazy
✅ Optimize images (WebP format, lazy loading)
✅ Minimize bundle size (tree shaking)
✅ Use React.memo for expensive components
✅ Avoid large useEffect chains
✅ Preload critical resources
✅ Use production build (npm run build)
```

---

## Debugging Re-renders via DevTools

### React Developer Tools (Browser Extension):

```
1. Install "React Developer Tools" Chrome extension
2. Open DevTools → "Components" tab
3. Click "Profiler" tab
4. Click "Record" button
5. Interact with your app
6. Stop recording
7. See which components re-rendered and WHY
```

### Highlight Re-renders:

```
1. Open React DevTools → Settings (⚙️)
2. Enable "Highlight updates when components render"
3. Interact with app
4. Components flash when they re-render
5. Too many flashes = performance problem!
```

### Console Debugging:

```jsx
function MyComponent({ name, count }) {
  console.log("MyComponent rendered!", { name, count });

  useEffect(() => {
    console.log("useEffect ran!");
  });

  return (
    <h1>
      {name}: {count}
    </h1>
  );
}
```

### Why Did You Render Library:

```bash
npm install @welldone-software/why-did-you-render
```

```jsx
// Shows WHY a component re-rendered in console
MyComponent.whyDidYouRender = true;
```

---

---

# 17. Understanding Full Stack Frameworks

---

## What are Full Stack Frameworks?

A **Full Stack Framework** is a framework that handles **both frontend (UI) and backend (server)** in a **single project and codebase**.

### Simple Interview Answer:

> Full stack frameworks allow you to build both the frontend and backend of a web application in a single project. Instead of maintaining separate React frontend and Node.js backend repositories, full stack frameworks like Next.js let you write API routes, server-side rendering, and UI components all in one place. This simplifies development, deployment, and data fetching.

### Traditional vs Full Stack Framework:

```
Traditional (Separate):
├── Frontend: React app (localhost:3000)
├── Backend: Node/Express API (localhost:5000)
├── Two separate repos
├── Two separate deployments
├── CORS issues
└── Complex data fetching

Full Stack Framework (Unified):
├── Frontend + Backend in ONE project
├── Single repo
├── Single deployment
├── No CORS issues
├── Built-in API routes
└── Server-side rendering out of the box
```

### How They Help:

```
✅ Single codebase for frontend and backend
✅ Built-in routing (file-based)
✅ Server-Side Rendering (SSR) for SEO
✅ Static Site Generation (SSG) for speed
✅ API routes built-in (no separate Express server)
✅ Automatic code splitting
✅ Image optimization
✅ Easy deployment (Vercel, Netlify)
✅ Better performance out of the box
```

---

## Overview of Modern Web Frameworks

### 1. Next.js (Most Popular – by Vercel)

```
Type: Full Stack React Framework
Rendering: SSR, SSG, ISR, Client-side
Key Features:
├── File-based routing (app/ directory)
├── API routes (backend in same project)
├── Server Components (React 18+)
├── Image optimization (<Image />)
├── Built-in CSS/Sass support
├── Middleware
└── App Router (new) + Pages Router (old)

Best For: Production React apps, e-commerce, blogs, SaaS
Used By: Netflix, TikTok, Twitch, Hulu
```

### 2. Remix (by Shopify)

```
Type: Full Stack React Framework
Rendering: SSR focused
Key Features:
├── Nested routing
├── Built-in form handling
├── Progressive enhancement
├── No static generation (SSR only)
├── Uses web standards (Request, Response)
└── Great for dynamic data-heavy apps

Best For: Dynamic apps, dashboards, data-heavy applications
```

### 3. Nuxt.js (Vue.js equivalent of Next.js)

```
Type: Full Stack Vue Framework
Rendering: SSR, SSG, SPA
Key Features:
├── File-based routing
├── Auto imports
├── Server routes
├── Modules ecosystem
└── Vue 3 + Composition API

Best For: Vue developers wanting full stack
```

### 4. SvelteKit (Svelte's full stack framework)

```
Type: Full Stack Svelte Framework
Rendering: SSR, SSG, SPA
Key Features:
├── No Virtual DOM (compiles to vanilla JS)
├── Extremely fast and small bundles
├── File-based routing
├── Built-in form actions
└── Simplest syntax of all frameworks

Best For: Performance-critical apps, small-medium projects
```

### 5. Astro

```
Type: Content-focused Framework
Rendering: Static by default, SSR optional
Key Features:
├── Islands Architecture (partial hydration)
├── Use React, Vue, Svelte together!
├── Zero JS by default (ships HTML)
├── Fastest for content sites
└── Great for blogs, docs, marketing sites

Best For: Content-heavy sites, blogs, documentation
```

### Framework Comparison Table (2025-2026):

| Framework     | Based On | SSR | SSG | API Routes | Learning Curve | Best For               |
| ------------- | -------- | --- | --- | ---------- | -------------- | ---------------------- |
| **Next.js**   | React    | ✅  | ✅  | ✅         | Medium         | All-purpose React apps |
| **Remix**     | React    | ✅  | ❌  | ✅         | Medium         | Dynamic data apps      |
| **Nuxt**      | Vue      | ✅  | ✅  | ✅         | Medium         | Vue full stack         |
| **SvelteKit** | Svelte   | ✅  | ✅  | ✅         | Low            | Fast lightweight apps  |
| **Astro**     | Any      | ✅  | ✅  | ✅         | Low            | Content sites          |

### How They Shape Modern Development:

```
2026 Trends:
├── Server Components → Less JS sent to browser
├── Edge Computing → Run code closer to users
├── AI Integration → Built-in AI features
├── Zero Config → Frameworks handle complexity
├── Performance First → Core Web Vitals matter
├── Type Safety → TypeScript everywhere
└── Full Stack → One framework does everything
```

### Simple Interview Answer:

> Modern full stack frameworks like Next.js have changed web development by combining frontend and backend into a single project. They provide server-side rendering for better SEO, file-based routing for simplicity, built-in API routes to eliminate separate backend servers, and automatic optimizations for performance. In 2026, the trend is moving towards server components, edge computing, and AI-integrated development, making these frameworks essential for building modern web applications.

---

## Final Quick Summary

```
13. Global State
    ├── Context API = Built-in, Provider + useContext
    ├── Zustand = Lightweight, no provider needed
    ├── Use global state only when many components need same data
    └── Don't overengineer – start with useState

14. Redux
    ├── Single store, actions, reducers (pure functions)
    ├── Flow: dispatch → action → reducer → new state → re-render
    ├── Thunk = async actions (API calls)
    ├── Redux Toolkit = Modern, less boilerplate (createSlice)
    └── Alternatives: Zustand (simple), Context (built-in)

15. Forms & Validation
    ├── React Hook Form = Fast, minimal re-renders
    ├── Controlled (state) vs Uncontrolled (ref)
    ├── Zod = Type-safe schema validation
    └── Combine: RHF + Zod resolver = best practice

16. Performance
    ├── React.memo = Skip re-render if props same
    ├── useMemo = Cache expensive calculations
    ├── useCallback = Cache function references
    ├── React.lazy + Suspense = Code splitting
    └── Test with Lighthouse + React DevTools Profiler

17. Full Stack Frameworks
    ├── Next.js = Most popular (React, SSR, SSG, API routes)
    ├── Remix = SSR focused, great for dynamic data
    ├── Single codebase for frontend + backend
    └── 2026: Server Components, Edge, AI integration
```

---

> 💡 **Interview Tips:**
>
> - **Context API** is for simple global state; **Redux Toolkit** for complex apps; **Zustand** for simplicity
> - Redux reducers must be **pure functions** – same input always gives same output, no mutations
> - **React Hook Form + Zod** is the modern standard for form validation
> - Don't use `React.memo` everywhere – **measure first, optimize second**
> - **Code splitting** with `React.lazy` improves **initial load time**
> - **Next.js** is the most in-demand full stack framework – know SSR vs SSG vs ISR
