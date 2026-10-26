# Mentor Summary: React Week 3: State and Props

## Key Concepts from Discussion
The focus this week is moving beyond static components to manage dynamic data using React’s built-in hooks and props system.

*   **Understanding State:** Students learned that while standard variables can hold data, they do not trigger re-renders when updated. State is introduced as a special mechanism for data that changes over time.
*   **The `useState` Hook:** This is the primary tool taught for local state management. Key takeaways include:
    *   Using **array destructuring** to access the state value and its updater function.
    *   The requirement for **immutability** (never mutating state directly).
    *   **Functional Updating:** The importance of passing a callback function (e.g., `setCount(prev => prev + 1)`) to the setter to avoid "stale state" bugs caused by React's batching of updates.
*   **Component Lifecycle:** Introduction to the three phases: **Mount** (appearing), **Update** (re-rendering), and **Unmount** (removal).
*   **Rules of Hooks:** Hooks must be called at the top level of a function and only within React components or custom hooks.
*   **Props & Data Flow:** 
    *   Props are used to pass data from parent to child components.
    *   Props are **read-only** (immutable) for the receiving component.
    *   **Destructuring Props:** Students are encouraged to destructure props directly in the function arguments for cleaner code.
    *   **The `children` Prop:** Passing elements between a component's opening and closing tags.
*   **The `key` Prop:** Reinforcing why `key` is essential for list reconciliation and why it is a "special" prop that isn't actually accessible inside the child component.

## Assignment Content Summary
Students transitioned their hardcoded Todo List into a dynamic, state-managed application with a more modular structure.

*   **Implementing State:** Students moved the hardcoded `todoList` array into a `useState` hook inside `App.jsx`.
*   **Props Refactoring:** 
    *   The `TodoList` component was updated to receive the `todoList` state as a prop.
    *   Students practiced destructuring this prop (e.g., `function TodoList({ todoList })`).
*   **Modularizing Items:** 
    *   A new `TodoListItem.jsx` component was created to render individual list items.
    *   The mapping logic in `TodoList` was refactored to return instances of `TodoListItem` instead of raw `<li>` elements.
*   **Prop Drilling:** Students passed individual `todo` objects from the `.map()` function in `TodoList` down into each `TodoListItem`.

## Potential Student Trouble Spots
*   **Stale Closures & Batching:** Students may struggle to understand why multiple `setCount(count + 1)` calls in a single handler don't increment the value multiple times, making the "functional update" pattern a frequent point of confusion.
*   **The "Key" Prop Misplacement:** A common error is placing the `key` prop inside the `TodoListItem` component's return statement (the `<li>` tag) rather than on the `TodoListItem` instance itself within the parent's `.map()` function.
*   **Direct Mutation:** Mentors may see students attempting to use methods like `.push()` on a state array instead of creating a new array via the setter function.
*   **Prop Destructuring Syntax:** Transitioning from `props.todo.title` to a destructured `{ todo }` in the function parameters can lead to syntax errors if they forget to wrap the argument in curly braces.
*   **State Location:** Students may try to initialize state inside `TodoList` instead of `App`, making it difficult to share that data with other components (like a future form).
