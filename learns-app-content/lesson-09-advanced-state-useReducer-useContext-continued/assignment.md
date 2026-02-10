<!-- h1, h2 already used by CTD Learns -->

> **📚 Important Note About This Assignment**
>
> This week's discussion introduced some of the most powerful React concepts you'll learn in this course! You're tackling **useReducer** and **useContext** - two advanced patterns that professional React developers use daily.
>
> This is also the **longest assignment in our curriculum**, involving significant refactoring of your existing code. Plan to dedicate **6-8 hours** spread across the week, working in focused sessions. Take breaks between sections - these concepts build on each other, so understanding each part thoroughly will make the next part easier.
>
> Remember: Every React developer has felt challenged by these patterns initially. Take your time, test frequently, commit to version control as you refactor, and don't hesitate to ask questions. You've got this! 🚀

### Expected App Capabilities

After completing this week's assignment, your todo app will demonstrate advanced React state management patterns:

✅ **useReducer Implementation**: Replace multiple useState calls with a single useReducer to manage complex todo state  
✅ **useContext Integration**: Eliminate prop drilling by using context for authentication state  
✅ **Centralized State Logic**: All todo operations handled through dispatched actions  
✅ **Clean Component Architecture**: Components access shared state without prop passing  
✅ **Optimistic Updates**: UI updates immediately with proper error rollback handling

### Instructions Part 1: Understanding the Problem

#### Current State Management Issues

Before starting, examine your current `TodosPage.jsx` and notice these problems:

**Multiple Related State Variables:**

- `todoList`, `error`, `filterError`, `isTodoListLoading`
- `sortBy`, `sortDirection`, `filterTerm`, `dataVersion`
- These are all related but managed separately

**Authentication Prop Drilling:**

- `App.jsx` → `Header` → `Logoff` (token, onSetToken, onSetEmail)
- `App.jsx` → `Logon` (onSetEmail, onSetToken)
- `App.jsx` → `TodosPage` (token)

These patterns become harder to maintain as applications grow.

### Instructions Part 2: Implementing useReducer

#### Understanding the Goal

In this section, you'll consolidate all todo-related state management into a single `useReducer` hook. Instead of managing 8 separate state variables (`todoList`, `error`, `filterError`, `isTodoListLoading`, `sortBy`, `sortDirection`, `filterTerm`, `dataVersion`) with individual `useState` calls, you'll use one reducer that manages all this related state together.

**Why this matters**: As applications grow, managing related state separately becomes error-prone and hard to coordinate. The reducer pattern gives you:

- **Centralized logic**: All state changes go through one predictable system
- **Coordinated updates**: Related state changes happen together (like clearing errors when starting new operations)
- **Easier debugging**: One place to look when state isn't updating correctly

#### Create the Todo Reducer

Create a new file `src/reducers/todoReducer.js`:

**Action Types**: Define actions for all todo operations (your names may vary slightly):

- Async operations: `FETCH_START`, `FETCH_SUCCESS`, `FETCH_ERROR`
- Todo mutations: `ADD_TODO_START`, `ADD_TODO_SUCCESS`, `ADD_TODO_ERROR`
- Similar patterns for `COMPLETE_TODO_*` and `UPDATE_TODO_*`
- UI operations: `SET_SORT`, `SET_FILTER`, `CLEAR_ERROR`, `RESET_FILTERS`

Create an exported object with all your action types:

```js
export const TODO_ACTIONS = {
  // Fetch operations
  FETCH_START: 'FETCH_START',
  FETCH_SUCCESS: 'FETCH_SUCCESS',
  FETCH_ERROR: 'FETCH_ERROR',
  
  // Add todo operations
  ADD_TODO_START: 'ADD_TODO_START',
  ADD_TODO_SUCCESS: 'ADD_TODO_SUCCESS',
  ADD_TODO_ERROR: 'ADD_TODO_ERROR',
  // ... continue for all operations
};
```

**Why export this object?** When dispatching actions, you'll use `TODO_ACTIONS.FETCH_START` instead of typing `'FETCH_START'` as a string. This prevents typos and gives you autocomplete support in your editor. If you mistype an action name, you'll get an error immediately instead of a silent bug.

**Initial State**: Create a single state object that consolidates all your current `TodosPage` state variables.

First, examine your current `TodosPage.jsx` and identify all the useState calls:

