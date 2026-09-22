# Mentor Summary: React Week 4: Hooks, Events, and State Updates

## Key Concepts from Discussion
The discussions this week provide a broader landscape of React hooks and the mechanics of browser interaction.

*   **Hooks Overview:** Beyond `useState`, students were introduced to several categories of hooks, including **Effects** (`useEffect`), **Refs** (`useRef`), and a brief mention of performance hooks (`useMemo`, `useCallback`).
*   **The `useEffect` Hook:** 
    *   Used for "stepping outside" React to synchronize with external systems like APIs or WebSockets.
    *   **Dependency Arrays:** The behavior varies based on whether the array is omitted (runs every render), empty (runs once at mount), or contains specific values (runs when those values change).
    *   **Cleanup Functions:** The importance of returning a function to prevent memory leaks (e.g., closing a WebSocket).
*   **React StrictMode:** A development-only tool that intentionally triggers double-mounting and double-invoking effects to help developers identify side effects and missing cleanups.
*   **The `useRef` Hook:** Used to hold values that persist across renders without triggering a re-render themselves; primarily used for direct DOM access, such as managing input focus.
*   **Synthetic Events:** React's wrapper around native browser events to ensure cross-browser consistency. Key methods discussed include `event.preventDefault()` to stop form-submission page refreshes.
*   **Child-to-Parent Communication:** Passing callback functions (handlers) via props to allow children to notify parents of state changes.

## Assignment Content Summary
Students transformed their static list into a functional form that adds items to the state dynamically.

*   **Dynamic State Initialization:** Students cleared their hardcoded arrays in `App.jsx`, initializing `todoList` with an empty array `useState([])`.
*   **Functional State Updates:** 
    *   Implemented an `addTodo` function in `App.jsx` that takes a title and creates a new object.
    *   Used the **functional update pattern** (`setTodoList(prev => [newTodo, ...prev])`) to ensure state consistency when the new state depends on the previous one.
    *   Used `Date.now()` as a temporary unique ID generator for new todos.
*   **Form Implementation in `TodoForm.jsx`:**
    *   Utilized the `useRef` hook to target the input element.
    *   Created a `handleAddTodo` handler that extracts the input value via `event.target.todoTitle.value`.
    *   The handler trims whitespace, calls the `onAddTodo` prop passed from the parent, resets the form, and uses the ref to **refocus the input** for a better user experience.

## Potential Student Trouble Spots
*   **StrictMode Confusion:** Students often think their code is buggy or running twice by accident when they see double logs in the console; they need to understand this is an intentional development feature.
*   **Missing `preventDefault()`:** Forgetting this inside the form handler causes the entire page to refresh on submit, clearing the React state.
*   **The "Rule of Refs":** Students might try to read or write to `ref.current` during the actual rendering phase (the top level of the component) instead of inside an effect or event handler, which leads to unpredictable behavior.
*   **Form `name` Attribute:** The assignment relies on `event.target.todoTitle.value`. If a student forgets to add `name="todoTitle"` to their `<input />`, this will return `undefined`.
*   **Spread Operator Mistakes:** When updating state, students may forget to spread the previous state, effectively overwriting their entire list with just the single new todo item.
