<!-- h1, h2 already used by CTD Learns -->
### Expected App Capabilities

After completing this week's assignment, your app should:

- contain a list of todos managed in state
- render each todo in a TodoListItem component

### Implement useState

1. Move `const todoList` and its value out of the App component, placing it between the file imports and the component declaration.
2. Rename `todoList` to `todos` so it does not interfere our state when we set that up.
3. Keep the mapping statement in TodoList component. *This will generate errors until we are done working with props.*
4. In App, import `useState`.
5. Implement `useState` inside the top of the App component.
   1. Use array destructuring to access the state value (`todoList`) and its accompanying state update function (`setTodoList`).
   2. Set `useState`'s default value to `todos`.

### Update TodoList Props

1. In TodoList.jsx, add `props` to the TodoList declaration's argument.
2. Replace the `todoList.map` with `props.todoList.map` in the return statement.
3. In App component, add a `todoList` prop to the TodoList instance and pass in the state value, `todoList`.

This will look something like:  `<TodoList todoList={todoList} />`.

At this point, the app should be error-free. You may need to refresh your browser window to clear the console.

### Refactor TodoList

While the `todoList` can be accessed by using `props.todoList` it's a good practice to destructure props out of the props argument so that they're directly accessible.

1. Replace the `props` with `{todoList}` so now your component declaration looks like: `function TodoList({todoList})...`
2. Remove `props.` from the `props.todoList...` in the return body.

### Implement a TodoListItem Component

1. Create a new file, TodoListItem.jsx, in the src directory.
2. Declare a TodoListItem component
   1. Use object destructuring to access the props, `todo`.
   2. Copy the return value from the `map` statement in TodoList component to this component's return statement.
   3. Refactor the return statement so it is a list item containing `todo.title` and remove the key props.
3. Import TodoListItem into TodoList.jsx
4. Replace the list item instance in the `map`'s return statement with an instance of TodoListItem.
   1. Keep the `key` props here as it is still needed.
   2. Add a `todo` props which takes in `todo`.

### Instructions Part 4: Final Steps and Submission

#### Test Your Application

Before submitting, verify that your app works correctly:

- The page loads without console errors
- Todos are managed in state using `useState`
- Todo items are rendered through the `TodoList` component using props
- Each todo is rendered by a `TodoListItem` component

#### Checkpoint: Check Your Understanding with AI

Choose 1–2 prompts below. Explain in your own words first, then ask AI for feedback.

> [!NOTE]
> Do not ask AI to complete the assignment code for you.

> - "I moved my todos into state with `useState`. Here is my understanding of why React state updates trigger re-renders while normal variable changes do not: [my explanation]. What did I get right, and what should I refine?"
> - "I updated `TodoList` to receive data through props. Here is my explanation of how data flows from `App` to `TodoList`: [my explanation]. Can you check my reasoning and point out anything inaccurate?"
> - "I refactored `function TodoList(props)` to `function TodoList({ todoList })`. Here is my explanation of what destructuring changes (and what it does not): [my explanation]. Is this accurate?"
> - "I created `TodoListItem` and now map returns component instances with a `key`. Here is my understanding of why `key` stays in the map and is not used inside `TodoListItem`: [my explanation]. What should I improve?"

#### Version Control and Submission

- Commit your changes to your local working branch.
- Push the branch to GitHub.
- In GitHub, create a PR (pull request) that compares your working branch to `main`.
- Copy the PR link and submit assignment.

### What You Accomplished This Week

Congratulations! You've successfully:

- ✅ Implemented React state management using the `useState` hook
- ✅ Learned to manage data flow between parent and child components using props
- ✅ Practiced props destructuring for cleaner, more readable code
- ✅ Created reusable components that accept and display dynamic data

> [!NOTE]
> **Important**: The concepts covered this — state management and props — are fundamental to all React development. Make sure you're comfortable with `useState`, props passing, and component communication before moving on to more advanced topics.

> [!NOTE]
> The AI review tool (known as AirHub) can check code and structure, but it does not run your code in a server environment to verify that aspect runs properly. We will have human reviewers checking this aspect, so you may receive a passing assignment from AirHub that could still need revisions after a human has checked that your work runs properly in the correct environment. If your AI and human reviewer feedbacks don't match, trust the human review.