#### Enhance VS Code's Built-in Linting with ESLint

A linter is a tool that performs a [static analysis](https://en.wikipedia.org/wiki/Static_program_analysis)of a codebase to flag syntax errors and bad practices without having to run the code. VS Code already provides basic static analysis for JavaScript but we can extend this with ESLint. The React template includes some sensible default rules when we installed it but we also have to install VS Code's [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) extension. The extension will allow VS Code to lint all project files and adds a tooltip when mousing over a flagged item. The tooltips usually include a brief summary of the rule violation and a link to more documentation. In the screenshot below, clicking the blue text "[react/jsx-key](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-key.md)" will open a browser window to documentation that explains the error, how to resolve it, and even rule configuration options for `eslint.config.js`.

![missing key props error in ide](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-01/missing-key.png)

A keen observer may have noticed a few unusual comments in an earlier screenshot. At the top of the file for the `App` component, there are commented lines that disable two rules. Those lines are suppressing errors ESLint would have flagged.

![eslint rules disabled in file by comment](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-01/eslint-disable.png)
In the following screenshot of an error tooltip, we have an error on `setTestList` being reported by VS Code's built-in static analysis and ESLint. `ts` stands for TypeScript even though it's evaluating JavaScript. This may be confusing since we're not using TypeScript but is just an odd detail resulting from how the internals of VS Code work.

![no unused vars tooltip](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-01/unused-vars.png)

When working with ESLint, we can to add and remove rules that suit our needs. ESLint provides three ways to modify rules:

1. ignore a rule for a file
2. ignore a rule for a single line
3. apply or ignore rules across a codebase

We have already seen how to turn off the rule for the entire file. Another way is to add the appropriate comment directly above a line of code which will then ignore only that instance of the warning. Finally, to change a rule's behavior across the codebase, it can be configured in `eslint.config.js` which is found at the root of the project. We can add, modify, or disable rules with this file. Keep in mind that the rules already in place conform to community best practices. Don't change them just to get rid of the squiggly lines in the code! Make sure you understand the rules you disable or enable and have a strong reason for doing so.

![eslint config rules disabled](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-01/eslint-rules-config.png)
