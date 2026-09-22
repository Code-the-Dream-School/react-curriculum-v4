# Mentor Summary: React Week 9: Advanced State with useReducer and useContext

## Key Concepts from Discussion
The focus is on professionalizing state management by centralizing logic and eliminating the "prop drilling" that often plagues growing applications.

*   **The `useReducer` Pattern:** Students explored the mental model shift from "updating a specific value" (`useState`) to "handling a user action" (`useReducer`). Key concepts include:
    *   **Dispatch Function:** A "messenger" that delivers an action object instead of a direct value.
    *   **Actions and Payloads:** Standardized objects with a `type` and optional `payload` data.
    *   **The Reducer Function:** A pure function that calculates the next state based on the current state and the incoming action, facilitating centralized and testable logic.
*   **The Context API:** Introduced as the primary solution for **prop drilling**.
    *   **`createContext` and `Provider`:** Tools for defining a data source and making it available to an entire component tree.
    *   **`useContext` Hook:** Allows any nested component to "consume" shared data (like auth or themes) directly, bypassing intermediate components.
*   **Performance Considerations:** Understanding that context changes trigger re-renders for all consumers. Mitigation strategies include **splitting contexts** and **memoizing provider values** with `useMemo`.

## Assignment Content Summary
Students performed a comprehensive refactor of their existing Todo application to implement these advanced patterns.

*   **Consolidating Todo State:** In `TodosPage.jsx`, students replaced **eight separate `useState` calls** (todoList, loading, errors, sorting, etc.) with a single `useReducer`.
*   **Implementing `todoReducer.js`:** 
    *   Defined standardized `TODO_ACTIONS` constants to prevent typos.
    *   Moved all update logic (Fetch, Add, Complete, Update, and UI sorting/filtering) into cases within the reducer.
    *   Refined **optimistic updates** with robust **error rollback logic** within the reducer function.
*   **Authentication Context:**
    *   Created an `AuthContext.jsx` with a custom `useAuth` hook that includes built-in error checking for provider wrapping.
    *   Implemented an `AuthProvider` component to manage `email` and `token` state, along with centralized `login` and `logout` functions.
*   **Global Integration:** Wrapped the entire application in the `AuthProvider` within `main.jsx` and refactored `Header`, `Logon`, `Logoff`, and `TodosPage` to consume auth state directly from the context.

## Potential Student Trouble Spots
*   **Complexity & Boilerplate:** Students may find the transition from one-line `useState` calls to multi-part reducer actions and cases verbose and overwhelming.
*   **Immutability Errors:** Mentors should watch for cases where students forget the **spread operator** (`...state`) in the reducer, accidentally wiping out other state properties during an update.
*   **Context Wrapping Errors:** Using the `useAuth` hook in a component that isn't a child of the `AuthProvider` in the component tree will trigger a runtime error.
*   **Rollback Logic Sync:** Ensuring the "START" actions properly capture the previous state so that "ERROR" actions can correctly revert the UI during a failed optimistic update.
*   **Performance Bottlenecks:** Students might attempt to put all application state into a single global context, which can lead to unnecessary re-renders across the app.
