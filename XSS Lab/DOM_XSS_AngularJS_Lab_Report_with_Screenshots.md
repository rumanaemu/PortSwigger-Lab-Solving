# Lab Report: DOM XSS in AngularJS Expression (Angle Brackets and Double Quotes HTML-encoded)

## Overview
This lab demonstrates a **DOM-based Cross-Site Scripting (XSS)** vulnerability that arises due to unsafe handling of user input within an AngularJS expression.  
Although the application encodes angle brackets and double quotes, AngularJS’s expression parsing allows for exploitation through crafted injection that breaks out of intended data binding.

**Lab Title:** DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded

**Goal:** Exploit the AngularJS expression context to execute arbitrary JavaScript.  
**Payload Used:**

```
{{$on.constructor('alert(1)')()}}
```

---

## Steps Performed
1. Identified that the web application uses **AngularJS** for data binding and reflects user input into the AngularJS expression context.  
2. Observed that angle brackets (`<`, `>`) and double quotes (`"`) are HTML-encoded, but AngularJS allows for expression injection via its interpolation syntax `{{...}}`.  
3. Injected the payload `{{$on.constructor('alert(1)')()}}` into the vulnerable search parameter.  
4. The payload was evaluated by AngularJS, resulting in successful code execution with `alert(1)` appearing in the browser.  
5. The lab confirmed as **Solved**.

---

## Evidence

### 1) Lab Solved Confirmation
![Lab Solved Screenshot](/images/DOM%20XSS%20in%20AngularJs%20lab%20solved.jpg)

### 2) JavaScript Alert Execution
![JavaScript Alert Screenshot](/images/DOM%20XSS%20in%20AngularJs.jpg)

> These screenshots show the execution of the injected payload and the final “Lab Solved” confirmation banner.

---

## Result
- Successfully triggered DOM-based XSS in AngularJS via expression injection.  
- JavaScript alert displayed confirming execution of arbitrary code.  
- Lab status: ✅ *Solved*

---

## Root Cause
- User input is bound directly to AngularJS expressions using the `{{ }}` interpolation syntax.  
- Angle brackets and double quotes are encoded, but expression syntax remains executable.  
- AngularJS automatically evaluates expressions within its scope, enabling arbitrary code execution when untrusted input is used.

---

## Mitigations
1. **Disable AngularJS expression evaluation for untrusted input** — use the `ng-non-bindable` directive or sanitization libraries.  
2. **Use proper templating** — avoid direct user-controlled data binding.  
3. **Upgrade AngularJS** — use latest secure frameworks or migrate away from vulnerable versions (e.g., AngularJS 1.7.x and earlier).  
4. **Content Security Policy (CSP):** Enforce strict CSP to prevent inline JavaScript execution.  
5. **Sanitize user input:** Use Angular’s `$sanitize` or similar libraries to clean data before binding.

---

## References
- [OWASP: DOM-based Cross Site Scripting (XSS)](https://owasp.org/www-community/attacks/DOM_Based_XSS)  
- [PortSwigger: DOM XSS in AngularJS](https://portswigger.net/web-security/cross-site-scripting/dom-based/angularjs)  
- [AngularJS Developer Guide: Security Considerations](https://docs.angularjs.org/guide/security)  
- [MDN: Content Security Policy (CSP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)

---
*End of report.*
