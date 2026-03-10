<!-- h1, h2 already used by CTD Learns -->
### Expected App Capabilities

After completing this week's assignment, your app should:

- Require user authentication before accessing todos
- Employ Vite's Server Proxy to relay API requests
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

> [!REMINDER]
> **Why We Use Vite Server Proxy in This Assignment**
> Some browsers block third-party cookies by default. If your frontend sends requests directly to another domain, authentication cookies may be rejected and logon can fail. In this assignment, we use a Vite Server Proxy so your browser talks to `localhost:3001/api/*` (same-origin), while Vite forwards those requests to the CTD backend.

#### Update Vite Configuration

The app needs to run on port 3001 and proxy all `/api/*` requests through Vite.

Copy and paste the `vite.config.js` configuration from the reference file:

- [Reference `vite.config.js` (react-todo-list-v4)](https://github.com/Code-the-Dream-School/react-todo-list-v4/blob/main/vite.config.js)

Your file should look similar to this:

```js
import { defineConfig, loadEnv } from 'vite';
import react from '@vitejs/plugin-react';

// https://vite.dev/config/ for more info about configuration options
export default ({ mode }) => {
  const env = loadEnv(mode, '.', '');

  return defineConfig({
    plugins: [react()],
    server: {
      port: 3001,
      proxy: {
        '/api': {
          target: env.VITE_TARGET,
          secure: false,
          changeOrigin: true,
          configure: (proxy) => {
            proxy.on('proxyRes', (proxyRes) => {
              const cookies = proxyRes.headers['set-cookie'];

              if (!cookies) {
                return;
              }

              const cookieArray = Array.isArray(cookies) ? cookies : [cookies];
              proxyRes.headers['set-cookie'] = cookieArray.map((cookie) =>
                cookie
                  .replace(/; *Secure/gi, '')
                  .replace(/; *SameSite=None/gi, '')
                  .replace(/; *Domain=[^;]+/gi, '')
              );
            });
          },
        },
      },
    },
  });
};
```

> [!NOTE]
>
> - `defineConfig.server.proxy['/api']` - '/api' is the path prefix Vite watches for proxying. Requests that start with `/api` are forwarded to `VITE_TARGET`, while non-API paths continue to be handled by the Vite dev server.
> - `defineConfig.server.proxy['/api'].changeOrigin: true` updates outgoing `Host`/origin information so proxied requests look like they are going directly to the target server. This helps backends that validate host or origin headers.
> - `defineConfig.server.proxy['/api'].configure` lets you hook into proxy events. In this config, it listens to proxy responses (`proxyRes`) and rewrites `Set-Cookie` headers for local development compatibility by removing cookie attributes that break local cross-domain testing.

#### Configure Environment Variables

- Create a copy of `.env.example` and name it `.env`.
- Make sure that it is listed in your `.gitignore`
- Add this variable to `.env`:

```text
VITE_TARGET=https://ctd-learns-node-l42tx.ondigitalocean.app
```

#### Verify Proxy Setup

1. Restart your dev server: `npm run dev`
2. Open browser DevTools and go to the Network tab
3. Confirm requests show `localhost:3001/api/*` URLs
4. Test logon and task operations before moving to Part 2

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

- Remove all todo-related code from `App.jsx` (anything that was moved over to TodosPage)
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
- Create an async `handleSubmit` function that:
  - uses `try/catch/finally` blocks
  - Prevents default form submission
  - Sets loading state to true
  - Makes a POST request to `/api/users/logon` with email and password in the request body
  - Includes headers for `Content-Type: application/json` and `credentials: 'include'`
  - On successful response (status 200 with name and csrfToken), calls `onSetEmail` and `onSetToken` props: This will be made when we update App.jsx.
  - On failure, sets appropriate error message
  - Finally sets loading state back to false

```jsx
// example fetch request structure
try{ 
  const response = await fetch('/api/users/logon', {
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
- Update the component to accept and destructure the `token` prop: `function TodosPage({ token })`
- Add new state variables for:
  - `error` (for displaying API errors, default empty string)
  - `isTodoListLoading` (for showing loading state, default false)
- Create an async function `fetchTodos` inside a `useEffect` hook that:
  - uses `try/catch/finally` blocks
  - Sets `isTodoListLoading` to true
  - Makes a GET request to `/api/tasks` with:
    - `X-CSRF-TOKEN` header set to the token prop
    - `credentials: 'include'`
  - Handles different response scenarios:
    - Success: parse response and update todoList state
      - *note 1: the response will be a JSON object that contains an array named `tasks` and another object, `pagination` that will be used in future lessons*.
      - *note 2: see the bottom of the assignment for an example fetch response.*
    - Status 401: throw 'unauthorized' error
    - Other non-ok responses: throw generic error
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
- Make a POST request to `/api/tasks` with:
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
- Make a PATCH request to `/api/tasks/${id}` with:
  - JSON body containing `isCompleted: true` and `createdAt: originalTodo.createdAt`
  - Same headers as above
- On failure: rollback to the original todo and set error message

#### Add Update Todo Functionality

Update the existing `updateTodo` function:

- Make the function async
- Store the original todo for rollback
- **Optimistically apply** the edited todo to state
- Make a PATCH request to `/api/tasks/${editedTodo.id}` with:
  - JSON body containing title, isCompleted, and createdAt
  - Same headers pattern
- On failure: rollback to original todo and show error message

#### Complete the TodosPage Return Statement

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

### Checkpoint: Check Your Understanding with AI

Choose 1–2 prompts below. Explain in your own words first, then ask AI for feedback.

> [!NOTE]
> Do not ask AI to complete the assignment code for you.

> - "I added a useEffect that calls fetchTodos when the token changes. Here's my explanation of why the token is in the dependency array and what would happen if I passed an empty array instead: [my explanation]. Is my reasoning correct?"
> - "I implemented an optimistic update for addTodo: I add the todo to state immediately, then make the API call. Here's why I think this improves user experience and what risks it introduces if the API call fails: [my explanation]. What did I miss?"
> - "My completeTodo function stores the original todo before updating state, then uses that stored value to roll back on failure. Here's my explanation of why the rollback is necessary and how it works: [my explanation]. Can you check my understanding and ask me one follow-up question?"
> - "I used a pessimistic update for the Logon component: the token is only set after a successful API response. Here's my explanation of why pessimistic updates make sense for authentication but optimistic updates make sense for adding todos: [my explanation]. What should I refine?"

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

### Example Fetch Response for `api/tasks`

```js
{
    "tasks": [
        {
            "id": 61,
            "title": "meal prep",
            "isCompleted": false,
            "priority": "medium",
            "createdAt": "2026-01-05T19:28:23.583Z",
            "User": {
                "name": "Cindy Wilmington",
                "email": "cindy.wilmingon@some.email"
            }
        },
        {
            "id": 60,
            "title": "write term paper",
            "isCompleted": false,
            "priority": "medium",
            "createdAt": "2026-01-05T19:28:21.541Z",
            "User": {
                "name": "Cindy Wilmington",
                "email": "cindy.wilmingon@some.email"
            }
        },
        {
            "id": 59,
            "title": "return library books",
            "isCompleted": false,
            "priority": "medium",
            "createdAt": "2026-01-05T19:28:19.750Z",
            "User": {
                "name": "Cindy Wilmington",
                "email": "cindy.wilmingon@some.email"
            }
        },
        {
            "id": 28,
            "title": "feed cats",
            "isCompleted": false,
            "priority": "medium",
            "createdAt": "2025-12-17T14:40:54.329Z",
            "User": {
                "name": "Cindy Wilmington",
                "email": "cindy.wilmingon@some.email"
            }
        },
        {
            "id": 27,
            "title": "approve PR",
            "isCompleted": true,
            "priority": "medium",
            "createdAt": "2025-12-17T14:40:54.329Z",
            "User": {
                "name": "Cindy Wilmington",
                "email": "cindy.wilmingon@some.email"
            }
        },
        {
            "id": 26,
            "title": "write documentation",
            "isCompleted": true,
            "priority": "medium",
            "createdAt": "2025-12-17T14:40:54.329Z",
            "User": {
                "name": "Cindy Wilmington",
                "email": "cindy.wilmingon@some.email"
            }
        },
        {
            "id": 25,
            "title": "deploy app",
            "isCompleted": true,
            "priority": "medium",
            "createdAt": "2025-12-17T14:40:54.329Z",
            "User": {
                "name": "Cindy Wilmington",
                "email": "cindy.wilmingon@some.email"
            }
        }
    ],
    "pagination": {
        "page": 1,
        "limit": 10,
        "total": 7,
        "pages": 1,
        "hasNext": false,
        "hasPrev": false
    }
}
```

> [!NOTE]
> The AI review tool (known as AirHub) can check code and structure, but it does not run your code in a server environment to verify that aspect runs properly. We will have human reviewers checking this aspect, so you may receive a passing assignment from AirHub that could still need revisions after a human has checked that your work runs properly in the correct environment. If your AI and human reviewer feedbacks don't match, trust the human review.