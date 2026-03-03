---
marp: true
theme: default
paginate: true
style: |
  section {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 28px;
  }
  h1 {
    color: #1a73e8;
  }
  h2 {
    color: #333;
  }
  code {
    background-color: #f0f0f0;
    padding: 2px 6px;
    border-radius: 4px;
  }
  pre code {
    font-size: 22px;
  }
  .columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
  }
---

# Using These Slides

These slides are written in **Marp** markdown. You can present them two ways:

**In VS Code:**
1. Install the **Marp for VS Code** extension
2. Open this `.md` file
3. Click the Marp preview icon (top right) or use `Marp: Open Preview`
4. To present: `Marp: Export Slide Deck` → HTML or PDF

**In the browser:**
1. Go to [marp.app](https://marp.app/)
2. Paste or upload this file
3. Click **Preview** to present

Feel free to edit these slides to fit your style!

---

# Intro to React & App Installation
## Week 1

Code the Dream · React Curriculum v4

---

# 🌡️ Warm-Up (5 min)

**Think back to building websites with plain HTML/CSS/JS.**

> *In the chat: What's one thing that felt repetitive or tedious? (e.g., updating the DOM, copying markup, managing state)*

We'll use your answers to motivate why React exists!

<!--
Speaker note: Common answers: "I kept rewriting the same HTML", "updating the page required so much code", "keeping the UI in sync with data was hard." These are exactly the problems React solves.
-->

---

# What is React?

React is a **JavaScript library** for building user interfaces.

- **Declarative** — describe *what* the UI should look like; React handles *how* to update the DOM
- **Component-based** — build small, reusable pieces that compose into a full app
- **Powered by a Virtual DOM** — React computes the minimal changes needed before touching the real DOM

> React doesn't replace HTML, CSS, or JS — it works alongside them.

---

# Imperative vs. Declarative

<div class="columns">
<div>

**Plain JS (Imperative)**
```js
const el = document.getElementById('title');
el.textContent = 'My Todos';
el.style.color = 'blue';
```
*You tell the browser every step.*

</div>
<div>

**React (Declarative)**
```jsx
function App() {
  return <h1>My Todos</h1>;
}
```
*You describe the result. React figures out the steps.*

</div>
</div>

<!--
Speaker note: This is the core mental shift. Students coming from jQuery or plain JS often try to "manually update the DOM" in React — this slide helps prevent that habit.
-->

---

# Creating a React App with Vite

Vite is our **build tool and dev server**. It's fast and modern.

```bash
# Inside your cloned (empty) repo:
npx create-vite@latest --template react .

# Install dependencies:
npm install

# Start the development server:
npm run dev
```

Then open: **http://localhost:5173**

> 📌 **Pause:** Did everyone get the dev server running? Any errors?

<!--
Speaker note: Common error — running the scaffold outside the repo folder. Remind students: cd into the repo first, then run the command. The `.` at the end means "scaffold into the current directory."
-->

---

# Project Structure

```
my-app/
├── public/          ← static assets (favicon, etc.)
├── src/
│   ├── App.jsx      ← your main component
│   ├── App.css
│   ├── main.jsx     ← entry point — mounts React to the DOM
│   └── index.css
├── index.html       ← the single HTML file
├── package.json     ← dependencies & scripts
└── vite.config.js   ← Vite configuration
```

> The files you'll edit most: `src/App.jsx` and files you create in `src/`.

---

# What Vite Does For You

- **Hot Module Replacement (HMR)** — changes appear in the browser *instantly*, no manual refresh
- **JSX transformation** — compiles your `.jsx` files into plain JavaScript
- **Dev server** — serves your app locally with fast feedback
- **Production builds** — `npm run build` bundles everything for deployment

> Vite is the engine running your app. You mostly don't need to configure it.

---

# 🤝 We Do: Explore the Project (10 min)

Let's look at the scaffolded code together.

1. Open `src/main.jsx` — what do you see?
2. Open `src/App.jsx` — what does it return?
3. Make a small change: edit the `<h1>` text
4. Watch HMR update the browser automatically

> **Discussion:** What files will we be working in most? What files should we leave alone?

<!--
Speaker note: The goal is to demystify the scaffolded code. Don't deep-dive into every file — just build familiarity. The HMR demo is always a hit; it builds confidence that the environment is working.
-->

---

# Cleaning Up the Template

Before building, we clear out the starter code:

1. Empty `App.css` (keep the file)
2. Empty `index.css` (keep the file)
3. Update `App.jsx`:

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

> Check the browser — no errors in the console?

---

# Adding Your First Todos

In `App.jsx`, add a `todoList` array and render it:

```jsx
function App() {
  const todoList = [
    { id: 1, title: 'Buy groceries' },
    { id: 2, title: 'Walk the dog' },
    { id: 3, title: 'Read a book' },
  ]

  return (
    <div>
      <h1>My Todos</h1>
      <ul>
        {todoList.map((todo) => (
          <li key={todo.id}>{todo.title}</li>
        ))}
      </ul>
    </div>
  )
}
```

<!--
Speaker note: Don't worry about explaining map, JSX, or key in depth here — those come in Lessons 02 and 03. Just help students get something rendering. The "key" prop warning will appear without it — that's a preview of Lesson 02.
-->

---

# 💪 You Do: Set Up Your Project (10 min)

If you haven't already:

1. Create a new **public GitHub repo** named `todo-list`
2. Clone it locally
3. Scaffold with Vite: `npx create-vite@latest --template react .`
4. Run `npm install` and `npm run dev`
5. Clean up the template and add 3 todos

*Timer: 10 minutes. Mentor is available for questions in chat.*

<!--
Speaker note: Let students work silently. Monitor chat for errors. Common issues: npm not found (Node not installed), port already in use, scaffold ran outside the repo.
-->

---

# Assignment This Week

**Goal:** A working Vite React app on GitHub with a todo list.

By the end of the assignment, your app should:
- Render a heading and a list of todos
- Be in a GitHub repo with a PR from `lesson-01-setup` → `main`
- Have a completed README

> **Reminder:** Always work on a **branch**, not directly on `main`.

---

# Wrap-Up (5 min)

**Check for understanding** — in the chat, answer one of these:

- What does `npm run dev` do?
- What is Vite's role in a React project?
- What's the difference between `public/` and `src/`?

**Coming up in Lesson 02:**
- Components: the building blocks of React UIs
- JSX: the HTML-like syntax inside JavaScript
- How to troubleshoot React errors

> Any questions before we close?
