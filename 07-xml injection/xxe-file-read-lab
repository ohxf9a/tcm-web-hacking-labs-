# XXE Injection Lab Write-up

## Overview

XXE (XML External Entity Injection) is a vulnerability that occurs when XML parsers process external entities defined in the DOCTYPE declaration. Even if the application does not use these features, the underlying XML parser may still support them, creating an attack surface.

## Lab Objective

Retrieve the /etc/passwd file from the target server using XXE injection.

## Attack Methodology

### Reconnaissance Phase

| Step | Action | Details |
|------|--------|---------|
| 1 | Identify XML Input Vectors | Look for endpoints accepting XML content-type, check POST requests with XML bodies |
| 2 | Determine XML Structure | Observe application's expected XML format, identify injection points |

### Exploitation Phase

| Step | Action | Payload/Command |
|------|--------|-----------------|
| 1 | Confirm XXE Vulnerability | `<!DOCTYPE test [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>` |
| 2 | Validate Exploitation | Check if response contains file contents |
| 3 | Extract Information | Enumerate users: www-data, peter, carlos, user, elmer, academy |

## Core Concepts

### XML Entity Types

| Type | Syntax | Description |
|------|--------|-------------|
| Internal Entity | `<!ENTITY name "value">` | Defined and used within the same XML document |
| External Entity | `<!ENTITY name SYSTEM "URI">` | Reference external resources using URIs |
| DOCTYPE Declaration | `<!DOCTYPE root [<!ENTITY name SYSTEM "file:///path">]>` | Defines document type and entity declarations |

## Attack Types

| Attack Type | Purpose | Example Payload |
|-------------|---------|-----------------|
| File Disclosure | Read local files | `<!ENTITY xxe SYSTEM "file:///etc/passwd">` |
| SSRF | Make server request internal resources | `<!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/">` |
| Blind XXE | OOB exfiltration when no response | Host DTD file, use `%eval;%exfil;` |
| Error-Based XXE | Force errors to leak info | `<!ENTITY % xxe SYSTEM "file:///nonexistent">` |

### File Disclosure Targets

| OS | Target Files |
|----|--------------|
| Linux | /etc/passwd, /etc/shadow, /etc/hosts, /proc/self/environ, /var/www/html/config.php |
| Windows | C:\windows\win.ini, C:\windows\system32\drivers\etc\hosts, C:\inetpub\wwwroot\web.config |

## Methodology Mindset

| Phase | Action Items |
|-------|--------------|
| 1. Identify Entry Points | Check Content-Type headers (application/xml, text/xml, application/soap+xml) |
| 2. Analyze XML Structure | Map elements, identify processed/returned fields, check validation |
| 3. Test Entity Injection | Define entity, reference in XML element, observe response |
| 4. Leverage Protocols | Use file://, http://, https://, ftp://, php:// |
| 5. Enumerate System | Read /etc/passwd, /etc/hosts, config files, service info |
| 6. Escalate Privileges | Find credentials, SSH keys, sensitive logs |
| 7. Bypass Techniques | Try different protocols, encoding, parameter entities |

## Common Defenses and Bypasses

| Defense | Bypass Technique |
|---------|------------------|
| Disable External Entities | Use alternative XML parsers, PHP wrappers |
| Input Validation | CDATA sections, encoding |
| Whitelist Allowed Entities | Parameter entities with DTD references |

## Tools and Resources

| Tool | Purpose |
|------|---------|
| Burp Suite Repeater | Test payloads |
| Burp Suite Intruder | Fuzzing |
| Burp Suite Collaborator | Detect blind XXE, OOB exfiltration |
| PayloadsAllTheThings | XXE wordlist |
| HackTricks | XML injection references |


## Key Takeaways

| # | Takeaway |
|---|----------|
| 1 | XML parsers process entities even when unused by the application |
| 2 | The DOCTYPE declaration is the injection point |
| 3 | Entity references must be properly formed with semicolons |
| 4 | Different systems require different file paths |
| 5 | Information disclosure leads to privilege escalation paths |
| 6 | Blind XXE requires OOB or error-based techniques |

## References for Further Study

- OWASP XXE Prevention Cheat Sheet
- PortSwigger Web Security Academy - XXE labs
- HackTricks - XML External Entity
- PayloadsAllTheThings - XXE Injection

---

## Notes on Screenshots

### Should You Add Screenshots?

**Yes, include screenshots for:**

| Screenshot Type | What to Show |
|-----------------|--------------|
| Burp Request | The XXE payload in Repeater |
| Burp Response | /etc/passwd contents in response |
| Lab Dashboard | Completed lab status |
| Successful Output | Key extracted data highlighted |

**Suggested Screenshots:**

1. **Burp Request with Payload**
   - Show the XML payload with DOCTYPE declaration
   - Highlight the injected entity reference

2. **Burp Response with Output**
   - Show the /etc/passwd file contents
   - Highlight user accounts found

3. **Lab Completion**
   - Show the lab dashboard with green checkmark
   - Shows the lab is solved

### Screenshot Organization
