# Lesson-08 Exercise : Todo List - Performance Optimization

This guide helps mentors review Lesson-08 submissions quickly and consistently.

---

## Functional Specifications

### User Requirements

A user should be able to:

- Sort todos by creation date or title, in ascending or descending order
- Search todos by title with a debounced filter input (no API call on every keystroke)
- See different error messages for sort/filter failures vs. general fetch failures
- Use "Clear Filter Error" and "Reset Filters" buttons to recover from filter/sort errors

### Acceptance Criteria

- [ ] SortBy dropdowns present and change the displayed todo order
- [ ] Network tab shows updated query parameters when sort options change
- [ ] FilterInput present; API call fires after a ~300ms delay when typing
- [ ] `dataVersion` increments after each successful add, complete, or update
- [ ] TodoList uses `useMemo` to compute its filtered list
- [ ] Filter/sort errors shown separately from general errors
- [ ] "Reset Filters" restores default sort and clears filter term
- [ ] No `console.log` statements left in submitted code

---

## Technical Specifications

### `TodosPage.jsx`

- `sortBy` (initial `'creationDate'`) and `sortDirection` (initial `'desc'`) state added
- `filterTerm` state and `debouncedFilterTerm` via `useDebounce(filterTerm, 300)`
- `fetchTodos` builds a `URLSearchParams` object with `sortBy`, `sortDirection`, and conditionally `find` (only when `debouncedFilterTerm` is non-empty)
- `useEffect` dependency array includes `token`, `sortBy`, `sortDirection`, and `debouncedFilterTerm`
- `dataVersion` state (initial `0`)
- `invalidateCache` wrapped in `useCallback(fn, [])`, increments `dataVersion` with functional update
- `invalidateCache()` called after each successful mutation in `addTodo`, `completeTodo`, and `updateTodo`
- `filterError` state (initial `''`); `fetchTodos` catch block distinguishes filter/sort errors from general errors
- `setFilterError('')` called on successful fetch
- JSX includes filter error display with "Clear Filter Error" and "Reset Filters" buttons

### `src/shared/SortBy.jsx`

- Accepts `sortBy`, `sortDirection`, `onSortByChange`, `onSortDirectionChange` props
- Two controlled `<select>` elements:
  - Sort field: options `creationDate` and `title`
  - Order: options `desc` and `asc`
- Default export

### `src/utils/useDebounce.js`

- Accepts `value` and `delay`
- Uses `useState` initialized to `value`
- `useEffect` sets a `setTimeout` to update debounced value after `delay`
- Cleanup function clears the timeout
- Dependency array: `[value, delay]`
- Returns the debounced value

### `src/shared/FilterInput.jsx`

- Accepts `filterTerm` and `onFilterChange` props
- Controlled input (`value={filterTerm}`, `onChange={e => onFilterChange(e.target.value)}`)
- Label with `htmlFor='filterInput'`, input with `id='filterInput'`
- Default export

### `TodoList.jsx`

- Accepts `dataVersion` prop
- `filteredTodoList` computed with `useMemo`:
  - Filters out `isCompleted` todos
  - Returns `{ version: dataVersion, todos: filteredTodos }`
  - Dependency array: `[todoList, dataVersion]`
- JSX renders `filteredTodoList.todos`

---

## Quality Checklist

- [ ] `useDebounce` cleanup function clears the timeout (prevents memory leaks)
- [ ] `debouncedFilterTerm` (not `filterTerm`) passed to `fetchTodos`
- [ ] `URLSearchParams` used -- not string concatenation
- [ ] `find` param only added to `URLSearchParams` when `debouncedFilterTerm` is non-empty
- [ ] `invalidateCache` uses functional update: `setDataVersion(prev => prev + 1)`
- [ ] `invalidateCache()` called after all three successful mutations
- [ ] `useMemo` dependency array includes both `todoList` and `dataVersion`
- [ ] Filter errors correctly distinguished from general fetch errors
- [ ] No `console.log` statements left in submitted code

---

## ❌ Common Issues to Watch For

- `useDebounce` missing the cleanup function -- causes stale state updates
- `filterTerm` sent directly to the API instead of `debouncedFilterTerm`
- `debouncedFilterTerm` missing from `useEffect` dependency array -- filter never triggers a re-fetch
- `find` param always included in URLSearchParams even when empty -- sends `find=` to API on every request
- `invalidateCache` not called after mutations -- `useMemo` never recalculates after changes
- `useMemo` missing `dataVersion` as a dependency -- stale filtered list after mutations
- Filter error not distinguished from general error -- "Reset Filters" button never appears

---

## References

- CTD React Curriculum Exercises Repo
  `Code-the-Dream-School/react-curriculum-v4-exercises`
- Lesson-08 Assignment
  `react-curriculum-v4/learns-app-content/lesson-08-advanced-state-useReducer-useContext/assignment.md`

> Note: Despite the folder name, lesson-08's assignment covers performance optimization (useCallback, useMemo, debounce). The useReducer/useContext content is in lesson-09.
