# Copilot Agent Instructions

## Repository Summary

This repository contains curriculum materials for a beginner React course, designed for use in Code the Dream's custom LMS (CTD Learns). It provides lesson materials, assignments, and mentor resources for an 11-week course covering React fundamentals through advanced topics. The repository also includes archived materials from the previous curriculum version (v3) for reference and comparison purposes.

## High-Level Repository Information

- **Size:** Large (hundreds of directories, thousands of files; mostly markdown and supporting assets)
- **Current Curriculum (v4) - 11 weeks:**
  - **Lesson 00:** Course Orientation and Local Environment Setup
  - **Lesson 01:** Intro to React, App Installation, and Working with Vite
  - **Lesson 02:** ReactDOM, Components, JSX, and Troubleshooting
  - **Lesson 03:** Component Lifecycle, Basic Hooks, State, and Props
  - **Lesson 04:** Hooks Continued, Events and Handlers
  - **Lesson 05:** Local State and Controlled Components, and Forms
  - **Lesson 06:** Re-usable Components, Project Organization, and Refactoring
  - **Lesson 07:** Data Fetching and UI Update Strategies
  - **Lesson 08:** Advanced Hooks: useCallback and useMemo, Optimizing a React App, and API-based Sort/Search
  - **Lesson 09:** Advanced State, useReducer, and useContext
  - **Lesson 10:** React Router
  - **Lesson 11:** Polishing an App for Your Portfolio, App and Data Security, and Deploying a React App
  - See `draft-artifacts/curriculum-design-notes/outline.md` for the full lesson-by-week outline
  - **Directory naming note:** lesson directories are sluggified (e.g., `lesson-01-intro-to-react-app-installation-vite`, not `lesson-01`)
- **Previous Curriculum (v3) - 15 weeks:** Archived in `references/v3/` for comparison and reference
  - Extended course structure covering similar topics over 15 weeks instead of 11
  - Includes additional weeks for styling, final project development, and React ecosystem overview
  - Contains 246 archived files including lessons, assignments, objectives, and references

## Major Architectural Elements

- **Key Directories:**
  - `learns-app-content/` — Current curriculum materials for CTD Learns LMS (v4)
    - `lesson-00` and `lesson-01-...` through `lesson-11-...` — Weekly content (sluggified lesson directories)
    - `class-specific/`, `reusable-content/` — Shared and class-specific resources
  - `mentor-content/` — Instructor and reviewer resources
  - `references/` — Reference materials and archived curriculum
    - `Instructional Design Handbook.md` — Curriculum design guidelines and best practices
    - `Revised Blooms Taxonomy Action Verbs.pdf` — Educational taxonomy reference
    - `v3/` — Complete archived curriculum from previous version (246 files)
      - `assignments/` — Week 0-15 assignment materials
      - `lessons/` — Week 0-15 lesson content
      - `objectives/` — Week 0-15 learning objectives
      - `references/` — Week 0-15 reference materials
      - `reusable-content/` — Shared v3 materials and curriculum outline
  - `supplementals/` — Additional tooling and setup guides
    - `eslint/`, `prettier/` — Code formatting and linting guides
  - `docs/` — Additional documentation
  - `draft-artifacts/` — Design notes and outlines
  - `.github/` — Issue templates and agent instructions
- **Configuration Files:**
  - `.markdownlint.json` — Markdown linting rules
  - `.prettierrc`, `.prettierignore` — Formatting preferences
  - `.vscode/` — VS Code workspace settings
- **Licensing and Conduct:**
  - `LICENSE` — MIT License
  - `CODE_OF_CONDUCT.md` — Contributor Covenant
  - `CONTRIBUTING.md` — Contribution guidelines

## Agent Workflow and Efficiency

- **Trust these instructions:** All major repo structure, curriculum outline, and config file locations are documented above. Only search if information is missing or appears incorrect.
- **For curriculum changes:** Edit markdown files in `learns-app-content/` for lesson content, or `mentor-content/` for instructor resources.
- **For configuration changes:** Edit `.markdownlint.json` or `.prettierrc` in the repo root.
- **For documentation:** See `README.md`, `CONTRIBUTING.md`, and `docs/`.
- **For repo setup or build:** No build steps or scripts are present; repo is content-only.
- **For internal notes:** Use markdown comments `<!-- note content -->` for any internal notes that should not display in rendered content.
- **For errors or workarounds:** Document any encountered errors and steps taken to resolve them in your output.
- **Do not waste time searching for code, build, or test scripts unless new files are added.**

## AI Usage Prompts for Students (Assignments and Exercises)

When updating lesson materials, add short AI-usage prompts that align with CTD's AI policy and encourage active learning. These prompts should guide students to think first, explain reasoning, and use AI for feedback, hints, and clarification rather than answers.

