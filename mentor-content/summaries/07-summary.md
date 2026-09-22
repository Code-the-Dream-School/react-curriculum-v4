# Mentor Summary: React Week 7: Data Fetching & UI Strategies

## Key Concepts from Discussion
The discussions this week move the application from local state to full-stack integration using external APIs.

*   **CRUD and HTTP Methods:** Students mapped Create, Read, Update, and Delete operations to their respective HTTP verbs: POST, GET, PUT/PATCH, and DELETE.
*   **Asynchronous JavaScript:** Comparison of **Promises** (using `.then()/.catch()/.finally()`) versus **Async/Await** for writing more readable, synchronous-style asynchronous code.
*   **Data Fetching with `useEffect`:** 
    *   Handling the **StrictMode double-mount** bug: Students learned to use a cleanup function with a boolean flag (e.g., `isRan`) to ignore the results of the first of two rapid-fire requests during development.
*   **UI Update Strategies:**
    *   **Pessimistic:** Waiting for server confirmation before updating the UI. Best for critical operations like authentication or bank transfers where data integrity is paramount.
    *   **Optimistic:** Updating the UI immediately to ensure high responsiveness, then syncing with the server in the background. This requires robust **rollback logic** to revert changes if the API call fails.
*   **Vite Server Proxy:** Introduction to using a proxy to handle **cross-origin cookie issues**. By routing requests through the same origin (localhost), students avoid browser blocks on third-party cookies during development.

## Assignment Content Summary
Students transformed their Todo app into a client for a live backend API, requiring authentication and persistent storage.

*   **Vite Configuration:** Updated `vite.config.js` and `.env` to set up a server proxy for `/api` requests, allowing for secure cookie-based authentication and CSRF token handling.
*   **Refactoring to `TodosPage`:** Moved all todo-specific state and logic out of `App.jsx` into a dedicated `TodosPage` component to keep the top-level layout clean.
*   **Pessimistic Authentication:** Built a `Logon` component that uses a pessimistic strategy—only updating the application's `token` and `email` state after a successful POST request to the logon endpoint.
*   **API Integration in `TodosPage`:** 
    *   Used `useEffect` to fetch the initial todo list when the component mounts or the auth token changes.
    *   Implemented **optimistic updates** for adding, completing, and editing todos. This includes storing a "rollback" version of the state to restore the UI if the backend request fails.
*   **Visual Feedback:** Added loading indicators and error display sections (with clear buttons) to handle the inherently uncertain nature of network requests.

## Potential Student Trouble Spots
*   **Proxy Failures:** If the `VITE_TARGET` environment variable is missing or the proxy isn't configured exactly as instructed in `vite.config.js`, all API calls will fail with 404 or connection errors.
*   **Rollback State Management:** A common error is failing to capture the "original" todo object *before* applying the optimistic update, which makes reverting to the correct state impossible during an API failure.
*   **Credential/Header Issues:** Forgetting to include `credentials: 'include'` or the `X-CSRF-TOKEN` header in fetch calls, which is required for the backend to recognize the session.
*   **StrictMode Confusion:** Students may still be confused by the "double-fire" behavior of `useEffect`. Mentors should reinforce the importance of the cleanup pattern for ignoring the first response.
*   **Dependency Array Bugs:** Mismanaging the `useEffect` dependency array (e.g., leaving out the `token`) can lead to missing data or infinite fetch loops.
