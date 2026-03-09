<!-- h1, h2 already used by CTD Learns -->

### Expected App Capabilities

After completing this week's assignment, your app should:

- manage todos using state
- allow users to add a todo
- retain the `input`'s focus when a todo is submitted with the button or enter key
- render entered todos in a list

### Instructions Part 1: Prepare State for Dynamic Todos

#### Update Initial State

Currently your app displays the hardcoded todos from previous weeks. To make the form truly functional, we need to start with an empty todo list so users can add their own todos.

1. In `App.jsx`, find where you initialize the `todoList` state with `useState`
2. Change the initial value from the hardcoded array to an empty array: `useState([])`
3. Delete the `todos` array since it's not needed.
4. Save and refresh your browser - you should now see an empty todo list

### Instructions Part 2: Create the Add Todo Handler

Now we'll create a function that can add new todos to our state.

#### Create the addTodo Function

1. In `App.jsx`, create a new function called `addTodo` above the return statement that:
   - Takes a `todoTitle` parameter
   - Creates a new todo object with an `id` property set to `Date.now()` and a `title` property set to the `todoTitle` parameter
   - We update the todoList state by functionally updating (because current state relies on previous state), adding our `newTodo`, and spreading the `previous` todoList. It should look something like: `setTodoList(previous => [newTodo, ...previous])`

> [!NOTE]
> We use `Date.now()` to generate a unique ID for each todo. In a real application, you'd typically use a more robust ID generation method, but this works well for our purposes.

#### Pass the Handler to TodoForm

1. Add an `onAddTodo` prop to the `TodoForm` component instance in your App's return statement:

```jsx
<TodoForm onAddTodo={addTodo} />
```

### Instructions Part 3: Handle Form Submission

Now we need to update the TodoForm component to handle form submissions and call the `addTodo` function.

#### Create Form Submit Handler

1. In `TodoForm.jsx`, import `useRef` at the top of the file:

```jsx
import { useRef } from 'react';
```

1. Add the `onAddTodo` prop to the TodoForm function parameters:

```jsx
function TodoForm({ onAddTodo }) {
```

1. Create a ref for the input field and a form submit handler inside the TodoForm component:

```jsx
function TodoForm({ onAddTodo }) {
  const inputRef = useRef();

  const handleAddTodo = (event) => {
    event.preventDefault();

    // Explore the event object (we'll remove this later)
    console.log('Event object:', event);
    console.log('Event target:', event.target);
    console.log('Input value:', event.target.todoTitle.value);

    // .trim prevents whitespace only todos
    const todoTitle = event.target.todoTitle.value.trim();
    if (todoTitle && todoTitle !== "") {
      onAddTodo(todoTitle);
      event.target.reset();
      inputRef.current.focus();
    }
  };

  return (
    // form JSX will go here
  );
}
```

#### Update the Form JSX

1. Update the form's return statement to use the handler and ref:

```jsx
return (
  <form>
    <label htmlFor="todoTitle">Todo</label>
    <input
      ref={inputRef}
      type="text"
      id="todoTitle"
      name="todoTitle"
      placeholder={'Todo text'}
      required
    />
    <button type="submit" onClick={handleAddTodo}>
      Add Todo
    </button>
  </form>
);
```

> [!note]
> Notice we removed the `disabled` attribute from the button and added `name="todoTitle"` to the input. The name attribute allows us to access the input's value using `event.target.todoTitle.value`.

### Instructions Part 4: Test and Clean Up

#### Test Your Form

1. Save your files and test the form in your browser
2. Try adding a few todos - they should appear in the list immediately
3. Check the browser console to see the logged event information
4. Notice how the input stays focused after submitting (thanks to the `useRef` hook)

#### Clean Up Console Logs

Once you've verified everything works and explored the event object:

1. Remove the three `console.log` statements from the `handleAddTodo` function
2. Your final handler should look like this:

```jsx
const handleAddTodo = (event) => {
  event.preventDefault();

  const todoTitle = event.target.todoTitle.value.trim();
  if (todoTitle) {
    onAddTodo(todoTitle);
    event.target.reset();
    inputRef.current.focus();
  }
};
```

### Instructions Part 5: Final Testing

#### Test Your Application

Before completing the assignment, verify that your app works correctly:

- The page loads with an empty todo list
- You can add new todos using the form
- Todos appear in the list immediately after submission
- The input field stays focused after adding a todo
- The form works with both the submit button and Enter key
- Empty or whitespace-only todos are not added

### Instructions Part 6: Final Steps and Submission

#### Checkpoint: Check Your Understanding with AI

Choose 1–2 prompts below. Explain in your own words first, then ask AI for feedback.

> [!NOTE]
> Do not ask AI to complete the assignment code for you.

> - "In handleAddTodo, I see event.preventDefault(). Here's my explanation of why this is necessary in a form submission: [my explanation]. Is this accurate?"
> - "I'm accessing the todo text with event.target.todoTitle.value. Here's my understanding of how the form field name connects to this: [my understanding]. Did I get this right?"
> - "I use Date.now() to create unique IDs for each todo. Here's why I think this is better than using array index: [my reasoning]. Would this work in production?"
> - "I added inputRef.current.focus() after adding a todo. Here is my explanation of what useRef does and why this keeps focus on the input: [my explanation]. What should I clarify?"

#### Version Control and Submission

- Commit your changes to your local working branch.
- Push the branch to GitHub.
- In GitHub, create a PR (pull request) that compares your working branch to `main`.
- Copy the PR link and submit assignment.

### What You Accomplished This Week

Congratulations! You've successfully:

- ✅ Implemented event handling in React using `onSubmit`
- ✅ Created functions that update state in response to user actions
- ✅ Used the `useRef` hook to manage focus and form interactions
- ✅ Learned to prevent default form behavior with `event.preventDefault()`
- ✅ Explored how to access form data through the event object
- ✅ Built a fully functional todo application with dynamic state updates
- ✅ Practiced proper form validation and user experience patterns

### Looking Ahead

In upcoming weeks, you'll learn to:

- Handle more complex user interactions and events
- Work with controlled components and form state
- Implement todo completion and deletion features
- Manage more sophisticated application state
- And much more...

> [!IMPORTANT]
> **Important**: The event handling patterns you've learned this week are fundamental to building interactive React applications. Make sure you understand how events trigger state updates and how components communicate through props before moving on.
