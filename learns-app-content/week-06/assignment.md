<!-- h1, h2 already used by CTD Learns -->
### Expected App Capabilities

After completing this week's assignment, your app should:

- be organized by feature and shared components
- use the same label and input on new todo form and edit todo
- allow the user to edit their todo's title

### Re-organize Project

*While running the Vite development server, these changes will throw errors about non-existent files until all of the import statements are corrected.*

![error fail to load file](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v4/refs/heads/main/learns-app-content/week-06/assets/does-file-exist.png)

- Create two new directories in `src`: `features` and `shared`
- Inside of `src/features`, create a new directory `TodoList`
- Move `TodoForm.jsx` into `src/features` and update the import statement in `App.jsx`
- Move `TodoList.jsx` and `TodoListItem.jsx` into `src/features/TodoList`. Update the import statement in `App.jsx`
- The project should have no errors at this point.
- Add all changes to the stage and then commit to the current working branch.

*Committing periodically allows us to set up a safe recovery point that we can get back to without undoing all changes to the working branch.*

#### Refactor Input and Label into a Re-usable Component

You will be refactoring out the label and the input from the `TodoForm` so that we can re-use it to allow users to edit existing todos.

- Inside of `src/shared` create a new file, `TextInputWithLabel.jsx`
- Create a component `TextInputWithLabel`
  - Add a label and an input that are wrapped in a React fragment to the return statement. Don't worry about props for now.
  - Make sure that the component is exported as a default export.

##### Determining TextInputWithLabel Props

We next have to determine which props we have to pass into the new component. In the TodoForm label and input, you'll see:

for the label:

- `htmlFor`

for the input:

- `type`
- `id`
- `ref`
- `onChange`

We can reduce this list of props by combining `htmlFor` and `id` since they used together to associate a label with its input. We will call the new props `elementId`. We also do not need to pass in `type` since the component will only be for text inputs. This leaves us with `elementId`, `ref`, `onChange`. We will also want to dynamically update our label's text (call it `labelText`) and include the todo's title(call it `value`). With this list of five props identified, we can update the `TextInputWithLabel` component.

- Destructure the props `elementId`, `labelText`, `onChange`, `ref`, and `value` from the props argument in `TextInputWithLabel`'s function definition.
- Assign props to the matching label and input props. You will end up with a component that looks like:

```jsx
{/*excerpt from TextInputWithLabel.jsx*/}
function TextInputWithLabel({
  elementId,
  labelText,
  onChange,
  ref,
  value,
}) {
  return (
    <>
      <label htmlFor={elementId}>{labelText}</label>
      <input
        type="text"
        id={elementId}
        ref={ref}
        value={value}
        onChange={onChange}
      />
    </>
  );
}

export default TextInputWithLabel;
```

#### Refactor TodoForm to use TextInputWithLabel

- Import `TextInputWithLabel` into `TodoForm.jsx`.
- Find the existing label then add an instance of `TextInputWithLabel` above it.
- Move the `ref`, `value`, and `onChange` props from the input to the component instance.
- Add an `elementId` prop and set it to the same value as the input's `id`.
- Add a `labelText` prop and set it to the string "Todo".
- The label and input elements are not needed anymore so remove them.

The form should behave the same as it did previously but you can see that there's a new component in the component tree:

![TextInputWithLabel in react dev tools](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v4/refs/heads/main/learns-app-content/week-06/assets/text-input-with-label-dev-tools.png)

### Allow Users to Edit Existing Todos

#### Toggle Between Todo Display and Edit

In the `TodoListItem` component:

- Add a state variable `isEditing` and its associated state update function. Use an `initialValue` of `false`;
- Import `TextInputWithLabel` component.
- In the return statement, create a ternary statement that evaluates `isEditing`
  - If true, display an instance of `TextInputWithLabel` with its props `value` set to `todo.title`. We will update all props later.
  - If false, display the existing form and its contents. The form will contain the checkbox input and the todo title.
- Surround the `{todo.title}` with a span element.
- Add an `onClick` handler to the `span` that toggles the `isEditing` state value to `true`.

Your list item should resemble:

```jsx
{/*excerpt from TodoListItem.jsx*/}
{/*...code*/}
<li>
    <form>
        {isEditing ? (
            <TextInputWithLabel value={todo.title}/>
        ) : (
            <>
                <label>
                    <input
                        type="checkbox"
                        id={`checkbox${todo.id}`}
                        checked={todo.isCompleted}
                        onChange={() => onCompleteTodo(todo.id)}
                    />
                </label>
                <span onClick={() => setIsEditing(true)}>{todo.title}</span>
            </>
        )}
    </form>
</li>
{/*code continues...*/}
```

At this point, when you click on the todo in the list, it will toggle between being displayed as text and showing in a form input:

![editing a todo](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v4/refs/heads/main/learns-app-content/week-06/assets/edit-todo.gif)

#### Extract Form Validation to Helper Function

Before we continue building the edit functionality, let's refactor the validation logic in `TodoForm` into a reusable helper function. Currently, the form validation is embedded directly in the JSX as `disabled={workingTodo === ''}`. As our application grows, we may need this same validation logic in other places, so extracting it into a helper function will make it easier to maintain and reuse.

- Create a new directory `src/utils` if it doesn't already exist
- Inside `src/utils`, create a new file called `todoValidation.js`
- Create and export a function called `isValidTodoTitle` that:
  - takes a `title` parameter
  - returns `true` if the title is not empty after trimming whitespace
  - returns `false` if the title is empty or only contains whitespace

