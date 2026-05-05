# Lesson-06 Exercise : Todo List - Refactoring & Reusable Components

This guide helps mentors review Lesson-06 submissions quickly and consistently.

---

## Functional Specifications

### User Requirements

A user should be able to:

- Add and complete todos exactly as before (no regressions)
- Click a todo title to enter inline edit mode
- Type an updated title in the edit input
- Click "Update" to save the new title
- Click "Cancel" to discard changes and restore the original title

### Acceptance Criteria

- [ ] Project reorganized: `src/features/`, `src/features/TodoList/`, `src/shared/`, `src/utils/` directories exist
- [ ] `TodoForm.jsx` is in `src/features/`
- [ ] `TodoList.jsx` and `TodoListItem.jsx` are in `src/features/TodoList/`
- [ ] `TextInputWithLabel.jsx` exists in `src/shared/` and is used in both TodoForm and TodoListItem
- [ ] `isValidTodoTitle` exported from `src/utils/todoValidation.js`
- [ ] Clicking a todo title enters edit mode
- [ ] Cancel resets to the original title and exits edit mode
- [ ] Update saves the new title (if valid) and exits edit mode
- [ ] App renders without errors after all refactoring

---

## Technical Specifications

### `src/shared/TextInputWithLabel.jsx`

- Accepts props: `elementId`, `labelText`, `onChange`, `ref`, `value`
- Returns a fragment with `<label htmlFor={elementId}>` and a controlled `<input type="text">`
- Exported as default

### `src/utils/todoValidation.js`

- Exports `isValidTodoTitle(title)` that returns `title.trim() !== ''`
- Used in both TodoForm (add button) and TodoListItem (update button)

### `TodoForm.jsx`

- Label and input replaced with a single `<TextInputWithLabel>` instance
- Props mapped: `elementId`, `labelText="Todo"`, `ref={inputRef}`, `value={workingTodoTitle}`, `onChange`
- Add button uses `disabled={!isValidTodoTitle(workingTodoTitle)}`

### `TodoListItem.jsx`

- `isEditing` state initialized to `false`
- `workingTitle` state initialized to `todo.title`
- Ternary in the return:
  - If `isEditing`: `<TextInputWithLabel>` with current `workingTitle`, plus Cancel and Update buttons
  - If not editing: checkbox form and `<span onClick={() => setIsEditing(true)}>{todo.title}</span>`
- `handleEdit(event)` updates `workingTitle` via `event.target.value`
- `handleCancel()` resets `workingTitle` to `todo.title` and sets `isEditing` to `false`
- `handleUpdate(event)` calls `event.preventDefault()`, calls `onUpdateTodo({ ...todo, title: workingTitle })`, sets `isEditing` to `false`
- Update button uses `disabled={!isValidTodoTitle(workingTitle)}`
- Form has `onSubmit={handleUpdate}`

### `App.jsx`

- `updateTodo(editedTodo)` maps over `todoList`, returning a spread of `editedTodo` for matching `id` and the unchanged todo otherwise
- `onUpdateTodo={updateTodo}` passed to `<TodoList>`

### `TodoList.jsx`

- `onUpdateTodo` destructured from props and passed down to each `<TodoListItem>`

---

## Quality Checklist

- [ ] All import paths updated correctly after file moves -- no broken imports
- [ ] `TextInputWithLabel` used in both create and edit contexts
- [ ] `workingTitle` initialized to `todo.title` (not empty string)
- [ ] `handleCancel` resets `workingTitle` back to `todo.title`
- [ ] `updateTodo` in App uses spread to avoid mutation
- [ ] `onUpdateTodo` prop chain complete: App → TodoList → TodoListItem
- [ ] No console errors or warnings

---

## ❌ Common Issues to Watch For

- Broken import paths after moving files (most common issue)
- `TextInputWithLabel` missing a prop or prop name misspelled
- `workingTitle` initialized to `''` instead of `todo.title`
- `handleCancel` not resetting `workingTitle` -- Cancel has no effect
- `handleUpdate` not calling `setIsEditing(false)` -- edit mode stays open after saving
- `onUpdateTodo` not threaded all the way through the prop chain (App → TodoList → TodoListItem)
- Stretch goal custom hook implemented with incorrect return shape (if attempted)

---

## References

- CTD React Curriculum Exercises Repo
  `Code-the-Dream-School/react-curriculum-v4-exercises`
- Lesson-06 Assignment
  `react-curriculum-v4/learns-app-content/lesson-06-reusable-components-project-organization-refactoring/assignment.md`
