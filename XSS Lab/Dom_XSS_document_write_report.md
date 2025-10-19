# Lab Report: DOM XSS in `document.write` Sink using Source `location.search`

## Overview
This lab demonstrates a **DOM-based Cross-Site Scripting (XSS)** vulnerability where data from `location.search` is inserted directly into the document using `document.write()`.  
When the application fails to properly sanitize or encode user input before writing it to the DOM, attackers can inject and execute arbitrary JavaScript.

**Lab Title:** DOM XSS in `document.write` sink using source `location.search`  
**Goal:** Exploit the DOM sink by injecting JavaScript code that executes an alert popup.  

**Payload Used:**
```html
"><svg onload=alert(1)>
```

---

## Steps Performed
1. Identified that the web application reads data from the URL’s query string (`location.search`) and writes it to the document using `document.write()`.  
2. Observed that the written HTML is not sanitized, allowing injection of arbitrary elements.  
3. Injected the SVG payload (`"><svg onload=alert(1)>\`) into the `search` parameter.  
4. When the page loaded, the injected SVG tag triggered its `onload` handler, executing `alert(1)`.  
5. The lab displayed **“Congratulations, you solved the lab!”** confirming successful exploitation.

---

## Evidence

### 1) Lab Solved Confirmation
![Lab Solved Screenshot](/images/Dom%20XSS%20in%20Document%20lab%20solved.jpg)

### 2) JavaScript Alert Execution
![JavaScript Alert Screenshot](/images/Dom%20XSS%20in%20Document.jpg)

> The screenshots clearly demonstrate the payload in action (alert popup) and the Web Security Academy’s solved confirmation banner.

---

## Result
- JavaScript executed successfully through DOM-based XSS in a `document.write()` sink.  
- Alert window popped up as proof of arbitrary code execution.  
- Lab marked ✅ *Solved*.

---

## Root Cause
- The application dynamically injects unsanitized user input into the page using `document.write()`.  
- No output encoding or sanitization is applied to the data derived from `location.search`.  
- `document.write()` directly interprets the injected string as HTML and script code.

---

## Mitigations
1. **Avoid using `document.write()`.** Prefer safer DOM manipulation methods like `textContent` or `innerText`.  
2. **Output Encoding:** Encode untrusted data before inserting it into the DOM.  
3. **Input Validation:** Restrict or sanitize user input to prevent inclusion of tags and event handlers.  
4. **Content Security Policy (CSP):** Implement a strict CSP to mitigate the effects of potential XSS vulnerabilities.  
5. **Framework Security:** Use modern front-end frameworks that handle encoding automatically.

---

## References
- [PortSwigger Web Security Academy: DOM XSS in document.write](https://portswigger.net/web-security/cross-site-scripting/dom-based/document-write-sink)  
- [OWASP: DOM-Based XSS](https://owasp.org/www-community/attacks/DOM_Based_XSS)  
- [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)

---

*End of report.*
