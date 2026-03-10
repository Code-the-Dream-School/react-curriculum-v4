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

Now that we understand both hooks individually, let's explore how they relate to each other.

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

At first glance, these might seem interchangeable — and in some cases, they can appear to behave similarly. However, this similarity is deceptive.

Let's look at how that plays out in real-world examples.

**Example 1: CPU-intensive work → `useMemo` is the right choice**

```jsx
// ❌ Without useMemo: this runs on every render
const processedData = doSomethingCPUIntensive(data);

// ✅ With useMemo: only re-runs when `data` changes
const processedData = useMemo(() => doSomethingCPUIntensive(data), [data]);
```

In this case, the performance issue comes from repeatedly running a heavy computation. We only need to recompute when `data` changes. Here, `useMemo` is perfect because it caches the result of the computation, saving time and CPU cycles.

**Example 2: Passing callbacks to children → `useCallback` is the right choice**

```jsx
function Parent({ data }) {
  // ❌ Without useCallback: this function is recreated on every render
  const handleClick = () => doSomethingInChildComponent(data);

  // ✅ With useCallback: stable function reference across renders
  const handleClick = useCallback(
    () => doSomethingInChildComponent(data),
    [data],
  );

  return <Child onClick={handleClick} />;
}
```

Here, the goal isn't to avoid heavy computation — it's to prevent unnecessary re-renders. If `handleClick` is recreated every time `Parent` renders, the `Child` component will think its props changed, causing a wasted render. `useCallback` fixes that by memoizing the function reference, keeping it stable between renders as long as its dependencies don't change.

#### Comparison Table

| Feature            | `useMemo`                                                    | `useCallback`                                                            |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------------------ |
| **Purpose**        | Memoize a **computed value**                                 | Memoize a **function definition**                                        |
| **Returns**        | The **result** of a computation                              | The **function itself**                                                  |
| **Common Use**     | Caching derived data (e.g., filtered lists, computed totals) | Preventing unnecessary re-renders caused by changing function references |
| **Example Return** | Any value (number, string, array, object)                    | Function                                                                 |
| **Syntax**         | `useMemo(() => expensive(), [deps])`                         | `useCallback(() => {}, [deps])`                                          |

#### When to Actually Use Them

React is already fast — only optimize when you have a real performance issue.

- Use `useMemo` for expensive computations you don't want recalculated every render.
- Use `useCallback` for functions passed to children to prevent unnecessary re-renders.
- Use `React.memo` to wrap child components that receive stable props and re-render too often.
- 🚫 Avoid all three when your components are simple, render quickly, or when memoization adds unnecessary complexity.

#### React.memo: Preventing Unnecessary Component Re-renders

`useMemo` and `useCallback` optimize work done _inside_ a component. But there's another layer of optimization: preventing a child component from re-rendering at all when its props haven't changed.

By default, when a parent component re-renders, all of its child components re-render too — even when their props are identical. `React.memo` wraps a component and tells React to skip re-rendering it unless its props have actually changed (using shallow comparison).

```jsx
// Before: re-renders every time the parent renders
function ProductCard({ name, price }) {
  console.log(`Rendering: ${name}`);
  return <div>{name} - ${price}</div>;
}
```

```jsx
import { memo } from 'react';

// After: skips re-render if name and price haven't changed
const ProductCard = memo(function ProductCard({ name, price }) {
  console.log(`Rendering: ${name}`);
  return <div>{name} - ${price}</div>;
});

export default ProductCard;
```

Since `React.memo` uses shallow comparison, it works well for primitive props like strings and numbers. But if a prop is an object, array, or — importantly — a function, React treats it as a "new" value on every render even when the contents are unchanged. In those cases, `memo` won't prevent re-renders on its own.

This is why `React.memo` pairs naturally with `useCallback`. Function props must be stable across renders for memoization to work:

