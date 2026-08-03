# TCM Security — Web Hacking Labs

My personal notes, lab write-ups, and scripts from working through TCM Security's Web Hacking course. This repo tracks hands-on practice in common web application vulnerability classes using Burp Suite, a Kali Linux VM, and Dockerized lab environments.

## Course Progress

- [x] HTTP Fundamentals
- [x] Information Gathering
- [ ] SQL Injection
- [x] XSS (Cross-Site Scripting)
- [x] Authentication Bypass
- [ ] File Upload
- [ ] Command Injection
- [x] XML Injection
- [x] JSON Web Tokens (JWT)
- [ ] Burp Suite Mastery


## Module Summaries

### 01 — Introduction / HTTP Fundamentals
Covered how HTTP requests and responses are structured, common methods (GET/POST/PUT), status codes, and how to intercept and modify traffic in Burp Suite's Proxy tab. Practiced reading raw requests and identifying where user input enters an application.

### 02 — Authentication
Explored common authentication weaknesses: predictable session tokens, missing account lockout, and username enumeration through differing error messages. Used Burp Suite's Repeater to manually test login flows for these issues.

### 07 — XML Injection
Practiced identifying and exploiting XML External Entity (XXE) injection in an application that parsed user-supplied XML. Learned how a malicious XML payload can be used to read local files or reach internal-only endpoints, and how disabling external entity resolution fixes it.

### 08 — XSS Injection
Worked through reflected and stored XSS scenarios — injecting payloads into input fields and observing execution in the browser. Documented the difference between reflected, stored, and DOM-based XSS, and how output encoding prevents each.

### JSON Web Tokens (JWT)
Practiced common JWT attacks: switching the algorithm to `none`, testing for weak signing secrets, and tampering with the payload to see if the server actually verifies the signature. Used this to understand why signature verification and algorithm allow-listing matter server-side.

## Tools & Setup

- Burp Suite Community Edition
- Kali Linux (VM)
- Docker (for spinning up vulnerable lab environments)
- nmap, ffuf, gobuster, sqlmap

## How to Use This Repo

1. Each numbered folder covers one vulnerability class or topic
2. Inside each folder: notes on the concept, the steps taken to exploit it, and what the fix looks like
3. Screenshots are included where useful to show the actual request/response or payload in action

## About Me

Computer Science student focused on security operations, currently building hands-on experience through TryHackMe, self-directed labs, and courses like this one. 
