# Lab Report: DOM XSS in `document.write` Sink using Source `location.search` inside a `<select>` Element

## Overview
This lab demonstrates a **DOM-based Cross-Site Scripting (XSS)** vulnerability that occurs when untrusted data from `location.search` is dynamically written into a `<select>` element using `document.write()`.  
Because the application fails to sanitize user input, an attacker can inject malicious JavaScript code that executes when the page is rendered.

**Lab Title:** DOM XSS in `document.write` sink using source `location.search` inside a select element  
**Goal:** Execute arbitrary JavaScript by injecting a payload into the vulnerable parameter.  

**Payload Used:**
```html
<script>alert('XSS')</script>
```

---

## Steps Performed
1. Inspected the application and found that the value of the `storeId` parameter from the query string is inserted into the DOM using `document.write()`.  
2. Confirmed that the data is written inside a `<select>` element without sanitization.  
3. Injected the payload `<script>alert('XSS')</script>` into the `storeId` parameter.  
4. Upon loading the page, the injected script tag was executed immediately.  
5. The lab displayed **“Congratulations, you solved the lab!”**, confirming successful exploitation.

---

## Evidence

### 1) Lab Solved Confirmation
![Lab Solved Screenshot](/images/DOM%20XSS%20in%20documentdotwrite%20lab%20solved.jpg)

### 2) JavaScript Alert Execution
![JavaScript Alert Screenshot](/images/DOM%20XSS%20in%20documentdotwrite.jpg)

> These screenshots show both the alert popup triggered by the injected script and the “Solved” confirmation message from the Web Security Academy.

---

## Result
- Successfully triggered a **DOM-based XSS** vulnerability via `document.write()` inside a `<select>` element.  
- The injected JavaScript executed within the victim’s browser.  
- Lab marked ✅ *Solved*.

---

## Root Cause
- The application uses `document.write()` to insert unescaped user input directly into a `<select>` context.  
- No validation, sanitization, or encoding of the URL parameter (`storeId`) before writing it to the DOM.  
- Browsers interpret injected HTML and execute script tags as active JavaScript code.

---

## Mitigations
1. **Avoid using `document.write()`:** Instead, use DOM-safe methods such as `textContent` or `appendChild()`.  
2. **Sanitize untrusted input:** Clean or filter user data before inserting it into HTML.  
3. **Use proper context-sensitive encoding:** Ensure that special characters like `<`, `>`, and `"` are safely encoded.  
4. **Apply Content Security Policy (CSP):** Restrict inline scripts and define allowed sources to minimize XSS risk.  
5. **Use modern frameworks:** Frameworks such as React or Angular automatically escape untrusted data in templates.

---

## References
- [PortSwigger Web Security Academy: DOM XSS in document.write](https://portswigger.net/web-security/cross-site-scripting/dom-based/document-write-sink)  
- [OWASP: DOM-Based Cross-Site Scripting (XSS)](https://owasp.org/www-community/attacks/DOM_Based_XSS)  
- [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)

---
*End of report.*
