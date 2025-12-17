<!-- h1, h2 already used by CTD Learns -->
> [!note]
> This week also includes a CoderByte assessment

> [!important]
> This assignment is **optional**. We understand that not everyone will want to deploy their application online or use this specific hosting service. You can focus on the styling and portfolio preparation aspects without deploying, or choose an alternative deployment platform that better suits your needs. If you choose another service, your mentors may not be able to assist with troubleshooting.

### Expected App Capabilities

After completing this week's assignment, your app should:

- Be professionally styled and ready to be incorporated into your portfolio
- Be hosted live using [Render](https://render.com/) static site hosting
- Maintain all existing CRUD and routing functionality
- Include a comprehensive README with live demo link
- Demonstrate security best practices and input validation
- Be responsive and accessible across different devices

### Part 1: Style Your Application

Transform your React application from a functional prototype into a portfolio-worthy project that showcases your development skills. Professional styling demonstrates attention to detail and user experience considerations that employers value.

#### 1.1 Choose Your Styling Approach

Select one of the following styling approaches based on your preferences and project needs:

1. **Option A: CSS Modules (Recommended for Beginners)**

- Already configured in Vite projects
- Scoped CSS prevents naming conflicts
- Uses familiar CSS syntax
- Create `.module.css` files for each component

2. **Option B: CSS-in-JS with Styled-Components**

- Dynamic styling based on props
- Theme support and component composition
- Install: `npm install styled-components`

3. **Option C: CSS Framework (Tailwind CSS)**

- Utility-first approach for rapid development
- Consistent design system
- Install: `npm install tailwindcss postcss autoprefixer`

#### 1.2 Implement Professional Styling

Apply the following styling improvements to enhance your application's visual appeal:

**Visual Polish Checklist:**

- [ ] Implement a consistent color scheme (primary, secondary, accent colors)
- [ ] Establish typography hierarchy with readable fonts and sizes
- [ ] Create responsive layouts that work on mobile, tablet, and desktop
- [ ] Add loading states for data fetching operations
- [ ] Design error states for failed operations
- [ ] Style empty states when no data is available
- [ ] Ensure sufficient color contrast so text is easily readable

**Component-Specific Styling:**

- [ ] **App Layout**: Clean, centered layout with proper spacing and visual hierarchy
- [ ] **Todo List**: Organized display of todo items with clear visual separation
- [ ] **Todo Items**: Consistent layout with checkbox, text, and action buttons aligned
- [ ] **Add Todo Form**: Clear input field with accessible labels and submit button
- [ ] **Navigation**: Clean routing between different views (All Todos, Active, Completed)
- [ ] **Input Components**: Professional styling for text inputs with proper focus states
- [ ] **Buttons**: Consistent styling across add, edit, delete, and filter actions
- [ ] **Checkboxes**: Custom styled checkboxes that clearly indicate completed vs. active states

**Interactive Elements:**

- [ ] Smooth hover transitions on clickable elements
- [ ] Focus indicators for keyboard navigation
- [ ] Disabled states for unavailable actions

#### 1.3 Responsive Design Implementation

Ensure your application works seamlessly across all device sizes:

**Responsive Testing:**

- [ ] Test on Chrome DevTools device emulation
- [ ] Verify touch-friendly button sizes (minimum 44px)
- [ ] Ensure readable text without zooming
- [ ] Check horizontal scrolling is eliminated

### Part 2: Implement Security Best Practices

Apply security measures to protect your application and user data:

#### 2.1 Input Validation and Sanitization

**Install Security Dependencies:**

First, install DOMPurify for proper input sanitization:

```bash
npm install dompurify
npm install --save-dev @types/dompurify
```

**Example Form Sanitization:**

```jsx
import DOMPurify from 'dompurify';

// Example: Sanitize user input using DOMPurify
const sanitizeInput = (input) => {
  return DOMPurify.sanitize(input.trim(), {
    ALLOWED_TAGS: [], // Remove all HTML tags
    ALLOWED_ATTR: []  // Remove all attributes
  });
};
```

**Implementation Tasks:**

- [ ] Install and configure DOMPurify for input sanitization
- [ ] Ensure input validation happens before sanitizing them using DOMPurify
- [ ] Add client-side validation to all form inputs
- [ ] Implement proper error messaging without exposing system details
- [ ] Add maximum length limits to text inputs

### Part 3: Optimize for Portfolio Presentation

Prepare your application to showcase your development skills:

#### 3.1 Code Quality Review

**Code Organization:**

- [ ] Remove commented-out code and console.log statements
- [ ] Ensure consistent indentation and formatting
- [ ] Add meaningful comments for complex logic
- [ ] Extract reusable components where appropriate
- [ ] Remove unused imports, dependencies, and variables

#### 3.2 Create Comprehensive Documentation

**README Requirements:**
Create a professional README.md that includes all of the following sections:

- [ ] **Project Title and Description**: Clear, concise overview of what your todo app does
- [ ] **Live Demo Link**: Direct link to your deployed application (if deploying)
- [ ] **Features List**: Bullet points of main functionality (add todos, mark complete, filter, etc.)
- [ ] **Technologies Used**: List of frameworks, libraries, and tools used
- [ ] **Screenshots**: Desktop and mobile views of your styled application
- [ ] **Getting Started Section**: Prerequisites and installation instructions
- [ ] **Available Scripts**: Explanation of npm scripts (dev, build, preview, etc.)
- [ ] **Design Decisions**: Brief explanation of your styling approach and choices
- [ ] **Future Improvements**: Features you would add with more time
- [ ] **License Information**: MIT license or other appropriate license
- [ ] **Contact Information**: Your GitHub profile or portfolio link

### Part 4: Deploy to Render

Deploy your application to Render's static site hosting for free public access:

#### 4.1 Prepare for Deployment

**Pre-deployment Checklist:**

- [ ] Ensure your application builds successfully: `npm run build`
- [ ] Test the production build locally: `npm run preview`
- [ ] Verify all environment variables are properly configured
- [ ] Commit and push all changes to your main branch
- [ ] Ensure your repository is public on GitHub

**Build Configuration:**
Verify your `package.json` includes the correct build script:

```json
{
  "scripts": {
    "build": "vite build",
    "preview": "vite preview"
  }
}
```

#### 4.2 Deploy to Render

**Step 1: Create Render Account**

1. Go to [render.com](https://render.com/)
2. Sign up using your GitHub account
3. Authorize Render to access your repositories

**Step 2: Create New Static Site**

1. Click "New +" in the Render dashboard
2. Select "Static Site"
3. Connect your GitHub repository
4. Configure the deployment settings:
   - **Build Command**: `npm run build`
   - **Publish Directory**: `dist`
   - **Branch**: `main`

**Step 3: Configure Environment Variables (if needed)**

1. In the Render dashboard, go to your site settings
2. Add any environment variables your app needs
3. Use the same variable names from your `.env` file

**Step 4: Deploy and Test**

1. Click "Create Static Site"
2. Wait for the build to complete (usually 2-5 minutes)
3. Test your live application thoroughly
4. Fix any deployment-specific issues and redeploy

### Part 5: Final Testing and Quality Assurance

#### 5.1 Cross-Device Testing

Test your deployed application on:

- [ ] Desktop browsers (Chrome, Firefox, Safari)
- [ ] Mobile devices (iOS and Android)
- [ ] Tablet devices
- [ ] Different screen orientations

#### 5.2 Functionality Verification

Ensure all features work correctly in the production environment:

- [ ] Todo creation (add new todos)
- [ ] Todo completion (check/uncheck todos)
- [ ] Todo editing (modify existing todo text)
- [ ] Todo deletion (remove todos)
- [ ] Todo filtering (All, Active, Completed views)
- [ ] Form validations (prevent empty todo submission)
- [ ] Navigation between different todo views
- [ ] Error handling and user feedback

### Submission Requirements

**For Week 11, submit your project URL instead of a pull request link.**

Submit the following:

- **Live Application URL** (if you deployed): Link to your deployed application
- **GitHub Repository**: Link to your completed and styled project repository
- **Updated README**: Including comprehensive documentation
- **Style Documentation**: Brief explanation of your styling approach and design decisions

### Evaluation Criteria

Your assignment will be evaluated on:

- **Visual Design**: Professional appearance, consistent styling, responsive design
- **Code Quality**: Clean, organized code with proper comments and structure
- **Functionality**: All features work correctly in the deployed environment
- **Documentation**: Comprehensive README with clear setup instructions
- **Security**: Proper input validation and secure coding practices
- **Accessibility**: Keyboard navigation and screen reader compatibility

### Troubleshooting Common Issues

**Build Failures:**

- Check for TypeScript errors or missing dependencies
- Verify environment variables are properly configured
- Ensure all imports use correct file paths (case-sensitive on Linux)

**Environment Variable Issues:**

- Remember that client-side environment variables must start with `VITE_`
- Check that sensitive data is not exposed in the client bundle

### Next Steps

After completing this assignment, consider these enhancements for your portfolio:

- Add unit tests for critical components
- Implement Progressive Web App (PWA) features
- Add dark/light theme toggle
- Implement data persistence with localStorage or a backend API
- Add drag and drop functionality for reordering todos
