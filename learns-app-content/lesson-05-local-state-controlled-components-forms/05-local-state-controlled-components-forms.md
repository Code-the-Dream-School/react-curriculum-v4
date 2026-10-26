<!-- h1, h2 already used by CTD Learns -->
### Expected App Capabilities

After completing this week's assignment, your app should:

- conditionally render a message when the todo list is empty
- disable the Add Todo button when the input is empty
- allow users to complete a todo by checking a checkbox
- utilize a controlled form component

Note: The variable names, prop names, and property names specified in this assignment (isCompleted, completeTodo, onCompleteTodo, filteredTodoList, workingTodoTitle) should be used exactly as written — each component relies on these exact names to connect to the others correctly.

Keep your existing code from previous lessons. This week's work builds on and modifies that code — it does not replace it.

### Instructions Part 1: Conditional Rendering for Empty List

> [!NOTE]
> This week we'll add conditional rendering and controlled components to make our todo app more user-friendly and interactive.

#### Add Empty State Message

When users first visit the app or complete all their todos, they should see a helpful message instead of an empty list.

1. In `TodoList.jsx`, replace the current return statement with a ternary operator that:
   - Checks if the `todoList` length equals zero
   - If true, renders a paragraph element with the text "Add todo above to get started"
   - If false, renders the existing unordered list with the mapped todos

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
2. Wrap the content inside the list item with an `input` element.
   - `type="checkbox"`
   - `checked` prop set to `todo.isCompleted`
   - `onChange` event handler that calls `onCompleteTodo` with the todo's id

Use exactly as written — your TodoListItem structure should look like this. The checkbox attributes and handler connect to the completeTodo logic above:

```jsx
return (
  <li>
      <input
        type="checkbox"
        checked={todo.isCompleted}
        onChange={() => onCompleteTodo(todo.id)}
      />
      {todo.title}
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

### Checkpoint: Check Your Understanding with AI

Choose 1–2 prompts below. Explain in your own words first, then ask AI for feedback.

> [!NOTE]
> Do not ask AI to complete the assignment code for you.

> - "I've added a ternary operator in TodoList to show 'Add todo above to get started' when the list is empty. I think my logic is: `todoList.length === 0 ? emptyMessage : todoList`. Is my understanding of this pattern correct for this use case?"
> - "I've wired up a controlled checkbox in TodoListItem with checked={todo.isCompleted} and onChange={() => onCompleteTodo(id)}. When I check the box, the todo disappears from the list. I predict this is because the TodoList filters out isCompleted items. Is that flow correct?"
> - "I converted the TodoForm to a controlled component: I added useState for workingTodoTitle, connected it to the input with value and onChange, and after submit I reset the input to empty string. Did I implement the controlled component pattern correctly? What's the best practice I should follow?"

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

### Closing Notes

**Important**: The controlled component pattern you've learned this week is essential for building robust React forms. Confirm you understand how state controls form inputs and how to handle user interactions before moving on.

> [!NOTE]
> The AI review tool (known as AirHub) can check code and structure, but it does not run your code in a server environment to verify that aspect runs properly. We will have human reviewers checking this aspect, so you may receive a passing assignment from AirHub that could still need revisions after a human has checked that your work runs properly in the correct environment. If your AI and human reviewer feedbacks don't match, trust the human review.

---

<details>
<summary>Rubric (for AirHub reviewer and mentors)</summary>

### Required Deliverables/Tasks

- **Conditional Rendering for Empty List (Part 1)** — In TodoList.jsx, the return statement uses a ternary operator to check if the list is empty. When empty, a paragraph element renders with the text "Add todo above to get started." When not empty, the existing unordered list with mapped todos renders. Use exactly as written: the message text "Add todo above to get started" is specified by the assignment. Note: after Part 2, this check uses `filteredTodoList` (not `todoList`) per the instruction to replace all JSX references.
- **Update Todo Data Structure (Part 2)** — The `addTodo` function in App.jsx creates todo objects with three properties: `id`, `title`, and `isCompleted` (set to `false` for new todos). Use exactly as written: the property name `isCompleted` must match — it is used in the checkbox, filter, and completeTodo logic.
- **completeTodo Function (Part 2)** — A function named `completeTodo` in App.jsx that takes an `id` parameter, maps through the `todoList` array, and for the matching todo returns a new object with `isCompleted` set to `true` (using the spread operator), leaving non-matching todos unchanged. Updates state with the resulting array. Use exactly as written: the function name `completeTodo` is passed as a prop and must match.
- **Pass Handler Through Components (Part 2)** — `completeTodo` is passed from App.jsx to TodoList as `onCompleteTodo` prop, then from TodoList to each TodoListItem instance as `onCompleteTodo`. Use exactly as written: the prop name `onCompleteTodo` must match across all three components.
- **TodoListItem Checkbox (Part 2)** — TodoListItem renders a checkbox input with `type="checkbox"`, `checked={todo.isCompleted}`, and `onChange={() => onCompleteTodo(todo.id)}`, placed alongside `{todo.title}` inside the `<li>`. Use exactly as written: the checkbox attributes and handler connect to the completeTodo logic.
- **Filter Completed Todos (Part 2)** — In TodoList.jsx, a `filteredTodoList` constant filters out todos where `isCompleted` is `true`. All JSX references to `todoList` in TodoList.jsx are replaced with `filteredTodoList` (including the conditional rendering check from Part 1). Use exactly as written: the variable name `filteredTodoList` is specified by the assignment.
- **Controlled Component Conversion (Part 3)** — In TodoForm.jsx, `useState` is imported and a state variable `workingTodoTitle` (initialized to an empty string) is created. The input's `value` prop is set to `workingTodoTitle`. An `onChange` handler updates state with `event.target.value`. The `handleAddTodo` function is updated to pass `workingTodoTitle` to `onAddTodo` and reset state to an empty string (replacing the previous `event.target.todoTitle.value` and `event.target.reset()` approach). Use exactly as written: the state variable name `workingTodoTitle` is specified by the assignment.
- **Disable Button for Empty Input (Part 4)** — The submit button's `disabled` prop is set to prevent submission when `workingTodoTitle` is empty or whitespace-only. The suggested implementation is `disabled={!workingTodoTitle.trim()}` but any expression achieving the same behavior is acceptable.
- **Functional Verification (Part 5)** — Empty state message displays when list is empty, todos can be added, button is disabled when input is empty, checkboxes appear on each todo, checking a todo removes it from the list, input updates as the user types (controlled), and the form clears after submission. Note: AirHub cannot run the dev server to verify runtime behavior; it is confirmed by human reviewers.
- **Checkpoint: Check Your Understanding with AI** — This is an ungraded learning activity. The prompts are reflective exercises that produce no code artifact. Do not assess these; they cannot be verified from submitted code.
- **Version Control and Submission** — Changes committed to the working branch, pushed to GitHub, and a PR created comparing the working branch to `main`.

### Optional Deliverables/Tasks

**None.** All tasks in this assignment are required.

</details>
