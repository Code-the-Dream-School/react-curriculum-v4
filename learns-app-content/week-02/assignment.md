<!-- h1, h2 already used by CTD Learns -->
### Expected App Capabilities

After completing this week's assignment, your app should:

- Contain a `TodoList` component that displays all todo items
- Contain a `TodoForm` component with a form containing:
  - A labeled input field for entering new todos
  - A submit button (non-functional for now)
- Display the same visual elements as week 1: heading, todo form, and todo list
- Maintain all functionality from the previous week

### Instructions Part 1: Pre-Work Setup

> [!note]
> Make sure your week-01 assignment has been reviewed and approved before proceeding.

#### Version Control Preparation

- **Merge your previous week's work**: Go to GitHub and merge your week-01 PR into the `main` branch
- **Update your local environment**:

  ```terminal
  git checkout main
  git pull
  ```

- **Create a new working branch**:

  ```terminal
  git checkout -b week-02-components
  git push origin week-02-components
  ```

- **Start your development environment**:

  ```terminal
  npm run dev
  ```

- Open your browser and navigate to `http://localhost:5173` to verify your app is working

### Instructions Part 2: Creating the TodoList Component

> [!note]
> This week we'll learn to organize our code into separate components. Don't worry if some concepts seem unclear - components will be covered in detail in the lessons.

#### Create the TodoList Component File

- Create a new file named `TodoList.jsx` in the `src` directory
- Set up a basic React component structure with:
  - A function named `TodoList`
  - An empty return statement (we'll fill this in soon)
  - An export statement at the bottom

Your new `TodoList.jsx` file should look like this:

```jsx
function TodoList() {
  return (

  );
}

export default TodoList;
```

#### Integrate TodoList into App

- In `App.jsx`, import the new TodoList component at the top of the file:

```jsx
import TodoList from './TodoList.jsx';
```

- In the App component's return statement, add a `<TodoList />` component below the heading:

```jsx
return (
  <div>
    <h1>Todo List</h1>
    <TodoList />
  </div>
);
```

#### Move Todo Logic to TodoList Component

- **Move the todos array**: Cut the `todos` array from `App.jsx` and paste it into the `TodoList` component, above the return statement
- **Move the todo display logic**: Cut the `<ul>` element and everything inside it from `App.jsx` and paste it into the TodoList return statement

> [!note]
> After moving the todos array, you'll see errors in the browser console like the image below. This is normal and expected since we're in the middle of refactoring.

![screen capture of ReferenceError in browser console](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v4/refs/heads/main/learns-app-content/week-02/assets/reference-error.png)

The error occurs because we moved the `todos` array but haven't moved the code that uses it yet. Understanding these error messages helps you debug issues as you develop.

And your `App.jsx` should now look like this:

- **Test your changes**: Refresh your browser - the errors should be gone and your app should look exactly the same as before, but now it's better organized.

### Instructions Part 3: Creating the TodoForm Component

Now we'll create a form component that will eventually allow users to add new todos.

#### Create the TodoForm Component File

- Create a new file named `TodoForm.jsx` in the `src` directory
- Set up the basic component structure:

```jsx
function TodoForm() {
  return (
    
  );
}

export default TodoForm;
```

#### Build the Form Structure

Add a form with input elements inside the TodoForm component's return statement:

```jsx
function TodoForm() {
  return (
    <form>
      <label htmlFor="todoTitle">Todo</label>
      <input type="text" id="todoTitle" />
      <button type="submit">Add Todo</button>
    </form>
  );
}

export default TodoForm;
```

> [!note]
> Notice we use `htmlFor` instead of `for` in React. This is because `for` is a reserved word in JavaScript, so React uses `htmlFor` for the HTML `for` attribute.

#### Integrate TodoForm into App

- Import the TodoForm component in `App.jsx`:
- Add the TodoForm component between the heading and TodoList in your App's return statement.

#### Verify Your Results

Your app should now display:

1. The "Todo List" heading
2. A form with a text input and "Add Todo" button
3. The list of three todos from last week

The app should look approximately like this:

![screen capture of todo list with new todo form in the browser](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-02/todo-list-with-form.png)

> [!note]
> The form won't actually add todos yet - that functionality will be added in future weeks. For now, we're focusing on component structure and organization.

### Instructions Part 4: Final Steps and Submission

#### Test Your Application

Before submitting, verify that your app works correctly:

- The page loads without console errors
- You can see the heading, form, and todo list
- The form contains a properly labeled input field and submit button
- All three todos from week 1 are still displayed

#### Version Control Submission

- Commit changes to your local working branch.
- Push the changes up to GitHub.
- In GitHub, create a PR (pull request) that compares the working branch to `main`.
- Copy the PR link and submit assignment.

### What You Accomplished This Week

Congratulations! You've successfully:

- ✅ Organized your code into separate, reusable React components
- ✅ Created a `TodoList` component that handles todo display logic
- ✅ Built a `TodoForm` component with proper form structure and accessibility
- ✅ Learned to import and use components in other components
- ✅ Practiced debugging common React errors during development
- ✅ Maintained the same visual functionality while improving code organization

### Looking Ahead

In upcoming weeks, you'll learn to:

- Make the form actually add new todos
- Handle user interactions and events
- Manage component state and data flow
- And much more...

> [!note]
> **Important**: Future assignments will assume you understand the concepts covered this week. Make sure you're comfortable with component creation, importing/exporting, version control, and the basic development workflow before moving on.
