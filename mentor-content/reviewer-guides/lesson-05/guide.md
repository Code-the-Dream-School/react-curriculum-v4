# Lesson-05 Exercise : Todo List - Controlled Forms & Completion

This guide helps mentors review Lesson-05 submissions quickly and consistently.

---

## Functional Specifications

### User Requirements

A user should be able to:

- See "Add todo above to get started" when the list is empty
- Have the Add Todo button disabled when the input is empty
- Check a checkbox next to a todo to mark it complete
- See completed todos disappear from the list immediately

### Acceptance Criteria

- [ ] Empty todo list shows "Add todo above to get started"
- [ ] Add Todo button is disabled when the input field is empty
- [ ] Each todo has a checkbox
- [ ] Checking a checkbox removes the todo from the list
- [ ] Form input is a controlled component (value tied to state)
- [ ] Form input clears after a todo is submitted

---

## Technical Specifications

### `App.jsx`

- `addTodo` updated to include `isCompleted: false` in the new todo object
  - Each todo now has three properties: `id`, `title`, and `isCompleted`
- `completeTodo` function that:
  - Accepts an `id` parameter
  - Maps over `todoList`, returning `{ ...todo, isCompleted: true }` for the match and the original todo otherwise
  - Updates state with the resulting array
- `onCompleteTodo={completeTodo}` passed to `<TodoList />`

### `TodoList.jsx`

- `filteredTodoList` created by filtering out todos where `isCompleted` is `true`
- Ternary in the return:
  - If `filteredTodoList.length === 0`, renders `<p>Add todo above to get started</p>`
  - Otherwise renders `<ul>` with mapped `TodoListItem` components
- `onCompleteTodo` destructured from props and passed to each `TodoListItem`

### `TodoListItem.jsx`

- `onCompleteTodo` destructured from props
- Checkbox input with:
  - `type="checkbox"`
  - `checked={todo.isCompleted}`
  - `onChange={() => onCompleteTodo(todo.id)}`

### `TodoForm.jsx`

- `useState` imported; `workingTodoTitle` state initialized to `''`
- Input has `value={workingTodoTitle}` and `onChange={e => setWorkingTodoTitle(e.target.value)}`
- `handleAddTodo` updated to:
  - Use `workingTodoTitle` instead of `event.target.todoTitle.value`
  - Reset the input by calling `setWorkingTodoTitle('')` (not `event.target.reset()`)
- Submit button has `disabled={!workingTodoTitle.trim()}`

---

## Quality Checklist

- [ ] `completeTodo` uses the spread operator -- no direct mutation of todo objects
- [ ] Empty state check targets `filteredTodoList`, not the raw `todoList`
- [ ] Controlled input: value and onChange both wired to state
- [ ] Button `disabled` condition uses `.trim()` to catch whitespace-only input
- [ ] Props chain complete: `onCompleteTodo` flows App → TodoList → TodoListItem
- [ ] No console errors or warnings

---

## ❌ Common Issues to Watch For

- Mutating the todo object in `completeTodo` instead of using the spread operator
- Filtering logic on the wrong property or using the wrong boolean (e.g., filtering out `!isCompleted` instead)
- Empty state check applied to `todoList` instead of `filteredTodoList` -- message shows even when there are active todos
- Not updating `handleAddTodo` to use `workingTodoTitle` -- still reading from `event.target`
- Forgetting to call `setWorkingTodoTitle('')` after submit -- input doesn't clear
- Button `disabled` condition missing `.trim()` -- whitespace-only input enables the button

---

## References

- CTD React Curriculum Exercises Repo
  `Code-the-Dream-School/react-curriculum-v4-exercises`
- Lesson-05 Assignment
  `react-curriculum-v4/learns-app-content/lesson-05-local-state-controlled-components-forms/assignment.md`
