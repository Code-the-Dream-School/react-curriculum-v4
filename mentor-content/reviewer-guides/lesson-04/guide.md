# Lesson-04 Exercise : Todo List - Adding Todos

This guide helps mentors review Lesson-04 submissions quickly and consistently.

---

## Functional Specifications

### User Requirements

A user should be able to:

- Add new todos using a form input and submit button
- Submit with the Enter key as well as the button
- See new todos appear in the list immediately after submission
- Have the input field retain focus after a todo is added
- Be prevented from adding empty or whitespace-only todos

### Acceptance Criteria

- [ ] `todoList` state is initialized to an empty array in `App.jsx`
- [ ] App renders without errors
- [ ] New todos appear in the list immediately after submission
- [ ] Input field stays focused after adding a todo
- [ ] Empty or whitespace-only input does not add a todo
- [ ] No `console.log` statements left in submitted code

---

## Technical Specifications

### `App.jsx`

- `todoList` state initialized with `useState([])`
- `addTodo` function that:
  - Accepts a `todoTitle` parameter
  - Creates a new todo object with `id: Date.now()` and `title: todoTitle`
  - Uses a **functional state update**: `setTodoList(previous => [newTodo, ...previous])`
- `<TodoForm onAddTodo={addTodo} />` in the return statement

### `TodoForm.jsx`

- `useRef` imported and used to create `inputRef`
- `onAddTodo` destructured from props
- `handleAddTodo` event handler that:
  - Calls `event.preventDefault()`
  - Trims the input value before checking
  - Calls `onAddTodo(todoTitle)` only when the title is non-empty
  - Resets the form with `event.target.reset()`
  - Refocuses the input with `inputRef.current.focus()`
- Input element has `name="todoTitle"` and the `ref={inputRef}` attached
- Form submit wired to `handleAddTodo` (via button `onClick` or form `onSubmit`)
- `disabled` attribute removed from the submit button

---

## Quality Checklist

- [ ] Functional update pattern used: `setTodoList(previous => [newTodo, ...previous])`
- [ ] `event.preventDefault()` present -- prevents page reload on submit
- [ ] `useRef` used for focus management (not `useState`)
- [ ] `name="todoTitle"` on the input so `event.target.todoTitle.value` works
- [ ] Whitespace-only input does not produce a todo (`.trim()` applied)
- [ ] No console errors or warnings

---

## ❌ Common Issues to Watch For

- Forgetting `event.preventDefault()` -- page reloads instead of submitting
- Not trimming whitespace, allowing blank todos to be added
- Mutating `todoList` directly instead of using the functional update pattern
- Using array index as `id` instead of `Date.now()`
- Missing `name="todoTitle"` on the input, breaking `event.target.todoTitle.value`
- `ref` not attached to the input element
- `console.log` statements left in the submitted code

---

## References

- CTD React Curriculum Exercises Repo
  `Code-the-Dream-School/react-curriculum-v4-exercises`
- Lesson-04 Assignment
  `react-curriculum-v4/learns-app-content/lesson-04-hooks-events-handlers/assignment.md`
