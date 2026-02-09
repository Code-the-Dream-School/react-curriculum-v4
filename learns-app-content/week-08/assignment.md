<!-- h1, h2 already used by CTD Learns -->

### Expected App Capabilities

After completing this week's assignment, your todo app will have:

- Server-side sorting by creation date or title in ascending/descending order
- Debounced search functionality that filters todos without flooding the API
- Performance optimizations using `useCallback` and `useMemo` hooks
- Enhanced error handling with specific recovery options for different error types
- Cache invalidation patterns to ensure data consistency after mutations

---

### Instructions Part 1: Implement Server-Side Sorting

#### Add Sort State and API Integration

1. **Update TodosPage.jsx state**:
   - Add two new state variables after existing state: `sortBy` with initial value `'creationDate'` and `sortDirection` with initial value `'desc'`

2. **Modify the fetchTodos function** to include sort parameters:
   - Create a `URLSearchParams` object inside the function with `sortBy` and `sortDirection` properties
   - Update the fetch URL to append the params to the base `/tasks` endpoint using template literals

   ```jsx
   const params = new URLSearchParams({
     sortBy,
     sortDirection,
   });
   const resp = await fetch(`${baseUrl}/tasks?${params}`, options);
   ```

> [!NOTE]
> **Understanding URLSearchParams Conversion**
>
> The `URLSearchParams` object automatically converts to a query string when used in template literals. When you write:
>
> ```jsx
> const params = new URLSearchParams({
>   sortBy: 'creationDate',
>   sortDirection: 'desc',
> });
> fetch(`${baseUrl}/tasks?${params}`, options);
> ```
>
> JavaScript automatically calls `params.toString()`, which converts the object to: `"sortBy=creationDate&sortDirection=desc"`
>
> This creates the final URL: `http://localhost:3001/tasks?sortBy=creationDate&sortDirection=desc`

3. **Update useEffect dependencies** to re-fetch when sort options change:
   - Add `sortBy` and `sortDirection` to the dependency array alongside the existing `token`

#### Create SortBy Component

4. **Create `src/shared/SortBy.jsx`**:
   - Create a functional component that accepts props: `sortBy`, `sortDirection`, `onSortByChange`, `onSortDirectionChange`
   - Create two select dropdowns with proper labels and htmlFor attributes
     - First dropdown: "Sort by" with options for 'creationDate' ("Creation Date") and 'title' ("Title")
     - Second dropdown: "Order" with options for 'desc' ("Descending") and 'asc' ("Ascending")
   - Use controlled component pattern with value and onChange handlers that call the respective prop functions

5. **Integrate SortBy** in TodosPage.jsx:
   - Place the component above TodoForm in the JSX
   - Pass the current sort state values and setState functions as props (recall the name of the props we added in the component definition)

#### Test Your Sorting

- Try different sort combinations and verify the API requests include the correct parameters
- Check the Network tab in DevTools to see, for example: `GET /tasks?sortBy=title&sortDirection=asc`

### Instructions Part 2: Implement Debounced Search with Performance Optimization

#### Create useDebounce Hook

Create a new file `src/utils/useDebounce.js` that exports a custom hook:

6. **Function setup**: Create a function called `useDebounce` that accepts `value` and `delay` parameters
   - `value`: any type (string for filter terms, but can be any value you want to debounce)
   - `delay`: number (the milliseconds to wait before updating, typically 300-500ms for search)
7. **Internal state**: Use `useState` to track the debounced value, initializing it with the incoming `value`
8. **Effect hook**: Use `useEffect` to set up a timer that updates the debounced value after the delay period
9. **Cleanup function**: Return a cleanup function from useEffect that clears the timeout to prevent memory leaks
10. **Dependencies**: Include `value` and `delay` in the useEffect dependency array
11. **Return value**: Return the debounced value from the hook

For reference, the finished function should look something like:

   ```jsx
   import { useState, useEffect } from 'react';

   function useDebounce(value, delay) {
     const [debouncedValue, setDebouncedValue] = useState(value);

     useEffect(() => {
       // Set up a timeout to update the debounced value after the delay
       const timeoutId = setTimeout(() => {
         setDebouncedValue(value);
       }, delay);

       // Cleanup function to clear the timeout if value changes before delay
       return () => {
         clearTimeout(timeoutId);
       };
     }, [value, delay]);

     return debouncedValue;
   }

   export default useDebounce;
   ```