```js
// Current individual state variables in TodosPage.jsx - your names may vary slightly.
const [todoList, setTodoList] = useState([]);
const [error, setError] = useState('');
const [filterError, setFilterError] = useState('');
const [isTodoListLoading, setIsTodoListLoading] = useState(true);
const [sortBy, setSortBy] = useState('createdDate');
const [sortDirection, setSortDirection] = useState('asc');
const [filterTerm, setFilterTerm] = useState('');
const [dataVersion, setDataVersion] = useState(0);
```

Transform these into a single initial state object in todoReducer.js:

```js
export const initialTodoState = {
  todoList: [],
  error: '',
  filterError: '',
  isTodoListLoading: true,
  sortBy: 'createdDate',
  sortDirection: 'asc',
  filterTerm: '',
  dataVersion: 0,
};
```

**Key principles for initial state:**

- Use the same property names as your current state variables for easier migration
- Set the same default values you currently use in useState calls
- Group all related state in one object - this makes state updates more coordinated
- Keep the structure flat and simple - avoid nested objects unless absolutely necessary

**Reducer Function**: Implement a reducer that handles each action type, ensuring:

- Optimistic updates (START actions apply changes immediately)
- Proper error rollback (ERROR actions restore previous state)
- Immutable state updates (always return new objects)

**Step-by-step Reducer Implementation:**

1. **Create the reducer function structure**:

   ```js
   export function todoReducer(state, action) {
     switch (action.type) {
       // We'll add cases here
       default:
         throw new Error(`Unknown action type: ${action.type}`);
     }
   }
   ```

2. **Implement your first case - FETCH_START**:

   ```js
   case TODO_ACTIONS.FETCH_START:
     return {
       ...state,
       isTodoListLoading: true,
       error: '',
       filterError: '',
     };
   ```

3. **Find the logic to migrate**: Look in your current `TodosPage.jsx` for the `fetchTodos` function. Find where `setIsTodoListLoading(true)` is called - this logic belongs in `FETCH_START`.

4. **Key patterns to follow**:
   - Always use the spread operator `...state` to preserve existing state
   - Only modify the properties that need to change
   - Each case should return a completely new state object
   - Clear related errors when starting new operations

5. **Repeat for each action**: For each remaining action type (`FETCH_SUCCESS`, `FETCH_ERROR`, `ADD_TODO_START`, etc.), find the corresponding logic in your existing functions and move it to the appropriate case.

**Where to find the logic**:

- **FETCH operations**: Look in the `useEffect` that loads todos
- **ADD_TODO operations**: Look in the `addTodo` function
- **COMPLETE_TODO operations**: Look in the `completeTodo` function
- **UPDATE_TODO operations**: Look in the `updateTodo` function
- **UI operations**: Look at `setSortBy`, `setFilterTerm`, error clearing buttons

#### Refactor TodosPage to Use Reducer

Replace multiple `useState` calls with a single `useReducer`:

1. **Import**: Add `useReducer` and your reducer utilities from the reducers directory
2. **State Setup**: Replace 8 useState calls with `useReducer(todoReducer, initialTodoState)`
3. **Destructure State**: Extract individual state pieces from the reducer state
4. **Update Functions**: Replace all `setXxx` calls with appropriate `dispatch` calls

**Import Example**:

```js
import {
  todoReducer,
  initialTodoState,
  TODO_ACTIONS,
} from '../../reducers/todoReducer';
```

**State Setup and Destructuring Example**:

Replace your multiple useState calls with useReducer and destructure the state:

```js
// Before: Multiple useState calls
const [todoList, setTodoList] = useState([]);
const [error, setError] = useState('');
const [filterError, setFilterError] = useState('');
const [isTodoListLoading, setIsTodoListLoading] = useState(true);
const [sortBy, setSortBy] = useState('createdDate');
const [sortDirection, setSortDirection] = useState('asc');
const [filterTerm, setFilterTerm] = useState('');
const [dataVersion, setDataVersion] = useState(0);

// After: Replace all useState calls with useReducer and destructuring useReducer's state
const [state, dispatch] = useReducer(todoReducer, initialTodoState);
const {
  todoList,
  error,
  filterError,
  isTodoListLoading,
  sortBy,
  sortDirection,
  filterTerm,
  dataVersion,
} = state;
```

