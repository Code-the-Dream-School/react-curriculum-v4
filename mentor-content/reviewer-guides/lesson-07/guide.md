# Lesson-07 Exercise : Todo List - API Integration & Authentication

This guide helps mentors review Lesson-07 submissions quickly and consistently.

---

## Functional Specifications

### User Requirements

A user should be able to:

- See a login form before accessing the todo list
- Log in with email and password, with an error message on failure
- Fetch their todos from the API after login
- Add, complete, and edit todos with changes persisted to the server
- See a loading indicator while API operations are in progress
- See an error message if an API operation fails, with a "Clear Error" button
- Experience immediate UI updates (optimistic) for CRUD operations

### Acceptance Criteria

- [ ] `.env` file created with `VITE_TARGET` and listed in `.gitignore`
- [ ] `vite.config.js` configured with Vite server proxy for `/api/*`
- [ ] Logon component renders with email and password form
- [ ] Incorrect credentials display an error message
- [ ] Successful login shows the todos page
- [ ] Todos are loaded from the API on login
- [ ] Loading indicator shown during fetch
- [ ] Todos can be added, completed, and edited -- all persisted to the API
- [ ] Failed API operations display an error and roll back the optimistic UI update
- [ ] No `console.log` statements left in submitted code

---

## Technical Specifications

### Environment & Proxy

- `.env` contains `VITE_TARGET=https://ctd-learns-node-l42tx.ondigitalocean.app`
- `.env` is in `.gitignore` -- verify it is **not** committed
- `vite.config.js` uses `loadEnv` and proxies `/api` to `VITE_TARGET` with cookie header rewriting

### `src/shared/Header.jsx`

- Simple component rendering an `<h1>` with "Todo List"
- Default export

### `src/features/Logon.jsx` (Pessimistic UI update)

- Controlled inputs for `email` and `password`
- `isLoggingOn` state for loading, `authError` state for error message
- `handleSubmit` uses `try/catch/finally`: POST to `/api/users/logon` with `credentials: 'include'`
- Token set **only after** a successful `200` response (pessimistic pattern)
- Calls `onSetEmail(data.name)` and `onSetToken(data.csrfToken)` on success
- Button disabled and shows "Logging in..." during `isLoggingOn`

### `App.jsx`

- `email` and `token` state managed here
- Conditionally renders `<Logon onSetEmail={setEmail} onSetToken={setToken} />` or `<TodosPage token={token} />`
- `<Header />` rendered above both

### `src/features/Todos/TodosPage.jsx`

- Accepts `token` prop
- `error` and `isTodoListLoading` state
- `useEffect` calls `fetchTodos` when `token` exists; `[token]` in dependency array
- GET `/api/tasks` with `X-CSRF-TOKEN: token` header and `credentials: 'include'`
- Response uses `data.tasks` array to set `todoList` state
- `addTodo`: optimistic add with `Date.now()` temp id, POST to `/api/tasks`, replaces temp todo with server response on success, removes temp todo and sets error on failure
- `completeTodo`: stores original todo, optimistic update, PATCH to `/api/tasks/${id}`, rolls back on failure
- `updateTodo`: stores original todo, optimistic update, PATCH to `/api/tasks/${editedTodo.id}`, rolls back on failure
- JSX includes error display (with "Clear Error" button) and loading indicator

---

## Quality Checklist

- [ ] `.env` is **not** committed to the repo
- [ ] Logon uses pessimistic update -- token set only after successful response
- [ ] All three CRUD functions use optimistic update with rollback on failure
- [ ] All fetch requests include `credentials: 'include'` and `X-CSRF-TOKEN` header
- [ ] `useEffect` for `fetchTodos` has `token` in the dependency array
- [ ] `data.tasks` used (not `data`) when setting todo list from fetch response
- [ ] Error state cleared appropriately when new operations start
- [ ] No console errors or warnings; no `console.log` statements left in code

---

## ❌ Common Issues to Watch For

- `.env` committed to git, or missing from `.gitignore`
- `vite.config.js` proxy not configured -- causes CORS errors on API calls
- Missing `credentials: 'include'` on fetch calls -- authentication cookies not sent
- Missing `X-CSRF-TOKEN` header -- API rejects all requests with 403
- `useEffect` missing `token` in dependency array -- todos never load, or stale closure
- Using `data` directly instead of `data.tasks` to set todo list
- Rollback not implemented -- a failed mutation leaves the UI in a broken state
- Pessimistic/optimistic confused in Logon -- token set before checking response status

---

## References

- CTD React Curriculum Exercises Repo
  `Code-the-Dream-School/react-curriculum-v4-exercises`
- Lesson-07 Assignment
  `react-curriculum-v4/learns-app-content/lesson-07-data-fetching-UI-update-strategies/assignment.md`
