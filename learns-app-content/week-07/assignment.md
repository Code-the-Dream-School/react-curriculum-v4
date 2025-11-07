<!-- h1, h2 already used by CTD Learns -->
### Expected App Capabilities

After completing this week's assignment, your app should:

- Require user authentication before accessing todos
- Fetch todos from a backend API and display them
- Allow users to add new todos that persist to the database
- Allow users to mark todos as complete with database updates
- Allow users to edit existing todo titles with persistence
- Show loading states during API operations
- Display error messages when API operations fail
- Use optimistic updates for better user experience

### Instructions Part 1: Environment Setup

#### Register with CTD's Todo List API

- Create a new account with the [CTD's Todo List app](https://react-todo-list-v3.onrender.com) using the Register button. Don't log in with Google.
- Add several test todos so that you'll have data to work with during development.

#### Configure Environment Variables

- Create a copy of `.env.example` and name it `.env`.
- Make sure that it is listed in your `.gitignore`
- Open `.env` and update the `VITE_BASE_URL` with the full URL provided by your instructor.

> [!note]
> Environment variables in Vite must start with `VITE_` to be accessible in your React app. The `import.meta.env.VITE_BASE_URL` syntax accesses these variables.

### Instructions Part 2: Project Organization and Component Extraction

#### Create the TodosPage Component

- Create a new directory: `src/features/Todos/`
- Move `src/features/TodoForm.jsx` to `src/features/Todos/TodoForm.jsx`
- Update the TodoForm import path in any files that reference it
- Move `src/features/TodoList/` directory to `src/features/Todos/TodoList/`
- Create a new file `src/features/Todos/TodosPage.jsx` that will contain all todo-related functionality
- Extract all todo state and functions from `App.jsx` and move them to `TodosPage`:
  - `todoList` state and `setTodoList`
  - `addTodo`, `completeTodo`, and `updateTodo` functions
  - TodoForm and TodoList imports and JSX
- The TodosPage component should accept no props initially (we'll add the token prop later)
- Export the TodosPage component as the default export

#### Create the Header Component

- Create a new directory: `src/shared/`
- Create `src/shared/Header.jsx` with a simple header that displays:
  - An `<h1>` with "Todo List" text
- Export Header as the default export

#### Update App Component

- Remove all todo-related code from `App.jsx` (anything that was moved over to TodoPage and Header)
- Import the new TodosPage and Header components
- Update App to render:
  - The Header component
  - The TodosPage component below it
- Your App.jsx should now be much simpler and focused only on top-level layout

> [!note]
> This organization separates concerns: App handles layout and will eventually handle authentication, while TodosPage handles all todo-specific logic. The shared/ directory contains components used across multiple features.

### Instructions Part 3: Create Authentication Component

#### Create the Logon Component with Pessimistic UI update

> [!note]
> **Pessimistic UI Updates**: In this authentication component, we wait for the server response before updating the UI (like setting the token). This is called a "pessimistic update" because we do not want the interface to update until after receiving a positive server response. This contrasts with the "optimistic updates" you'll implement for todo operations later.

- Create a new file `src/features/Logon.jsx`
- Destructure out the props, `onSetEmail`, and `onSetToken` in the component definition arguments.
  - Give each an empty placeholder function as a default value. eg: `function NewComponent({ newProp = () => {})`
  - We will make these handler functions when we update App.jsx then can remove these placeholder functions.
- Set up authentication state using `useState` for:
  - `email` and `password` (controlled form inputs, `initialValue` of empty strings)
  - `authError` (to display login failure, `initialValue` of an empty string)
  - `isLoggingOn` (to show loading state during login, `initialValue` of false)
- Access the base URL using `const baseUrl = import.meta.env.VITE_BASE_URL`
- Create an async `handleSubmit` function that:
  - uses `try/catch/finally` blocks
  - Prevents default form submission
  - Sets loading state to true
  - Makes a POST request to `${baseUrl}/user/logon` with email and password in the request body
  - Includes headers for `Content-Type: application/json` and `credentials: 'include'`
  - On successful response (status 200 with name and csrfToken), calls `onSetEmail` and `onSetToken` props: This will be made when we update App.jsx.
  - On failure, sets appropriate error message
  - Finally sets loading state back to false

```jsx
// example fetch request structure
try{ 
  const response = await fetch(`${baseUrl}/user/logon`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    credentials: 'include',
    body: JSON.stringify({ email, password })
  });
  const data = await response.json();
  if (response.status === 200 && data.name && data.csrfToken) {
    onSetEmail(data.name);
    onSetToken(data.csrfToken);
  } else {
    setAuthError(`Authentication failed: ${data?.message}`);
  }
} catch (error) {
  setAuthError(`Error: ${error.name} | ${error.message}`);
} finally {
  setIsLoggingOn(false);
}
  // code continues...
```

> [!note]
> Use controlled inputs with `value` and `onChange` props. The login button should be disabled during the loading state and show different text ("Logging in..." vs "Log On").

- Render a form with:
  - Email and password inputs (both required)
  - Labels properly associated with inputs using `htmlFor`
  - Submit button that shows loading state
  - An Error message at the top when `authError` exists.

### Instructions Part 4: Update App Component for Authentication

#### Add Authentication State to App

- Import the new `Logon` component and existing `Header` component in `App.jsx`
- Add two new state variables using `useState`:
  - `email` (initialized as empty string)
  - `token` (initialized as empty string)
- Update the component's return to conditionally render:
  - If `token` exists: render `<TodosPage token={token} />`
  - If no `token`: render `<Logon onSetEmail={setEmail} onSetToken={setToken} />`
- Include the `Header` component above the conditional rendering, passing `token`, `onSetToken`, and `onSetEmail` as props
- In Logon.jsx, remove the placeholder functions (`()=> {}`) defined as default values on the destructured props.

At this point, you should be able to log into your app. With conditional rendering it should now render the TodosPage with an empty todo list and your todo form.

### Instructions Part 5: Implement Data Fetching in TodosPage

#### Add API Integration with useEffect

- Update `src/features/Todos/TodosPage.jsx` to add API data fetching
- Add a constant for the base URL above the component definition: `const baseUrl = import.meta.env.VITE_BASE_URL`
- Add new state variables for:
  - `error` (for displaying API errors, default empty string)
  - `isTodoListLoading` (for showing loading state, default false)
- Create an async function `fetchTodos` inside a `useEffect` hook that:
  - uses `try/catch/finally` blocks
  - Sets `isTodoListLoading` to true
  - Makes a GET request to `${baseUrl}/tasks` with:
    - `X-CSRF-TOKEN` header set to the token prop
    - `credentials: 'include'`
  - Handles different response scenarios:
    - Status 401: throw 'unauthorized' error
    - Other non-ok responses: throw generic error
    - Success: parse JSON and update todoList state
  - Catches errors and sets error state
  - Always sets loading to false in finally block
- Make the useEffect depend on the `token` and only call `fetchTodos` when `token` exists

> [!note]
> The useEffect hook runs when the component mounts and whenever the token changes. This ensures we fetch fresh data when a user logs in.

#### Implement Optimistic Updates for Adding Todos

Transform the existing `addTodo` function to work with the API:

- Make the function async
- Keep the logic in `addTodo` that creates a new todo object before sending it to the API.
- **Optimistically update** the state immediately by adding the new todo to the list
- Make a POST request to `${baseUrl}/tasks` with:
  - JSON body containing `title` and `isCompleted`
  - `Content-Type: application/json` and `X-CSRF-TOKEN` headers
  - `credentials: 'include'`
- On success: replace the temporary todo with the real todo from the server response
- On failure: remove the failed todo from the list and set an `error` message

> [!note]
> This pattern makes apps feel instantly responsive while maintaining data integrity. When successful, the background API call replaces the temporary todo (with timestamp ID) with the server's response containing the actual database ID and server-generated timestamps. This happens without the user noticing!

#### Add Complete Todo Functionality

Update the existing `completeTodo` function:

- Make the function async
- Store the original todo before making changes (for potential rollback)
- **Optimistically update** the todo as completed in state
- Make a PATCH request to `${baseUrl}/tasks/${id}` with:
  - JSON body containing `isCompleted: true` and `createdTime: originalTodo.createdTime`
  - Same headers as above
- On failure: rollback to the original todo and set error message

#### Add Update Todo Functionality

Update the existing `updateTodo` function:

- Make the function async
- Store the original todo for rollback
- **Optimistically apply** the edited todo to state
- Make a PATCH request to `${baseUrl}/tasks/${editedTodo.id}` with:
  - JSON body containing title, isCompleted, and createdTime
  - Same headers pattern
- On failure: rollback to original todo and show error message

#### Complete the TodoPage Return Statement

Update the JSX return to include:

- Error display section above the form that shows when `error` state exists, with a "Clear Error" button
- Loading indicator above the form that shows when `isTodoListLoading` is true

### Instructions Part 6: Test and Explore API Integration

#### Explore Application Functionality

1. Open the browser console to observe network requests and state changes
2. Test the login flow with your provided credentials
3. Add several todos and observe the optimistic updates in action
4. Try completing and editing todos to see the API integration
5. Intentionally disconnect your network to test error handling and rollback behavior

#### Understanding Optimistic Updates

Observe how the app provides excellent user experience:

1. When you add a todo, it appears immediately (optimistic update)
2. The API call happens in the background
3. If successful, the temporary ID gets replaced with a real server ID
4. If the API call fails, the todo gets removed and an error message displays

This pattern makes apps feel instant while maintaining data consistency.

#### Clean Up Console Logs

Once you've observed how the API integration works, remove any `console.log` statements you added for learning purposes. Keep only the error handling that displays messages to users.

### Instructions Part 7: Final Testing & Submission

#### Verify Application Functionality

Before submitting, verify that your app:

- Shows the login form when not authenticated
- Displays loading states during operations
- Shows error messages when things go wrong
- Updates the UI optimistically for better user experience
- Handles network failures gracefully with rollback functionality
- Maintains todos in state between operations

### What You Accomplished This Week

Congratulations! You've successfully:

- ✅ Implemented user authentication with controlled forms and async API calls
- ✅ Learned to use useEffect for data fetching when components mount
- ✅ Built optimistic updates that improve user experience
- ✅ Created comprehensive error handling with user-friendly messages
- ✅ Implemented rollback functionality for failed API operations
- ✅ Used environment variables to configure API endpoints
- ✅ Managed loading states to provide visual feedback during operations
- ✅ Integrated CSRF token authentication for secure API communication
- ✅ Built a full-stack todo application with persistent data

### Looking Ahead

In upcoming weeks, you'll learn about:

- Advanced Hooks: useCallback and useMemo for performance optimization
- Optimizing React applications and implementing efficient API-based sorting and searching
- Managing complex application state with useReducer and useContext
- And much more...