This destructuring lets you access individual state pieces (like `todoList`, `error`, etc.) just like before, but now they're all managed through the single reducer.

#### Step-by-Step useEffect Refactoring

##### 1. Refactor the useEffect employing fetchTodos

Your current useEffect probably has multiple setState calls. Replace them with dispatch calls:

```js
// Before: Multiple setState calls
setIsTodoListLoading(true);
setError('');
setFilterError('');

// After: Single dispatch call
dispatch({ type: TODO_ACTIONS.FETCH_START });
```

```js
// Before: Setting todos on success
const todos = await resp.json();
setTodoList(todos);
setIsTodoListLoading(false);

// After: Dispatch with payload
const todos = await resp.json();
dispatch({
  type: TODO_ACTIONS.FETCH_SUCCESS,
  payload: { todos },
});
```

```js
// Before: Setting errors
setError(`Error fetching todos: ${error.message}`);
setIsTodoListLoading(false);

// After: Dispatch with error payload
dispatch({
  type: TODO_ACTIONS.FETCH_ERROR,
  payload: {
    message: `Error fetching todos: ${error.message}`,
    isFilterError: false,
  },
});
```

##### 2. Apply the Same Pattern to Other Functions

Refactor all remaining functions following the same dispatch pattern:

- **addTodo function**: Replace `setTodoList()` calls with `ADD_TODO_START`, `ADD_TODO_SUCCESS`, `ADD_TODO_ERROR` dispatches
- **completeTodo function**: Replace state updates with `COMPLETE_TODO_START`, `COMPLETE_TODO_SUCCESS`, `COMPLETE_TODO_ERROR` dispatches  
- **updateTodo function**: Replace state updates with `UPDATE_TODO_START`, `UPDATE_TODO_SUCCESS`, `UPDATE_TODO_ERROR` dispatches
- **Event handlers**: Replace `setSortBy()`, `setFilterTerm()` calls with `SET_SORT`, `SET_FILTER` dispatches
- **Error clearing**: Replace `setError('')` calls with `CLEAR_ERROR`, `CLEAR_FILTER_ERROR` dispatches

##### 3. Update Prop Handlers

Replace setState calls in your JSX event handlers:

```js
// Before:
onSortByChange={(newSortBy) => setSortBy(newSortBy)}

// After:
onSortByChange={(newSortBy) =>
  dispatch({
    type: TODO_ACTIONS.SET_SORT,
    payload: { sortBy: newSortBy, sortDirection },
  })
}
```

**Apply This Pattern Throughout Your Component**:

Remember: Every place you directly call a `setXxx` function should be replaced with a `dispatch` call that uses the appropriate action type and payload structure.

#### 🎯 Part 2 Checkpoint: Validate Your useReducer Implementation

Before moving to useContext, ensure your useReducer refactoring is working correctly:

##### Test These Core Functions

1. **Add a todo**: New todo should appear immediately (optimistic update)
2. **Complete a todo**: Todo should be marked complete immediately
3. **Sort todos**: Changing sort criteria should re-fetch and display correctly
4. **Filter todos**: Typing in filter should search todos
5. **Error handling**: Check that errors display and can be cleared

##### Success Indicators

✅ No console errors when interacting with todos  
✅ All todo operations work exactly like before the refactor  
✅ State updates happen through dispatch calls only  
✅ Your TodosPage component has only one `useReducer` call instead of multiple `useState` calls  

##### Part 2 Debug Check

Add this temporary console.log to your reducer to see actions:

```js
export function todoReducer(state, action) {
  console.log('Dispatched action:', action.type, action.payload); // Remove this before committing
  switch (action.type) {
    // ... your cases
  }
}
```

> **⚠️ Commit Your Progress**: Once useReducer is working, commit your changes: `git add . && git commit -m "Implement useReducer for todo state management"`

### Instructions Part 3: Implementing useContext

#### Understanding the Context Goal

In this section, you'll eliminate "prop drilling" by using React Context for authentication state. Currently, your authentication data (`email`, `token`) and functions (`onSetEmail`, `onSetToken`) are passed as props through multiple component levels:

```text
App.jsx (manages email, token)
├── Header (receives token, onSetToken, onSetEmail)
│   └── Logoff (receives token, onSetToken, onSetEmail)  
└── Logon (receives onSetEmail, onSetToken)
└── TodosPage (receives token)
```

