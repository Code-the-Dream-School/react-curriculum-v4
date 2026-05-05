# Lesson-10 Exercise : Todo List - React Router

This guide helps mentors review Lesson-10 submissions quickly and consistently.

---

## Functional Specifications

### User Requirements

A user should be able to:

- Navigate between Home, About, Login, Todos, and Profile pages via a nav bar
- See the current page's nav link highlighted as active
- Be redirected to login when accessing a protected route while unauthenticated
- Be redirected back to their intended destination after logging in
- Filter todos by status (all / active / completed) using the URL
- See a 404 page for any unrecognized URL

### Acceptance Criteria

- [ ] `react-router` v7 installed; all imports use `'react-router'` (not `'react-router-dom'`)
- [ ] `BrowserRouter` wraps the app in `main.jsx`
- [ ] Pages created: `HomePage`, `LoginPage`, `AboutPage`, `TodosPage`, `ProfilePage`, `NotFoundPage`
- [ ] `RequireAuth` redirects unauthenticated users to `/login` with location preserved in state
- [ ] All routes configured in `App.jsx`; catch-all `*` route is last
- [ ] `Navigation` component uses `NavLink` with active styling
- [ ] `StatusFilter` uses `useSearchParams` -- `?status=` appears in the URL when filtering
- [ ] `TodoList` handles `statusFilter` prop for all / active / completed
- [ ] Logout navigates to `/login`
- [ ] Invalid URLs show the 404 page

---

## Technical Specifications

### `main.jsx`

- Wrapper order: `StrictMode` → `BrowserRouter` → `AuthProvider` → `App`

### `src/components/RequireAuth.jsx`

- Checks `isAuthenticated` from `useAuth()`
- If not authenticated: calls `navigate('/login', { state: { from: location } })` via `useNavigate` and `useLocation`
- Returns a redirect message while navigating; returns `children` when authenticated

### `src/pages/LoginPage.jsx`

- Reads `location.state?.from?.pathname` (defaults to `/todos`) for post-login redirect
- `useEffect` redirects if already authenticated
- Calls `login(email, password)` from `useAuth()`

### `src/pages/HomePage.jsx`

- `useEffect` redirects authenticated users to `/todos`, unauthenticated users to `/login`

### `src/pages/ProfilePage.jsx`

- Uses `useAuth()` to get `token` and `email`
- Fetches `/api/tasks` with `X-CSRF-TOKEN` header to calculate and display total, completed, and active todo counts

### `src/pages/NotFoundPage.jsx`

- Simple 404 page with a `Link` back to `/`

### `App.jsx`

- Imports `Routes`, `Route` from `'react-router'`
- Route structure:
  - `/` → `<HomePage />`
  - `/about` → `<AboutPage />`
  - `/login` → `<LoginPage />`
  - `/todos` → `<RequireAuth><TodosPage /></RequireAuth>`
  - `/profile` → `<RequireAuth><ProfilePage /></RequireAuth>`
  - `*` → `<NotFoundPage />` (must be last)

### `src/shared/Navigation.jsx`

- Imports `NavLink` from `'react-router'`
- `navLinkStyle` function receives `{ isActive }` and returns style object (bold + underline when active)
- Shows About always; shows Todos + Profile when authenticated; shows Login when not

### `src/shared/StatusFilter.jsx`

- Uses `useSearchParams` from `'react-router'`
- Removes `status` param for 'all' (keeps URL clean); sets `status` param for 'active' or 'completed'

### `TodoList.jsx`

- Accepts `statusFilter` prop (default `'active'`)
- `useMemo` updated: switch statement handles `'all'`, `'active'`, `'completed'`; `statusFilter` added to dependency array
- Empty state message is context-aware based on `statusFilter`

### `src/features/Logoff.jsx`

- Uses `useNavigate` from `'react-router'`; calls `navigate('/login')` after successful logout

---

## Quality Checklist

- [ ] All React Router imports use `'react-router'`, not `'react-router-dom'`
- [ ] `BrowserRouter` is *outside* `AuthProvider` in `main.jsx`
- [ ] `RequireAuth` passes `location` in state so redirect-back-after-login works
- [ ] `LoginPage` reads `location.state?.from?.pathname` for the redirect destination
- [ ] Catch-all `*` route is the last `<Route>` in `App.jsx`
- [ ] `NavLink` uses a style/className function (not hardcoded active styling)
- [ ] `StatusFilter` removes the `status` param for 'all' (`searchParams.delete('status')`)
- [ ] `TodosPage` has been moved to `src/pages/`
- [ ] No console errors during navigation transitions

---

## ❌ Common Issues to Watch For

- Importing from `react-router-dom` instead of `react-router` (v7 breaking change)
- `BrowserRouter` placed inside `AuthProvider` -- breaks `useNavigate` in auth components
- `RequireAuth` not preserving `location` in state -- after login, user is always sent to `/todos` regardless of where they were going
- `NavLink` active styling hardcoded -- does not change as the user navigates
- Catch-all `*` route placed before other routes -- every URL matches 404
- `StatusFilter` not calling `searchParams.delete('status')` for 'all' -- URL shows `?status=all` instead of being clean
- `ProfilePage` not passing `X-CSRF-TOKEN` header -- API returns 401 for stats fetch
- `Logoff` not navigating after logout -- user sees a blank or broken state

---

## References

- CTD React Curriculum Exercises Repo
  `Code-the-Dream-School/react-curriculum-v4-exercises`
- Lesson-10 Assignment
  `react-curriculum-v4/learns-app-content/lesson-10-react-router/assignment.md`
