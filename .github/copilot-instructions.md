# Copilot Agent Instructions

## Repository Summary

This repository contains curriculum materials for a beginner React course, designed for use in Code the Dream's custom LMS (CTD Learns). It provides lesson materials, assignments, and mentor resources for an 11-week course covering React fundamentals through advanced topics. The repository also includes archived materials from the previous curriculum version (v3) for reference and comparison purposes.

## High-Level Repository Information

- **Size:** Large (hundreds of directories, thousands of files; mostly markdown and supporting assets)
- **Current Curriculum (v4) - 11 weeks:**
  - **Week 00:** Course Orientation and Local Environment Setup
  - **Week 01:** Intro to React, App Installation, and Working with Vite
  - **Week 02:** ReactDOM, Components, JSX, and Troubleshooting
  - **Week 03:** Component Lifecycle, Basic Hooks, State, and Props
  - **Week 04:** Hooks Continued, Events and Handlers
  - **Week 05:** Local State and Controlled Components, and Forms
  - **Week 06:** Re-usable Components, Project Organization, and Refactoring
  - **Week 07:** Data Fetching and UI Update Strategies
  - **Week 08:** Advanced Hooks: useCallback and useMemo, Optimizing a React App, and API-based Sort/Search
  - **Week 09:** Advanced State, useReducer, and useContext
  - **Week 10:** React Router
  - **Week 11:** Polishing an App for Your Portfolio, App and Data Security, and Deploying a React App
  - See `draft-artifacts/curriculum-design-notes/outline.md` for the full week-by-week outline
- **Previous Curriculum (v3) - 15 weeks:** Archived in `references/v3/` for comparison and reference
  - Extended course structure covering similar topics over 15 weeks instead of 11
  - Includes additional weeks for styling, final project development, and React ecosystem overview
  - Contains 246 archived files including lessons, assignments, objectives, and references

## Major Architectural Elements

- **Key Directories:**
  - `learns-app-content/` — Current curriculum materials for CTD Learns LMS (v4)
    - `week-00` to `week-11` — Weekly content
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
- [ ] **Imports appropriate**: Imports are included when they are part of the surrounding discussion or critical for understanding
- [ ] **Component exports**: Components are properly exported as default or named
- [ ] **Props alignment**: Props passed match component function parameters
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
