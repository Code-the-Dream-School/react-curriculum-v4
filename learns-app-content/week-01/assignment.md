<!-- h1, h2 already used by CTD Learns -->
<!-- draft exercise composition v1-->
### Expected App Capabilities

After completing this week's assignment, your app should:

- be in a version controlled directory linked to a GitHub repo
- use Vite's React template using JavaScript (no TS or SWC)
- start with no console errors or warnings
- render a title and an unordered list of todos

### Instructions Part 1: Repo Setup for the Todo App

#### Create new public repo on GitHub

- give it the name "todo-list" or something similar and description
- do not add a .gitignore or a license
- clone the repo to your local environment

### Instructions Part 2: Installation

#### Scaffold Vite Using CLI

>[!note]
>remain on `main` branch

- Bootstrap a new project: `npm create vite@latest . -- --template react`
- After any prompts, install the project dependencies using NPM: `npm install`

You will end up with a project structure that looks similar to the following:

![screen capture of the newly installed project directory](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v4/refs/heads/main/learns-app-content/week-01/assets/project-directory.png)

### Instructions Part 3: Project Setup

> [!note]
> Don't worry if you encounter unfamiliar concepts like JSX, components, or React syntax in this assignment. These topics will be covered in detail in upcoming lessons. For now, follow the instructions step by step.

> [!note]
>.
>
> - Some of the cleanup steps may cause errors to appear in the browser window and console but they will resolve themselves

#### Version Control Tasks

- Commit all file changes to `main` and push changes to GitHub.
- Create and check out a new branch, `week-01-setup`.
- Publish this branch to GitHub.

#### Clean up Template

- Start the development server with the command: `npm run dev`
- Open a browser and navigate to `http://localhost:5173`.
- Delete the contents of App.css but keep the file.
- Delete the contents of index.css but keep the file.
- Clean up App's code:
  - Remove all imports except for App.css
  - Delete the line containing `const [count, setCount] = useState(0)`
  - Remove everything from the return statement.
  - In the now empty return statement, add a div containing an h1 for the title for the app

Your App component should look like:

```jsx
import './App.css'

function App() {

  return (
    <div>
      <h1>My Todos</h1>
    </div>
  )
}

export default App
```

Refresh the page and you'll end up with something that looks like the screencap below. Note that the console contains no errors.

![screen capture of app title in browser](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v4/refs/heads/main/learns-app-content/week-01/assets/title-screencap.png)

#### Add First Todos

In App.jsx:

- Inside the App component, above the return statement, create an array named `todos` containing 3 empty objects.
- Populate each object with a todo using the following object keys: `id` and `title`.

```jsx
{/*extract from App.jsx*/}
function App() {
const todos = [
    {id: 1, title: "review resources"},
    {id: 2, title: "take notes"},
    {id: 3, title: "code out app"},
]
{/*code continues...*/}
```

- Create an unordered list using html tags below the heading (h1).
- Place an empty code block between the list's opening and closing tags `<ul>{}</ul>`
- Inside the code block, map the todos to html that will render a list item per todo.

```jsx
{/*extract from App.jsx*/}
{/*...code*/}
return (
    <div>
        <h1>Todo List</h1>
        <ul>
            {todos.map(todo => <li key={todo.id}>{todo.title}</li>)}
        </ul>
    </div>
  );
{/*code continues...*/}
```

In the browser, you should have a list of 3 todos under the app's title:

![screen capture of the todos in browser](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v4/refs/heads/main/learns-app-content/week-01/assets/todos-screencap.png)

### Instructions Part 4: Final Steps

#### Update the Project's README

> [!note]
> Write in with whatever language you are most comfortable. We suggest you also use [markdown](https://gist.github.com/Myndex/5140d6fe98519bb15c503c490e713233) for any formatting - it's a simple document markup syntax that can be learned very quickly.

- Open the README.md in the root of the project and empty its contents.
- Write some basics about the project. Include at least the following:
  - App name and description
  - Installation instructions
  - How to run the development server

#### Version Control Closeout Tasks

- Commit changes to your local working branch.
- Push the changes up to GitHub (publish the branch there if you have not done so already).
- In GitHub, create a PR (pull request) that compares the working branch to `main`.
- Copy the PR link and submit assignment.
