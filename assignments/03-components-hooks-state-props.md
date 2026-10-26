As a reminder, each week you'll work on an exercise and an assignment.  Both have you building code, but they happen in two different repositories.  

Use this link to access your Exercise instructions for this lesson: [Lesson 3 Exercise](https://github.com/Code-the-Dream-School/react-curriculum-v4-exercises/blob/main/src/exercises/lesson-03/instructions.md)

Use this link to access your Assignment instructions for this lesson: [Lesson 3 Assignment](https://github.com/Code-the-Dream-School/react-curriculum-v4/blob/27_1/learns-app-content/lesson-03-components-hooks-state-props/03-components-hooks-state-props.md)

Be sure to follow the Assignment Submission form instructions closely so your two links are submitted in the proper order.

---

<details>
<summary>Rubric (for AirHub reviewer and mentors)</summary>

### Required Deliverables/Tasks

- **Implement useState** — The `todoList` array from prior lessons is moved outside the App component and renamed to `todos`. The App component imports `useState` and initializes state with `const [todoList, setTodoList] = useState(todos)`. Use exactly as written (each component relies on these exact names to connect to the others correctly): the state variable `todoList`, the updater `setTodoList`, and the initial data variable `todos` must use these exact names.
- **Update TodoList Props** — The TodoList component accepts `props` and uses `props.todoList.map(...)` to render todos. In App.jsx, `<TodoList todoList={todoList} />` passes the state value as a prop. Use exactly as written: the prop name `todoList` must match the destructured name used in TodoList.
- **Refactor TodoList** — The `props` parameter in the TodoList function signature is replaced with destructuring: `function TodoList({ todoList })`. The `props.` prefix is removed from the map call in the return body.
- **TodoListItem Component** — A new file `TodoListItem.jsx` in the `src` directory containing a `TodoListItem` component that accepts a destructured `todo` prop and renders a list item displaying `todo.title`. Use exactly as written (each component relies on these exact names to connect to the others correctly): the file name `TodoListItem.jsx`, function name `TodoListItem`, and prop name `todo` must match.
- **TodoListItem Integration** — `TodoListItem` imported in `TodoList.jsx`. The `.map()` return statement in TodoList uses `<TodoListItem key={todo.id} todo={todo} />` — the `key` prop remains on the map call (not inside TodoListItem).
- **Checkpoint: Check Your Understanding with AI** — This is an ungraded learning activity. The prompts are reflective exercises that produce no code artifact. Do not assess these; they cannot be verified from submitted code.
- **Version Control and Submission** — Changes committed to the working branch, pushed to GitHub, and a PR created comparing the working branch to `main`.
- **No Console Errors** — The assignment states the app should work without console errors after each section is completed. Note: AirHub cannot run the dev server to verify this; it is confirmed by human reviewers.

### Optional Deliverables/Tasks

**None.** All tasks in this assignment are required.

</details>