**Why this is problematic**: As your app grows, you'll have authentication props flowing through components that don't need them, just to reach components that do. This makes components harder to reuse and maintain.

**The Context solution**: Context lets components access authentication state directly, without props being passed through intermediate components:

```text
AuthProvider (manages email, token, login, logout)
├── Header (useAuth hook - direct access)
│   └── Logoff (useAuth hook - direct access)
└── Logon (useAuth hook - direct access)  
└── TodosPage (useAuth hook - direct access)
```

#### Create the AuthContext

Create `src/contexts/AuthContext.jsx` with:

**Context Setup**: Use `createContext()` and custom `useAuth()` hook.

```js
import { createContext, useContext, useState } from 'react';

// Create the context
const AuthContext = createContext();

// Custom hook with error checking
export function useAuth() {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within an AuthProvider');
  }
  return context;
}
```

The `useAuth` hook is a custom hook that provides a clean interface for components to access authentication state. It includes error checking to ensure components are properly wrapped with the AuthProvider.

**Why this pattern?** The `useAuth` hook:

- **Simplifies component code**: Components just call `useAuth()` instead of `useContext(AuthContext)`
- **Provides error protection**: If a component tries to use `useAuth` outside an AuthProvider, it gets a clear error message instead of undefined behavior
- **Centralizes context access**: All auth-related context usage goes through this single hook

##### AuthProvider Component Structure Setup

Create the AuthProvider component in the same file that will wrap your application and manage authentication state:

```js
export function AuthProvider({ children }) {
  // State for authentication
  const [email, setEmail] = useState('');
  const [token, setToken] = useState('');
  
  // Functions will go here...
  
  // Context value object
  const value = {
    email,
    token,
    isAuthenticated: !!token,
    login,
    logout,
  };
  
  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
}
```

**Key structure elements:**

- **useState for auth data**: `email` and `token` state variables
- **children prop**: Allows this component to wrap other components
- **Context.Provider**: Makes the value available to all child components

##### Implementing the Login

In the AuthProvider component, login function handles the API call and updates authentication state on success:

```js
const login = async (userEmail, password) => {
  try {
    const options = {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email: userEmail, password }),
      credentials: 'include',
    };
    
    const res = await fetch(`${baseUrl}/user/logon`, options);
    const data = await res.json();
    
    if (res.status === 200 && data.name && data.csrfToken) {
      // Success: Update state
      setEmail(data.name);
      setToken(data.csrfToken);
      return { success: true };
    } else {
      // Failure: Return error
      return {
        success: false,
        error: `Authentication failed: ${data?.message}`,
      };
    }
  } catch (error) {
    return {
      success: false,
      error: 'Network error during login',
    };
  }
};
```

**Key login patterns:**

- **Return success/error objects**: Components can check `result.success` and handle `result.error`
- **Update state on success**: Only call `setEmail` and `setToken` when authentication succeeds
- **Include credentials**: `credentials: 'include'` ensures cookies are sent with requests
- **Error handling**: Catch network errors and API errors consistently

##### Implementing the Logout Function

Create a logout function that clears authentication state and calls the logout API:

- **Check if token exists**: If no token, just clear local state
- **Call logout API**: Send POST request to `/user/logoff` with CSRF token
- **Clear state always**: Whether API succeeds or fails, clear local authentication
- **Return result object**: Use same success/error pattern as login

##### Provider and Context Values

The `value` object contains everything components need from the authentication context:

```js
const value = {
  email,           // Current user's email
  token,           // CSRF token for API requests
  isAuthenticated: !!token,  // Computed boolean for auth status
  login,           // Function to authenticate user
  logout,          // Function to clear authentication
};
```

**The Provider JSX**: `<AuthContext.Provider value={value}>{children}</AuthContext.Provider>` makes this value available to all descendant components. Any component wrapped by AuthProvider can call `useAuth()` to access these values and functions.

**Context Value**: Provide `{ email, token, isAuthenticated, login, logout }`

#### Setup Context Provider

**In `main.jsx`**: Wrap your `<App />` with `<AuthProvider>` to provide the Auth context.

#### Remove Prop Drilling

**Update App.jsx**:

- Remove authentication useState calls
- Use `useAuth()` to access `isAuthenticated`
- Remove all authentication props from child components

**Update Header.jsx**:

