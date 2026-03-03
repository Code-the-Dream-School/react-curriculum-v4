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

# Advanced Hooks: useMemo, useCallback & API Sort/Search
## Week 8

Code the Dream · React Curriculum v4

---

# 🌡️ Warm-Up (5 min)

> *In the chat: Have you ever used an app that felt slow or laggy as it got more data? What did that feel like as a user?*

Today we learn how React avoids unnecessary work — and how to help it when needed.

<!--
Speaker note: Students might mention slow list rendering, search that lags, or filters that take a beat to apply. Performance problems become real when there's more data. This week we learn to handle that.
-->

---

# The Problem: Unnecessary Work

Every time a component re-renders, everything inside it runs again:

```jsx
function TodoList({ todoList, filterTerm }) {
  // This runs on EVERY render — even unrelated ones
  const filteredList = todoList.filter(
    (todo) => todo.title.includes(filterTerm)
  )

  return <ul>...</ul>
}
```

If `TodoList` re-renders for an unrelated reason, we're recomputing `filteredList` unnecessarily.

---

# useMemo: Cache a Computed Value

```jsx
import { useMemo } from 'react'

function TodoList({ todoList, filterTerm, dataVersion }) {
  const filteredList = useMemo(
    () => todoList.filter((todo) => todo.title.includes(filterTerm)),
    [todoList, filterTerm, dataVersion] // only recompute when these change
  )

  return (
    <ul>
      {filteredList.map((todo) => <TodoListItem key={todo.id} todo={todo} />)}
    </ul>
  )
}
```

`useMemo` skips the computation if the dependencies haven't changed.

---

# useCallback: Cache a Function

When you pass a function as a prop, it's re-created on every render:

```jsx
// Without useCallback — new function reference every render
function addTodo(title) { ... }

// With useCallback — same reference if dependencies haven't changed
const addTodo = useCallback(
  (title) => {
    const newTodo = { id: Date.now(), title }
    setTodoList([newTodo, ...todoList])
  },
  [todoList] // only recreate when todoList changes
)
```

Prevents unnecessary re-renders in child components that receive the function as a prop.

---

# useMemo vs. useCallback

| Hook | What it caches | Use when |
|------|---------------|----------|
| `useMemo` | A **computed value** | Expensive calculations (filtering, sorting) |
| `useCallback` | A **function** | Passing callbacks to child components |

> ⚠️ **Don't over-optimize!** Both hooks have overhead. Only use them when you've identified a real performance problem.

<!--
Speaker note: The most common mistake is using useMemo/useCallback everywhere "just in case." This can actually slow things down. Use them when you notice a problem, not preemptively.
-->

---

# API-Based Sort vs. Local Sort

<div class="columns">
<div>

**Local sort**
- Sort the data already in state
- Fast, no network request
- Limited to data you already have
- Good for: small datasets

</div>
<div>

**API-based sort**
- Send sort params to the server
- Server returns sorted data
- Works on the full dataset
- Good for: large datasets, paginated data

</div>
</div>

> This week: we send sort preferences to the API as query parameters.

---

# Server-Side Sorting

Add `sortBy` and `sortDirection` state, then include them in the fetch:

```jsx
const [sortBy, setSortBy] = useState('creationDate')
const [sortDirection, setSortDirection] = useState('desc')

// Inside fetchTodos:
const params = new URLSearchParams({ sortBy, sortDirection })
const response = await fetch(`${baseUrl}/tasks?${params}`, ...)
```

Add `sortBy` and `sortDirection` to the `useEffect` dependency array so the list re-fetches when they change.

---

# Debouncing: Don't Fetch on Every Keystroke

Without debouncing, every character typed triggers a network request.

A custom debounce hook delays the value update:

```jsx
// src/utils/useDebounce.js
function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value)

  useEffect(() => {
    const timer = setTimeout(() => setDebouncedValue(value), delay)
    return () => clearTimeout(timer)  // cleanup: cancel if value changes again
  }, [value, delay])

  return debouncedValue
}
```

Use it: `const debouncedFilterTerm = useDebounce(filterTerm, 300)`

---

# Cache Invalidation with dataVersion

After a mutation (add/complete/update), we need the filtered list to refresh:

```jsx
const [dataVersion, setDataVersion] = useState(0)

const invalidateCache = useCallback(
  () => setDataVersion((v) => v + 1),
  []
)

// After addTodo succeeds:
invalidateCache()

// In TodoList — dataVersion in useMemo dependencies:
const filteredList = useMemo(
  () => todoList.filter(...),
  [todoList, filterTerm, dataVersion]
)
```

> 📌 **Pause:** Why does incrementing `dataVersion` force `useMemo` to recompute?

<!--
Speaker note: Because `dataVersion` is in the dependency array. When it changes, useMemo sees that a dependency changed and runs the computation again, even if todoList and filterTerm are the same.
-->

---

# 🤝 We Do: Build the useDebounce Hook (10 min)

Let's write it together from scratch:

```jsx
function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value)

  useEffect(() => {
    const timer = setTimeout(
      () => setDebouncedValue(value),
      delay
    )
    return () => clearTimeout(timer)
  }, [value, delay])

  return debouncedValue
}
```

**Discussion:** What does the cleanup function do? What happens without it?

<!--
Speaker note: The cleanup runs when the effect runs again (i.e., when value changes). It cancels the previous timer. Without cleanup, every keystroke would start a timer, and multiple fetches would fire after the delay. With cleanup, only the last timer fires.
-->

---

# 💪 You Do: Add the SortBy Component (8 min)

1. Create `src/shared/SortBy.jsx` with two `<select>` dropdowns:
   - **Sort by:** `creationDate` | `title`
   - **Order:** `desc` | `asc`
2. Accept `sortBy`, `sortDirection`, `onSortByChange`, `onSortDirectionChange` as props
3. Wire it into `TodosPage.jsx` above `<TodoForm>`
4. Verify: changing the dropdowns should re-fetch and re-sort the list

*Timer: 8 minutes.*

<!--
Speaker note: Let students work silently. Be available for questions in chat. Remind them that the select elements should be controlled (value + onChange). The key thing to verify is that changing a sort option triggers a new fetch.
-->

---

# Wrap-Up (5 min)

**Check for understanding** — in the chat:

- What's the difference between `useMemo` and `useCallback`?
- Why do we use debouncing for search inputs?
- When is API-based sort better than local sort?

**Coming up in Lesson 09:**
- `useReducer` — organizing complex state into a single reducer
- `useContext` — sharing state without prop drilling

> Any questions before we close?
