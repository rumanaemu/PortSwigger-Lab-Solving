# Lab Report: Stored XSS into HTML Context with Nothing Encoded

## Overview
This lab demonstrates a **Stored Cross-Site Scripting (XSS)** vulnerability where user-supplied input is stored on the server and later reflected into the **HTML context** without any encoding or sanitization. As a result, attackers can inject malicious JavaScript payloads that execute automatically for any user viewing the affected page.

**Lab Title:** Stored XSS into HTML context with nothing encoded  
**Goal:** Execute a stored XSS payload to trigger a JavaScript alert popup.

**Payload Used:**
```html
<script>alert('XSS')</script>
```

---

## Steps Performed
1. Discovered a comment submission form that allowed user input to be posted to the website.  
2. Tested the form by injecting a harmless payload to verify whether it was reflected unencoded in the HTML response.  
3. Identified that the submitted comment appeared directly in the HTML body without sanitization.  
4. Injected the payload `<script>alert('XSS')</script>` into the comment field.  
5. Upon submission and reloading of the blog post, the script executed automatically, displaying an alert popup.  
6. Verified the successful exploitation when the page displayed **“Congratulations, you solved the lab!”**.

---

## Evidence

### 1) Stored Payload Reflected and Executed
![Payload Execution Screenshot](/images/Stored%20XSS%20in%20to%20HTML%20contex.jpg)

### 2) Lab Solved Confirmation
![Lab Solved Screenshot](/images/Stored%20XSS%20in%20to%20HTML%20context_lab%20solved.jpg)

> The screenshots confirm the XSS payload execution and the successful completion of the Web Security Academy lab.

---

## Result
- Successfully exploited a **Stored XSS** vulnerability.  
- Payload `<script>alert('XSS')</script>` executed in the victim’s browser when viewing the affected page.  
- The lab was marked ✅ **Solved**.

---

## Root Cause
- The application stored untrusted user input and rendered it directly in the HTML body without performing output encoding or sanitization.  
- This allowed arbitrary script execution whenever the affected page was viewed.

---

## Mitigations
1. **Output Encoding:** Encode user-supplied data before including it in the HTML response.  
2. **Input Validation:** Sanitize inputs on both client and server sides to prevent injection of executable code.  
3. **Content Security Policy (CSP):** Enforce CSP headers to block inline JavaScript execution.  
4. **Use Framework Security APIs:** Rely on built-in template escaping mechanisms to safely render dynamic content.  
5. **Sanitize Stored Data:** Clean any previously stored user inputs to remove malicious payloads.

---

## References
- [PortSwigger Web Security Academy: Stored XSS into HTML Context](https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded)  
- [OWASP: Cross-Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/)  
- [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)

---
*End of report.*