#### Create FilterInput Component

12. **Create `src/shared/FilterInput.jsx`** - a controlled input component:
    - Accept `filterTerm` and `onFilterChange` as props
    - Create a `div` container with a `label` and `input` element
    - Set the label text to "Search todos:" and use `htmlFor='filterInput'` for accessibility
    - Configure the input with:
      - `id='filterInput'` (matches the label's htmlFor)
      - `type='text'`
      - `value` prop connected to `filterTerm`
      - `onChange` handler that calls `onFilterChange(e.target.value)`
      - `placeholder='Search by title...'` for user guidance
    - Export the component as default

#### Integrate FilterInput in TodosPage.jsx

13. **Add filter state** after existing state in TodosPage.jsx:
    - Import `useDebounce` from your utils directory at the top of the file
    - Add the filter state variables:

   ```jsx
   const [filterTerm, setFilterTerm] = useState('');
   const debouncedFilterTerm = useDebounce(filterTerm, 300);
   ```

14. **Create filter handler function**:
    - Create a function that accepts the new filter term and calls setFilterTerm
    - Name the function something descriptive like `handleFilterChange`
    - No need for `useCallback` since this function is simple and only calls setState
    - Pattern: `const handleFilterChange = (newTerm) => { setFilterTerm(newTerm); };`

15. **Update fetchTodos function** to include filter when present:
    - Modify your existing URLSearchParams creation to conditionally include the `find` property
    - Start with an object containing your sort parameters: `{ sortBy, sortDirection }`
    - Use an if statement to check if `debouncedFilterTerm` has a value
    - If it does, add the find property to the object: `if (debouncedFilterTerm) { paramsObject.find = debouncedFilterTerm; }`
    - Then create the URLSearchParams with this object

    The updated code should resemble:

    ```jsx
    const paramsObject = {
      sortBy,
      sortDirection,
    };
    if (debouncedFilterTerm) {
      paramsObject.find = debouncedFilterTerm;
    }
    const params = new URLSearchParams(paramsObject);
    ```

16. **Update useEffect dependencies**:
    - Add `debouncedFilterTerm` to your existing dependency array

17. **Add FilterInput to the UI** between SortBy and TodoForm:
    - Place the FilterInput component with appropriate props for the current filter term and change handler

### Instructions Part 3: Implement useMemo with Cache Invalidation

The cache invalidation pattern we're implementing uses a "data version" approach to control when `useMemo` recalculates. Since `useMemo` depends on its dependencies array, we can force recalculation by incrementing a version number whenever data changes through CRUD operations (add, complete, update todos). This ensures our memoized values stay fresh after mutations while still providing performance benefits during normal rendering.

#### Add Data Version Pattern

18. **Add cache invalidation state** to TodosPage.jsx:
    - Add a new state variable called `dataVersion` with an initial value of `0`

19. **Create cache invalidation function**:
    - Create a function called `invalidateCache` using `useCallback`
    - The function should increment `dataVersion` by 1 using the functional update form: `setDataVersion(prev => prev + 1)`
    - Include a console.log message: "Invalidating memo cache after todo mutation"
    - Use an empty dependency array since it only uses the setState function and the functional update form

> [!NOTE]
> While `useCallback` isn't strictly necessary for this simple function, we're using it here to give you hands-on practice with the optimization hook. In real applications, you'd typically only use `useCallback` when the function is passed as a prop to child components or included in dependency arrays.

20. **Call invalidateCache after successful mutations**. Add this line after each successful API operation in `addTodo`, `completeTodo`, and `updateTodo`:

    ```jsx
    invalidateCache();
    ```

    For example, in `addTodo` after replacing the temp todo:

    ```jsx
    setTodoList((currentList) =>
      currentList.map((todo) => (todo.id === tempId ? savedTodo : todo))
    );
    invalidateCache(); // Add this line
    ```

#### Optimize TodoList with useMemo

21. **Update `src/features/Todos/TodoList/TodoList.jsx`**:

    - Add `dataVersion` as a prop to the component function parameters
    - Create a memoized `filteredTodoList` using `useMemo`
    - Inside the useMemo function:
      - Add a console.log message: `"Recalculating filtered todos (v${dataVersion})"`
      - Return an object with `version` and `todos` properties
      - Set `version` to `dataVersion` and `todos` to the filtered todoList that excludes completed todos
    - Include both `todoList` and `dataVersion` in the dependency array
    - Use `filteredTodoList.todos` instead of `todoList` in your JSX rendering

22. **Pass dataVersion to TodoList** in TodosPage.jsx:
    - Add `dataVersion` as a prop to your existing TodoList component

---

### Instructions Part 4: Enhanced Error Handling

#### Add Separate Error States

23. **Add filter error state** to TodosPage.jsx:
    - Add a new state variable called `filterError` with an initial value of an empty string
    - Use `useState` to create the state and setter function

24. **Update error handling in fetchTodos** to distinguish error types:

    ```jsx
    } catch (error) {
      if (debouncedFilterTerm || sortBy !== 'creationDate' || sortDirection !== 'desc') {
        setFilterError(`Error filtering/sorting todos: ${error.message}`);
      } else {
        setError(`Error fetching todos: ${error.message}`);
      }
    } finally {
    ```

25. **Clear filter errors on successful fetch**:
    - After the line `setTodoList(data);` in the try block of your `fetchTodos` function, add `setFilterError('');` to clear any previous filter errors when data loads successfully.

26. **Add filter error UI** after the elements for the existing error:
    - Create a conditional block that displays when `filterError` has a value
    - Inside the block, create a `div` containing:
      - A paragraph element displaying the filter error message
      - A "Clear Filter Error" button that calls `setFilterError('')` when clicked
      - A "Reset Filters" button that when clicked:
        - Clears the filter term: `setFilterTerm('')`
        - Resets sort by: `setSortBy('creationDate')`
        - Resets sort direction: `setSortDirection('desc')`
        - Clears the filter error: `setFilterError('')`

---

### Instructions Part 5: Final Testing & Submission

#### Test Your Application

Open your browser's Developer Tools and test the following:

1. **Console Logging**:
   - Watch for `"Recalculating filtered todos (v0)"` messages on initial load
   - Add/edit/complete todos and see version numbers increment
   - Verify `"Invalidating memo cache"` messages after mutations

2. **Network Tab**:
   - See debounced API calls when typing in search (300ms delay)
   - Verify URLs include correct parameters: `?sortBy=title&sortDirection=asc&find={{your search term}}`

3. **Performance Features**:
   - Sort todos by different criteria
   - Search with debounced input
   - Test error recovery with network disabled

4. **Error Handling**:
   - Disable network and try operations
   - Verify different error messages for CRUD vs filter operations
   - Test "Reset Filters" functionality

5. **Clean Up for Submission**:
   - Remove all console.log statements from your code before submitting
   - These were added for educational purposes to observe hook behavior during development

---

### What You Accomplished This Week

✅ **Server-side sorting** with creation date and title options  
✅ **Debounced search functionality** to prevent API flooding  
✅ **useCallback optimization** for event handlers to prevent unnecessary re-renders  
✅ **useMemo with cache invalidation** using data version pattern  
✅ **Enhanced error handling** with specific recovery options  
✅ **URLSearchParams integration** for clean API query construction  
✅ **Performance logging** to understand when optimizations occur

---

### Looking Ahead

Next lesson, we'll explore **Advanced State Management** with `useReducer` and `useContext` to handle more complex application state patterns, moving beyond simple `useState` for larger applications.

---

### Troubleshooting Tips

**Console Errors**: If you see dependency warnings, ensure all `useCallback` and `useMemo` hooks include the correct dependencies in their arrays.

**API Issues**: Make sure that you are logged in and your query parameters are correctly named

**Performance**: Use React DevTools Profiler to verify your optimizations are working - you should see fewer re-renders with proper `useCallback` usage.

**Debouncing**: If search seems instant, verify your `useDebounce` hook is properly delaying API calls by watching the Network tab.
