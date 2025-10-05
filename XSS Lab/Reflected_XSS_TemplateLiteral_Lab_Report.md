# Lab Report: Reflected XSS into a Template Literal (Unicode-escaped angle brackets, quotes, backslash, backticks)

## Overview
This lab demonstrates a **reflected cross-site scripting (XSS)** vulnerability where user input is reflected into a JavaScript template literal.  
Even though the application Unicode-escapes angle brackets, quotes, backslashes, and backticks, it fails to neutralize the **`${...}`** template literal expression injection.

**Lab title:** Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped

**Goal:** Exploit the reflection into the template literal to execute JavaScript.  
**Payload:**

```
${alert(1)}
```

---

## Steps Performed
1. Identified that user input is embedded inside a JavaScript template literal.  
2. Noticed that escaping for `<`, `>`, quotes, backticks, and backslashes was applied, but the `${...}` sequence remained unmitigated.  
3. Injected the payload `${alert(1)}` into the vulnerable search parameter.  
4. The browser executed the payload inside the template literal, successfully triggering an `alert(1)`.  
5. The lab confirmed as **Solved**.

---

## Evidence

### 1) Lab solved confirmation
![Lab solved screenshot](/images/Reflected%20XSS%20into%20a%20template%20literal%20with%20angle%20brackets%201%20lab%20solved.jpg)

### 2) JavaScript alert execution
![JavaScript alert screenshot](/images/Reflected%20XSS%20into%20a%20template%20literal%20with%20angle%20brackets%20lab%201.jpg)

---

## Result
- Successfully executed arbitrary JavaScript using template literal injection.  
- Browser displayed the alert box (`1`).  
- Lab marked as **Solved** (green confirmation visible).

---

## Root Cause
- The application reflects user input into a JavaScript template literal without sanitizing or preventing the `${...}` syntax.  
- Escaping dangerous characters (like `<`, `>`, quotes, backslashes, backticks) does not protect against **expression injection** inside template literals.

---

## Mitigations
1. **Never place untrusted input inside JavaScript code**, especially inside template literals.  
2. **Proper encoding/escaping** — serialize input safely (e.g., JSON.stringify) before embedding into JavaScript.  
3. **Whitelist input** where possible (e.g., limit search terms to alphanumeric and spaces).  
4. **Content Security Policy (CSP):** Deploy strict CSP rules to reduce XSS impact.  
5. **Use safe DOM APIs** (`textContent`, `setAttribute`) instead of dynamically building JavaScript with user input.

---

## References
- [OWASP: Cross Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/)  
- [PortSwigger Web Security Academy: Reflected XSS](https://portswigger.net/web-security/cross-site-scripting/reflected)  
- [MDN: Template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)  
- [PortSwigger XSS Labs](https://portswigger.net/web-security/cross-site-scripting)

---
*End of report.*
