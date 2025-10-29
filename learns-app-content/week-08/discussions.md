## Discussion Topics

### Advanced Hooks

React's render cycle is fast, but as components grow more complex — involving expensive calculations, large lists, or repeated re-renders — optimization becomes essential. This week we'll introduce `useMemo` and `useCallback`, two hooks designed specifically for these performance cases.

Both hooks cache values between renders to prevent unnecessary recalculations or redefinitions, improving performance — but they do so in different ways.

#### useMemo: Caching the Result of a Function

`useMemo` is used when you want to memoize a computed value. It runs the function you pass to it only when its dependencies change, and otherwise reuses the last result.

For example, suppose we have a list of items and need to filter them based on a search term:

```jsx
import { useState, useMemo } from 'react';

function FilteredList({ items }) {
  const [query, setQuery] = useState('');

  const filtered = useMemo(() => {
    console.log('Filtering items...');
    return items.filter((item) =>
      item.toLowerCase().includes(query.toLowerCase()),
    );
  }, [items, query]);

  return (
    <div>
      <input
        placeholder="Search..."
        value={query}
        onChange={(e) => setQuery(e.target.value)}
      />
      <ul>
        {filtered.map((item) => (
          <li key={item}>{item}</li>
        ))}
      </ul>
    </div>
  );
}
```

Every time the user types, React re-renders the component — but `useMemo` ensures the filtering logic only runs when `items` or `query` actually change. Without it, even unrelated state updates would trigger a full re-filtering of the list.

**Key Points about useMemo:**

- Returns a **memoized value** (the result of the computation)
- Only recalculates when dependencies change
- Useful for expensive calculations that shouldn't run on every render
- The function inside cannot accept parameters — it uses values from the surrounding scope

#### useCallback: Caching the Function Definition Itself

While `useMemo` caches a result, `useCallback` caches a function.

Imagine you're passing a callback to a child component that re-renders whenever that callback changes. Since React redefines all functions inside a component body on every render, that child would re-render unnecessarily.

We can fix that using `useCallback`:

```jsx
import { useCallback, useState } from 'react';

function CounterButton({ onIncrement }) {
  console.log('Button rendered');
  return <button onClick={onIncrement}>Increment</button>;
}

export default function Counter() {
  const [count, setCount] = useState(0);

  const handleIncrement = useCallback(() => {
    setCount((c) => c + 1);
  }, []); // function only recreated if dependencies change

  return (
    <div>
      <p>Count: {count}</p>
      <CounterButton onIncrement={handleIncrement} />
    </div>
  );
}
```

Here, `useCallback` ensures `handleIncrement` maintains the same reference between renders, so the `CounterButton` component doesn't re-render unnecessarily.

**Key Points about useCallback:**

- Returns a **memoized function** (not the result of calling it)
- Prevents function recreation on every render
- Essential when passing callbacks to optimized child components
- Uses the setter function pattern to avoid stale closures

### Optimizing React

#### Compare and Contrast: useMemo vs useCallback

Now that we understand both hooks individually, let's explore how they relate to each other. Here's a crucial insight: `useCallback` is actually just a specialized version of `useMemo`!

```jsx
// These two are essentially equivalent:
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);

const memoizedCallback = useMemo(() => {
  return () => {
    doSomething(a, b);
  };
}, [a, b]);
```

The difference is that `useCallback` returns the function itself, while `useMemo` returns the result of calling a function.

#### Comparison Table

| Feature            | `useMemo`                                                    | `useCallback`                                                            |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------------------ |
| **Purpose**        | Memoize a **computed value**                                 | Memoize a **function definition**                                        |
| **Returns**        | The **result** of a computation                              | The **function itself**                                                  |
| **Common Use**     | Caching derived data (e.g., filtered lists, computed totals) | Preventing unnecessary re-renders caused by changing function references |
| **Example Return** | Any value (number, string, array, object)                    | Function                                                                 |
| **Syntax**         | `useMemo(() => expensive(), [deps])`                         | `useCallback(() => {}, [deps])`                                          |

#### When to Actually Use Them

**The Golden Rule: Don't optimize prematurely!**

React is already very fast. Most components don't need optimization. Both of these hooks are performance optimizations — not requirements. Overusing them can make your code harder to read and sometimes even slower due to the overhead of maintaining caches.

**✅ Use them when:**

- You have expensive computations running on every render (`useMemo`)
- You are passing callback props to deeply nested or memoized child components (`useCallback`)
- You notice unnecessary re-renders in React DevTools
- Processing large datasets (1000+ items)
- Complex calculations take more than 16ms (affects 60fps)

**❌ Avoid them when:**

