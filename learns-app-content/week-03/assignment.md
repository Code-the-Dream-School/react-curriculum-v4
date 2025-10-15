<!-- h1, h2 already used by CTD Learns -->
### Expected App Capabilities

After completing this week's assignment, your app should:

- contain a list of todos managed in state
- render each todo in a TodoListItem component

### Implement useState

1. Move `const todos` and its array from TodoList.jsx back to App.jsx, placing it between the file imports and the component declaration.
2. Keep the mapping statement in TodoList component. *This will generate errors until we are done working with props.*
3. In App, import `useState`.
4. Implement `useState` inside the top of the App component.
   1. Use array destructuring to access the state value (`todos`) and its accompanying state update function (`setTodos`).
   2. Set `useState`'s default value to `todos`.

### Update TodoList Props

1. In TodoList.jsx, add `props` to the TodoList declaration's argument.
2. Replace the `todos.map` with `props.todos.map` in the return statement.
3. In App component, add a `todos` prop to the TodoList instance and pass in the state value, `todos`.

This will look something like:  `<TodoList todos={todos} />`.

At this point, the app should be error-free. You may need to refresh your browser window to clear the console.

### Refactor TodoList

While the `todos` can be accessed by using `props.todos` it's a good practice to destructure props out of the props argument so that they're directly accessible.

1. Replace the `props` with `{todos}` so now your component declaration looks like: `function TodoList({todos})...`
2. Remove `props.` from the `props.todos...` in the return body.

### Implement a TodoListItem Component

1. Create a new file, TodoListItem.jsx, in the src directory.
2. Declare a TodoListItem component
   1. Use object destructuring to access the props, `todo`.
   2. Copy the return value from the `map` statement in TodoList component to this component's return statement.
   3. Refactor the return statement so it is a list item containing `todo.title` and remove the key props.
3. Import TodoListItem into TodoList.jsx
4. Replace the list item instance in the `map`'s return statement with an instance of TodoList.
   1. Keep the `key` props here as it is still needed.
   2. Add a `todo` props which takes in `todo`.

### What You Accomplished This Week

Congratulations! You've successfully:

- ✅ Implemented React state management using the `useState` hook
- ✅ Learned to manage data flow between parent and child components using props
- ✅ Practiced props destructuring for cleaner, more readable code
- ✅ Created reusable components that accept and display dynamic data

> [!note]
> **Important**: The concepts covered this week—state management and props—are fundamental to all React development. Make sure you're comfortable with `useState`, props passing, and component communication before moving on to more advanced topics.
