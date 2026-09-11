<!-- h1, h2 already used by CTD Learns -->

### Expected App Capabilities

After completing this week's assignment, your app should:

- support multi-page navigation with dedicated routes for todos, profile, about, and login
- protect routes that require authentication to access
- filter todos by URL search parameters (`/todos?status=completed`)
- use programmatic navigation after login/logout with proper redirects
- display a navigation component with active state indicators
- handle 404 errors for unknown routes
- preserve intended destinations after login

Note: The route paths specified in this assignment (/, /about, /login, /todos, /profile) and the status filter values (all, active, completed) should be used exactly as written — they are referenced across multiple components and must match for navigation and filtering to work correctly. Component names (HomePage, LoginPage, AboutPage, TodosPage, ProfilePage, NotFoundPage, RequireAuth, StatusFilter, Navigation) should also be exact since they are imported by other files.

Keep your existing code from previous lessons. This week's work builds on and modifies that code — it does not replace it.

---

### Instructions Part 1: Install React Router and Setup Basic Routing

#### Install React Router v7

1. **Install the React Router package**:
   - Open your terminal in the project root
   - Run: `npm install react-router`
   - This adds React Router v7 to your dependencies

> [!NOTE]
> **React Router v7 Import Change**
>
> React Router v7 uses a different import path than previous versions. Instead of importing from `react-router-dom`, you'll import directly from `react-router`:
>
> ```jsx
> // v7 (what we're using)
> import { BrowserRouter, Routes, Route } from 'react-router';
> 
> // v6 (older version)
> import { BrowserRouter, Routes, Route } from 'react-router-dom';
> ```

#### Setup Router Provider in main.jsx

2. **Add BrowserRouter to `src/main.jsx`**:
   - Import `BrowserRouter` from 'react-router' at the top of the file
   - Wrap the existing `<AuthProvider>` with `<BrowserRouter>`
   - The wrapper order should be: `StrictMode` → `BrowserRouter` → `AuthProvider` → `App`

   Use exactly as written — the wrapper nesting order (StrictMode → BrowserRouter → AuthProvider → App) is required for routing and context to work correctly. The import path for AuthProvider depends on your file structure:

   ```jsx
   import { StrictMode } from 'react';
   import { createRoot } from 'react-dom/client';
   import { BrowserRouter } from 'react-router';
   import './index.css';
   import App from './App.jsx';
   import { AuthProvider } from './contexts/AuthContext';

   createRoot(document.getElementById('root')).render(
     <StrictMode>
       <BrowserRouter>
         <AuthProvider>
           <App />
         </AuthProvider>
       </BrowserRouter>
     </StrictMode>
   );
   ```

#### Transform App.jsx from Conditional Rendering to Routing

3. **Replace conditional rendering with Routes** in `src/App.jsx`:
   - Remove the `useAuth` import and the `isAuthenticated` logic
   - Import `Routes` and `Route` from 'react-router'
   - Replace the conditional JSX (`{isAuthenticated ? <TodosPage /> : <Logon />}`) with a `<Routes>` structure

   Example — this is the starting structure before routes are added. The complete version with all routes is shown in step 11:

   ```jsx
   import './App.css';
   import { Routes, Route } from 'react-router';
   // Page imports will go here
   import Header from './shared/Header';

   function App() {
     return (
       <>
         <Header />
         <Routes>
           {/* Routes will go here */}
         </Routes>
       </>
     );
   }

   export default App;
   ```

#### Understanding the Route Structure

Your application will use this routing structure:

- **Public routes**: `/`, `/about`, `/login` - accessible without authentication
- **Protected routes**: `/todos`, `/profile` - require authentication  
- **Catch-all route**: `*` - handles 404 errors for unmatched URLs

### Instructions Part 2: Create Page Components

#### Create the HomePage Component

4. **Create `src/pages/HomePage.jsx`**:
   - This component handles the root route (`/`) and redirects users based on authentication status
   - Import `useEffect` from 'react', `useNavigate` from 'react-router', and `useAuth` from your contexts
   - Use `useEffect` to check authentication and navigate accordingly
   - Authenticated users go to `/todos`, unauthenticated users go to `/login`

   ```jsx
   import { useEffect } from 'react';
   import { useNavigate } from 'react-router';
   import { useAuth } from '../contexts/AuthContext';

   function HomePage() {
     const { isAuthenticated } = useAuth();
     const navigate = useNavigate();

     useEffect(() => {
       if (isAuthenticated) {
         navigate('/todos', { replace: true });
       } else {
         navigate('/login', { replace: true });
       }
     }, [isAuthenticated, navigate]);

     return (
       <div>
         <p>Redirecting...</p>
       </div>
     );
   }

   export default HomePage;
   ```