### Core Principles

- **Active learning over passive prompting:** Students should explain their own reasoning, then ask for feedback.
- **AI as a teaching assistant, not an answer key:** Encourage hints, debugging help, and concept checks.
- **Validation:** Remind students to verify AI guidance with official docs and testing.
- **No AI during technical assessments:** Coderbyte and other assessments prohibit AI use.

### Prompt Types to Include

**Retrieval Practice (Explain in your own words):**

- Ask students to explain a concept in their own words, then ask AI to critique and refine.
- Example prompt: "I just learned about [concept]. Here is my understanding: [your explanation]. What did I get right, and what should I refine?"

**Predict-then-Check:**

- Ask students to predict output or behavior before running code, explain why, then verify.
- Example prompt: "Looking at this code: [paste code]. I predict it will [prediction] because [reasoning]. Am I correct? If not, what am I misunderstanding?"

**Scaffold Removal (Hints only):**

- Ask AI for high-level hints or questions rather than solutions.
- Example prompt: "I am stuck on Task 3. Can you give me 3 high-level hints without giving the answer?"
- Example prompt: "I am seeing this error: [error]. Ask me 3 questions to help me solve it on my own."

### Policy Reminders to Embed Where Relevant

- **Intro-level guidance:** Do not use AI to write code for you. Use it to clarify concepts, explain your own understanding, or get hints.
- **Advanced-level guidance (React/Node/Python):** If a student copies AI-generated code, they must add a code comment and include AI attribution in the submission form, explaining what was used and how it was modified.
- **Cross-checking:** Encourage students to compare AI guidance with official docs and reputable sources.
- **Discernment:** Remind students AI can be wrong or outdated; they must run and test any AI-assisted code.

### Additional Encouraged Uses

- **Time management prompts:** Help students break work into smaller tasks or plan catch-up schedules.
- **Translation prompts:** Allow students to request translations or simplified explanations in their native language.

## CTD Swag Demo Application Reference

When building code examples for weekly discussions, **ALWAYS** reference the comprehensive CTD Swag documentation in `references/demos-context/CTD-Swag/`:

### Available Reference Materials

- **COMPONENTS.md** - Complete list of 21 React components with architecture details
- **HELPER-FUNCTIONS.md** - Complete list of 25+ helper functions organized by purpose
- **CUSTOM-HOOKS.md** - Analysis of React hooks usage (Note: No custom hooks currently implemented)

### Code Example Guidelines

**Use Authentic CTD Swag References When:**

- Demonstrating component extraction patterns (Cart/CartItem, ProductList/ProductCard)
- Showing helper function examples (getCartPrice, sortByBaseName, filterByQuery)
- Illustrating component organization (features/, layout/, pages/, shared/)
- Teaching refactoring concepts with real-world context

**Label Hypothetical Examples When:**

- Creating examples not currently in CTD Swag
- Demonstrating concepts beyond the current application scope
- Introducing new patterns for educational purposes

**Required Labeling for Hypothetical Examples:**

```jsx
// Hypothetical example - not currently implemented in CTD Swag
function useShoppingCart() {
  // Custom hook logic...
}
```

### Architectural Consistency

**Follow CTD Swag Patterns:**

- Event handlers: `handle*` prefix (handleAddItemToCart, handleAuthenticate)
- Calculations: `get*` prefix (getCartPrice, getTotal)
- Component organization: Feature-based directory structure
- Props naming: Consistent with existing patterns (baseName, itemCount)

**Built-in Hooks Usage (No Custom Hooks):**

- useState (8 instances in App.jsx, multiple across components)
- useReducer (cart state management)
- useEffect (lifecycle and side effects)
- useCallback (memoization in App.jsx)
- useRef (Footer component year reference)
- useParams, useNavigate (React Router)

**Educational Value:**

- Maintains consistency with established demo application
- Provides realistic, implementable examples
- Builds upon student's existing knowledge base
- Shows authentic component extraction scenarios

## When Performing a Code Review

- You are a React curriculum writer who is experienced in web development and React.
- Review changes for clarity, correctness, and adherence to curriculum structure and formatting guidelines.
- Identify gaps in the materials that that would be helpful for junior developers to know.
- Ensure edits do not break markdown formatting, links, or lesson navigation.
- The repo does not use TypeScript or TSX.
- Check for consistency with the curriculum outline and weekly topic objectives.
- Flag any missing, unclear, or out-of-scope changes for further review.
- Do not suggest code or build/test commands unless new scripts or automation are added to the repo.
- Always document any errors found and steps taken to resolve or report them.

## Technical Validation Section

### React Code Examples Validation

