<!-- h1, h2 already used by CTD Learns -->
### Expected App Capabilities

After completing this week's assignment, your app should:

- conditionally render a message when the todo list is empty
- disable the Add Todo button when the input is empty
- allow users to complete a todo by checking a checkbox
- utilize a controlled form component

### Instructions Part 1: Conditional Rendering for Empty List

> [!note]
> This week we'll add conditional rendering and controlled components to make our todo app more user-friendly and interactive.

#### Add Empty State Message

When users first visit the app or complete all their todos, they should see a helpful message instead of an empty list.

1. In `TodoList.jsx`, replace the current return statement with a ternary operator that:
   - Checks if the `todoList` length equals zero
   - If true, renders a paragraph element with the text "Add todo above to get started"
   - If false, renders the existing unordered list with the mapped todos

Your conditional rendering should follow this pattern:

### Instructions Part 2: Add Todo Completion Feature

Now we'll add the ability for users to mark todos as complete using checkboxes.

#### Update Todo Data Structure

1. In `App.jsx`, find the `addTodo` function and update the new todo object to include an `isCompleted` property set to `false`
2. Each todo should now have three properties: `id`, `title`, and `isCompleted`

#### Create Complete Todo Handler

1. In `App.jsx`, create a new function called `completeTodo` above the return statement that:
   - Takes an `id` parameter
   - Maps through the `todoList` array
   - For each todo, checks if `todo.id` matches the provided `id`
   - If it matches, returns a new object that spreads the current todo and sets `isCompleted` to `true`
   - If it doesn't match, returns the todo unchanged
   - Updates the todoList state with the resulting array

> [!note]
> Use the spread operator (`{...todo, isCompleted: true}`) to create a new object rather than mutating the existing one. This follows React's immutability principle.

#### Pass Handler Through Components

1. In `App.jsx`, add an `onCompleteTodo` prop to the `TodoList` component, passing in your `completeTodo` function
2. In `TodoList.jsx`, add `onCompleteTodo` to the component's props using destructuring
3. Pass the `onCompleteTodo` prop to each `TodoListItem` component instance

#### Update TodoListItem with Checkbox

1. In `TodoListItem.jsx`, add `onCompleteTodo` to the component's props using destructuring
2. Update the component's return statement to wrap the content in a form element
3. Add a checkbox input above the todo title with:
   - `type="checkbox"`
   - `checked` prop set to `todo.isCompleted`
   - `onChange` event handler that calls `onCompleteTodo` with the todo's id

Your TodoListItem structure should look like:

```jsx
return (
  <li>
    <form>
      <input
        type="checkbox"
        checked={todo.isCompleted}
        onChange={() => onCompleteTodo(todo.id)}
      />
      {todo.title}
    </form>
  </li>
);
```

#### Filter Completed Todos

1. In `TodoList.jsx`, create a `filteredTodoList` constant that filters out todos where `isCompleted` is `true`
2. Replace all references to `todoList` in the JSX with `filteredTodoList`

Now when users check a todo's checkbox, it will disappear from the list as it's marked complete.

### Instructions Part 3: Convert to Controlled Component

We'll convert the TodoForm from an uncontrolled to a controlled component for better form management.

#### Add Local State to TodoForm

1. In `TodoForm.jsx`, import `useState` from React
2. Create a state variable called `workingTodoTitle` with its setter function, initialized to an empty string

#### Connect Input to State

1. Add a `value` prop to the input element, setting it to `workingTodoTitle`
2. Add an `onChange` event handler to the input that:
   - Takes the event object as a parameter
   - Calls the state setter function with `event.target.value`

#### Update Form Submit Handler

1. In the `handleAddTodo` function, remove the lines that:
   - Get the title value from `event.target.todoTitle.value`
   - Reset the form with `event.target.reset()`
2. Update the `onAddTodo` call to pass `workingTodoTitle` instead of the extracted title
3. After calling `onAddTodo`, reset the input by calling the state setter with an empty string

### Instructions Part 4: Disable Button for Empty Input

Let's prevent users from submitting empty todos by disabling the button when appropriate.

#### Add Button Disable Logic

1. In `TodoForm.jsx`, add a `disabled` prop to the button element
2. Set the `disabled` prop to `true` when `workingTodoTitle` is an empty string or contains only whitespace

You can use this condition: `disabled={!workingTodoTitle.trim()}`

### Instructions Part 5: Final Testing

#### Test Your Application

Before completing the assignment, verify that your app works correctly:

- The page shows "Add todo above to get started" when the list is empty
- You can add new todos using the form
- The Add Todo button is disabled when the input is empty
- Each todo displays with a checkbox
- Checking a todo's checkbox removes it from the list (marks it complete)
- The input field updates as you type (controlled component behavior)
- The form clears after submitting a todo

### What You Accomplished This Week

Congratulations! You've successfully:

- ✅ Implemented conditional rendering to show different UI states
- ✅ Created interactive checkboxes that update application state
- ✅ Built a complete todo completion workflow with proper state management
- ✅ Converted forms from uncontrolled to controlled components
- ✅ Added simple form validation through button disable logic
- ✅ Practiced state management patterns with filtering and mapping
- ✅ Enhanced user experience with dynamic UI feedback

### Looking Ahead

In upcoming weeks, you'll learn to:

- Build more complex reusable components
- Organize and refactor larger applications
- Work with external data and APIs
- Implement advanced state management patterns
- And much more...

> [!note]
> **Important**: The controlled component pattern you've learned this week is essential for building robust React forms. Make sure you understand how state controls form inputs and how to handle user interactions before moving on.
