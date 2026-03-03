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

# React Router
## Week 10

Code the Dream · React Curriculum v4

---

# 🌡️ Warm-Up (5 min)

**Think about your current todo app.**

> *In the chat: If you paste `http://localhost:5173/profile` into the browser address bar right now, what happens? What should happen?*

*(Try it if you're not sure!)*

<!--
Speaker note: Right now, the app either shows nothing or the same page — because there's no routing. React Router solves this. Students often haven't thought about what makes "a page" in an SPA.
-->

---

# SPAs and the Routing Problem

Traditional websites: each URL → server returns a new HTML page.

React SPAs (Single Page Applications): one HTML file, JavaScript handles everything.

**The problem without React Router:**
- Navigating to `/todos` or `/profile` returns 404 (no HTML file exists)
- The browser's Back/Forward buttons don't work as expected
- You can't share a URL that links to a specific "page"

React Router solves this with **client-side routing**.

---

# React Router: Core Components

Install (v7 — imports from `'react-router'`):
```bash
npm install react-router
```

Key components:

| Component | Purpose |
|-----------|---------|
| `<BrowserRouter>` | Wraps the app, enables routing |
| `<Routes>` | Container for all route definitions |
| `<Route>` | Maps a URL path to a component |
| `<Link>` | Navigate without a page reload |
| `<NavLink>` | Like `Link`, but with active-state styling |

---

# Setting Up Routes in App.jsx

```jsx
import { Routes, Route } from 'react-router'

function App() {
  return (
    <>
      <Header />
      <Routes>
        <Route path="/" element={<HomePage />} />
        <Route path="/todos" element={<TodosPage />} />
        <Route path="/about" element={<AboutPage />} />
        <Route path="/login" element={<LoginPage />} />
        <Route path="/profile" element={<ProfilePage />} />
        <Route path="*" element={<NotFoundPage />} />
      </Routes>
    </>
  )
}
```

> The `*` route is the catch-all — it matches any URL that didn't match above.

---

# Wrapping the App in main.jsx

Order matters:

```jsx
// main.jsx
import { BrowserRouter } from 'react-router'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <BrowserRouter>
      <AuthProvider>
        <App />
      </AuthProvider>
    </BrowserRouter>
  </StrictMode>
)
```

`BrowserRouter` must be the outermost wrapper so that routing hooks work everywhere.

---

# Navigation: Link vs. NavLink vs. useNavigate

```jsx
// Link — basic navigation
<Link to="/todos">Go to Todos</Link>

// NavLink — adds active class styling
<NavLink
  to="/todos"
  style={({ isActive }) => ({ fontWeight: isActive ? 'bold' : 'normal' })}
>
  Todos
</NavLink>

// useNavigate — programmatic navigation
const navigate = useNavigate()
function handleLogin() {
  await login(credentials)
  navigate('/todos')   // redirect after login
}
```

---

# Useful Router Hooks

| Hook | What it gives you |
|------|------------------|
| `useNavigate()` | Function to navigate programmatically |
| `useParams()` | URL segment values (e.g., `/todos/:id` → `{ id: '42' }`) |
| `useSearchParams()` | Read/write URL query params (`?status=completed`) |
| `useLocation()` | Current URL, state, and search string |

---

# Protected Routes: Require Authentication

Create a wrapper that redirects unauthenticated users:

```jsx
// src/components/RequireAuth.jsx
import { useNavigate, useLocation } from 'react-router'
import { useAuth } from '../contexts/AuthContext'

function RequireAuth({ children }) {
  const { isAuthenticated } = useAuth()
  const navigate = useNavigate()
  const location = useLocation()

  if (!isAuthenticated) {
    navigate('/login', { state: { from: location } })
    return null
  }
  return children
}
```

---

# Using RequireAuth in Routes

```jsx
<Route
  path="/todos"
  element={
    <RequireAuth>
      <TodosPage />
    </RequireAuth>
  }
/>
<Route
  path="/profile"
  element={
    <RequireAuth>
      <ProfilePage />
    </RequireAuth>
  }
/>
```

> `location.state.from` lets `LoginPage` redirect back to the intended page after login.

---

# URL-Based Filtering with useSearchParams

```jsx
// StatusFilter.jsx
function StatusFilter() {
  const [searchParams, setSearchParams] = useSearchParams()
  const status = searchParams.get('status') || 'all'

  return (
    <select
      value={status}
      onChange={(e) => setSearchParams({ status: e.target.value })}
    >
      <option value="all">All</option>
      <option value="active">Active</option>
      <option value="completed">Completed</option>
    </select>
  )
}
```

URL becomes: `/todos?status=completed` — shareable and bookmarkable!

---

# 🤝 We Do: Add Basic Routing (10 min)

Let's add React Router to the app together:

1. `npm install react-router`
2. Wrap `<App />` with `<BrowserRouter>` in `main.jsx`
3. Create a simple `NotFoundPage` component
4. Add `<Routes>` and `<Route>` elements in `App.jsx` for `/todos`, `/login`, and `*`

**Discussion:** What should happen when you navigate to `/` before logging in?

<!--
Speaker note: Walk through this live. Show that clicking a Link navigates without a page reload. Show that the `*` route catches unknown URLs. The HomePage could use useEffect + useNavigate to redirect based on isAuthenticated.
-->

---

# 💪 You Do: Create RequireAuth (8 min)

1. Create `src/components/RequireAuth.jsx`
2. Use `useAuth()` to check `isAuthenticated`
3. If not authenticated, navigate to `/login` (pass current `location` in state)
4. If authenticated, return `children`
5. Wrap the `/todos` and `/profile` routes with `<RequireAuth>`

Test: log out and try visiting `/todos` directly — do you get redirected?

*Timer: 8 minutes.*

<!--
Speaker note: Let students work silently. Be available for questions in chat. Common issue: the redirect causes an infinite loop if the login page is also wrapped in RequireAuth. Make sure /login does NOT use RequireAuth. Another issue: using useNavigate outside a Router — remind them that BrowserRouter must be in main.jsx before any hooks are used.
-->

---

# Wrap-Up (5 min)

**Check for understanding** — in the chat:

- What is the difference between `Link` and `useNavigate`?
- Why do we put `<BrowserRouter>` in `main.jsx` instead of `App.jsx`?
- How does `RequireAuth` know which page the user was trying to reach?

**Coming up in Lesson 11:**
- Making your app portfolio-ready (styling, README)
- Security best practices (XSS, env vars, input validation)
- Deploying to Vercel 🚀

> Any questions before we close?
