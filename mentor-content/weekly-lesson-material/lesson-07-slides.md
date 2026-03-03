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

# Data Fetching & UI Update Strategies
## Week 7

Code the Dream · React Curriculum v4

---

# 🌡️ Warm-Up (5 min)

**Think about "liking" a post on Instagram or Twitter/X.**

> *In the chat: When you tap the heart, what do you think happens — does the app wait for the server to confirm before showing the like, or does it update instantly?*

*(Hint: try tapping "like" on a slow connection and watch carefully)*

<!--
Speaker note: Most students have noticed this: the like appears instantly even on slow connections, but sometimes "reverts" if the network fails. That's optimistic UI! This warm-up directly previews the key concept of the lesson.
-->

---

# Data Fetching: The Basics

Use `fetch` with `async/await` to talk to APIs:

```js
async function fetchTodos() {
  try {
    const response = await fetch(`${baseUrl}/tasks`, {
      headers: { 'X-CSRF-Token': token }
    })
    if (!response.ok) throw new Error('Failed to fetch')
    const data = await response.json()
    setTodoList(data.todoList)
  } catch (error) {
    setError(error.message)
  }
}
```

- `await` pauses until the Promise resolves
- Always check `response.ok` — a 404 or 500 won't throw automatically
- Use `try/catch` to handle network failures

---

# Fetching on Mount with useEffect

```jsx
useEffect(() => {
  async function fetchTodos() {
    setIsTodoListLoading(true)
    try {
      const response = await fetch(...)
      const data = await response.json()
      setTodoList(data.todoList)
    } catch (error) {
      setError(error.message)
    } finally {
      setIsTodoListLoading(false)
    }
  }
  fetchTodos()
}, []) // empty array = run once on mount
```

> Define the async function *inside* the effect, then call it immediately.

<!--
Speaker note: Common question: "Why can't the effect callback itself be async?" Because useEffect expects the callback to either return nothing or return a cleanup function. Async functions return a Promise, which breaks this.
-->

---

# Loading States & Error Handling

Your UI should handle all three scenarios:

```jsx
if (isTodoListLoading) return <p>Loading your todos...</p>

if (error) return (
  <div>
    <p>{error}</p>
    <button onClick={() => setError(null)}>Clear Error</button>
  </div>
)

return <TodoList todoList={todoList} />
```

> 📌 **Pause:** What would happen if we didn't show a loading state? What would the user see?

<!--
Speaker note: Without a loading state, the user would see an empty list momentarily, which looks like a bug. A loading indicator makes the wait feel intentional.
-->

---

# Pessimistic vs. Optimistic Updates

<div class="columns">
<div>

**Pessimistic**
Wait for the server to confirm, then update UI

✅ Always accurate
⏳ Feels slower

Good for: payments, destructive actions

</div>
<div>

**Optimistic**
Update UI immediately, sync with server in background. Roll back on error.

✅ Feels instant
⚠️ Must handle failures

Good for: likes, adding items, toggling

</div>
</div>

---

# Optimistic Update Pattern

```jsx
async function addTodo(todoTitle) {
  const newTodo = { id: Date.now(), title: todoTitle }

  // 1. Update UI immediately
  setTodoList([newTodo, ...todoList])

  try {
    // 2. Sync with server
    const response = await fetch(`${baseUrl}/tasks`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json', 'X-CSRF-Token': token },
      body: JSON.stringify({ title: todoTitle })
    })
    if (!response.ok) throw new Error('Failed to add todo')
    const data = await response.json()
    // 3. Replace temp item with real server response
    setTodoList([data.todo, ...todoList])
  } catch {
    // 4. Rollback on failure
    setTodoList(todoList)
    setError('Failed to add todo. Please try again.')
  }
}
```

---

# App Structure for Lesson 07

This week we significantly reorganize `App.jsx`:

```
App
├── <Header />                   (new shared component)
└── token ? <TodosPage /> : <Logon />

TodosPage (owns all todo state + API calls)
├── <SortBy />  (coming in Lesson 08)
├── <TodoForm onAddTodo={addTodo} />
└── <TodoList todoList={todoList} ... />

Logon (handles authentication)
```

> `App.jsx` becomes the **routing/auth coordinator**. `TodosPage` owns all todo logic.

---

# 🤝 We Do: Trace an API Call (10 min)

Let's trace through `completeTodo` together:

> *"When the user checks a checkbox, what needs to happen?"*

1. User clicks checkbox → `onCompleteTodo(todo.id)` fires
2. Optimistically update: map todoList, set `isCompleted: true`
3. PATCH request to `/tasks/:id`
4. On success: nothing else needed (already updated)
5. On failure: rollback to previous list, show error

**Discussion:** For `completeTodo`, should we use pessimistic or optimistic? Why?

<!--
Speaker note: Optimistic is the right call here — checking a box feels like it should respond instantly. The data isn't critical (unlike a payment). Walk through the rollback case and ask "how do we know what to rollback to?"
-->

---

# 💪 You Do: Implement fetchTodos (10 min)

In `TodosPage.jsx`:

1. Add `isTodoListLoading` and `error` state
2. Write `fetchTodos` as an async function
3. Call it inside a `useEffect` with `[]` dependency
4. Show a loading message while fetching
5. Show an error message (with a "Clear Error" button) if fetch fails

*Timer: 10 minutes. Check the Network tab in DevTools — do you see the request?*

<!--
Speaker note: Let students work silently. Be available for questions in chat. Encourage them to check the Network tab in browser DevTools to see the actual request and response. This builds debugging skills they'll use throughout the course.
-->

---

# Wrap-Up (5 min)

**Check for understanding** — in the chat:

- What's the difference between pessimistic and optimistic UI updates?
- Why do we define the async function *inside* `useEffect` instead of making the callback itself async?
- What should happen to the UI if a network request fails?

**Coming up in Lesson 08:**
- `useMemo` and `useCallback` — performance optimization hooks
- Server-side sorting and debounced search

> Any questions before we close?
