# Lab Report: File Path Traversal (Traversal sequences stripped non-recursively)

## Overview
This lab demonstrates a **File Path Traversal** vulnerability where the application attempts to mitigate traversal by removing traversal sequences (e.g. `../`) in a **non-recursive** manner. Because the stripping is non-recursive or incomplete, carefully encoded or transformed traversal payloads can still reach the file system and allow access to sensitive files.

**Goal:** retrieve the contents of `/etc/passwd` by bypassing the non-recursive stripping of traversal sequences.

---

## Steps to Exploitation

### 1. Initial Request
The application used a request parameter `filename` to fetch image files. A simple traversal attempt like `../../../../etc/passwd` was either stripped or blocked by the server-side filter.

### 2. Bypass using encoded / alternate traversal sequences
Because the application strips literal `../` sequences in a single pass (non-recursively), it can be bypassed by sending traversal sequences in an alternative encoding or format that the stripper does not normalize first. Common bypass techniques include URL-encoding or using alternate path segment patterns that the stripper doesn't detect.

Example payload using alternate traversal pattern:

```
GET /image?filename=....//....//....//....//etc/passwd HTTP/2
Host: <LAB-ID>.web-security-academy.net
Cookie: session=<valid-session-cookie>
User-Agent: Mozilla/5.0
Accept: image/avif,image/webp,*/*
```

**Explanation:**
- The server-side filter strips literal `../` sequences but does not normalize sequences like `....//`. When the filesystem or path normalization resolves `....//` segments, they can act as traversal (`../..`) and allow access to `../../../../etc/passwd`. This allows the file to be read.

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
![Exploit Request and Response](/images/traversal%20sequences%20stripped1.jpg)

### Lab Solved Confirmation
![Lab Solved](/images/traversal%20sequences%20stripped-lab%20solved1.jpg)

---

## Result
- Retrieved sensitive system file: `/etc/passwd`  
- Confirmed path traversal vulnerability by bypassing non-recursive stripping of traversal sequences  
- Lab status:  
  ✅ *"Congratulations, you solved the lab!"*

---

## Root Cause
- The application attempted to remove traversal sequences but did not canonicalize or decode the input prior to filtering.  
- The filter operated non-recursively (or on the literal string) and failed to account for encoded or alternate traversal representations.

---

## Mitigation
1. **Canonicalize input** — Decode and normalize input before applying any filtering, then validate the canonical path.  
2. **Use Whitelisting** — Restrict file access to a specific directory and a whitelist of filenames.  
3. **Reject encoded traversal** — After canonicalization, reject paths containing traversal components.  
4. **Avoid direct file paths** — Use indirect references (IDs or mappings) instead of exposing file paths to users.  
5. **Use secure APIs** — Use APIs that perform proper path normalization and avoid manual string-based filtering.

---

## Skills Demonstrated
- Testing for path traversal vulnerabilities  
- Bypassing non-recursive or naive traversal filters using encoded or alternate traversal sequences  
- Retrieving sensitive files from the server (`/etc/passwd`)  
- Capturing and documenting exploitation steps with Burp Suite

---

## References
- [PortSwigger Academy: File path traversal, traversal sequences blocked with absolute path bypass](https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively)  
- [OWASP: Path Traversal](https://owasp.org/www-community/attacks/Path_Traversal) 

--- 

*End of report.*