- The component is small and renders quickly
- Computations are simple or infrequent
- Adding memoization makes the code less clear without real performance benefit
- Working with primitive values that don't need memoization

**A good rule of thumb:** Use `useMemo` for expensive calculations, and `useCallback` for event handlers or callbacks passed to children.

### API-Based Sort and Search

As your app grows, you'll often need to fetch and manipulate data from APIs. At first, it might seem easy to fetch everything and use JavaScript's `.filter()` or `.sort()` locally — but that doesn't scale well.

When building applications that display data, you face a fundamental choice: where should filtering, sorting, and pagination happen? Let's explore both approaches.

#### Local Data Manipulation

When you process data locally, you download ALL the data once, then manipulate it in the browser using JavaScript.

**Local sorting or filtering works fine when:**

- Your dataset is small (e.g., a few dozen to a few hundred items)
- The data doesn't change frequently
- You can afford to keep everything in memory
- Users need instant feedback without network latency

**Example:**

```jsx
const sortedUsers = useMemo(() => {
  return [...users].sort((a, b) => a.name.localeCompare(b.name));
}, [users]);
```

This is quick and simple — but if `users` is a list of 10,000 records, sorting it locally on every re-render will slow your app down.

**Advantages of Local Manipulation:**

- Instant feedback (no network delay)
- Works offline once data is loaded
- Reduces server load
- Better for real-time interactions

**Disadvantages:**

- Large initial download
- High memory consumption
- Data can become stale
- Limited by browser's processing power

#### API-Based Operations

Instead of fetching everything at once, a more scalable approach is to:

1. Let the server handle sorting and searching
2. Fetch only what you need

**Example with query parameters:**

```jsx
async function fetchUsers({ search, sortBy, page }) {
  const response = await fetch(
    `/api/users?search=${search}&sort=${sortBy}&page=${page}`,
  );
  return await response.json();
}
```

This approach has two key benefits:

- The backend handles heavy operations like sorting and filtering efficiently
- The frontend stays fast, only rendering a limited subset of data

**Advantages of API-Based Operations:**

- Smaller data payloads
- Always fresh data
- Can handle massive datasets
- Leverages database optimization

**Disadvantages:**

- Network latency on every operation
- Requires internet connection
- Increases server load
- May need rate limiting

#### Implementing Long List Pagination

Pagination is essential when handling large datasets. Instead of loading 1,000+ records at once, we fetch results page by page.

Here's a complete example with proper error handling and loading states:

```jsx
import { useEffect, useState, useCallback } from 'react';

function UserList() {
  const [page, setPage] = useState(1);
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(false);
  const [totalPages, setTotalPages] = useState(0);

  const fetchUsers = useCallback(async () => {
    setLoading(true);
    try {
      const res = await fetch(`/api/users?page=${page}&limit=20`);
      const data = await res.json();
      setUsers(data.results);
      setTotalPages(data.totalPages);
    } catch (error) {
      console.error('Failed to fetch users:', error);
    } finally {
      setLoading(false);
    }
  }, [page]);

  useEffect(() => {
    fetchUsers();
  }, [fetchUsers]);

  return (
    <div>
      <h2>
        User List (Page {page} of {totalPages})
      </h2>

      {loading ? (
        <p>Loading...</p>
      ) : (
        <ul>
          {users.map((user) => (
            <li key={user.id}>{user.name}</li>
          ))}
        </ul>
      )}

      <div className="flex gap-2 mt-2">
        <button
          disabled={page === 1 || loading}
          onClick={() => setPage((p) => p - 1)}>
          Previous
        </button>
        <span>
          Page {page} of {totalPages}
        </span>
        <button
          disabled={page === totalPages || loading}
          onClick={() => setPage((p) => p + 1)}>
          Next
        </button>
      </div>
    </div>
  );
}
```

**This setup:**

- Uses `useCallback` to memoize the `fetchUsers` function - without it there is an infinite loop created between `useEffect` and `fetchUsers`!
- Uses `useEffect` to trigger the fetch whenever `page` changes so it re-fetches only whenever the page changes. This approach makes `useEffect` depend on page indirectly through the memoized function, while keeping the fetch logic separate and reusable.
- The “Previous” and “Next” buttons update the page, triggering a new API request.
- Prevents unnecessary re-fetching when unrelated state updates occur
- Includes proper loading states and error handling
- Shows total pages for better user experience

#### Choosing the Right Approach

**Use Local Manipulation when:**

- Dataset < 1,000 items
- Data rarely changes
- Instant feedback is critical
- Offline support is needed

**Use API-Based when:**

- Dataset > 10,000 items
- Data changes frequently
- Complex queries are needed
- Multiple users need consistent views
