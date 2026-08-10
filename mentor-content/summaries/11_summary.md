# Mentor Summary: React Week 11: Portfolio Polish, Security, and Deployment

## Key Concepts from Discussion
The final week transitions from core React mechanics to professional development standards and web security.

*   **Professional Styling Approaches:** The sources introduce various methods to move beyond basic CSS, including **CSS Modules** (scoped styles), **CSS-in-JS** (styled-components), and utility-first frameworks like **Tailwind CSS**.
*   **Security Fundamentals:** 
    *   **XSS Prevention:** Understanding React’s built-in escaping and the dangers of bypassing it with `dangerouslySetInnerHTML`.
    *   **Environment Variable Safety:** The critical distinction between public variables (prefixed with `VITE_` for client exposure) and private backend secrets that must never reach the client bundle.
    *   **Client-Side Storage:** Awareness that `localStorage` and `sessionStorage` are inherently insecure and should only store non-sensitive data.
*   **Production Architecture:** 
    *   **Authentication vs. Authorization:** Reinforcing that while the UI can be customized based on roles, true authorization **must** be enforced on the backend.
    *   **CORS:** How the browser handles cross-origin requests via simple/complex request patterns and preflight checks.
    *   **HTTPS and CSP:** The necessity of secure connections and how Content Security Policies (CSP) act as a secondary defense against malicious resource loading.

## Assignment Content Summary
The final assignment is a multi-part effort to finalize the "Todo List" project for a public portfolio.

*   **Part 1: Visual Polish:** Students apply a consistent design system, including primary/accent colors, typography hierarchy, and responsive layouts. They must also implement UI states for **loading**, **errors**, and **empty lists**.
*   **Part 2: Security Implementation:** Installing and configuring **DOMPurify** to sanitize user inputs and implementing client-side validation (e.g., maximum length limits).
*   **Part 3: Code & Documentation:** Cleaning up the codebase (removing `console.log` and unused imports) and writing a comprehensive README with a live demo link, technology list, and design decisions.
*   **Part 4: Deployment:** Hosting the app live on **Vercel**. A key task is configuring a `vercel.json` file with **rewrites** to ensure production API calls to the CTD backend bypass the need for a local development proxy.
*   **Part 5: Video Demonstration:** A required 3–5 minute presentation where the student demonstrates the app's CRUD functionality, navigation, and shares their learning reflections.

## Potential Student Trouble Spots
*   **Production Proxy Configuration:** If the `vercel.json` file is missing or contains incorrect rewrite rules, the app will fail to communicate with the API in the production environment, often resulting in 404 errors.
*   **Environment Variable Leakage:** Students might accidentally prefix sensitive secrets with `VITE_`, unintentionally bundling them into the public client-side code.
*   **Case-Sensitive Path Errors:** Imports that work on local Windows machines might fail during the Vercel build process because the production Linux environment is case-sensitive.
*   **Deployment-Specific CORS Issues:** Even if the frontend is correct, the backend may need to be updated to allow the new production domain (e.g., `*.vercel.app`) to make requests.
*   **Sanitization Placement:** Ensuring that input validation runs **before** sanitization via DOMPurify to provide accurate user feedback.
