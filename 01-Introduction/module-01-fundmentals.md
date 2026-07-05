TCM Web Hacking Course - My Notes
TCM Course Module: 1 - Introduction to Web Application Fundamentals
Date Started: 5/ Jul /2026
Status: Completed

📚 What I Learned
Web applications operate on a client-server model where the browser (client) communicates with servers using HTTP protocol. The backend consists of web servers (serving static files) and app servers (running application logic). The frontend uses HTML/CSS/JavaScript to create interactive interfaces. The DOM acts as a bridge between JavaScript and the webpage structure. Modern apps often use frameworks like React/Angular and additional infrastructure components like load balancers, databases, and WAFs. Understanding how all these pieces interact is crucial for identifying attack surfaces.

🔍 Key Concepts
Concept	Explanation
Client-Server Architecture	Browser sends HTTP requests → Server processes → Returns HTTP responses with content
HTTP Protocol	Stateless protocol; uses methods (GET/POST/PUT/DELETE) and status codes to communicate actions/results
DOM	Browser's programmatic interface that allows JS to dynamically modify webpage content and structure
Cookies	Small pieces of data stored client-side for session tracking; can be secured with HttpOnly, Secure, and SameSite flags
Frontend Frameworks	Provide structure for complex apps (routing, state management, data binding, API integration)
🛠️ Tools Used
Browser DevTools → Network tab: Inspect HTTP requests/responses, view headers, cookies, and payload data

Burp Suite → Intercept and modify HTTP traffic between browser and server

curl → Send custom HTTP requests from command line for testing

Postman → API testing and request crafting

💥 Attack Vectors
Information Disclosure via Headers

Server headers may reveal technology stack, versions, and OS details

Steps: Intercept request → Analyze response headers → Identify version info → Research known vulnerabilities

Cookie Manipulation

Cookies may contain session IDs or sensitive data

Steps: Capture cookies → Decode/modify → Replay with tampered values → Test for privilege escalation

Method Tampering

Testing for unsupported/restricted HTTP methods

Steps: Send OPTIONS request → Check allowed methods → Test TRACE, PUT, DELETE → Look for misconfigurations

🚫 Prevention
Secure Cookie Flags: Always set HttpOnly (prevents JS access), Secure (HTTPS only), and SameSite (CSRF protection)

Header Sanitization: Remove/obfuscate server/version information from response headers

Method Restriction: Disable unnecessary HTTP methods; implement proper authentication for sensitive methods

HTTPS Enforcement: Use Strict-Transport-Security (HSTS) header

Input Validation: Validate all client-side input on server-side

🧪 Lab Write-ups
Lab 1: HTTP Request Analysis

Lab 2: Cookie Manipulation Exercise

💡 Pro Tips from TCM
"Always look at the full HTTP conversation, not just the request or response individually. The interaction between them often reveals vulnerabilities."

"Learn what normal looks like first—then you'll spot abnormalities faster. Use a clean browser without extensions for baseline testing."

"Burp Suite's Repeater is your best friend for testing modifications; Intruder for automation. But always start manually to understand what you're automating."


MDN HTTP Overview

HTTP Status Codes Reference

OWASP Secure Headers Project

Burp Suite User Guide
