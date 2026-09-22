# Mentor Summary: React Week 2: Components, JSX, and Troubleshooting

## Key Concepts from Discussion
The discussions centered on how React applications are structured and how JSX is transformed into executable JavaScript.

*   **ReactDOM & The Root:** Students learned how `createRoot` in `main.jsx` targets the `div#root` in the `index.html` entry point to render the component tree.
*   **Component Fundamentals:** 
    *   Components as pure functions that must return a single React element.
    *   Strict naming conventions: **PascalCase** for components and matching filenames (e.g., `TodoList.jsx`).
    *   **The Component Tree:** Visualizing parent-child relationships and how they dictate data flow.
*   **JSX Rules:**
    *   Differences from HTML: `className` instead of `class`, `htmlFor` instead of `for`, and the requirement for all tags to be closed or self-closing.
    *   Embedding expressions using curly braces `{}` and the utility of **Fragments** (`<></>`) for grouping siblings.
*   **Built-in Components:** Introduction to **StrictMode** (to catch impure renders and side effects) and **Fragments**.
*   **Troubleshooting Tools:** Using the Vite error overlay, terminal output, and **React Developer Tools** (Components and Profiler tabs).

## Assignment Content Summary
Students refactored their Week 1 code to move from a single `App` component into a three-component architecture.

*   **Version Control:** Merged `lesson-01` into `main` and started a new `lesson-02-components` branch.
*   **Refactoring TodoList:**
    *   Created `TodoList.jsx`.
    *   Moved the `todoList` array and the mapping logic (`<ul>`) from `App.jsx` into this new component.
*   **Building TodoForm:**
    *   Created `TodoForm.jsx` containing a form with a labeled input and a submit button.
    *   Implemented proper accessibility using `htmlFor` on labels.
    *   Note: The submit button is currently `disabled` as functionality is added in later weeks.
*   **Composition:** Imported and integrated both `TodoForm` and `TodoList` back into `App.jsx`.

## Potential Student Trouble Spots
*   **The "Refactor Crash":** When students move the `todoList` array to the new component before moving the logic that maps over it, the app will temporarily crash with a "todoList is not defined" error. This is an intentional teaching moment for reading stack traces.
*   **Reserved Words:** Forgetting to use `htmlFor` for labels or `className` for CSS classes, which are common errors when copying raw HTML into JSX.
*   **Component Purity & StrictMode:** Students may be confused why their components are mounting/rendering twice. Mentors can explain how `StrictMode` intentionally triggers this in development to find side effects.
*   **Export/Import Errors:** Confusion between default and named exports, or forgetting to include the `.jsx` extension in Vite imports.
*   **Fragment Requirements:** Attempting to return multiple sibling elements from a component without wrapping them in a Fragment or a container `div`.
