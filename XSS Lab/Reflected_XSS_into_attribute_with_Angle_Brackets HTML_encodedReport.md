# Lab Report: Reflected XSS into Attribute with Angle Brackets HTML-encoded

## Overview
This lab demonstrates a **Reflected Cross-Site Scripting (XSS)** vulnerability that occurs when user input is reflected inside an **HTML attribute** context. Although the web application encodes angle brackets (`<` and `>`), it fails to properly escape **double quotes**, allowing an attacker to break out of the attribute and inject malicious code.

**Lab Title:** Reflected XSS into attribute with angle brackets HTML-encoded  
**Goal:** Inject a payload into an HTML attribute and trigger a JavaScript `alert(1)`.

**Payload Used:**
```html
"onmouseover="alert(1)
```

---

## Steps Performed
1. Analyzed the page source and identified that the `search` parameter value was reflected inside an HTML attribute, such as:  
   ```html
   <input value="USER_INPUT">
   ```
2. Observed that while angle brackets were HTML-encoded, the attribute quotes were not sanitized.  
3. Injected the payload `"onmouseover="alert(1)` to break out of the attribute value and create a new event handler.  
4. When hovering over the affected element, the JavaScript `alert(1)` executed successfully.  
5. The Web Security Academy confirmed the exploit with the message **“Congratulations, you solved the lab!”**.

---

## Evidence

### 1) XSS Payload Execution
![Payload Execution Screenshot](/images/Reflected%20XSS%20into%20attribute%20with%20angle%20brackets%20HTML.jpg)

> The screenshot confirms that the injected attribute-based payload triggered a JavaScript alert popup, proving successful exploitation.

---

## Result
- Successfully executed a **Reflected XSS attack** within an HTML attribute context.  
- The payload executed as expected, displaying an alert box.  
- The lab was marked ✅ **Solved**.

---

## Root Cause
- The application improperly handled user-supplied data when embedding it inside an HTML attribute.  
- Only angle brackets were encoded, leaving quotes (`"`) unescaped, which allowed the payload to terminate the attribute and inject executable event code.

---

## Mitigations
1. **Contextual Output Encoding:** Encode all special characters based on output context — for attributes, encode quotes (`"` and `'`).  
2. **Use HTML Escaping Libraries:** Always sanitize user input before reflecting it in HTML templates.  
3. **Avoid Untrusted Data in Event Handlers:** Never place user-controlled values inside attributes like `onmouseover`, `onclick`, or similar.  
4. **Implement a Strong Content Security Policy (CSP):** Block inline scripts to mitigate the impact of XSS.  
5. **Input Validation:** Apply strict whitelisting for expected input patterns to reduce risk.

---

## References
- [PortSwigger Web Security Academy: Reflected XSS into attribute with angle brackets HTML-encoded](https://portswigger.net/web-security/cross-site-scripting/reflected/lab-escape-attribute-context)  
- [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)  
- [MDN: HTML Attribute Encoding](https://developer.mozilla.org/en-US/docs/Glossary/Attribute)

---
*End of report.*
