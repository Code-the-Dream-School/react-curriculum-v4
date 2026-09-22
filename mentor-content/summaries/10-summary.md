# Mentor Summary: React Week 10: React Router

## Key Concepts from Discussion
The focus this week is on overcoming the limitations of standard SPAs (broken "Back" buttons and unshareable URLs) by using **React Router** to mimic traditional multi-page navigation.

*   **Routing Architecture:** 
    *   **`BrowserRouter`:** Provides the context for history management and navigation logic.
    *   **`Routes` and `Route`:** Perform pattern-matching on URL segments to render specific UI elements.
    *   **Declarative Navigation:** Using `Link` and `NavLink` (which includes active state tracking) instead of anchor tags to prevent full-page refreshes.
*   **Routing Hooks:**
    *   **`useParams`:** Extracts dynamic segments (e.g., `:id`) for data fetching or item lookups.
    *   **`useSearchParams`:** Manages query strings (e.g., `?status=completed`) to keep UI state bookmarkable and shareable.
    *   **`useNavigate`:** Facilitates programmatic navigation (e.g., redirecting after a form submission) or moving through history with delta values (e.g., `navigate(-1)`).
    *   **`useLocation`:** Provides information about the current URL, often used to preserve a user's intended destination during login redirects.
*   **Catch-all Routes:** Implementation of the `*` path to handle 404 errors.

## Assignment Content Summary
Students refactored their application to support a multi-page structure with protected access.

*   **Router Setup:** Wrapped the application in `BrowserRouter` within `main.jsx`, following a specific provider order: `StrictMode` → `BrowserRouter` → `AuthProvider` → `App`.
*   **Page-Based Reorganization:** 
    *   Moved `TodosPage` to a new `src/pages/` directory.
    *   Created `HomePage` (for redirects), `AboutPage`, `ProfilePage` (with API-driven statistics), and `NotFoundPage`.
*   **Authentication Guard:** 
    *   Implemented a **`RequireAuth` wrapper component** that checks for authentication status.
    *   Unauthenticated users attempting to access `/todos` or `/profile` are redirected to `/login`, while their original location is stored in state to allow for post-login redirects.
*   **Navigation & Styling:** Built a shared `Navigation` component using `NavLink`. Students practiced using the `isActive` callback to apply conditional bold/underline styling to active links.
*   **URL-Based Filtering:** 
    *   Integrated a `StatusFilter` component that uses `useSearchParams` to manage todo filtering (`all`, `active`, `completed`).
    *   Updated the `TodoList` useMemo logic to derive its filtered state directly from the URL parameters.

## Potential Student Trouble Spots
*   **Import Package Confusion:** With React Router v7, students must ensure they are importing from `react-router` rather than the older `react-router-dom` package.
*   **Catch-all Route Placement:** If the `*` (404) route is not placed last in the `Routes` component, it may intercept valid paths.
*   **Provider Nesting:** Students might encounter errors if they attempt to use router hooks (like `useNavigate`) in components that are not children of the `BrowserRouter`.
*   **State Preservation on Redirect:** Implementing the "redirect back to the original page" logic in `RequireAuth` using `useLocation` state can be a complex concept for students to grasp.
*   **NavLink Styling Syntax:** The `style` or `className` prop on `NavLink` accepts a function that receives an `isActive` object; students often struggle with this functional syntax compared to standard string props.
*   **URL vs. Memory State:** Students may find it challenging to synchronize the `select` dropdown value with the `useSearchParams` hook, especially when clearing the parameter for the "all" view.
