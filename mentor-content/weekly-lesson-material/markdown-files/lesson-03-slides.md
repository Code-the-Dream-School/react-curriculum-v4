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

# Component Lifecycle, Hooks, State & Props
## Week 3

Code the Dream · React Curriculum v4

---

# 🌡️ Warm-Up (5 min)

**Without looking at notes — try to answer in the chat:**

> *What's the difference between a variable and state in React? Why can't we just use `let` to store data that changes?*

Don't worry if you're not sure — we're about to find out!

<!--
Speaker note: Most students will guess "state updates the UI" which is exactly right. If someone says "I don't know" — great, that's what this lesson is for. Don't give the answer yet; let curiosity sit for a moment.
-->

---

# Why Plain Variables Don't Work

```jsx
function Counter() {
  let count = 0              // ❌ changing this won't update the UI

  function increment() {
    count = count + 1        // React doesn't know this happened
    console.log(count)       // value changes, but the screen stays at 0
  }

  return <button onClick={increment}>{count}</button>
}
```

React only re-renders a component when **state changes**.
Changing a plain variable doesn't trigger a re-render.

---

# useState: React's Memory

```jsx
import { useState } from 'react'

function Counter() {
  const [count, setCount] = useState(0)  // [currentValue, setter]

  function increment() {
    setCount(count + 1)   // ✅ tells React to re-render
  }

  return <button onClick={increment}>{count}</button>
}
```

- `useState(0)` — `0` is the **initial value**
- `count` — the current value
- `setCount` — the function to update it and trigger a re-render

---

# State is Immutable

Never mutate state directly — **always use the setter**:

```jsx
// ❌ Wrong — mutating state directly
todoList.push(newTodo)

// ✅ Correct — create a new array
setTodoList([newTodo, ...todoList])
```

React uses **shallow comparison** to detect changes.
If you mutate the existing value, React won't see a difference.

> 📌 **Pause:** What does "shallow comparison" mean? Does anyone know?

<!--
Speaker note: Shallow comparison means React checks if the reference is the same object, not if the contents changed. When you mutate an array in place and pass it to setState, React sees the same reference and skips the re-render.
-->

---

# Component Lifecycle

Every component goes through three phases:

| Phase | What happens |
|-------|-------------|
| **Mount** | Component appears in the DOM for the first time |
| **Update** | State or props change → component re-renders |
| **Unmount** | Component is removed from the DOM |

React's hooks let you "hook into" these phases.
(More on `useEffect` next week!)

---

# Props: Passing Data to Children

Props let a **parent** pass data **down** to a **child**:

```jsx
// Parent — App.jsx
<TodoList todoList={todoList} />

// Child — TodoList.jsx
function TodoList({ todoList }) {
  return (
    <ul>
      {todoList.map((todo) => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  )
}
```

- Props are **read-only** — a child cannot modify them
- Use destructuring: `{ todoList }` instead of `props.todoList`

---

# Props Flow One Way

```
App (owns todoList state)
  │
  │  todoList={todoList}
  ▼
TodoList (receives todoList as prop)
  │
  │  todo={todo}
  ▼
TodoListItem (receives single todo as prop)
```

Data flows **down** — never up, never sideways (for now).

---

# 🤝 We Do: Add useState to App (10 min)

Let's upgrade the todo list to use state.

1. Move the `todoList` array **above** the App component (outside it)
2. Import `useState` from React
3. Replace the array with state:

```jsx
import { useState } from 'react'

const todos = [
  { id: 1, title: 'Buy groceries' },
  { id: 2, title: 'Walk the dog' },
]

function App() {
  const [todoList, setTodoList] = useState(todos)
  // ...
}
```

**Discussion:** Why move the initial data outside the component?

<!--
Speaker note: Moving it outside prevents the array from being re-created on every render. Ask: "What would happen if it stayed inside?" — a new array reference on every render, potentially causing issues later.
-->

---

# 🤝 We Do: Pass todoList as a Prop (5 min)

Now connect App → TodoList via props:

```jsx
// App.jsx
<TodoList todoList={todoList} />

// TodoList.jsx
function TodoList({ todoList }) {
  return (
    <ul>
      {todoList.map((todo) => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  )
}
```

> Open React DevTools → Components tab. Can you see the `todoList` prop on `<TodoList>`?

---

# 💪 You Do: Create TodoListItem (8 min)

Extract each list item into its own component:

1. Create `src/TodoListItem.jsx`
2. Accept a `todo` prop and render the title:

```jsx
function TodoListItem({ todo }) {
  return <li>{todo.title}</li>
}
export default TodoListItem
```

3. Import and use it in `TodoList.jsx`:
```jsx
{todoList.map((todo) => (
  <TodoListItem key={todo.id} todo={todo} />
))}
```

*Timer: 8 minutes. Where does the `key` prop go — on TodoListItem or inside it?*

<!--
Speaker note: The key prop goes on the outermost element in the map — in this case <TodoListItem>. It doesn't go inside TodoListItem. This is a very common source of confusion.
-->

---

# Wrap-Up (5 min)

**Check for understanding** — in the chat:

- Where does `key` go when rendering a list of components?
- Can a child component change its own props? Why or why not?
- What does `useState` return?

**Coming up in Lesson 04:**
- `useEffect` — syncing with the outside world
- `useRef` — accessing DOM elements directly
- Events — making your app respond to user actions

> Any questions before we close?
