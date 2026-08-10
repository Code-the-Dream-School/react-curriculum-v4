# Mentor Summary: React Week 1: Introduction & Installation

## Key Concepts from Discussion
The focus this week is transitioning from imperative DOM manipulation to React’s **declarative approach**.

*   **React & SPAs:** Introduction to React as a library for building **Single-Page Applications (SPAs)** using a declarative model for UI updates.
*   **Component-Based Architecture:** Emphasis on "separation of concerns" by dividing the UI into independent, reusable components.
*   **The Virtual DOM:** Students learned how React uses a lightweight copy of the DOM and a **diffing algorithm** to optimize performance.
*   **React 18 Features:** Brief mention of **concurrent rendering** for prioritizing urgent updates like user input.
*   **Vite Ecosystem:** Vite is used as the build tool. Key features discussed include:
    *   **esbuild** for fast pre-bundling during development.
    *   **Hot Module Replacement (HMR)** for maintaining state during code changes.
    *   **JSX Transformation:** Automatic conversion of JSX to plain JavaScript.

## Assignment Content Summary
Students initialized their long-term "Todo List" project using **Vite**.

*   **Project Scaffolding:** Initialized using `npx create-vite@latest --template react .` (specifically using the standard JavaScript template, not TS or SWC).
*   **Template Cleanup:** Students were tasked with stripping the default Vite boilerplate from `App.jsx`, `App.css`, and `index.css` to start with a clean slate.
*   **Basic Implementation:**
    *   Created a hardcoded `todoList` array of objects (containing `id` and `title`).
    *   Rendered the list inside a `<ul>` using the `.map()` function.
    *   Applied the `key` prop to list items.
*   **Workflow:** Practice with Git branching (`lesson-01-setup`), pushing to GitHub, and opening a Pull Request (PR) for submission.

## Potential Student Trouble Spots
While mentors should find these straightforward, students may encounter friction in these areas:

*   **Vite Configuration Prompts:** During setup, students might accidentally select **TypeScript** or **JavaScript SWC** instead of the required standard **JavaScript** template.
*   **JSX Syntax:** Understanding the role of curly braces `{}` to embed JavaScript expressions within HTML-like syntax, specifically when using `.map()`.
*   **Transient Errors:** The instructions warn that "cleanup steps" (deleting template code) may cause temporary browser errors until the replacement code is fully implemented.
*   **Branching Confusion:** Ensuring they are working on the `lesson-01-setup` branch rather than `main` for their initial commits.
*   **Mapping logic:** Properly returning the `<li>` element within the `.map()` function and correctly assigning the `key` prop to the outermost element of the returned fragment.
