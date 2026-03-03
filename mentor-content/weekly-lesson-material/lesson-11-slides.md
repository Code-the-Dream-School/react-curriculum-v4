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

# Portfolio Polish, Security & Deployment
## Week 11

Code the Dream · React Curriculum v4

---

# 🎉 First: Congratulations!

You built a full-stack React application over 10 weeks.

Your app can:
- ✅ Manage todos (add, edit, complete)
- 🔐 Authenticate users
- 🌐 Sync data with a real API
- 🗺️ Navigate between pages
- ⚡ Optimize with advanced hooks

This week: **make it something you're proud to show off.**

<!--
Speaker note: Take a genuine moment to celebrate. Many students don't realize how much they've built. Enumerate the specific skills — this is the kind of app that's actually impressive on a portfolio.
-->

---

# 🌡️ Warm-Up (5 min)

> *In the chat: If a recruiter looked at your GitHub profile right now, what would they see? What would you want them to see instead?*

This week, we close that gap.

<!--
Speaker note: This is intentionally a little uncomfortable. Students often have repos with no README, placeholder text, console.logs everywhere, or half-finished code. The goal is to motivate polishing.
-->

---

# What Makes an App Portfolio-Ready?

**Code quality**
- No `console.log` statements
- No commented-out dead code
- No unused imports or variables
- Consistent formatting (run Prettier)

**README** (the most-read file in any repo)
- What it does + live demo link
- Screenshots
- How to install and run it
- Tech stack

**Functionality**
- All features work (test them!)
- Handles empty states, errors, and loading

---

# Styling: Choose Your Approach

| Option | Good for |
|--------|---------|
| Regular CSS | Keeping things simple |
| CSS Modules | Scoped styles, no class name conflicts |
| Styled-components | CSS-in-JS with dynamic styles |
| Tailwind CSS | Utility-first, fast to prototype |

> **Tip:** Pick one and use it consistently. A simple, clean design beats a fancy broken one.

**Visual Polish Checklist:**
- Consistent color scheme and typography
- Responsive on mobile and desktop
- Visible hover and focus states on interactive elements

---

# Security: XSS and Input Validation

**Cross-Site Scripting (XSS)** — an attacker injects malicious scripts into your UI.

React's JSX automatically escapes output — so this is safe:
```jsx
<p>{userInput}</p>   // ✅ React escapes this
```

**Danger:** using `dangerouslySetInnerHTML` with unsanitized content:
```jsx
<div dangerouslySetInnerHTML={{ __html: userInput }} />  // ❌ Don't do this
```

If you must render HTML: use **DOMPurify**:
```js
import DOMPurify from 'dompurify'
const safe = DOMPurify.sanitize(userInput, { ALLOWED_TAGS: [] })
```

---

# Security: Environment Variables

API keys and secrets should **never** be in your code:

```bash
# .env.local (never commit this file)
VITE_BASE_URL=https://your-api.onrender.com
```

```js
// In your code — uses the env variable
const baseUrl = import.meta.env.VITE_BASE_URL
```

> ⚠️ **Important:** All `VITE_` variables are **visible to users** in the browser bundle. Never put secret API keys (that should only exist on a server) in a Vite app.

---

# Security: Input Validation

Always validate and sanitize user input:

```js
import DOMPurify from 'dompurify'

function isValidTodoTitle(title) {
  const sanitized = DOMPurify.sanitize(title.trim(), {
    ALLOWED_TAGS: [],
    ALLOWED_ATTR: [],
  })
  return sanitized.length > 0 && sanitized.length <= 200
}
```

- **Client-side validation** improves UX (fast feedback)
- **Server-side validation** is the real security layer (always required)
- Both are needed — one without the other is incomplete

---

# Deploying to Vercel

1. Run `npm run build` locally — fix any build errors
2. Run `npm run preview` — verify the production build works
3. Push all changes to `main` on GitHub
4. Go to [vercel.com](https://vercel.com) → sign in with GitHub
5. Import your `todo-list` repository
6. Add environment variables (your `VITE_BASE_URL`) in Vercel dashboard
7. Deploy — Vercel auto-detects Vite and runs `npm run build`

Your live URL: `https://your-project.vercel.app`

---

# Writing a Great README

```markdown
# Todo List App

A full-stack todo app built with React, featuring user authentication,
real-time API sync, and multi-page navigation.

**[Live Demo](https://your-project.vercel.app)**

## Features
- Add, edit, and complete todos
- User authentication
- Sort and filter todos
- Deployed on Vercel

## Tech Stack
React · Vite · React Router · React Context

## Getting Started
npm install && npm run dev
```

---

# 🤝 We Do: README Review (10 min)

Share your screen (or paste your README in chat) and let's review it together.

**Checklist:**
- [ ] Does it have a live demo link?
- [ ] Does it explain what the app does?
- [ ] Does it list the tech stack?
- [ ] Does it have install/run instructions?
- [ ] Is there at least one screenshot?

> *Mentor: do this as a group review — look at 1–2 student READMEs and give specific feedback.*

<!--
Speaker note: Be encouraging but specific. "Add a screenshot" is more helpful than "make it better." If time allows, show a strong README from an open source project for comparison.
-->

---

# 💪 You Do: Polish Plan (10 min)

Take 10 minutes to make a personal polish list:

1. **Code cleanup:** What `console.log`s or commented code needs to go?
2. **Functionality gaps:** Any features that don't quite work yet?
3. **Styling:** What visual improvement would have the biggest impact?
4. **README:** What's missing?
5. **Deployment:** Will you deploy? To Vercel or another platform?

Write your list and share one item in chat.

*Start your timer. This is your personal action plan!*

<!--
Speaker note: Let students work silently for the first 5 minutes, then invite sharing. Validate all plans — even small improvements count. Remind them: a clean, working simple app beats a complex broken one every time.
-->

---

# What's Next After This Course?

You've built a solid React foundation. Where to go from here:

**Deepen React**
- TypeScript with React
- Testing with Jest + React Testing Library

**Full-Stack React Frameworks**
- Next.js or Remix (server-side rendering, file-based routing)

**State Management**
- Redux Toolkit, Zustand, or Jotai for larger apps

**Backend (Code the Dream's next step!)**
- Node.js & Express — your React knowledge transfers directly

---

# Wrap-Up: Course Reflection (5 min)

**In the chat — share one of these:**

- What's the most useful concept you learned in this course?
- What are you most proud of building?
- What React topic do you want to explore more?

**Final reminder:**
- Assignment is optional but highly encouraged
- Deploy your app — having a live URL matters for portfolios
- Keep building! The best next step is a personal project

> It's been a pleasure working with you. Go build something great! 🚀