#### Create the LoginPage Component

5. **Create `src/pages/LoginPage.jsx`**:
   - Move the login functionality from `src/features/Logon.jsx` to this new page component
   - Add navigation logic to redirect after successful login using `useNavigate` and `useLocation`
   - Handle preserving the intended destination from protected route redirects

   Key imports and logic to include:

   ```jsx
   import { useState, useEffect } from 'react';
   import { useNavigate, useLocation } from 'react-router';
   import { useAuth } from '../contexts/AuthContext';

   function LoginPage() {
     const { login, isAuthenticated } = useAuth();
     const navigate = useNavigate();
     const location = useLocation();
     
     // Get intended destination from location state, default to /todos
     const from = location.state?.from?.pathname || '/todos';

     // Redirect if already authenticated
     useEffect(() => {
       if (isAuthenticated) {
         navigate(from, { replace: true });
       }
     }, [isAuthenticated, navigate, from]);

     // Handle login form submission
     async function handleSubmit(e) {
       e.preventDefault();
       // ... existing login logic
       const result = await login(email, password);
       if (result.success) {
         // useEffect will handle redirect
       }
     }
     
     // ... rest of component with form JSX
   }
   ```

#### Create the AboutPage Component

6. **Create `src/pages/AboutPage.jsx`**:
   - This is a simple informational component about your todo app
   - Include information about features, technologies used, etc.
   - Add sections for app features and technologies used (React, React Router, Vite)
   - Export the component as default

#### Move TodosPage to Pages Directory

7. **Move `src/features/Todos/TodosPage.jsx` to `src/pages/TodosPage.jsx`**:
   - Cut the file from the features directory and paste it in the pages directory
   - Update the import paths in the moved file to account for the new location

#### Create Additional Page Components

8. **Create `src/pages/ProfilePage.jsx`**:
   - Create a user profile page that displays user information and todo statistics
   - Use the `useAuth` hook to access user name and token
   - Fetch todo statistics from the API to display total/completed/active counts

