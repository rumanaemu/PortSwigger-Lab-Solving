# Lab Report: Stored DOM XSS

## Overview
This lab demonstrates a **Stored DOM-based Cross-Site Scripting (XSS)** vulnerability. In this type of XSS, malicious input is stored on the server (e.g., in a blog post, comment, or user profile) and later retrieved and executed within the victim’s browser when the data is dynamically processed by client-side JavaScript.

**Lab Title:** Stored DOM XSS  
**Goal:** Inject a stored XSS payload that executes arbitrary JavaScript code upon page load.

**Payload Used:**
```html
<img src=1 onerror=alert(1)>
```

---

## Steps Performed
1. Observed that the application stored user input in a blog post or comment field and later displayed it using **client-side JavaScript** without proper sanitization.  
2. Crafted the payload `<img src=1 onerror=alert(1)>` to inject JavaScript through an image tag’s `onerror` event handler.  
3. Submitted the payload through the vulnerable input field.  
4. Navigated to the post or page that rendered the stored content.  
5. The JavaScript executed automatically upon page load, showing an alert popup.  
6. Verified that the lab displayed **“Congratulations, you solved the lab!”** to confirm successful exploitation.

---

## Evidence

### 1) Payload Execution Screenshot
![Payload Execution Screenshot](/images/Stored%20DOM%20XSS.jpg)

### 2) Lab Solved Confirmation
![Lab Solved Confirmation](/images/Stored%20DOM%20XSS%20lab%20solved.jpg)

> The screenshots confirm that the stored XSS payload executed successfully and that the lab was completed.

---

## Result
- Successfully exploited a **Stored DOM XSS** vulnerability by injecting persistent JavaScript code.  
- The payload executed whenever the affected content was viewed by a user.  
- The lab was marked ✅ **Solved**.

---

## Root Cause
- The web application stored untrusted user input and later reflected it back into the DOM without sanitization or encoding.  
- JavaScript on the client-side dynamically inserted stored data into the HTML, creating an execution sink (such as `innerHTML` or `document.write`).

---

## Mitigations
1. **Contextual Output Encoding:** Properly encode stored data before rendering it into HTML or JavaScript contexts.  
2. **Sanitize User Input:** Use trusted sanitization libraries (e.g., DOMPurify) to remove unsafe tags and attributes.  
3. **Use Safe DOM Methods:** Replace dangerous functions like `innerHTML` and `document.write()` with safer alternatives like `textContent` or DOM manipulation APIs.  
4. **Implement Content Security Policy (CSP):** Prevent execution of injected scripts by enforcing strict CSP headers.  
5. **Validate Input at Both Ends:** Apply validation on both client-side and server-side to ensure stored data does not contain malicious payloads.

---

## References
- [PortSwigger Web Security Academy: Stored DOM XSS](https://portswigger.net/web-security/cross-site-scripting/dom-based/stored)  
- [OWASP: DOM-Based Cross Site Scripting (XSS)](https://owasp.org/www-community/attacks/DOM_Based_XSS)  
- [MDN: innerHTML Security Considerations](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML#security_considerations)  
- [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)

---
*End of report.*
