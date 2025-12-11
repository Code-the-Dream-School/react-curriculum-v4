# Week 01 Exercise : About Me Page

This guide helps mentors review Week 01 submissions quickly and consistently.

---

## Functional Specifications

### User Requirements

A user should be able to see a student’s “About Me” information, including:

- A personal heading  
- An introductory paragraph  
- A list of hobbies/interests  

### Acceptance Criteria

- [ ] Student work is only in: `src/exercises/week-01/studentWork.jsx`  
- [ ] Page renders with **no errors**  
- [ ] Page displays:
  - [ ] A personal heading  
  - [ ] An introduction paragraph  
  - [ ] A list of hobbies/interests  
- [ ] Weekly folder includes a **screen capture** of the rendered page (demonstrates that student is working with development)

---

## Technical Specifications

Inside `studentWork.jsx`, the component should include:

- A `const` for the student’s **name**
- A `const` for the student’s **age**
- A `const` for **hobbies/interests** as an **array**
- Semantically appropriate JSX elements:
  - `h1` for the main heading  
  - `p` for the intro paragraph  
  - `ul`/`ol` + `li` for the hobbies list  
- Hobbies/interests rendered using **`.map()`**

No keys are required for the list items.

---

## Quality Checklist

- [ ] Component exports correctly (`export default ...`)  
- [ ] JSX is valid and returns a single root wrapper  
- [ ] No unnecessary complexity:
  - No hooks  
  - No props  
  - No styling required  
- [ ] No console errors or warnings  

---

## ❌ Common Issues to Watch For

- Variables created **outside** the component  
- Hobbies list **written manually** instead of mapped  
- Missing `export default` statement  
- Broken JSX (unclosed tags, missing return)  
- Work placed in the wrong directory or file name  

---

## References

- CTD React Curriculum Exercises Repo  
  `Code-the-Dream-School/react-curriculum-v4-exercises`  
- Week 01 Assignment  
  `react-curriculum-v4/learns-app-content/week-01/assignment.md`

