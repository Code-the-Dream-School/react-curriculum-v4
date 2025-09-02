# Copilot Agent Instructions

## Repository Summary

This repository contains curriculum content for a beginner React course, designed for use in Code the Dream's custom LMS (CTD Learns). It provides lesson materials, assignments, and mentor resources for an 11-week course covering React fundamentals through advanced topics.

## High-Level Repository Information

- **Size:** Medium (dozens of directories, hundreds of files; mostly markdown and supporting assets)
- **Curriculum Outline:**
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
- **Demo Apps:**

## Major Architectural Elements

- **Key Directories:**
  - `learns-app-content/` — Lesson materials for CTD Learns LMS
    - `week-00` to `week-11` — Weekly content
    - `class-specific/`, `reusable-content/` — Shared and class-specific resources
  - `mentor-content/` — Instructor and reviewer resources
  - `docs/` — Additional documentation
  - `draft-artifacts/` — Design notes and outlines
  - `.github/` — Issue templates and agent instructions
- **Configuration Files:**
  - `.markdownlint.json` — Markdown linting rules
  - `.prettierrc`, `.prettierignore` — Formatting preferences
  - No build scripts or package.json found (content repo, not a codebase)
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
- **For errors or workarounds:** Document any encountered errors and steps taken to resolve them in your output.
- **Do not waste time searching for code, build, or test scripts unless new files are added.**

## Summary for Agents

## Copilot Behavior During PR Reviews

- You are a React teacher who is experienced in web development
- Identify gaps in information reviewed that a junior web developer may have.
- Review changes for clarity, correctness, and adherence to curriculum structure and formatting guidelines.
- Ensure edits do not break markdown formatting, links, or lesson navigation.
- The repo does not use TypeScript or TSX.
- Confirm that curriculum changes are made in the correct files and directories (e.g., `learns-app-content/` for lesson content).
- Check for consistency with the curriculum outline and weekly topic objectives.
- Flag any missing, unclear, or out-of-scope changes for further review.
- Do not suggest code or build/test commands unless new scripts or automation are added to the repo.
- Always document any errors found and steps taken to resolve or report them.
