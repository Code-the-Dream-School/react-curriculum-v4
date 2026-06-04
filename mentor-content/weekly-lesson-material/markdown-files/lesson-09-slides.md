---
marp: true
theme: default
paginate: true
style: |
  section {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 28px;
  }
  h1 {
    color: #1a73e8;
  }
  h2 {
    color: #333;
  }
  code {
    background-color: #f0f0f0;
    padding: 2px 6px;
    border-radius: 4px;
  }
  pre code {
    font-size: 22px;
  }
  .columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
  }
---

# Using These Slides

These slides are written in **Marp** markdown. You can present them two ways:

**In VS Code:**
1. Install the **Marp for VS Code** extension
2. Open this `.md` file
3. Click the Marp preview icon (top right) or use `Marp: Open Preview`
4. To present: `Marp: Export Slide Deck` → HTML or PDF

**In the browser:**
1. Go to [marp.app](https://marp.app/)
2. Paste or upload this file
3. Click **Preview** to present

Feel free to edit these slides to fit your style!

---

# Advanced State: useReducer & useContext
## Week 9

Code the Dream · React Curriculum v4

---

# 🌡️ Warm-Up (5 min)

**Look at `TodosPage.jsx` in your project right now.**

> *In the chat: How many `useState` calls does it have? What's the biggest challenge with managing all that state separately?*

<!--
Speaker note: Students should have about 8 separate useState calls by now: todoList, error, isTodoListLoading, sortBy, sortDirection, filterTerm, dataVersion, filterError. The pain point: updating multiple related pieces of state in sync, and the component body becoming hard to read.
-->

---

# The Problem: Too Much State

After Lesson 08, `TodosPage` has ~8 separate state variables:

```jsx
const [todoList, setTodoList] = useState([])
const [error, setError] = useState(null)
const [isTodoListLoading, setIsTodoListLoading] = useState(false)
const [sortBy, setSortBy] = useState('creationDate')
const [sortDirection, setSortDirection] = useState('desc')
const [filterTerm, setFilterTerm] = useState('')
const [dataVersion, setDataVersion] = useState(0)
const [filterError, setFilterError] = useState(null)
```

These are all **related** — they describe the same feature. A reducer organizes them.

---

# The Reducer Pattern

A reducer is a **pure function** that describes how state changes:

```js
function reducer(currentState, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { ...currentState, count: currentState.count + 1 }
    case 'RESET':
      return { ...currentState, count: 0 }
    default:
      return currentState
  }
}
```

- Takes the **current state** and an **action**
- Returns a **new state** (never mutates existing state)
- Each `case` handles one type of state change

---

# useReducer Hook

```jsx
import { useReducer } from 'react'

const [state, dispatch] = useReducer(todoReducer, initialTodoState)

// Instead of: setTodoList([...])
// You write:
dispatch({ type: TODO_ACTIONS.FETCH_SUCCESS, payload: { todoList: data } })
```

- `state` — the full state object
- `dispatch` — the function to send actions to the reducer
- Actions have a `type` (what happened) and optional `payload` (the data)

> 📌 **Pause:** How is this different from `useState`? What's the main benefit?

<!--
Speaker note: With useState, the "how to update" logic is scattered across the component. With useReducer, all state transitions live in one place (the reducer). This makes the component body cleaner and state logic easier to test.
-->

---

# Defining Actions and Initial State

```js
// src/reducers/todoReducer.js
export const TODO_ACTIONS = {
  FETCH_START: 'FETCH_START',
  FETCH_SUCCESS: 'FETCH_SUCCESS',
  FETCH_ERROR: 'FETCH_ERROR',
  ADD_TODO_OPTIMISTIC: 'ADD_TODO_OPTIMISTIC',
  ADD_TODO_SUCCESS: 'ADD_TODO_SUCCESS',
  ADD_TODO_ERROR: 'ADD_TODO_ERROR',
  // ... etc.
}

export const initialTodoState = {
  todoList: [],
  error: null,
  isTodoListLoading: false,
  sortBy: 'creationDate',
  sortDirection: 'desc',
  filterTerm: '',
  dataVersion: 0,
  filterError: null,
}
```

---

# A Reducer Case: FETCH_SUCCESS

```js
function todoReducer(state, action) {
  switch (action.type) {
    case TODO_ACTIONS.FETCH_START:
      return { ...state, isTodoListLoading: true, error: null }

    case TODO_ACTIONS.FETCH_SUCCESS:
      return {
        ...state,
        todoList: action.payload.todoList,
        isTodoListLoading: false,
      }

    case TODO_ACTIONS.FETCH_ERROR:
      return {
        ...state,
        error: action.payload.error,
        isTodoListLoading: false,
      }

    default:
      return state
  }
}
```

---

# The Problem: Prop Drilling

Auth state (`email`, `token`) lives in `App.jsx` but is needed deep in the tree:

```
App (has token)
└── TodosPage (needs token) ← passed as prop
    └── Header (needs email for logout) ← passed as prop
        └── Logoff (needs token for logout) ← passed as prop
```

Passing props through components that don't use them is called **prop drilling**.

> Prop drilling works, but it makes components harder to reuse and refactor.

---

# useContext: Share State Without Drilling

Three steps:

**1. Create the context**
```jsx
// src/contexts/AuthContext.jsx
const AuthContext = createContext(null)
```

**2. Provide the value**
```jsx
<AuthContext.Provider value={{ email, token, login, logout }}>
  <App />
</AuthContext.Provider>
```

**3. Consume in any descendant**
```jsx
function Header() {
  const { email, logout } = useContext(AuthContext)
}
```

---

# AuthProvider: Wrapping the App

```jsx
// src/contexts/AuthContext.jsx
export function AuthProvider({ children }) {
  const [email, setEmail] = useState(null)
  const [token, setToken] = useState(null)

  async function login(credentials) {
    // POST to /users/logon, set email + token on success
  }

  function logout() {
    setEmail(null)
    setToken(null)
  }

  return (
    <AuthContext.Provider
      value={{ email, token, isAuthenticated: !!token, login, logout }}
    >
      {children}
    </AuthContext.Provider>
  )
}
```

---

# 🤝 We Do: Write Reducer Cases Together (10 min)

Let's write the `ADD_TODO_OPTIMISTIC` and `ADD_TODO_ERROR` cases:

```js
case TODO_ACTIONS.ADD_TODO_OPTIMISTIC:
  return {
    ...state,
    todoList: [action.payload.newTodo, ...state.todoList]
  }

case TODO_ACTIONS.ADD_TODO_ERROR:
  return {
    ...state,
    todoList: action.payload.previousList,  // rollback!
    error: action.payload.error,
  }
```

**Discussion:** Why do we pass `previousList` in the error payload?

<!--
Speaker note: When addTodo starts optimistically, it dispatches ADD_TODO_OPTIMISTIC — but it also needs to remember the previous list. So it captures the old todoList and includes it in the error action's payload for rollback. The reducer doesn't have access to previous state directly, only the current state.
-->

---

# 💪 You Do: Create AuthContext (8 min)

1. Create `src/contexts/AuthContext.jsx`
2. Create `AuthContext` with `createContext(null)`
3. Create `AuthProvider` with `email` and `token` state and a `login` function stub
4. Export a `useAuth()` custom hook: `export function useAuth() { return useContext(AuthContext) }`
5. Wrap `<App />` with `<AuthProvider>` in `main.jsx`
6. In `App.jsx`, use `useAuth()` instead of local state

*Timer: 8 minutes.*

<!--
Speaker note: Let students work silently. Be available for questions in chat. Common issue: forgetting to import useContext. Another: calling useAuth() outside of a component. Consider adding a guard: if (!context) throw new Error('useAuth must be used inside AuthProvider').
-->

---

# Wrap-Up (5 min)

**Check for understanding** — in the chat:

- When would you choose `useReducer` over multiple `useState` calls?
- What is prop drilling, and how does `useContext` help?
- What's the difference between a context's **creator**, **provider**, and **consumer**?

**Coming up in Lesson 10:**
- React Router — real multi-page navigation
- Protected routes, URL parameters, programmatic navigation

> Any questions before we close?
