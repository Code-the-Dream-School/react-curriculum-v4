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

# Reusable Components, Organization & Refactoring
## Week 6

Code the Dream · React Curriculum v4

---

# 🌡️ Warm-Up (5 min)

> *In the chat: Have you ever copy-pasted code from one place to another in the same project? What were the downsides when something needed to change?*

*(This is the exact problem reusable components solve!)*

<!--
Speaker note: Students will mention updating the same code in multiple places, fixing a bug in one spot but forgetting another, inconsistent behavior. These are the pain points that motivate this lesson.
-->

---

# What Makes a Good Reusable Component?

A component is worth extracting when:

- **The same UI pattern appears in multiple places**
- **The logic is complex enough to isolate**
- **It has a single, clear responsibility**

Examples of good reusable components:
- `TextInputWithLabel` (a labeled input used in multiple forms)
- `Button` (consistent styling + behavior)
- `ErrorMessage`, `LoadingSpinner`, `Modal`

> 📌 **Pause:** Can you think of any repeated UI in your todo app right now?

---

# Helper Functions vs. Custom Hooks

<div class="columns">
<div>

**Helper Function**
Pure JavaScript — no React
```js
// src/utils/todoValidation.js
function isValidTodoTitle(title) {
  return title.trim() !== ''
}
```
Use for: calculations, formatting, validation logic

</div>
<div>

**Custom Hook**
Uses React hooks inside
```js
// src/hooks/useEditableTitle.js
function useEditableTitle(initial) {
  const [title, setTitle] = useState(initial)
  const [isEditing, setIsEditing] = useState(false)
  return { title, setTitle, isEditing, setIsEditing }
}
```
Use for: stateful logic you want to reuse

</div>
</div>

---

# Project Organization

Organizing by **feature** and **purpose** keeps things findable:

```
src/
├── features/
│   ├── TodoList/
│   │   ├── TodoList.jsx
│   │   └── TodoListItem.jsx
│   └── TodoForm.jsx
├── shared/
│   └── TextInputWithLabel.jsx
└── utils/
    └── todoValidation.js
```

> Rule of thumb: if something is used in only one feature, it lives there. If used in multiple places, it goes in `shared/` or `utils/`.

---

# TextInputWithLabel: A Reusable Component

The label + input pattern appears in TodoForm *and* the edit form. Extract it:

```jsx
// src/shared/TextInputWithLabel.jsx
function TextInputWithLabel({
  elementId, labelText, onChange, ref, value
}) {
  return (
    <>
      <label htmlFor={elementId}>{labelText}</label>
      <input
        type="text"
        id={elementId}
        ref={ref}
        value={value}
        onChange={onChange}
      />
    </>
  )
}
export default TextInputWithLabel
```

---

# Using TextInputWithLabel in TodoForm

Before:
```jsx
<label htmlFor="todoTitle">Todo</label>
<input type="text" id="todoTitle" ref={inputRef}
  value={workingTodoTitle}
  onChange={(e) => setWorkingTodoTitle(e.target.value)} />
```

After:
```jsx
<TextInputWithLabel
  elementId="todoTitle"
  labelText="Todo"
  ref={inputRef}
  value={workingTodoTitle}
  onChange={(e) => setWorkingTodoTitle(e.target.value)}
/>
```

---

# Editing a Todo: The Plan

In `TodoListItem`, we'll add an edit mode:

```
isEditing = false → show: [checkbox] [title text] [Edit button]
isEditing = true  → show: [TextInputWithLabel] [Update] [Cancel]
```

New state needed:
- `isEditing` (boolean)
- `workingTitle` (string, starts as `todo.title`)

New handlers:
- `handleEdit` (update `workingTitle`)
- `handleCancel` (reset `workingTitle`, set `isEditing` to false)
- `handleUpdate` (call `onUpdateTodo`, set `isEditing` to false)

---

# Refactoring: Moving Files

When you move a file, update all the imports.

Vite will show an error immediately if an import is broken:

```
Error: Failed to resolve import "./TodoList" from "src/App.jsx"
```

**Checklist when moving files:**
1. Move the file to the new location
2. Update the import in any file that references it
3. Check the browser — Vite will tell you if anything broke

> Pro tip: do one move at a time and verify before moving the next.

---

# 🤝 We Do: Reorganize the Project (10 min)

Let's move files together:

1. Create `src/features/TodoList/` directory
2. Move `TodoList.jsx` and `TodoListItem.jsx` there
3. Move `TodoForm.jsx` to `src/features/`
4. Update the imports in `App.jsx`

**Watch Vite's error overlay — it will show exactly which import is broken.**

> *Mentor: share your screen and do this live. Show how to read Vite's import errors.*

<!--
Speaker note: Common misconception: "I need to update the export statement too." No — exports don't change. Only imports that reference the old path need to change.
-->

---

# 💪 You Do: Create TextInputWithLabel (8 min)

1. Create `src/shared/TextInputWithLabel.jsx` with the component shown earlier
2. Replace the `<label>` and `<input>` in `TodoForm.jsx` with `<TextInputWithLabel>`
3. Pass the correct props: `elementId`, `labelText`, `ref`, `value`, `onChange`

Verify in the browser: the form should still work exactly as before.

*Timer: 8 minutes.*

<!--
Speaker note: Let students work silently. Be available for questions in chat. Common issue: forgetting to pass `ref` as a prop. In React 19, `ref` can be passed as a regular prop. If using React 18, they may need `forwardRef` — check the curriculum version.
-->

---

# Wrap-Up (5 min)

**Check for understanding** — in the chat:

- When should you extract a helper function vs. a custom hook?
- Where does `TextInputWithLabel` live — `features/` or `shared/`? Why?
- What happens in Vite when you move a file without updating the import?

**Coming up in Lesson 07:**
- Fetching real data from an API
- Loading states and error handling
- Pessimistic vs. optimistic UI update strategies

> Any questions before we close?