```jsx
import { memo, useCallback, useState } from 'react';

// Memoized component - skips re-render if props haven't changed
const ProductCard = memo(function ProductCard({
  name,
  price,
  productId,
  onAddToCart,
}) {
  console.log(`Rendering: ${name}`);
  return (
    <div>
      <span>
        {name} - ${price}
      </span>
      <button onClick={() => onAddToCart(productId)}>Add to Cart</button>
    </div>
  );
});

function ProductList({ products }) {
  const [cart, setCart] = useState([]);

  // ✅ useCallback ensures onAddToCart is the same reference across renders
  const handleAddToCart = useCallback((id) => {
    setCart((prev) => [...prev, id]);
  }, []);

  return (
    <ul>
      {products.map((product) => (
        <ProductCard
          key={product.id}
          name={product.name}
          price={product.price}
          productId={product.id}
          onAddToCart={handleAddToCart}
        />
      ))}
    </ul>
  );
}
```

Without `useCallback`, `handleAddToCart` would be a new function reference on every render of `ProductList`. Even though `ProductCard` is wrapped in `memo`, the shallow comparison would detect a changed `onAddToCart` prop and trigger a re-render anyway.

**Key Points about React.memo:**

- Wraps a **component** to skip re-renders when props haven't changed
- Uses **shallow comparison** — primitives work well; objects, arrays, and functions need stable references
- Pairs with **`useCallback`** when passing function props to memoized components
- Like `useMemo` and `useCallback`, don't add it everywhere — only when you have a confirmed performance issue

### API-Based Sort and Search

As your app grows, you'll often need to fetch and manipulate data from APIs. At first, it might seem easy to just fetch all your data and handle sorting or filtering in JavaScript using .filter() or .sort(). That's perfectly fine when your dataset is small — but as your app scales, this approach quickly breaks down. Large datasets increase load times, consume more memory, and cause noticeable lag as the browser struggles to process everything locally.

When building applications that display data, you face a fundamental choice: where should filtering, sorting, and pagination happen — in the client or on the server? Let's explore both approaches.

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

#### Limiting Network Requests

While API-based operations solve our performance problems with large datasets, they introduce a new challenge: most APIs have usage restrictions to protect their infrastructure and manage costs. These limitations mean we can't simply make unlimited requests whenever users interact with our app.

