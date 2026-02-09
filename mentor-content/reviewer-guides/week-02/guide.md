# Lesson-02 Exercises : Snack Ranking App

This guide helps mentors review Lesson-02 submissions quickly and consistently.

---

## Functional Specifications

### User Requirements

A user should be able to view a "Snack Ranking App" that displays:

- A heading for the app  
- A sorted list of snacks ranked from favorite (#1) downward  
- A footer message  

### Acceptance Criteria

- [ ] Student created each required component file inside:  
  `src/exercises/lesson-02/`
- [ ] App renders **without errors**  
- [ ] Page displays:
  - [ ] Heading from `SnackHeader`  
  - [ ] Sorted snack list from `SnackList`  
  - [ ] Footer message from `SnackFooter`  
- [ ] `studentWork.jsx` renders `<SnackApp />`  
- [ ] Screenshot of the working app is included in the lesson-02 folder  

---

## Technical Specifications

Students should create **four components**, each in its own file:

### 1. `SnackHeader.jsx`

- Default export  
- Returns a simple heading element  
- JSX should be clean and semantically appropriate (`h1`, `header`, etc.)

### 2. `SnackList.jsx`

- Default export  
- Contains an **array of snack objects** inside the component  
  - Each snack must include `name` and `rank`  
  - Rank `1` = favorite  
- Array should **begin in least–favorite → most–favorite order**  
- Must use JavaScript’s **`.sort()`** to produce a new array sorted:
  - **Ascending by rank** (1 → 2 → 3 → …)
- Render the list using:
  - An ordered list **OR**  
  - `.map()` inside JSX  
- Should include **3–5+ snacks**

### 3. `SnackFooter.jsx`

- Default export  
- Returns a simple footer message

### 4. `SnackApp.jsx` (or similarly named app wrapper)

- Imports the three components  
- Assembles them into one return statement  

### 5. `studentWork.jsx`

- Imports `SnackApp`  
- Renders:  

  ```jsx
  export default function StudentWork() {
    return <SnackApp />;
  }

---

## Quality Checklist

- [ ] All components export correctly (`export default ...`)  
- [ ] JSX is valid and returns a single root wrapper  
- [ ] `.sort()` is used correctly to rank snacks from favorite (#1) to least  
- [ ] `.map()` is used when rendering snack items (unless an `<ol>` is used properly)  
- [ ] No unnecessary complexity:
  - No hooks  
  - No styling required  
  - No props  
- [ ] No console errors or warnings  

---

## ❌ Common Issues to Watch For

- Snack array declared **outside** the component  
- Forgetting to use `.sort()` (resulting in an unsorted list)  
- Mutating the original snacks array instead of returning a **new** sorted array  
- Missing imports or incorrect file paths  
- Component not exported as default  
- Manual `<li>` elements instead of using `.map()`  
- Missing screenshot in the Lesson-02 folder  

---

## References

- CTD React Curriculum Exercises Repo  
  `Code-the-Dream-School/react-curriculum-v4-exercises`  
- Lesson-02 Assignment  
  `react-curriculum-v4/learns-app-content/lesson-02/assignment.md`