9. **Create `src/pages/NotFoundPage.jsx`**:
   - Create and export a 404 error page for unmatched routes
   - Include helpful navigation links back to main parts of the app (remember to use React-Router's `Link` component)

### Instructions Part 3: Implement Route Protection and Configure Routes

#### Create the RequireAuth Component

10. **Create `src/components/RequireAuth.jsx`**:
    - This component is a wrapper component that protects routes that require authentication
    - Import `useLocation` and `useNavigate` from React Router to handle redirects
    - Import `useEffect` from React and `useAuth` from your AuthContext
    - Create a `RequireAuth` function component that accepts `children` as props
      - Use `useAuth()` to get the current authentication status
      - Use `useLocation()` to capture the current page location
      - Use `useNavigate()` to programmatically navigate to the login page
      - In a `useEffect` hook, check if the user is not authenticated:
        - If not authenticated, navigate to '/login' and pass the current location in state for preservation
        - This allows users to return to their intended destination after logging in
      - Return a loading message while redirecting if not authenticated
      - Return the `children` components if the user is authenticated

#### Configure All Routes in App.jsx

11. **Complete the routes configuration** in `src/App.jsx`:
    - Import all your page components at the top
    - Import the `RequireAuth` component
    - Add all route definitions inside the `<Routes>` component

    Use exactly as written — the route paths, RequireAuth wrapping, and component names must match. Import paths depend on your file structure:

    ```jsx
    import './App.css';
    import { Routes, Route } from 'react-router';
    import HomePage from './pages/HomePage';
    import AboutPage from './pages/AboutPage';
    import LoginPage from './pages/LoginPage';
    import TodosPage from './pages/TodosPage';
    import ProfilePage from './pages/ProfilePage';
    import NotFoundPage from './pages/NotFoundPage';
    import RequireAuth from './components/RequireAuth';
    import Header from './shared/Header';

    function App() {
      return (
        <>
          <Header />
          <Routes>
            <Route path='/' element={<HomePage />} />
            <Route path='/about' element={<AboutPage />} />
            <Route path='/login' element={<LoginPage />} />
            <Route
              path='/todos'
              element={
                <RequireAuth>
                  <TodosPage />
                </RequireAuth>
              }
            />
            <Route
              path='/profile'
              element={
                <RequireAuth>
                  <ProfilePage />
                </RequireAuth>
              }
            />
            <Route path='*' element={<NotFoundPage />} />
          </Routes>
        </>
      );
    }

    export default App;
    ```

#### Test Basic Routing

12. **Test the routing setup**:
    - Start your dev server with `npm run dev`
    - Try navigating to different URLs manually in the browser address bar
    - Test: `http://localhost:5173/about`, `http://localhost:5173/login`
    - Notice how the browser URL changes but the page doesn't reload
    - Now use your browser's back and forward buttons to show that your browser is preserving navigation history

### Instructions Part 4: Create Navigation Component and Add Authentication Navigation

#### Create the Navigation Component

13. **Create `src/shared/Navigation.jsx`**:
    - This component provides navigation links throughout the app
    - Import `NavLink` from React Router and `useAuth` from your AuthContext
    - Use `NavLink` instead of regular `Link` for automatic active state styling
    - Create a `navLinkStyle` function that accepts an object with `isActive` property
      - **Note:** `NavLink` automatically calls your style function and passes it an object containing `isActive: true/false` based on whether the current URL matches the link's destination
      - This allows you to conditionally style links based on the current route without manually tracking which page is active
    - Style active links with bold font weight and underline, inactive links with default styling
    - Return a `<nav>` element containing an unordered list
    - Style the list to remove default bullets and display items horizontally with gap spacing
      - hint: `listStyle: 'none', display: 'flex', gap: '1rem', padding: 0`
    - Create navigation items for different routes:
      - Always show an "About" link that navigates to '/about'
      - For authenticated users, show "Todos" link (to '/todos') and "Profile" link (to '/profile')
      - For unauthenticated users, show "Login" link (to '/login')
    - Use conditional rendering based on `isAuthenticated` from `useAuth()`

#### Update Header Component

14. **Update `src/shared/Header.jsx`** to include navigation:
    - Import the new `Navigation` component
    - Add it to the Header JSX between the title and the logout button

#### Add Navigation to Logout Flow

15. **Update logout navigation** in `src/features/Logoff.jsx`:
    - Import `useNavigate` from 'react-router'
    - Add navigation to login page after successful logout

    Add these changes to the Logoff component:

    ```jsx
    import { useState } from 'react';
    import { useNavigate } from 'react-router';  // Add this import
    import { useAuth } from '../contexts/AuthContext';

    function Logoff() {
      const { logout } = useAuth();
      const navigate = useNavigate();  // Add this hook
      
      async function handleLogoff() {
        setIsLoggingOff(true);
        setError('');

        const result = await logout();

        if (result.success) {
          navigate('/login');  // Add this navigation
        } else {
          setError(result.error);
          setIsLoggingOff(false);
        }
      }
      
      // ... rest of component
    }
    ```

#### Test Navigation Flow

16. **Test the complete authentication and navigation flow**:
    - Try accessing `/todos` when not logged in - should redirect to login
    - Log in and verify you're redirected back to todos
    - Use the navigation links to move between pages
    - Test logout from different pages - should always return to login
    - Notice how the active navigation link is highlighted

### Instructions Part 5: Implement URL-Based Status Filtering

#### Create the StatusFilter Component

17. **Create `src/shared/StatusFilter.jsx`**:
    - This component uses URL search parameters to manage todo status filters
    - Users can filter todos by **all**, **active**, or **completed** status
    - The filter state is stored in the URL, making it bookmarkable and shareable

    Use exactly as written — the status values ('all', 'active', 'completed') must match between this component and TodoList for filtering to work correctly:

    ```jsx
    import { useSearchParams } from 'react-router';

    function StatusFilter() {
      const [searchParams, setSearchParams] = useSearchParams();
      const currentStatus = searchParams.get('status') || 'all';

      const handleStatusChange = (status) => {
        if (status === 'all') {
          // Remove status param for 'all' to keep URL clean
          searchParams.delete('status');
        } else {
          searchParams.set('status', status);
        }
        setSearchParams(searchParams);
      };

      return (
        <div>
          <label htmlFor='statusFilter'>Show:</label>
          <select
            id='statusFilter'
            value={currentStatus}
            onChange={(e) => handleStatusChange(e.target.value)}
          >
            <option value='all'>All Todos</option>
            <option value='active'>Active Todos</option>
            <option value='completed'>Completed Todos</option>
          </select>
        </div>
      );
    }

    export default StatusFilter;
    ```

#### Integrate URL Parameters in TodosPage

18. **Update `src/pages/TodosPage.jsx`** to use URL-based filtering:
    - Import `useSearchParams` from 'react-router'
    - Import the new `StatusFilter` component  
    - Read the status filter from URL parameters
    - Pass the status filter to the TodoList component

    Add these changes to TodosPage.jsx:

    ```jsx
    // Add these imports at the top
    import { useSearchParams } from 'react-router';
    import StatusFilter from '../shared/StatusFilter';

    function TodosPage() {
      const { token } = useAuth();
      const [searchParams] = useSearchParams();  // Add this line
      const [state, dispatch] = useReducer(todoReducer, initialTodoState);
      
      // Get status filter from URL, default to 'all'
      const statusFilter = searchParams.get('status') || 'all';  // Add this line

      // ... existing useEffect and functions

      return (
        <div>
          {/* ... existing error handling JSX */}
          <SortBy /* ... existing props */ />
          <StatusFilter />  {/* Add this component */}
          <FilterInput /* ... existing props */ />
          <TodoForm onAddTodo={addTodo} />
          <TodoList
            todoList={todoList}
            onCompleteTodo={completeTodo}
            onUpdateTodo={updateTodo}
            dataVersion={dataVersion}
            statusFilter={statusFilter}  /* Add this prop */
          />
        </div>
      );
    }
    ```

#### Update TodoList to Handle Status Filtering

19. **Update `src/features/Todos/TodoList/TodoList.jsx`** to filter by status:
    - Add `statusFilter` as a prop parameter
    - Update the `useMemo` logic to filter todos based on the status parameter
    - Update the empty state message to be context-aware

    Use exactly as written — the `statusFilter` prop name and status values ('completed', 'active', 'all') must match the StatusFilter component. The empty state messages are examples; use your own wording if you prefer:

    ```jsx
    import { useMemo } from 'react';
    import TodoListItem from './TodoListItem.jsx';

    function TodoList({
      todoList,
      onCompleteTodo,
      onUpdateTodo,
      dataVersion,
      statusFilter = 'active',  // Add this prop with default
    }) {
      const filteredTodoList = useMemo(() => {
        console.log(`Recalculating filtered todos (v${dataVersion}) - Status: ${statusFilter}`);
        // Remember to remove this console.log before submission once you have completed this assignment.

        let filteredTodos;
        switch (statusFilter) {
          case 'completed':
            filteredTodos = todoList.filter((todo) => todo.isCompleted);
            break;
          case 'active':
            filteredTodos = todoList.filter((todo) => !todo.isCompleted);
            break;
          case 'all':
          default:
            filteredTodos = todoList;
            break;
        }

        return {
          version: dataVersion,
          todos: filteredTodos,
        };
      }, [todoList, dataVersion, statusFilter]);

      const getEmptyMessage = () => {
        switch (statusFilter) {
          case 'completed':
            return 'No completed todos yet. Complete some tasks to see them here.';
          case 'active':
            return 'No active todos. Add a todo above to get started.';
          case 'all':
          default:
            return 'Add todo above to get started.';
        }
      };

      return filteredTodoList.todos.length === 0 ? (
        <p>{getEmptyMessage()}</p>
      ) : (
        <ul>
          {filteredTodoList.todos.map((todo) => (
            <TodoListItem
              key={todo.id}
              todo={todo}
              onCompleteTodo={onCompleteTodo}
              onUpdateTodo={onUpdateTodo}
            />
          ))}
        </ul>
      );
    }

    export default TodoList;
    ```

#### Test URL-Based Filtering

20. **Test the URL parameter functionality**:
    - Navigate to `/todos` and change the status filter dropdown  
    - Watch how the browser URL updates to include `?status=completed` or `?status=active`
    - Use the browser's back/forward buttons to navigate through filter history
    - Copy a filtered URL (like `/todos?status=completed`) - note that accessing it later will require re-authentication
    - For testing URL persistence without auth, navigate to `/about` and bookmark it - this will work reliably

> [!NOTE]
> **Why URL-Based State Matters**
>
> Storing filter state in the URL provides several benefits:
>
> - **Browser-friendly**: Back/forward buttons work intuitively
> - **Persistent**: Filter state survives page refreshes (though protected routes require re-authentication)
> - **Bookmarkable**: Public pages like `/about` can be bookmarked and shared freely
> - **Shareable**: Filtered URLs preserve user intent, even if authentication is required to access them

### Instructions Part 6: Complete ProfilePage Implementation

#### Implement User Statistics in ProfilePage

21. **Complete the ProfilePage implementation**:
    - Import necessary hooks: `useState` and `useEffect` from React, and `useAuth` from your AuthContext
    - Use relative API paths (for example, `/api/tasks`) so requests go through the proxy configuration
    - Create state variables for todo statistics (total, completed, active counts), loading state, and error handling
    - Use `useAuth()` to access the user's name and authentication token
    - Create a ProfilePage component that displays user account information (name, status)
    - Add a todo statistics section that shows total, completed, and active todo counts
    - Calculate and display completion percentage when user has todos
    - Handle loading and error states with appropriate UI feedback
    - Use conditional rendering to show loading messages, error messages, or statistics

    The key data fetching logic should be implemented in a `useEffect`:

    ```jsx
    useEffect(() => {
      async function fetchTodoStats() {
        if (!token) return;

        try {
          setLoading(true);
          setError('');

          const options = {
            method: 'GET',
            headers: { 'X-CSRF-TOKEN': token },
            credentials: 'include',
          };

          const response = await fetch('/api/tasks', options);

          if (response.status === 401) {
            throw new Error('Unauthorized');
          }

          if (!response.ok) {
            throw new Error('Failed to fetch todos');
          }

          const todos = await response.json();

          // Calculate statistics
          const total = todos.length;
          const completed = todos.filter((todo) => todo.isCompleted).length;
          const active = total - completed;

          setTodoStats({ total, completed, active });
        } catch (err) {
          setError(`Error loading statistics: ${err.message}`);
        } finally {
          setLoading(false);
        }
      }

      fetchTodoStats();
    }, [token]);
    ```

### Instructions Part 7: Testing and Error Handling

#### Test Direct URL Navigation

22. **Test direct URL access scenarios**:
    - Type `/profile` directly in the address bar when authenticated - should show profile
    - Type `/todos` directly when not authenticated - should redirect to login
    - Try `/about` from both authenticated and unauthenticated states - should work in both cases
    - Test `/nonexistent` - should show 404 page
    - Verify that all direct navigation scenarios work correctly

#### Test Complete Authentication Flow

23. **Test the complete user journey**:
    - Start from `/` (root) and verify proper authentication-based redirects
    - Test logging in and being redirected to the intended destination
    - Navigate through all pages using the navigation menu
    - Test logout from different pages - should always redirect to login
    - Use browser back/forward buttons to ensure navigation history works properly

### Instructions Part 8: Final Testing & Integration

#### Comprehensive Application Testing

24. **Verify all React Router features are working**:
    - **Multi-page navigation**: Test navigation between all pages (Home, About, Login, Todos, Profile)
    - **Protected routes**: Verify `/todos` and `/profile` redirect to login when not authenticated
    - **Authentication flow**: Test login redirects you to intended destination
    - **URL filtering**: Test todo status filters update the URL (note: refreshing requires re-authentication)
    - **404 handling**: Test invalid URLs show the NotFound page
    - **Browser integration**: Test back/forward buttons and bookmark `/about` to verify URL persistence

#### Authentication and Navigation Integration

25. **Test authentication with navigation**:
    - Log out and try to access `/todos` - should redirect to login with destination preserved
    - Log in from the redirect and verify you're taken to `/todos`
    - Test logout from different pages - should always redirect to `/login`

#### Clean Up for Submission

26. **Remove development artifacts**:
    - Remove any `console.log` statements added during development
    - Ensure all components are properly formatted and commented
    - Test the application one final time in production mode: `npm run build && npm run preview`

---

### Checkpoint: Check Your Understanding with AI

Choose 1–2 prompts below. Explain in your own words first, then ask AI for feedback.

> [!NOTE]
> Do not ask AI to complete the assignment code for you.

> - "I wrapped protected routes in a RequireAuth component that checks for a token and redirects to login if it's missing. Here's my explanation of why this pattern is better than putting the auth check inside each page component: [my explanation]. Is my reasoning correct?"
> - "I used useSearchParams to store the todo status filter in the URL so the filter persists on page refresh. Here's how I understand the difference between managing filter state with useState versus useSearchParams, and why URL-based state matters for shareability: [my explanation]. What did I get right, and what should I refine?"
> - "I used NavLink instead of Link for my navigation menu because NavLink adds an 'active' class to the current route's link. Here's my explanation of when I'd choose NavLink over Link and how React Router determines which link is active: [my explanation]. Can you check my understanding and ask me one follow-up question?"
> - "I added a catchall Route with path='*' so that unmatched URLs show a 404 page instead of a blank screen. Here's my explanation of why the order of Route components matters and what would happen if the catchall route were listed first: [my explanation]. What did I miss?"

### What You Accomplished This Week

✅ **Multi-page application structure** with client-side routing  
✅ **Protected route implementation** with authentication guards  
✅ **URL-based state management** for todo filtering  
✅ **Navigation component** with active state styling  
✅ **Programmatic navigation** with proper redirect handling  
✅ **404 error handling** with user-friendly recovery options  
✅ **Browser integration** with back/forward button support  
✅ **Deep linking support** for bookmarkable filtered views

---

### Looking Ahead

Next week, we'll focus on **polishing your application for portfolio presentation** and **deployment preparation**, including security enhancements, performance optimization, and preparing your app for production deployment.

---

### Troubleshooting Tips

**Navigation Issues**: If navigation doesn't work, check that `BrowserRouter` is properly wrapping your app in `main.jsx` and that you're importing from 'react-router' (not 'react-router-dom') in React Router v7.

**Authentication Flow**: If auth redirects aren't working, verify that `RequireAuth` is properly wrapping protected routes and that the `useAuth` hook is accessible in all components.

**URL Parameters**: If filters don't persist in URLs, check that `useSearchParams` is being used correctly and that `setSearchParams` is called when filters change.

**404 Handling**: If the 404 page doesn't appear for invalid routes, ensure the catch-all route (`*`) is the last route in your `Routes` component.

> [!NOTE]
> The AI review tool (known as AirHub) can check code and structure, but it does not run your code in a server environment to verify that aspect runs properly. We will have human reviewers checking this aspect, so you may receive a passing assignment from AirHub that could still need revisions after a human has checked that your work runs properly in the correct environment. If your AI and human reviewer feedbacks don't match, trust the human review.

---

<details>
<summary>Rubric (for AirHub reviewer and mentors)</summary>

### Required Deliverables/Tasks

- **Install React Router (Part 1, Step 1)** — React Router v7 is installed as a dependency (`react-router` package). Imports throughout the app use `'react-router'` (not `'react-router-dom'`).
- **BrowserRouter Setup (Part 1, Step 2)** — In `main.jsx`, the app is wrapped with `<BrowserRouter>` from `'react-router'`. Use exactly as written: the wrapper nesting order must be StrictMode → BrowserRouter → AuthProvider → App. The import path for AuthProvider depends on the student's file structure; do not fail for a different path as long as the import resolves.
- **App.jsx Route Configuration (Parts 1 & 3, Steps 3 & 11)** — App.jsx imports `Routes` and `Route` from `'react-router'`, removes the previous conditional rendering (`isAuthenticated ? <TodosPage /> : <Logon />`), and defines routes for all pages. Use exactly as written: the route paths must be `'/'` (HomePage), `'/about'` (AboutPage), `'/login'` (LoginPage), `'/todos'` (TodosPage, wrapped in RequireAuth), `'/profile'` (ProfilePage, wrapped in RequireAuth), and `'*'` (NotFoundPage). Import paths depend on the student's file structure; do not fail for different directory layouts as long as imports resolve.
- **HomePage Component (Part 2, Step 4)** — A page component (suggested location: `src/pages/HomePage.jsx`) that handles the root route. Uses `useAuth` and `useNavigate` to redirect authenticated users to `'/todos'` and unauthenticated users to `'/login'`, with `{ replace: true }`. Use exactly as written: the redirect paths must match the route configuration. The file path is a suggested convention; do not fail for a different location.
- **LoginPage Component (Part 2, Step 5)** — A page component (suggested location: `src/pages/LoginPage.jsx`) that incorporates the login functionality previously in `Logon.jsx`. Uses `useNavigate` and `useLocation` from React Router. Preserves the intended destination from protected route redirects using `location.state?.from?.pathname` (defaulting to `'/todos'`). Redirects already-authenticated users via `useEffect`. Example — adapt to your own layout: the student's existing login form and error handling are incorporated into this component; the specific form layout may vary.
- **AboutPage Component (Part 2, Step 6)** — A page component (suggested location: `src/pages/AboutPage.jsx`) with informational content about the app. Example — adapt to your own layout: the specific content, sections, and styling are the student's choice.
- **Move TodosPage (Part 2, Step 7)** — TodosPage is relocated to the pages directory (suggested: `src/pages/TodosPage.jsx`) with import paths updated accordingly. The file path is a suggested convention; do not fail for a different location as long as imports resolve.
- **ProfilePage Component (Parts 2 & 6, Steps 8 & 21)** — A page component (suggested location: `src/pages/ProfilePage.jsx`) that displays user information and todo statistics. Uses `useAuth` for user data and token. Fetches todos from the API in a `useEffect` and calculates total, completed, and active counts with a completion percentage. Handles loading and error states. Example — adapt to your own layout: the specific layout and styling are the student's choice, but data fetching, statistics calculation, and loading/error states must be present.
- **NotFoundPage Component (Part 2, Step 9)** — A page component (suggested location: `src/pages/NotFoundPage.jsx`) for unmatched routes. Includes navigation links using React Router's `Link` component. Example — adapt to your own layout: the specific content and links are the student's choice.
- **RequireAuth Component (Part 3, Step 10)** — A wrapper component (suggested location: `src/components/RequireAuth.jsx`) that protects routes requiring authentication. Accepts `children` as props. Uses `useAuth` for authentication status, `useLocation` to capture the current location, and `useNavigate` to redirect unauthenticated users to `'/login'` with the current location passed in state for destination preservation. Returns children when authenticated.
- **Navigation Component (Part 4, Step 13)** — A component (suggested location: `src/shared/Navigation.jsx`) using `NavLink` from React Router with a style function that accepts `{ isActive }` for active state styling (bold, underline). Conditionally renders links based on authentication: always shows "About" (`'/about'`); authenticated users see "Todos" (`'/todos'`) and "Profile" (`'/profile'`); unauthenticated users see "Login" (`'/login'`). Use exactly as written: the link paths must match the route configuration.
- **Header Update (Part 4, Step 14)** — Header.jsx includes the Navigation component between the title and the logout button.
- **Logoff Navigation Update (Part 4, Step 15)** — Logoff.jsx imports `useNavigate` from `'react-router'` and navigates to `'/login'` after successful logout.
- **StatusFilter Component (Part 5, Step 17)** — A component (suggested location: `src/shared/StatusFilter.jsx`) using `useSearchParams` from React Router for URL-based status filtering. Provides a select dropdown with three options. Use exactly as written: the status values `'all'`, `'active'`, and `'completed'` must match — they are used by TodoList's filtering logic. When `'all'` is selected, the status parameter is removed from the URL.
- **TodosPage URL Filtering Integration (Part 5, Step 18)** — TodosPage imports `useSearchParams` and the StatusFilter component. Reads the status filter from URL parameters (`searchParams.get('status') || 'all'`). Passes `statusFilter` as a prop to TodoList. StatusFilter is rendered in the JSX.
- **TodoList Status Filtering (Part 5, Step 19)** — TodoList accepts a `statusFilter` prop and filters todos using a switch statement: `'completed'` shows only completed, `'active'` shows only active, `'all'` shows all. The `useMemo` dependency array includes `statusFilter`. Empty state messages vary by filter status. Use exactly as written: the `statusFilter` prop name and status values must match the StatusFilter component. Example — adapt to your own layout: the empty state message text may vary.
- **Functional Verification (Parts 7 & 8, Steps 22–25)** — Multi-page navigation works between all pages; protected routes redirect to login when not authenticated; login redirects to the intended destination; URL filtering updates the browser URL; the 404 page shows for invalid URLs; browser back/forward buttons work; logout redirects to login from any page. Note: AirHub cannot run the dev server to verify runtime behavior; it is confirmed by human reviewers.
- **Console.log Cleanup (Part 8, Step 26)** — Any `console.log` statements added during development are removed before final submission.
- **Checkpoint: Check Your Understanding with AI** — This is an ungraded learning activity. The prompts are reflective exercises that produce no code artifact. Do not assess these; they cannot be verified from submitted code.
- **Version Control and Submission** — Changes committed to the working branch, pushed to GitHub, and a PR created comparing the working branch to `main`.

### Optional Deliverables/Tasks

**None.** All tasks in this assignment are required.

</details>