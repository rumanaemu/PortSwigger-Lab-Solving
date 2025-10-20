# Lab Report: DOM XSS in jQuery Selector Sink using a `hashchange` Event

## Overview
This lab demonstrates a **DOM-based Cross-Site Scripting (XSS)** vulnerability that occurs when untrusted input from `location.hash` is used directly inside a jQuery selector. The vulnerability is triggered whenever the URL hash changes (`hashchange` event). If user-controlled data is inserted into a selector expression without sanitization, arbitrary JavaScript can be executed.

**Lab Title:** DOM XSS in jQuery selector sink using a hashchange event  
**Goal:** Exploit the vulnerable jQuery selector logic to execute arbitrary JavaScript and trigger an alert popup.

**Payload Used:**
```html
"><img src=x onerror=alert(1)>
```

---

## Steps Performed
1. Observed that the web application dynamically processed data from the **URL hash fragment** (after `#`) using jQuery.  
2. Determined that the input was inserted directly into a selector expression that executes when the `hashchange` event occurs.  
3. Crafted the payload `"><img src=x onerror=alert(1)>` to break out of the context and inject executable HTML.  
4. Loaded the crafted URL and observed that the payload executed immediately after the hash was processed.  
5. The lab displayed **“Congratulations, you solved the lab!”**, confirming successful exploitation.

---

## Evidence

### 1) Lab Solved Confirmation
![Lab Solved Screenshot](/images/DOM%20XSS%20in%20jQuery%20selector%20sink%20using%20a%20hashchange%20event%20lab%20solved.jpg)

> The screenshot confirms that the injected payload executed successfully and that the lab was marked as *Solved* by the Web Security Academy.

---

## Result
- Successfully exploited a **DOM-based XSS** vulnerability through a jQuery selector sink triggered by the `hashchange` event.  
- The injected code executed within the browser context.  
- Lab marked ✅ *Solved*.

---

## Root Cause
- The application directly concatenated untrusted hash values into a jQuery selector expression.  
- No input sanitization or validation was applied before using the hash as part of the selector.  
- As a result, injected payloads were interpreted as executable HTML or script code when the DOM was updated.

---

## Mitigations
1. **Avoid using untrusted input in jQuery selectors.** Always sanitize or escape user input before inserting it into selector or HTML contexts.  
2. **Use safe jQuery methods:** Replace `$(location.hash)` or `$(...)` with methods that treat strings as text instead of selectors.  
3. **Validate the URL fragment:** Use whitelisting to allow only expected values in `location.hash`.  
4. **Implement Content Security Policy (CSP):** Prevent execution of injected inline scripts.  
5. **Use frameworks or libraries** that auto-escape input and protect against selector-based DOM injection.

---

## References
- [PortSwigger Web Security Academy: DOM XSS in jQuery selector sink using hashchange event](https://portswigger.net/web-security/cross-site-scripting/dom-based/jquery-selector-sink)  
- [OWASP: DOM-Based Cross Site Scripting (XSS)](https://owasp.org/www-community/attacks/DOM_Based_XSS)  
- [jQuery Documentation: Selectors](https://api.jquery.com/category/selectors/)  
- [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)

---
*End of report.*
