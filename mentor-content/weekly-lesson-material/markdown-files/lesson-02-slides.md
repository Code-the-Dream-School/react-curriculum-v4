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

# ReactDOM, Components, JSX & Troubleshooting
## Week 2

Code the Dream · React Curriculum v4

---

# 🌡️ Warm-Up (5 min)

**Look at any website you use regularly (Gmail, Twitter, a news site).**

> *In the chat: Name one "chunk" of the UI that repeats — something you'd want to build once and reuse everywhere.*

*(Examples: a navigation bar, a tweet card, a product listing)*

<!--
Speaker note: This activates the "component thinking" mindset before you name it. Students often pick email rows, nav items, or product cards. Validate all answers — they're all correct.
-->

---

# How React Gets on the Page

`src/main.jsx` is the entry point:

```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import App from './App.jsx'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>
)
```

- `createRoot` targets the `<div id="root">` in `index.html`
- `.render()` puts your React app inside it
- `StrictMode` helps catch bugs in development

---

# What is a Component?

A component is a **JavaScript function** that returns JSX (UI).

```jsx
function TodoList() {
  return (
    <ul>
      <li>Buy groceries</li>
      <li>Walk the dog</li>
    </ul>
  )
}

export default TodoList
```

Rules:
- Name must start with a **capital letter**
- Must return a **single root element** (or a Fragment)
- Should be a **pure function** — no side effects in the body

---

# The Component Tree

Components nest inside each other to form a tree:

```
App
├── TodoForm
└── TodoList
    └── TodoListItem (×3)
```

- **Parent** components pass data **down** to children
- Data flows **one direction** — parent → child
- Each component is responsible for its own piece of the UI

> This is the mental model you'll use throughout the whole course.

---

# JSX: HTML-like Syntax in JavaScript

JSX lets you write UI structure alongside logic:

```jsx
function App() {
  const title = 'My Todos'
  return (
    <div>
      <h1>{title}</h1>
      <p>You have {3} todos today.</p>
    </div>
  )
}
```

- Use `{}` to embed any JavaScript expression
- JSX is **not** HTML — it compiles to `React.createElement()` calls
- Vite handles the compilation automatically

---

# JSX Rules to Know

| HTML | JSX |
|------|-----|
| `class="..."` | `className="..."` |
| `for="..."` | `htmlFor="..."` |
| `<br>` | `<br />` (self-closing) |
| `<img>` | `<img />` (self-closing) |
| Inline style: `style="color: red"` | `style={{ color: 'red' }}` |

> JSX must have **one root element**. Wrap siblings in a `<div>` or `<>...</>` (Fragment).

---

# Conditional Rendering

Two common patterns:

**Ternary** — show one thing or another:
```jsx
{isLoading ? <p>Loading...</p> : <TodoList />}
```

**Logical `&&`** — show something or nothing:
```jsx
{error && <p className="error">{error}</p>}
```

> 📌 **Pause:** When would you use `&&` vs a ternary? Any guesses?

<!--
Speaker note: Use `&&` when there's nothing to show in the "false" case. Use ternary when you want to show something different. Common mistake: using `&&` with `0` as the left side — it renders `0`!
-->

---

# Troubleshooting React Apps

**Three tools to know:**

1. **Vite error overlay** — a red screen in the browser shows the exact error and line
2. **Browser console** — warnings (like missing `key`) and runtime errors
3. **React Developer Tools** — inspect the component tree, see props and state

> 💡 When you see a red error: *read it carefully*. React errors are usually descriptive.

<!--
Speaker note: Encourage students to read the full error message before asking for help. Many errors tell you exactly what's wrong and where. "Common misconception: red screen = broken forever. It just means there's an error to fix."
-->

---

# React Developer Tools

Install the browser extension: **React Developer Tools**

What you can do:
- **Components tab** — see your component tree, inspect props, trigger state changes
- **Profiler tab** — measure render performance (we'll use this later)

> Open DevTools → "Components" tab and look at your todo app's tree.

---

# 🤝 We Do: Create TodoList Together (10 min)

Let's extract the todo list into its own component.

1. Create `src/TodoList.jsx`
2. Move the `todoList` array and `<ul>` rendering into it

```jsx
function TodoList() {
  const todoList = [
    { id: 1, title: 'Buy groceries' },
    { id: 2, title: 'Walk the dog' },
  ]
  return (
    <ul>
      {todoList.map((todo) => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  )
}
export default TodoList
```

**Discussion:** What should `App.jsx` look like now?

<!--
Speaker note: Walk through this live. Ask students to predict the import statement in App.jsx before showing it. "What would we call it? Where does the file live? How do we import it?"
-->

---

# 🤝 We Do: What Goes in TodoForm? (5 min)

We need a form for adding todos. Let's plan it together.

> *In the chat: What HTML elements should a "add todo" form contain?*

We need:
- A text `<input>` for the todo title
- A `<label>` for accessibility
- A `<button>` to submit

**What props might TodoForm eventually need?**

---

# 💪 You Do: Create TodoForm (8 min)

Create `src/TodoForm.jsx` with this structure:

```jsx
function TodoForm() {
  return (
    <form>
      <label htmlFor="todoTitle">Todo</label>
      <input type="text" id="todoTitle" />
      <button type="submit" disabled>Add Todo</button>
    </form>
  )
}
export default TodoForm
```

Then import and render `<TodoForm />` in `App.jsx` above `<TodoList />`.

*Timer: 8 minutes. Check the browser — do you see the form?*

<!--
Speaker note: Let students work silently. Be available for questions in chat. Common issue: forgetting to import the component in App.jsx, or not using a capital letter in the component name.
-->

---

# Wrap-Up (5 min)

**Check for understanding** — pick one and answer in chat:

- What's the difference between a component and a regular function?
- Why do we need `key` when rendering a list?
- What does `className` mean in JSX?

**Coming up in Lesson 03:**
- `useState` — how React tracks changing data
- Props — how components share data with each other
- Component lifecycle — when does React render?

> Any questions before we close?
