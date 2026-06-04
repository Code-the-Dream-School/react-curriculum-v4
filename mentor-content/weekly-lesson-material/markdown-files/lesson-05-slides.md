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

# Local State, Controlled Components & Forms
## Week 5

Code the Dream · React Curriculum v4

---

# 🌡️ Warm-Up (5 min)

**Think about a search bar on a website like YouTube or Amazon.**

> *In the chat: As you type in the search bar, what is the app doing? Does anything change before you press Enter?*

*(Think about autocomplete, character limits, or disabled buttons)*

<!--
Speaker note: Students will mention autocomplete suggestions, character counters, "search" appearing as you type. All of these require the app to know the current input value at every keystroke — that's a controlled component!
-->

---

# Controlled vs. Uncontrolled Inputs

<div class="columns">
<div>

**Uncontrolled** (DOM owns the value)
```jsx
// React can only read the value
// when something happens (submit, etc.)
<input type="text" ref={inputRef} />
```
- Simple, less code
- Can't react to each keystroke
- Good for simple forms

</div>
<div>

**Controlled** (React owns the value)
```jsx
const [title, setTitle] = useState('')

<input
  value={title}
  onChange={(e) => setTitle(e.target.value)}
/>
```
- React knows the value at all times
- Enables validation, disabling buttons, etc.

</div>
</div>

---

# Controlled Component: The Update Cycle

Every keystroke triggers this cycle:

```
User types a character
        ↓
onChange fires with the new value
        ↓
setWorkingTodoTitle(newValue) is called
        ↓
React re-renders the component
        ↓
input renders with updated value={workingTodoTitle}
```

> The input's value is always what React says it is.

---

# Converting TodoForm to Controlled

```jsx
import { useState } from 'react'

function TodoForm({ onAddTodo }) {
  const [workingTodoTitle, setWorkingTodoTitle] = useState('')

  function handleAddTodo(event) {
    event.preventDefault()
    if (workingTodoTitle.trim()) {
      onAddTodo(workingTodoTitle)
      setWorkingTodoTitle('')   // reset via state, not DOM
    }
  }

  return (
    <form onSubmit={handleAddTodo}>
      <input
        value={workingTodoTitle}
        onChange={(e) => setWorkingTodoTitle(e.target.value)}
      />
      <button type="submit">Add Todo</button>
    </form>
  )
}
```

---

# Disabling the Button When Input is Empty

Now that React knows the input value, we can use it:

```jsx
<button
  type="submit"
  disabled={!workingTodoTitle.trim()}
>
  Add Todo
</button>
```

- `trim()` ignores whitespace-only input
- The button is disabled automatically when there's nothing to submit

> 📌 **Pause:** Could we have done this with an uncontrolled input? Why or why not?

<!--
Speaker note: No — with an uncontrolled input, React only knows the value when you explicitly read it (e.g., on submit). You couldn't check it on every keystroke without a controlled component.
-->

---

# Conditional Rendering

**Ternary** — show one or the other:
```jsx
{todoList.length === 0
  ? <p>Add a todo above to get started!</p>
  : <ul>...</ul>
}
```

**Logical `&&`** — show or show nothing:
```jsx
{error && <p className="error">{error}</p>}
```

> Use whichever reads more clearly. Both are correct.

---

# Adding Todo Completion

1. Add `isCompleted: false` to new todos in `addTodo`
2. Create `completeTodo` in App:

```jsx
function completeTodo(id) {
  const updatedList = todoList.map((todo) =>
    todo.id === id ? { ...todo, isCompleted: true } : todo
  )
  setTodoList(updatedList)
}
```

3. Pass `onCompleteTodo={completeTodo}` down through `TodoList` → `TodoListItem`
4. Add a checkbox in `TodoListItem`

---

# Checkbox in TodoListItem

```jsx
function TodoListItem({ todo, onCompleteTodo }) {
  return (
    <li>
      <input
        type="checkbox"
        checked={todo.isCompleted}
        onChange={() => onCompleteTodo(todo.id)}
      />
      {todo.title}
    </li>
  )
}
```

And in `TodoList`, filter out completed todos:

```jsx
const filteredTodoList = todoList.filter((todo) => !todo.isCompleted)
```

---

# 🤝 We Do: Convert TodoForm Together (10 min)

Let's convert `TodoForm` to a controlled component step by step:

1. Add `useState('')` for `workingTodoTitle`
2. Add `value` and `onChange` to the input
3. Update `handleAddTodo` to use `workingTodoTitle`
4. Reset with `setWorkingTodoTitle('')`
5. Disable the button: `disabled={!workingTodoTitle.trim()}`

**Watch the React DevTools — can you see the state updating as you type?**

<!--
Speaker note: Have students open React DevTools → Components → TodoForm while you live code. They can see the workingTodoTitle state updating with each keystroke. This makes the abstract concept concrete.
-->

---

# 💪 You Do: Add the Empty List Message (8 min)

In `TodoList.jsx`, update the return statement:

```jsx
return (
  <>
    {todoList.length === 0
      ? <p>Add a todo above to get started!</p>
      : <ul>
          {filteredTodoList.map((todo) => (
            <TodoListItem
              key={todo.id}
              todo={todo}
              onCompleteTodo={onCompleteTodo}
            />
          ))}
        </ul>
    }
  </>
)
```

Also add the completion checkbox if you haven't yet.

*Timer: 8 minutes.*

<!--
Speaker note: Let students work silently. Be available for questions in chat. Common issue: forgetting to pass onCompleteTodo through TodoList to TodoListItem (it needs to be in both the props and the JSX for each layer).
-->

---

# Wrap-Up (5 min)

**Check for understanding** — answer in chat:

- What makes a form "controlled"?
- Why do we use `workingTodoTitle` state instead of just reading the input's value on submit?
- How does `disabled={!workingTodoTitle.trim()}` work?

**Coming up in Lesson 06:**
- Making components reusable
- Organizing your project into features and shared components
- Refactoring: extracting logic without breaking things

> Any questions before we close?
