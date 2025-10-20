# Lab Report: Reflected XSS into a JavaScript String (Angle Brackets HTML-encoded)

## Overview
This lab demonstrates a **reflected cross-site scripting (XSS)** vulnerability where user input is reflected into a **JavaScript string** context on the client side. Although angle brackets (`<` and `>`) are HTML-encoded by the application, the developer failed to properly escape characters for the **JavaScript** context, allowing an attacker to break out of the string and execute arbitrary code.

**Lab Title:** Reflected XSS into a JavaScript string with angle brackets HTML-encoded  
**Goal:** Inject input that escapes the JavaScript string context and triggers `alert(1)`.

**Payload Observed:**  
```
'-alert(1)-'
```
(Shown reflected in the page and causing `alert(1)` to execute in the browser.)

---

## Steps Performed
1. Inspected how the `search` query parameter is used in client-side JavaScript and observed that the page reflects the parameter inside a JavaScript string.  
2. Confirmed that angle brackets were HTML-encoded but characters relevant to JavaScript string parsing were not properly escaped.  
3. Crafted and injected a payload designed to break out of the string and call `alert(1)`.  
4. Loaded the crafted URL and observed an alert dialog appear, demonstrating successful code execution.  
5. Captured screenshots showing the reflected payload, the alert popup, and the lab’s “Solved” confirmation.

---

## Example exploit (conceptual)
If the vulnerable parameter is `search`, a conceptual exploit URL might look like:

```
https://<LAB_HOST>/?search='-alert(1)-'
```

URL-encoding may be required when sending the payload in practice depending on the browser and transport method.

---

## Evidence (screenshots)

### 1) Payload execution (alert popup)
![Payload Execution](/images/Reflected%20XSS%20into%20attribute%20into%20a%20Javascript%20string.jpg)

### 2) Lab solved confirmation
![Lab Solved](/images/Reflected%20XSS%20into%20attribute%20into%20a%20Javascript%20string%20lab%20solved.jpg)

> Both images were provided by the user and show the injected value reflected on the page and the resulting JavaScript `alert(1)` confirming vulnerability exploitation.

---

## Result
- Successfully executed JavaScript code by breaking out of a reflected JavaScript string.  
- The lab was marked as **Solved** after the exploit was confirmed.

---

## Root Cause
- The application reflected user-controlled input into a JavaScript string without proper context-aware escaping for the JavaScript context.  
- HTML-encoding angle brackets alone is insufficient when data is embedded within JavaScript code — characters such as quotes, backslashes, and sequence termination must be handled correctly for that context.

---

## Mitigations
1. **Context-aware encoding:** Escape or serialize data specifically for JavaScript contexts (e.g., JSON-encode values and parse them safely).  
2. **Avoid injecting untrusted input into executable contexts:** Prefer presenting user data as plain text (`textContent`) instead of embedding it in scripts.  
3. **Use safe APIs:** Build data using safe DOM APIs or framework features that automatically handle escaping.  
4. **Input validation:** Restrict characters and use whitelists where feasible.  
5. **Content Security Policy (CSP):** Implement CSP to reduce the impact of XSS by disallowing inline scripts and restricting script sources.

---

## References
- [PortSwigger: Reflected DOM XSS resources](https://portswigger.net/web-security/cross-site-scripting)  
- [OWASP: XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)  
- [MDN: Safely using JSON to pass data to JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON)

---
*End of report.*
