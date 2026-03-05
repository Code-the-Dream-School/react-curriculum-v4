# Alternative Setup: Using `vercel.json` for Online Deployment

## Overview

This supplemental guide explains how to configure a React todo app for online deployment on Vercel using a `vercel.json` file. The configuration routes `/api/*` requests from your deployed frontend to the CTD backend API, so students can keep using relative API paths in the app.

## Why This Configuration Helps

When your app is deployed, browser requests to the API can fail if routes are not configured correctly. A Vercel rewrite provides a clean deployment path:

- Frontend requests `/api/tasks`
- Vercel forwards that request to `https://ctd-learns-node-l42tx.ondigitalocean.app/api/tasks`
- The app stays consistent between local development and online deployment

## Implementation

### Step 1: Create `vercel.json` in Your Project Root

In the same folder as your `package.json`, create a file named `vercel.json`.

Use this exact configuration:

```json
{
  "$schema": "https://openapi.vercel.sh/vercel.json",
  "rewrites": [
    {
      "source": "/api/:path*",
      "destination": "https://ctd-learns-node-l42tx.ondigitalocean.app/api/:path*"
    }
  ],
  "headers": [
    {
      "source": "/api/:path*",
      "headers": [
        {
          "key": "X-Same-Origin-Proxy",
          "value": "vercel"
        }
      ]
    }
  ]
}
```

### Step 2: Commit and Push

Commit your `vercel.json` file and push your branch to GitHub.

### Step 3: Deploy on Vercel

1. Log in to [Vercel](https://vercel.com/)
2. Import your GitHub repository (or redeploy if already connected)
3. Let Vercel build and deploy your app
4. Open your deployment URL

## Verification

After deployment, open your app and test the main task flows:

1. Log in
2. Load tasks
3. Create, edit, and delete a task

Then open browser DevTools (Network tab) and confirm your frontend is requesting `/api/*` on your deployed domain.

## Troubleshooting

### API Requests Fail After Deploy

**Problem**: You see API errors or failed requests.

**Solutions:**

- Verify `vercel.json` is in the project root
- Verify the file is valid JSON (no trailing commas)
- Confirm your requests use `/api/*` paths, not direct backend URLs
- Redeploy after pushing changes

### App Works Locally but Not on Vercel

**Problem**: Local proxy works, deployed app fails.

**Solutions:**

- Confirm `vercel.json` was committed to the deployed branch
- In Vercel, check deployment logs and inspect the latest build
- Re-run deployment from the latest commit

## Additional Resources

- [Vercel `vercel.json` documentation](https://vercel.com/docs/project-configuration)
- [Vercel rewrites documentation](https://vercel.com/docs/rewrites)
- [Reference `vercel.json` (CTD deploy branch)](https://github.com/Code-the-Dream-School/react-todo-list-v4/blob/deploy/vercel.json)
