# Mentor Summary: React Week 5: Conditional Rendering & Controlled Components

This document summarizes the Lesson 5 curriculum, focusing on adaptive UI patterns and the "controlled component" strategy for form management.

## Key Concepts from Discussion
The discussions this week focus on making the UI more interactive and synchronizing form data with React state.

*   **Conditional Rendering Techniques:** Students explored several patterns for adapting the UI based on state:
    *   **Ternary Operator:** Used for mutually exclusive views (e.g., "loading" vs. "content" or "empty list" vs. "populated list").
    *   **Logical `&&` Operator:** Used to toggle elements on or off when there is no "else" case.
    *   **Functions/Switch-Case:** Employed for complex logic where multiple different UI states are possible based on a single piece of data.
*   **Controlled Components:** Introduction to the "Single Source of Truth" pattern for forms. Unlike uncontrolled components that use `useRef` to poll the DOM, controlled components use `useState` to drive input values and `onChange` handlers to update that state. 
    *   Benefits include easier validation, dynamic UI updates (like disabling buttons), and better testability.
*   **Local vs. Lifted State:** The curriculum discusses performance and management trade-offs of keeping state local to a component (like a "working copy" of a form) versus lifting it to a common parent.
*   **Immutability in Updates:** Reinforcement of using the spread operator to create new objects/arrays during state updates rather than mutating the original data.

## Assignment Content Summary
Students added a "completion" feature to their Todo List and refactored their form logic.

*   **Conditional Empty State:** In `TodoList.jsx`, students implemented a ternary operator to show the message "Add todo above to get started" only when the list is empty.
*   **Todo Completion Feature:**
    *   Updated the todo object structure to include an `isCompleted` boolean.
    *   Implemented a `completeTodo` handler in `App.jsx` that uses `.map()` and the spread operator to update a specific item's status.
    *   Added a checkbox to `TodoListItem.jsx` where the `checked` prop is tied to `todo.isCompleted`.
*   **List Filtering:** Updated `TodoList.jsx` to create a `filteredTodoList` that only displays items where `isCompleted` is false, causing completed items to "disappear" from the main view.
*   **Controlled Form Refactor:** 
    *   Refactored `TodoForm.jsx` from using `useRef` to `useState` (`workingTodoTitle`).
    *   The input is now "controlled," with its `value` tied to state and its `onChange` updating that state.
*   **Basic Form Validation:** Implemented logic to disable the "Add Todo" button if the input is empty or contains only whitespace using `.trim()`.

## Potential Student Trouble Spots
*   **Filtering vs. Deleting:** Students may be confused when items disappear after checking the box. Mentors can clarify that the data still exists in the `todoList` state in `App`, but is being filtered out of the view in `TodoList`.
*   **The "One Character Behind" Bug:** When using `console.log` inside an `onChange` handler to check state, students often see the *previous* value because state updates are asynchronous.
*   **Spread Operator for Immutability:** When updating the `isCompleted` property, students may struggle with the syntax for returning a new object while maintaining other properties (e.g., `{...todo, isCompleted: true}`).
*   **Prop Drilling Friction:** This week requires passing the completion handler from `App` -> `TodoList` -> `TodoListItem`. Students might forget to destructure the prop at every level.
*   **Controlled Input Reset:** Students may forget to call the state setter with an empty string (`""`) after the form is submitted, leaving the text in the input box.