- Remove props parameter
- Use `useAuth()` to access `isAuthenticated`

**Update Logon.jsx**:

- Remove props parameter
- Use `useAuth()` to access `login()` method
- Handle login success/error from context

**Update Logoff.jsx**:

- Remove props parameter
- Use `useAuth()` to access `logout()` method
- Handle logout success/error from context

**Update TodosPage.jsx**:

- Remove `token` prop
- Use `useAuth()` to access `token` for API calls

#### 🎯 Part 3 Checkpoint: Validate Your useContext Implementation

Before moving to final testing, ensure your useContext refactoring is working correctly:

##### Test Authentication Flow

1. **Login process**: Enter credentials and verify successful login
2. **UI updates**: Header should show logout button when authenticated
3. **Todo access**: TodosPage should load todos after login
4. **Logout process**: Logout should return to login form
5. **Direct access**: Try refreshing page - should maintain auth state

##### Context Success Indicators

✅ No authentication props being passed between components  
✅ All components use `useAuth()` hook to access auth state  
✅ Login and logout work exactly like before the refactor  
✅ App.jsx no longer manages email/token state directly  
✅ AuthProvider is wrapping your App in main.jsx  

##### Part 3 Debug Check

Add this temporary console.log to your useAuth hook:

```js
export function useAuth() {
  const context = useContext(AuthContext);
  console.log('Auth context:', context); // Remove this later
  if (!context) {
    throw new Error('useAuth must be used within an AuthProvider');
  }
  return context;
}
```

> **⚠️ Commit Your Progress**: Once useContext is working, commit your changes: `git add . && git commit -m "Implement useContext for authentication state"`

## Instructions Part 4: Final Testing and Verification

#### Test Your Application

Verify all functionality works after the refactor:

1. **Authentication Flow**:
   - Login form appears when not authenticated
   - Login process updates UI correctly
   - Logout button appears when authenticated
   - Logout process returns to login form

2. **Todo Operations**:
   - Add new todos (optimistic updates work)
   - Complete todos (immediate UI update)
   - Edit todo titles (inline editing functions)
   - Delete todos (if implemented)

3. **Advanced Features**:
   - Sort by different criteria (creation date, title, completion)
   - Filter todos by text search
   - Error handling displays appropriate messages
   - Loading states show during async operations

4. **Error Testing**:
   - Network errors display and can be cleared
   - Failed operations rollback optimistically applied changes
   - Multiple error types (general vs. filter errors) work correctly

#### Validate State Management

**useReducer Benefits**: Notice how:

- All todo state is managed in one place
- State transitions are predictable through actions
- Related state updates are coordinated
- Async operations have consistent patterns

**useContext Benefits**: Notice how:

- No authentication props are passed between components
- Components access auth state directly where needed
- Adding new auth-dependent components is simple
- Authentication logic is centralized

### Instructions Part 5: Understanding the Patterns

#### When to Use useReducer

Use `useReducer` when you have:

- Multiple state variables that are related
- Complex state update logic
- State updates that depend on previous state
- Need for consistent patterns across similar operations

#### When to Use useContext

Use `useContext` when you have:

- Props being passed through multiple component levels
- State needed by components throughout your app
- Global application state (auth, theme, settings)
- Want to avoid "prop drilling"

#### Best Practices Demonstrated

**Reducer Actions**: Use descriptive action types and consistent payload structures.

**Optimistic Updates**: Apply UI changes immediately for better user experience, with rollback on errors.

**Context Boundaries**: Keep contexts focused - AuthContext only handles authentication, not todos.

**Error Boundaries**: Separate error types (general vs. filter errors) for better user experience.

### What You Accomplished This Week

✅ **Mastered useReducer**: Consolidated 8 state variables into a single, manageable reducer  
✅ **Eliminated Prop Drilling**: Removed authentication prop passing through 4 component levels  
✅ **Implemented Context Pattern**: Created reusable AuthProvider with custom hook  
✅ **Improved Architecture**: Separated concerns between state management and UI components  
✅ **Enhanced User Experience**: Maintained optimistic updates with proper error handling  
✅ **Applied Advanced Patterns**: Demonstrated real-world React state management solutions

### Looking Ahead

 This week's assignment was a doozy! Pat yourself on the back for a job well done! Next week, we'll explore React Router to add navigation and multiple pages to your application, building on the solid state management foundation you've established this week.
