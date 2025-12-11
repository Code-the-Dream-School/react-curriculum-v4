# Week 03 Exercise : React Bug Hunt

This guide helps mentors review Week 03 submissions quickly and consistently.

---

## Functional Specifications

### User Requirements

A user should be able to view **three small components**, each originally containing a bug, and see each one behaving correctly after fixes:

- Bug #1 — Effect running on every render  
- Bug #2 — Mutated state preventing re-render  
- Bug #3 — Props not updating the UI  

Each component must:

- Render without errors  
- Behave correctly after the bug is fixed  
- Include a written explanation in the provided "Explanation" section  

### Acceptance Criteria

- [ ] All work is located in: `src/exercises/week-03/`  
- [ ] Student fixes all **three** bugs:
  - [ ] `BugEffectLoop.jsx`  
  - [ ] `BugMutatedState.jsx`  
  - [ ] `BugProps.jsx`
- [ ] Each component includes a completed **Explanation** section at the bottom  
- [ ] `studentWork.jsx` imports all three bug components and renders them together  
- [ ] Screenshot of the final rendered page is included in the week-03 folder  
- [ ] Page loads without errors or warnings  

---

## Technical Specifications

### Bug #1 — Effect Issue (`BugEffectLoop.jsx`)
- Student should update `useEffect` so it runs **only once**, on mount  
  - This means adding an empty dependency array `[]`  
- Behavior should no longer loop or repeatedly trigger  
- Explanation section should mention:
  - Why `useEffect` without dependencies runs on every render  
  - Why adding dependencies (or an empty array) fixes lifecycle behavior  

---

### Bug #2 — Mutated State (`BugMutatedState.jsx`)
- Student should correct the state update so React can detect and re-render  
- Fix typically involves:
  - Avoiding direct mutation (e.g., `count++`)  
  - Using functional updates or `setCount(count + 1)`  
- Explanation section should mention:
  - React's rule that **state must not be mutated directly**  
  - Why mutation prevents React from triggering a re-render  

---

### Bug #3 — Props Not Updating UI (`BugProps.jsx`)
- Component should not store prop values in regular variables  
- Student should use `useState` or rely directly on props  
- Updated value must trigger a re-render when a button is clicked  
- Explanation section should mention:
  - Why React **does not track regular variables** for UI updates  
  - Why state is required for changes to appear on screen  

---

## `studentWork.jsx` Requirements

- [ ] Imports all three bug components  
- [ ] Renders them inside the `<StudentWork />` component  
- [ ] No unnecessary logic beyond layout  

Example structure:
     ```jsx
export default function StudentWork() {
  return (
    <>
      <BugEffectLoop />
      <BugMutatedState />
      <BugProps />
    </>
  );
  }

--- 

## Quality Checklist

- [ ] All three bugs are fixed correctly  
- [ ] Each component exports correctly (`export default ...`)  
- [ ] Explanation sections are present, readable, and demonstrate understanding  
- [ ] JSX is valid and returns a single root wrapper  
- [ ] No console errors or warnings  
- [ ] No unnecessary complexity (solutions should stay focused on the bug fixes)  

---

## ❌ Common Issues to Watch For

- Missing dependency array in **Bug #1**  
- Direct state mutation in **Bug #2**  
- Updating plain variables instead of state in **Bug #3**  
- Props being copied into state unnecessarily  
- Missing or vague explanations  
- Components not imported or not rendered in `studentWork.jsx`  
- Missing screenshot in the Week 03 folder  

---

## References

- CTD React Curriculum Exercises Repo  
  `Code-the-Dream-School/react-curriculum-v4-exercises`  
- Week 03 Assignment  
  `react-curriculum-v4/learns-app-content/week-03/assignment.md`

