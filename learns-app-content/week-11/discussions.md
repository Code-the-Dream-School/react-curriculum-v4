<!-- h1, h2 already used by CTD Learns -->

### Introduction

- Connect to previous learning  
- Preview this week’s content
- Explain real-world applications
- Overview the week’s coding assignment

### Polishing an App for Your Portfolio

- styling
- code cleanliness and adherence to best practices
- documentation

### App and Data Security

#### Client-Side Security (React Developer Direct Control)

- XSS Prevention
  - JSX escaping and safe practices
  - Safe use of `dangerouslySetInnerHTML`
  - Input validation patterns
- Environment variables and API keys
  - Never expose secrets in client code
  - Proper `.env` file usage
- Client side data stores
  - localStorage, sessionStorage, indexedDB security
  - Data sensitivity considerations
- Dependency vulnerabilities
  - `npm audit` and keeping packages updated
  - Third-party script security (CDNs, analytics)
  - Package integrity and avoiding suspicious packages

#### Application Architecture Security

- Authentication vs authorization
  - Understanding concepts and frontend implementation
  - Session management basics
- CORS
  - API integration understanding
  - Cross-origin request implications
- CSRF tokens
  - Frontend token handling
  - Request security patterns
- Error handling
  - Display generic error messages
  - Error boundaries for security
  - Preventing sensitive info exposure

#### Deployment Concerns

- Development vs production builds
  - Source map protection
  - Removing dev tools and debugging info
- HTTPS everywhere
  - Secure connections requirement
- CSP (Content Security Policy)
  - Header configuration impact on React apps
  - Script and resource loading restrictions

#### Topic 2 Check-for-understanding Questions

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
