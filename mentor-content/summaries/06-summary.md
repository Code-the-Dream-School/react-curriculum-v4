# Mentor Summary: React Week 6: Reusable Components & Project Organization

## Key Concepts from Discussion
The discussion materials provide a framework for identifying and extracting reusable logic and components.

*   **Reusable Component Criteria:** Students learned that any UI element is a candidate for extraction if it shares the **same underlying structure** and **accepts consistent props**. 
*   **The `children` Prop:** This is introduced as a mechanism for creating flexible "wrapper" components (like a `Dialog` or `Card`) that act as a layout outlet for nested JSX.
*   **Helper Functions vs. Custom Hooks:**
    *   **Helper Functions:** Pure JavaScript functions for business logic or data transformations that do not rely on React state or lifecycles.
    *   **Custom Hooks:** Functions starting with `use` that encapsulate stateful logic and can access other React hooks.
*   **Project Architecture:** Transitioning from a flat directory to a **feature-based structure**. This includes folders for `features/` (logic grouped by functionality), `shared/` (UI used across features), and `layout/` (structural pieces like headers/footers).
*   **Extraction Logic:** Students are taught a "decision tree" approach to determine which variables, handlers, and imports must follow a piece of JSX when it is moved into its own component.

## Assignment Content Summary
The assignment tasks students with a major refactor of their existing "Todo List" application while adding an interactive editing feature.

*   **File Reorganization:** Moving components into a new directory structure (e.g., placing `TodoForm` in a `features/` folder and creating a `layout/` folder for UI wrappers).
*   **Modularization:** Extracting specific items (like `CartItem` or `TodoListItem`) from larger mapping functions to improve readability and isolation.
*   **Inline Editing Feature:** Implementing a togglable "isEditing" state in the list items. When the todo text is clicked, it replaces the static text with an input field for updates.
*   **Validation Extraction:** Moving form validation logic out of the JSX and into a **dedicated helper function** to prepare for reuse.
*   **Advanced Hook (Stretch Goal):** Optionally extracting the editing logic into a custom hook named `useEditableTitle`.

## Potential Student Trouble Spots
*   **Import Path Errors:** Moving files into subdirectories often breaks relative imports, especially for non-JavaScript assets like images which VS Code may not auto-fix correctly.
*   **The "Key" Prop Misplacement:** During the refactor of list items, students often mistakenly place the `key` prop inside the new component's return statement rather than on the component instance within the `.map()` function.
*   **Prop/Handler Drilling:** When extracting a child component (like a list item), students must correctly identify and pass all necessary callback handlers (e.g., `onCompleteTodo`) as props.
*   **Hook vs. Helper Confusion:** Students might struggle to decide whether a piece of logic belongs in a pure utility file or a custom hook, particularly if the logic involves side effects or state.
*   **Complex Conditional Rendering:** Managing the toggle between "view mode" and "edit mode" inside a list item can lead to syntax errors within the JSX ternary operators.
