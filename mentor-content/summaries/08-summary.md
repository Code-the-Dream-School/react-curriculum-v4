# Mentor Summary: React Week 8: Performance Optimization & API Strategies

## Key Concepts from Discussion
The discussions this week center on optimizing the render cycle and scaling data operations for larger datasets.

*   **Performance Hooks (`useMemo` & `useCallback`):**
    *   **`useMemo`:** Used to cache the **result** of expensive computations, ensuring they only re-run when specific dependencies change.
    *   **`useCallback`:** Used to cache a **function definition** itself. This is critical for maintaining stable function references across renders, particularly when passing callbacks to optimized child components.
*   **`React.memo`:** A higher-order component that prevents a child from re-rendering unless its props have changed via **shallow comparison**. It is often paired with `useCallback` to ensure function props remain stable.
*   **Local vs. API-Based Operations:**
    *   **Local:** Best for small datasets (<1,000 items), providing instant feedback but consuming more browser memory.
    *   **API-Based:** Essential for massive datasets. The server handles heavy lifting (sorting/filtering), and the client fetches only what is needed via query parameters (e.g., `?sortBy=title&find=searchTerm`).
*   **Network Optimization:** Techniques like **caching** search results via memoization and **throttling/debouncing** to limit frequent API requests and preserve rate limits.
*   **Pagination:** Strategies for fetching data page-by-page to maintain performance and reduce initial load times.

## Assignment Content Summary
Students refactored their Todo application to incorporate memoization and server-side filtering/sorting.

*   **Server-Side Search & Filter:**
    *   Implemented a `FilterInput` component.
    *   Updated API calls to include dynamic parameters for `sortBy`, `sortDirection`, and a `debouncedFilterTerm`.
*   **Memoization with Cache Invalidation:**
    *   Students implemented a **"data version" pattern** to manage `useMemo`.
    *   A `dataVersion` state is incremented via an `invalidateCache` function after every successful CRUD operation (add, complete, or update).
    *   The `TodoList` uses `useMemo` to filter the list (removing completed items), depending on both the `todoList` and the `dataVersion` to ensure the cache stays fresh after mutations.
*   **Advanced Error Handling:** Added specialized state (`filterError`) to distinguish between general fetch failures and errors specific to sorting or filtering operations.
*   **Optimization Practice:** Used `useCallback` for memoizing fetch functions to prevent infinite render loops when they are included in `useEffect` dependency arrays.

## Potential Student Trouble Spots
*   **Infinite Loops:** Forgetting to wrap fetch functions in `useCallback` before adding them to a `useEffect` dependency array will cause the component to re-render and re-fetch indefinitely.
*   **Shallow Comparison Pitfalls:** Students may be confused when `React.memo` fails to prevent a re-render because they passed a new object or array literal as a prop, which fails the shallow equality check.
*   **Stale Cache:** If a student forgets to call `invalidateCache()` after a successful API mutation, the `useMemo` hook will continue returning the old "cached" version of the list, making it appear as if the mutation failed.
*   **Debounce Logic:** Implementing the 300ms delay for search terms can be tricky; students might struggle with ensuring the API call only fires after the user stops typing.
*   **Over-Optimization:** Mentors may see students attempting to wrap every function or value in `useCallback`/`useMemo`, adding unnecessary complexity to simple components.