For example, at the time of their writing, [Spoonacular's introductory tier for API access](https://spoonacular.com/food-api/pricing) includes a 150 requests per day restriction and a 1 request per second limitation. Without careful planning, a single enthusiastic user could exhaust your entire daily quota in minutes by repeatedly searching or paginating through results.

To work efficiently within these constraints, we'll implement two essential techniques: **caching** to reuse data we've already fetched, and **throttling** to prevent requests from happening too quickly.

##### Caching Search Results Using Memoization

Caching is a technique used to store data fetched from the server in a temporary storage location, such as the browser's memory or local storage. This stored data can be quickly accessed when needed without making repeated network requests. In our case, if a user searches for "chicken" several times, the app uses API quotas fetching data that we've already had access to. Even without the 150 request per day limitation, we can still employ caching to save the user's bandwidth and speed up repeated searches.

One approach to this is to store the query and its search results in a lookup object. A lookup object is just a normal JavaScript object that uses the search query as a key to hold the search results. The example object below contains 2 searches, one for chicken and one for spaghetti. If a user searches for "spaghetti, tomato" again, we can access the previous search's results in the object with `searchCache["spaghetti, tomato"]`.

```javascript
//example lookup object with cached search results

const searchCache = {
  chicken: [
    {
      id: 123,
      title: 'red lentil soup with chicken and turnips',
      sourceUrl: '... recipe URL',
    },
    {
      id: 456,
      title: 'chicken enchilada quinoa casserole',
      sourceUrl: '...recipe URL',
    },
  ],
  'spaghetti, tomato': [
    { id: 789, title: 'spaghetti pomodoro', sourceUrl: '...recipe URL' },
    { id: 234, title: 'spaghetti carbonara', sourceUrl: '...recipe URL' },
    { id: 567, title: 'baked spaghetti', sourceUrl: '...recipe URL' },
  ],
};
```

After visualizing the cache, we'll create an empty state object to store queries and their associated search results.

```jsx
const [searchCache, setSearchCache] = useState({});
```

We next update the useEffect containing our search logic that fires every time a search term is submitted:

```js
// extract from App.jsx
//...code
useEffect(() => {
  if (!term) {
    return;
  }
  if (searchCache[term]) {
    console.log(`term ${term} found, returning cache...`);
    setRecipes([...searchCache[term]]);
    setTerm('');
    return;
  }
  async function getRecipes() {
    console.log(`getRecipes()`);
    const options = {
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': `${KEY}`,
      },
    };
    try {
      const resp = await fetch(
        `${BASE_URL}/complexSearch?includeIngredients=${term}&addRecipeInformation=true`,
        options,
      );
      if (resp.ok) {
        console.log('response okay');
        // resp includes number, offset, totalResults
        const recipeList = await resp.json();
        setRecipes([...recipeList.results]);
        console.log(`caching search for "${term}"`);
        setSearchCache((prev) => ({
          ...prev,
          [term]: [...recipeList.results],
        }));
        setTerm('');
      }
    } catch (e) {
      console.log(e);
    }
  }
  getRecipes();
}, [term]);
//code continues...
```

##### Throttling Request Rates

Another type of common API requirement is that a set amount of time must elapse before it will accept another request for processing. This use of throttling, also known as rate limiting, is a protective measure to prevent over-taxing the API's infrastructure.

The easiest way to throttle the response is to limit the availability of the buttons. We can temporarily set state to disable the button and then use `setTimeout` to re-enable them after a certain time has elapsed. We'll look at the handler functions since they end up with the logic to perform the throttle:

```js
// extract from App.jsx
//...code
function pageForward() {
  setIsPaginationDisabled(true);
  const maxPages = Math.ceil(resultsCount / paginationSize);
  const currentPage = Math.ceil(currentOffset / paginationSize) + 1;
  if (currentPage >= maxPages) {
    return;
  }
  setNextOffset(currentOffset + paginationSize);
  setTimeout(() => setIsPaginationDisabled(false), 1000);
}

function pageBack() {
  setIsPaginationDisabled(true);
  if (currentOffset <= 0) {
    return;
  }
  setNextOffset(currentOffset - paginationSize);
  setTimeout(() => setIsPaginationDisabled(false), 1000);
}
//code continues...
```

We can look at the network requests in the network activity tab in our browser to figure out how long it's taking the request to process. For this API, the response time averages out to 350ms. We can then reduce the timeout delay by this amount, making the interface a little friendlier to use without running against the API request speed limitations.

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
- Uses `useEffect` to trigger the fetch whenever `page` changes. This approach makes `useEffect` depend on page indirectly through the memoized function, keeping the fetch logic separate and reusable.
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

### Check Your Understanding with AI

Explain in your own words first, then ask for feedback on what is accurate and what needs revision.

1. Open your preferred AI chatbot.
2. Choose one concept from this lesson and explain it from memory.
3. Ask the AI to critique your explanation and point out gaps.
4. Revise your explanation and compare it to this week's examples.

**Example prompts**:

> - "I learned that `useCallback` memoizes a function so it isn't re-created on every render. Here's my explanation of why passing a new function reference on every render can cause unnecessary re-renders in child components, and how `useCallback` solves that: [my explanation]. What did I get right, and what should I refine?"
> - "I learned about `React.memo` and how it prevents unnecessary re-renders in child components. Here's my explanation of how it works, why it uses shallow comparison, and why it needs to be paired with `useCallback` when function props are involved: [my explanation]. What did I get right, and what should I refine?"
> - "I learned about debouncing as a technique to delay function execution until a user stops a repeated action like typing. Here's my explanation of what debouncing is and why it matters for reducing unnecessary API calls: [my explanation]. What did I get right, and what should I refine?"
> - "I compared local data manipulation to API-based sort and search. Here's my explanation of when each approach is the better choice and what factors drive that decision: [my explanation]. Can you check my reasoning and ask me one follow-up question?"
