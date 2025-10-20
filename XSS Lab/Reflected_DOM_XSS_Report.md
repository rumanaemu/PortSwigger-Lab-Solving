# Lab Report: Reflected DOM XSS

## Overview
This lab demonstrates a **Reflected DOM-based Cross-Site Scripting (XSS)** vulnerability. The attack occurs when user input from the URL is reflected directly into JavaScript code executed by the browser without proper sanitization or encoding.  
In this case, the vulnerable parameter in the query string is processed by client-side JavaScript and inserted into the DOM in an unsafe way.

**Lab Title:** Reflected DOM XSS  
**Goal:** Inject and execute arbitrary JavaScript through a reflected client-side parameter to trigger an alert box.

**Payload Used:**
```javascript
"-alert(1)//
```

---

## Steps Performed
1. The vulnerable page accepted input through the `search` parameter, which was directly reflected into a JavaScript function call in the DOM.  
2. The developer failed to escape or sanitize the reflected data, allowing attacker-controlled code to execute.  
3. Injected the payload `"-alert(1)//` into the `search` parameter in the URL.  
4. The JavaScript function parsed the input and executed the injected code.  
5. The alert popup appeared, confirming script execution.  
6. The Web Security Academy confirmed the exploit with the message **“Congratulations, you solved the lab!”**.

---

## Evidence

### 1) XSS Payload Execution
![Payload Execution Screenshot](/mnt/data/Reflected DOM XSS.jpg)

### 2) Lab Solved Confirmation
![Lab Solved Screenshot](/mnt/data/Reflected DOM XSS lab solved.jpg)

> The screenshots show both the execution of the payload via an alert popup and the successful lab completion message.

---

## Result
- Successfully exploited **Reflected DOM XSS** by injecting a payload that broke out of a JavaScript string and executed arbitrary code.  
- Verified by an alert popup and the lab confirmation banner.  
- Lab marked ✅ *Solved*.

---

## Root Cause
- The application directly embedded user input from the URL into a JavaScript context without proper escaping or sanitization.  
- This allowed attackers to break out of the intended code structure and inject new JavaScript.  
- The issue originated from unsafe use of `document.write()` or inline event handlers that use untrusted data.

---

## Mitigations
1. **Never use untrusted data in JavaScript without escaping.**  
   Use proper output encoding for the JavaScript context.
2. **Use DOM-safe methods:** Avoid `document.write()` and similar functions that interpret input as HTML/JavaScript.  
3. **Implement strict Content Security Policy (CSP):** Block inline scripts and unauthorized sources.  
4. **Validate and sanitize all user input:** Remove or encode special characters that could alter the code structure.  
5. **Use framework-provided safe APIs:** Many modern frameworks handle input sanitization automatically.

---

## References
- [PortSwigger Web Security Academy: Reflected DOM XSS](https://portswigger.net/web-security/cross-site-scripting/dom-based/reflected)  
- [OWASP: DOM-Based Cross-Site Scripting (XSS)](https://owasp.org/www-community/attacks/DOM_Based_XSS)  
- [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)  
- [MDN: JavaScript Escaping and Encoding](https://developer.mozilla.org/en-US/docs/Glossary/escaping)

---
*End of report.*
