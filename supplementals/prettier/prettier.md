#### Automatic Code Formatting with Prettier

Clean code is vital for any project, especially in a team codebase, but having to think about formatting details detracts from our attention which is better spent on crafting useful code. We can delegate formatting to tool that automates the process of applying formatting rules consistently across a codebase.

One of the most popular JavaScript formatting tools is [Prettier](https://prettier.io/). Like ESLint, we include a configuration file in our codebase and we have to install [a plugin for VS Code](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode). When configured correctly, it frees us from having to think about formatting! Let's go ahead and set that up.

Since this is a development tool and is not a part of the app, we install Prettier as a dev dependency. `npm install --save-dev prettier`. The shorthand of this command is `npm i -D prettier`. After installing, we create a configuration file, `.prettierrc` at the root directory of the project. Inside `.prettierrc` we add a JSON object containing some starting rules:

```JSON
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "es5"
}
```

We then have to install Prettier plugin and configure VS Code to use it as the default code formatter. After installing the plugin, open VS Code's settings and filter for "format". For `Editor: Default Formatter` select Prettier and then check the box for `Editor: Format on Save`.

![searching for format in ide settings](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v4/refs/heads/main/supplementals/prettier/assets/search-format.png)

Code before and after saving with Prettier enabled:

![code before and after prettier formatting](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v4/refs/heads/main/supplementals/prettier/assets/prettier-before-after.png)

- semicolons are consistently used
- quotes on imports are single quotes instead of double quotes
- the spacing between the array's brackets and its elements has been removed
- there are only one empty line between code blocks
