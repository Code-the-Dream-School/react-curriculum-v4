<!-- h1, h2 already used by CTD Learns -->

### Expected App Capabilities

After completing this week's assignment, your app should:

- manage todos using state
- allow users to add a todo
- retain the `input`'s focus when a todo is submitted with the button or enter key
- render entered todos in a list

Note: The function names, prop names, and attribute values specified in this assignment (addTodo, onAddTodo, handleAddTodo, inputRef, todoTitle) should be used exactly as written — each component relies on these exact names to connect to the others correctly.

Keep your existing code from previous lessons. This week's work builds on and modifies that code — it does not replace it.

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
   - Updates the todoList functionally ( because current state relies on previous state )
   - Adds our `newTodo` while spreading the `previous` todoList. It should look something like: `setTodoList(previous => [newTodo, ...previous])`

> [!NOTE]
> We use `Date.now()` to generate a unique ID for each todo. In a real application, you'd typically use a more robust ID generation method, but this works well for our purposes.

#### Pass the Handler to TodoForm

1. Add an `onAddTodo` prop to the `TodoForm` component instance in your App's return statement:

Use exactly as written — the prop name and function reference must match:

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

Use exactly as written — the console.log statements are temporary so you can visualize parts of your code's behavior in the console. These will be removed later in this assignment. The rest of the handler logic is required:

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

1. Update the correct elements in the return statement to use the handler and ref:

Use exactly as written — the name="todoTitle" attribute, ref, and onSubmit handler must match the code above. The placeholder text ("Todo text") is an example; use your own if you prefer:

```jsx
return (
  <form onSubmit={handleAddTodo}>
    <label htmlFor="todoTitle">Todo</label>
    <input
      ref={inputRef}
      type="text"
      id="todoTitle"
      name="todoTitle"
      placeholder={'Todo text'}
      required
    />
    <button type="submit">
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
2. Use exactly as written — your final handler should look like this:

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

### Closing Notes

**Important**: The event handling patterns you've learned this week are fundamental to building interactive React applications. Confirm you understand how events trigger state updates and how components communicate through props before moving on.

> [!NOTE]
> The AI review tool (known as AirHub) can check code and structure, but it does not run your code in a server environment to verify that aspect runs properly. We will have human reviewers checking this aspect, so you may receive a passing assignment from AirHub that could still need revisions after a human has checked that your work runs properly in the correct environment. If your AI and human reviewer feedbacks don't match, trust the human review.

---

<details>
<summary>Rubric (for AirHub reviewer and mentors)</summary>

### Required Deliverables/Tasks

- **Update Initial State (Part 1)** — In App.jsx, the `useState` initial value is changed from the hardcoded array to an empty array `useState([])`. The previously hardcoded `todos` array is deleted.
- **addTodo Function (Part 2)** — A function named `addTodo` in App.jsx, above the return statement, that takes a `todoTitle` parameter, creates a new todo object with `id` set to `Date.now()` and `title` set to the parameter, and uses a functional state update: `setTodoList(previous => [newTodo, ...previous])`. Use exactly as written: the function name `addTodo` is passed as a prop and must match. Example — adapt to your own layout: the local variable names inside the function (e.g., `newTodo`, `previous`) are flexible.
- **Pass Handler to TodoForm (Part 2)** — The `addTodo` function is passed to `TodoForm` via an `onAddTodo` prop: `<TodoForm onAddTodo={addTodo} />`. Use exactly as written: the prop name `onAddTodo` must match the destructured parameter in TodoForm.
- **useRef Import and Setup (Part 3)** — `useRef` imported from React in TodoForm.jsx. A ref created with `const inputRef = useRef()` inside the component.
- **handleAddTodo Handler (Part 3)** — A form submit handler named `handleAddTodo` in TodoForm that: calls `event.preventDefault()`, reads the input value via `event.target.todoTitle.value`, trims whitespace and rejects empty input, calls `onAddTodo(todoTitle)`, resets the form with `event.target.reset()`, and refocuses the input with `inputRef.current.focus()`. Use exactly as written: the handler name `handleAddTodo` is referenced in the form's `onSubmit`.
- **Updated Form JSX (Part 3)** — The form element uses `onSubmit={handleAddTodo}`. The input has `ref={inputRef}`, `name="todoTitle"`, `id="todoTitle"`, and `required`. The button changes from `disabled` to `type="submit"`. Use exactly as written: `name="todoTitle"` is required — the handler accesses the value through this name via `event.target.todoTitle.value`. Example — adapt to your own layout: the `placeholder` text is the student's choice.
- **Console.log Cleanup (Part 4)** — The three temporary `console.log` statements are removed from the final `handleAddTodo` function. The redundant `&& todoTitle !== ""` check is simplified to just `if (todoTitle)`.
- **Functional Verification (Part 5)** — The app loads with an empty todo list, todos can be added via the form, they appear immediately, the input retains focus after submission, empty/whitespace submissions are rejected, and both the button and Enter key work. Note: AirHub cannot run the dev server to verify runtime behavior; it is confirmed by human reviewers.
- **Checkpoint: Check Your Understanding with AI (Part 6)** — This is an ungraded learning activity. The prompts are reflective exercises that produce no code artifact. Do not assess these; they cannot be verified from submitted code.
- **Version Control and Submission (Part 6)** — Changes committed to the working branch, pushed to GitHub, and a PR created comparing the working branch to `main`.

### Optional Deliverables/Tasks

**None.** All tasks in this assignment are required.

</details>