```javascript
// src/utils/todoValidation.js
export function isValidTodoTitle(title) {
  return title.trim() !== '';
}
```

- In `TodoForm.jsx`, import the helper function:

```javascript
import { isValidTodoTitle } from '../utils/todoValidation';
```

- Update the button's disabled attribute to use the helper function:

```jsx
<button disabled={!isValidTodoTitle(workingTodo)}>Add Todo</button>
```

The form should behave exactly the same as before, but now the validation logic is centralized and can be reused throughout the application. This helper function demonstrates how pure JavaScript functions can be extracted from React components to improve code organization and reusability.

- Add all changes to the stage and commit to the current working branch.

#### Continue Building Edit Functionality

Now let's continue with the todo editing functionality.

- Create a new state variable, `workingTitle` and its associated state update function. Set the `initialValue` to `todo.title`
- Create a `handleCancel` event helper that:
  - resets the `workingTitle` to `todo.title`
  - sets `isEditing` state value to `false`
  - there is no need to include an argument
- Add a button below the `TextInputWithLabel` component:
  - Set its type to "button" so it does not act as a submit button when inside a form element.
  - Attach `handleCancel` to an `onClick` prop
  - Add the text "Cancel" in the button.

#### Create Local State and Controlled Form

- Create a `handleEdit` event helper that:
  - takes an `event` argument
  - uses the `event.target.value` to update the `workingTitle` state value
- Update `TextInputWithLabel`
  - `value` should now use `workingTitle`
  - add an `onChange` prop to `TextInputWithLabel` that takes `handleEdit`

You should now be able to make changes to the todo in the input. We cannot save quite yet though!

![including a cancel button on edit-able todo](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v4/refs/heads/main/learns-app-content/week-06/assets/edit-todo-cancel-button.gif)

#### Update Todo State from Local State

- In the `App` component, create an `updateTodo` function that:
  - takes an `editedTodo` argument.
  - maps through the todos, comparing each `todo.id` with the updated todo's id
    - if it matches - return a new object that destructures the `editedTodo`
    - if not a match - return the current todo.
  - saves the array returned by `map` to `updatedTodos`
  - updates the `todoList` state value with `updatedTodos`.
- Pass `updateTodo` to a new prop in `TodoList` instance and name it `onUpdateTodo`.
- In `TodoList`, destructure that helper out of the component's props then pass it to `TodoListItem` in the same manner.
- In `TodoListItem`:
  - destructure `onUpdateTodo` out of props.
  - Create a helper function, `handleUpdate` that
    - takes event object
    - if `isEditing` is false, return immediately to exit the function
    - calls `event.preventDefault()`
    - calls `onUpdateTodo` and pass in an object that destructures `todo` and sets the `title` equal to `workingTitle`.
    - sets `isEditing` state value to false.
- Add another button below the "Cancel" button:
  - Give it the text "Update"
  - Set its type to "button"
  - add a click handler that takes `handleUpdate`
  - reuse the validation helper we extracted from the Add Todo button earlier
- Add an `onSubmit` handler to the form element and pass it `handleUpdate`

![adding and completing multiple todos](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v4/refs/heads/main/learns-app-content/week-06/assets/add-todos.gif)

### Stretch Goal (Optional): Create Custom Hook

*Note: Custom hooks are an advanced React pattern! This stretch goal demonstrates how stateful logic can be extracted and reused. Even if you don't implement it, you're encouraged to read and understand this section.*

If you're feeling adventurous, you can extract the editing state management from `TodoListItem` into a reusable custom hook. This will allow us to share stateful logic between components as our application grows in complexity.

- Create a new file `src/hooks/useEditableTitle.js`
- Create a custom hook that manages the editing state:

```javascript
// src/hooks/useEditableTitle.js
import { useState } from 'react';

export function useEditableTitle(initialTitle) {
  const [isEditing, setIsEditing] = useState(false);
  const [workingTitle, setWorkingTitle] = useState(initialTitle);

  const startEditing = () => {
    setWorkingTitle(initialTitle);
    setIsEditing(true);
  };

  const cancelEdit = () => {
    setWorkingTitle(initialTitle);
    setIsEditing(false);
  };

  const updateTitle = (newTitle) => {
    setWorkingTitle(newTitle);
  };

  const finishEdit = () => {
    setIsEditing(false);
    return workingTitle;
  };

  return {
    isEditing,
    workingTitle,
    startEditing,
    cancelEdit,
    updateTitle,
    finishEdit
  };
}
```

- In `TodoListItem`, import and use the custom hook:

```jsx
// Replace the useState calls with:
const {
  isEditing,
  workingTitle,
  startEditing,
  cancelEdit,
  updateTitle,
  finishEdit
} = useEditableTitle(todo.title);

// Update the event handlers:
// handleEdit becomes: (event) => updateTitle(event.target.value)
// handleCancel becomes: cancelEdit
// handleUpdate becomes: (event) => {
//   if (!isEditing) return;
//   event.preventDefault();
//   const finalTitle = finishEdit();
//   onUpdateTodo({ ...todo, title: finalTitle });
// }
// setIsEditing(true) becomes: startEditing()
```

This pattern shows how custom hooks can encapsulate related state and logic, making components cleaner and enabling reuse across different components that need similar editing functionality.

### Closing Notes

At this point, we've created a solid MVP (minimum viable product). Next week, we are going to work with a Node/Express API to synchronize the todo list remotely so a user can save todos.
