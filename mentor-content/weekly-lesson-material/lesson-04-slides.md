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

# Hooks Continued, Events & Handlers
## Week 4

Code the Dream · React Curriculum v4

---

# 🌡️ Warm-Up (5 min)

**Think about a form on any website — like a sign-up form.**

> *In the chat: When the user clicks "Submit," what should happen? List as many steps as you can think of.*

*(Hint: think about what the app needs to do, not just what appears on screen)*

<!--
Speaker note: Good answers: prevent page reload, read the input value, validate it, update state, clear the form, refocus the input. All of these come up in this lesson. This warm-up primes students for the full flow they're about to implement.
-->

---

# useEffect: Syncing with the Outside World

`useEffect` runs **after** a render. Use it for side effects:

```jsx
import { useEffect } from 'react'

function App() {
  const [todoList, setTodoList] = useState([])

  useEffect(() => {
    // runs after the component renders
    document.title = `You have ${todoList.length} todos`
  }, [todoList]) // re-run whenever todoList changes
}
```

- The **dependency array** controls when the effect runs
- `[]` — run once, after the first render only
- `[value]` — run after any render where `value` changed
- *(no array)* — run after every render

---

# useEffect Dependency Array

| Pattern | When it runs |
|---------|-------------|
| `useEffect(() => {...}, [a, b])` | After renders where `a` or `b` changed |
| `useEffect(() => {...}, [])` | Once, after the first render |
| `useEffect(() => {...})` | After every render |

> 📌 **Pause:** If we're fetching data when a component loads, which pattern would we use?

<!--
Speaker note: Empty array `[]` — we only want to fetch once on mount, not on every re-render. This comes up directly in Lesson 07. Plant the seed now.
-->

---

# useRef: A Persistent Value (No Re-renders)

`useRef` stores a value that **persists across renders** but doesn't trigger one:

```jsx
import { useRef } from 'react'

function TodoForm() {
  const inputRef = useRef()

  function handleSubmit(event) {
    event.preventDefault()
    // after adding a todo, put focus back on the input
    inputRef.current.focus()
  }

  return (
    <form onSubmit={handleSubmit}>
      <input ref={inputRef} type="text" />
      <button type="submit">Add Todo</button>
    </form>
  )
}
```

---

# Events in React

React wraps browser events in **synthetic events** — same API, cross-browser compatible.

```jsx
function handleClick(event) {
  event.preventDefault()       // stop default browser behavior
  event.stopPropagation()      // stop event bubbling up
  console.log(event.target)    // the element that was clicked
}

<button onClick={handleClick}>Click me</button>
```

- Pass the **function reference**, not a call: `onClick={handleClick}` ✅
- Not: `onClick={handleClick()}` ❌ (that calls it immediately)

---

# Handler Props: Child → Parent Communication

Children can't update parent state directly — but they can **call a function the parent provides**:

```jsx
// App.jsx (parent)
function addTodo(todoTitle) {
  const newTodo = { id: Date.now(), title: todoTitle }
  setTodoList([newTodo, ...todoList])
}

<TodoForm onAddTodo={addTodo} />

// TodoForm.jsx (child)
function TodoForm({ onAddTodo }) {
  function handleSubmit(event) {
    event.preventDefault()
    onAddTodo(event.target.elements.todoTitle.value)
  }
  // ...
}
```

---

# Data Flow with Handler Props

```
App
├── state: todoList
├── addTodo() ──────────────────────────────┐
│                                            │
└── <TodoForm onAddTodo={addTodo} />         │
         │                                   │
         └── user submits form ──────────────┘
             → calls onAddTodo(title)
             → App updates todoList state
             → TodoList re-renders
```

> The parent owns the state. The child owns the interaction.

---

# 🤝 We Do: Build addTodo Together (10 min)

Let's wire up adding todos.

**In `App.jsx`:**
```jsx
function addTodo(todoTitle) {
  const newTodo = { id: Date.now(), title: todoTitle }
  setTodoList([newTodo, ...todoList])
}

<TodoForm onAddTodo={addTodo} />
```

**Questions as we go:**
- Why `Date.now()` for the id?
- Why `[newTodo, ...todoList]` and not `[...todoList, newTodo]`?
- What happens if we forget `event.preventDefault()`?

<!--
Speaker note: Date.now() gives a unique timestamp. Prepending the new todo shows it at the top. Without preventDefault, the page refreshes (old HTML form behavior) and state is lost.
-->

---

# 🤝 We Do: Handle Form Submission (5 min)

**In `TodoForm.jsx`:**
```jsx
import { useRef } from 'react'

function TodoForm({ onAddTodo }) {
  const inputRef = useRef()

  function handleAddTodo(event) {
    event.preventDefault()
    const title = inputRef.current.value.trim()
    if (title) {
      onAddTodo(title)
      event.target.reset()
      inputRef.current.focus()
    }
  }

  return (
    <form onSubmit={handleAddTodo}>
      <input ref={inputRef} type="text" id="todoTitle" />
      <button type="submit">Add Todo</button>
    </form>
  )
}
```

---

# 💪 You Do: Test the Full Flow (8 min)

Your app should now let users add todos. Verify:

1. Type a todo title and click "Add Todo" — does it appear?
2. After submitting, does the input clear?
3. Does focus return to the input automatically?
4. What happens if you submit an empty input? (Should do nothing)

Try adding `console.log(todoList)` inside `addTodo` to trace state changes in DevTools.

*Timer: 8 minutes.*

<!--
Speaker note: Let students work silently. Be available for questions in chat. Common issue: forgetting to pass onAddTodo from App to TodoForm, or misspelling the prop name. Remind them to check the component in React DevTools to see what props it received.
-->

---

# Wrap-Up (5 min)

**Check for understanding** — answer in chat:

- Why do we write `onClick={handleClick}` instead of `onClick={handleClick()}`?
- What is the dependency array in `useEffect` for?
- How does a child component communicate back to a parent?

**Coming up in Lesson 05:**
- Controlled components — letting React "own" form input values
- Local state — keeping working values separate from app state
- Conditional rendering patterns

> Any questions before we close?
