# Lesson-09 Exercise : Todo List - useReducer & useContext

This guide helps mentors review Lesson-09 submissions quickly and consistently.

---

## Functional Specifications

### User Requirements

A user should experience no change in visible behavior -- this assignment is a structural refactor. All existing features (login, fetch, add, complete, edit, sort, filter, error handling) must continue to work exactly as before.

### Acceptance Criteria

- [ ] `src/reducers/todoReducer.js` created and exported
- [ ] `TodosPage` uses a single `useReducer` call -- no individual `useState` for todo-related state
- [ ] All state updates in `TodosPage` go through `dispatch`
- [ ] `src/contexts/AuthContext.jsx` created with `AuthProvider` and `useAuth`
- [ ] `main.jsx` wraps app in `<AuthProvider>`
- [ ] No authentication props passed between components
- [ ] Login, logout, and all todo operations work correctly
- [ ] No console errors or warnings

---

## Technical Specifications

### `src/reducers/todoReducer.js`

- Exports `TODO_ACTIONS` object with named constants for all action types:
  - Fetch: `FETCH_START`, `FETCH_SUCCESS`, `FETCH_ERROR`
  - Add: `ADD_TODO_START`, `ADD_TODO_SUCCESS`, `ADD_TODO_ERROR`
  - Complete: `COMPLETE_TODO_START`, `COMPLETE_TODO_SUCCESS`, `COMPLETE_TODO_ERROR`
  - Update: `UPDATE_TODO_START`, `UPDATE_TODO_SUCCESS`, `UPDATE_TODO_ERROR`
  - UI: `SET_SORT`, `SET_FILTER`, `CLEAR_ERROR`, `CLEAR_FILTER_ERROR`, `RESET_FILTERS`
- Exports `initialTodoState` with all 8 properties matching prior `useState` defaults
- Exports `todoReducer(state, action)`:
  - Every case spreads `state` before overriding only changed properties
  - `FETCH_START` sets `isTodoListLoading: true` and clears errors
  - `FETCH_SUCCESS` sets `todoList`, clears loading and errors
  - `FETCH_ERROR` sets error fields, clears loading
  - Optimistic START actions apply changes immediately; ERROR actions restore prior state from `action.payload`
  - `default` throws an error for unknown action types

### `TodosPage.jsx`

- Single `const [state, dispatch] = useReducer(todoReducer, initialTodoState)`
- Individual state values destructured from `state`
- All `setXxx()` calls replaced with `dispatch({ type: TODO_ACTIONS.XXX, payload: ... })`
- No `useState` calls for todo-related state remaining

### `src/contexts/AuthContext.jsx`

- `AuthContext` created with `createContext()`
- `useAuth()` hook: calls `useContext(AuthContext)`, throws descriptive error if context is undefined
- `AuthProvider({ children })`:
  - Manages `email` and `token` state
  - `login(userEmail, password)`: POST to `/api/users/logon`, sets state on success, returns `{ success, error }`
  - `logout()`: clears state whether API succeeds or fails, returns `{ success, error }`
  - Provides `{ email, token, isAuthenticated: !!token, login, logout }` via context value
  - Renders `<AuthContext.Provider value={value}>{children}</AuthContext.Provider>`

### Updated Components

- `main.jsx`: `<BrowserRouter>` → `<AuthProvider>` → `<App>`
- `App.jsx`: removes `email`/`token` state, uses `useAuth()` for `isAuthenticated`, no auth props on children
- `Header.jsx`: no props, uses `useAuth()` for `isAuthenticated` if needed
- `Logon.jsx`: no props, uses `useAuth().login()`
- `Logoff.jsx`: no props, uses `useAuth().logout()`
- `TodosPage.jsx`: no `token` prop, uses `useAuth().token`

---

## Quality Checklist

- [ ] All reducer cases use the spread operator -- no state mutation
- [ ] `TODO_ACTIONS` constants used throughout (not hardcoded strings)
- [ ] Optimistic ERROR cases include rollback: previous state restored from payload
- [ ] `useAuth()` throws a descriptive error when used outside `AuthProvider`
- [ ] `AuthProvider` wraps the app at the top level in `main.jsx` (not inside `App`)
- [ ] No auth props passed through any component
- [ ] `logout()` clears state regardless of whether the API call succeeds

---

## ❌ Common Issues to Watch For

- Hardcoded action type strings instead of `TODO_ACTIONS` constants -- typos cause silent bugs
- Mutating state in reducer cases (forgetting to spread `state`)
- Missing action types for some operations -- those operations silently stop working
- ERROR action cases not rolling back to prior state -- failed operations leave corrupted UI
- `useAuth` called outside of `AuthProvider` -- causes cryptic "Cannot read properties of undefined" errors
- `AuthProvider` added as a child inside `App` instead of wrapping it in `main.jsx`
- `logout()` only clearing state on API success -- user stays logged in if the logoff API call fails
- Auth state left partially in `App.jsx` instead of fully removed

---

## References

- CTD React Curriculum Exercises Repo
  `Code-the-Dream-School/react-curriculum-v4-exercises`
- Lesson-09 Assignment
  `react-curriculum-v4/learns-app-content/lesson-09-advanced-state-useReducer-useContext-continued/assignment.md`
