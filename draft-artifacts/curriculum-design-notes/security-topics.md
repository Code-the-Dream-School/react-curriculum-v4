One of CTD's staff engineers that focuses on Labs' app security showed me a checklist that he works from. We can distill down to what is relevant. A lot of them are backend focused but I think these may be relevant: 1, 7 , 8, 11, 15, 16, 19, 20, 23, 24, 26, 27, 28

1. Display Generic Error Messages
2. ~~Suppress Framework Generated Errors~~
3. ~~Log All Authentication Activities~~
4. ~~Log All Privelege Changes~~
5. ~~Log Administrative Activities~~
6. ~~Log Access to Sensitive Data~~
7. Do Not Log Inappropriate Data
8. Use HTTPS Everywhere
9. ~~Disable HTTP Access for All Protected Resources~~
10. ~~Store User Passwords Using a Strong, Iterative, Salted Hash~~
11. Don't Hardcode Credentials
12. ~~Develop a Strong Password Reset System~~
13. ~~Implement a Strong Password Policy and Move Towards Passwordless Authentication~~
14. ~~Implement Account Lockout against Brute Force Attacks~~
15. Store Database Credentials and API keys Securely
16. Regenerate Session Tokens
17. ~~Implement an Idle Session Timeout~~
18. ~~Implement an Absolute Session Timeout~~
19. Invalidate the Session after Logout
20. Use Secure Cookie Attributes (HttpOnly, Secure and SameSite Flags)
21. ~~Conduct Contextual Output Encoding~~
22. ~~Use Parameterized SQL Queries~~
23. Use Tokens to Prevent Forged Requests
24. Validate Uploaded Files
25. ~~Use the Nosniff Header for Uploaded Content~~
26. Validate the Source of Input
27. Use Secure HTTP Response Headers
28. Deserialize Untrusted Data with Proper Controls
29. ~~Apply Access Controls Checks Consistently~~
