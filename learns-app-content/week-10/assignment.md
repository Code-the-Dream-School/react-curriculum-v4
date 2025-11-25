<!-- h1, h2 already used by CTD Learns -->

### Expected App Capabilities

After completing this week's assignment, your todo app will be transformed from a single-page application to a multi-page experience with:

✅ **Multi-page navigation** with dedicated routes for todos, profile, about, and login  
✅ **Protected routes** that require authentication to access  
✅ **URL-based todo filtering** using search parameters (`/todos?status=completed`)  
✅ **Programmatic navigation** after login/logout with proper redirects  
✅ **Navigation component** with active state indicators  
✅ **404 error handling** for unknown routes  
✅ **Authentication flow** that preserves intended destinations after login

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

   Your `main.jsx` should look like this:

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

   Your new App.jsx structure should look like this:

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

    Your complete App.jsx should look like this:

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

#### Add Navigation Logic to AuthContext

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
    - Import the base URL from environment variables using `import.meta.env.VITE_BASE_URL`
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

          const response = await fetch(`${baseUrl}/tasks`, options);

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