- **JSX Syntax**: Verify all JSX code is syntactically correct and follows React conventions
- **Component Structure**: Ensure proper component definition, export statements, and naming conventions
- **Props Consistency**: Check that props passed match props received across all component examples
  - Verify prop names are consistent (e.g., `baseName` vs `name`)
  - Ensure all required props are passed and documented
  - Check for prop destructuring alignment
- **Hook Usage**: Verify hooks follow Rules of Hooks (top-level, function components only)
- **Component Naming**: Ensure consistent naming across files and references

### Cross-Reference Validation

- **Component Names**: Check that component names match between file names, function names, and imports
- **File References**: Verify all file paths and component references are accurate
- **Code Continuity**: Ensure code examples build logically from previous examples

## Review Checklists

### Assignment Review Checklist

- [ ] **Instructions are clear and sequential**: Each step builds logically on the previous
- [ ] **Code examples work**: All provided code can be executed without errors
- [ ] **Props match**: Props passed to components match what components expect
- [ ] **File structure is consistent**: Component files, imports, and exports align
- [ ] **Difficulty appropriate**: Assignment complexity matches week's learning objectives
- [ ] **Prerequisites clear**: Required knowledge from previous weeks is identified
- [ ] **Expected outcomes defined**: Clear description of what the final result should look like

### Discussion Content Review Checklist

- [ ] **Concepts introduced progressively**: New ideas build on established knowledge
- [ ] **Code examples are appropriate**: Examples include necessary imports when part of discussion; complex logic may use descriptive placeholders; code marked as "extract" or "snippet" may be incomplete
- [ ] **Technical accuracy**: All React patterns and syntax are correct
- [ ] **Terminology consistent**: React terms used consistently throughout
- [ ] **Visual aids relevant**: Screenshots and diagrams support the text content
- [ ] **Cross-references accurate**: Links to other lessons and resources work

### Exercise Review Checklist

- [ ] **Clear learning objectives**: What students should accomplish is explicit
- [ ] **Appropriate scaffolding**: Sufficient guidance without giving away solutions
- [ ] **Error handling**: Common mistakes are addressed or prevented
- [ ] **Testing guidance**: How students can verify their work is explained
- [ ] **Extension opportunities**: Optional challenges for advanced students

### Code Example Validation Checklist

- [ ] **Syntax correctness**: Code follows JavaScript/React syntax rules
- [ ] **CTD Swag consistency**: Examples reference authentic components and functions from references/demos-context/CTD-Swag/
- [ ] **Hypothetical examples labeled**: Non-CTD Swag examples clearly marked as "hypothetical" or "potential enhancement"
- [ ] **Architectural alignment**: Follows CTD Swag patterns (handle* for events, get* for calculations, feature-based organization)
- [ ] **Hook usage authentic**: References actual built-in hooks used in CTD Swag (no custom hooks currently implemented)
- [ ] **Imports appropriate**: Imports are included when they are part of the surrounding discussion or critical for understanding
- [ ] **Component exports**: Components are properly exported as default or named
- [ ] **Props alignment**: Props passed match component function parameters and CTD Swag naming conventions
- [ ] **Key attributes**: List items have appropriate `key` props
- [ ] **Event handlers**: Event handler syntax is correct and appropriate
- [ ] **State management**: useState and other hooks used correctly
- [ ] **Code extracts acceptable**: Code blocks marked as "extract from [filename]" or similar may be incomplete by design
- [ ] **Logical placeholders**: Complex logic (4+ lines) can use descriptive placeholders when not critical to the discussion

## Error Categories and Severity Guidelines

### Critical Errors (Fix Immediately)

**These break student workflow and prevent code from functioning:**

- Syntax errors in code examples
- Props mismatch between parent and child components
- Missing or incorrect import/export statements
- Component naming inconsistencies that break references
- Incorrect hook usage that violates Rules of Hooks
- Missing required dependencies or setup steps

### Important Errors (Fix Before Publication)

**These cause confusion but don't break functionality:**

- Unclear or ambiguous instructions
- Inconsistent terminology across lessons
- Missing context or prerequisites
- Outdated screenshots or visual aids
- Broken internal links between lessons
- Component names that don't match between text and code

### Minor Errors (Address in Next Review Cycle)

**These are cleanup items that improve quality:**

- Typos and grammar issues
- Inconsistent formatting
- Missing alt text for images
- Code style inconsistencies
- Outdated external links
- Missing or incomplete comments in code examples

### Review Prioritization Guidelines

1. **Critical errors halt review**: Fix immediately before proceeding
2. **Important errors cluster**: Address all in the same section before moving on
3. **Minor errors batch**: Collect and fix together at the end of review
4. **Document patterns**: Note recurring error types for process improvement
