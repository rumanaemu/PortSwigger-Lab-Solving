# Lab Report: DOM XSS in `innerHTML` Sink using Source `location.search`

## Overview
This lab demonstrates a **DOM-based Cross-Site Scripting (XSS)** vulnerability where user input from `location.search` is directly written to a web page using the `innerHTML` property.  
When untrusted data is inserted into the DOM via `innerHTML`, it can execute arbitrary JavaScript if the data contains HTML tags or event handlers.

**Lab Title:** DOM XSS in `innerHTML` sink using source `location.search`  
**Goal:** Inject a malicious payload to execute arbitrary JavaScript.  

**Payload Used:**
```html
<img src=1 onerror=alert(1)>
```

---

## Steps Performed
1. Identified that user-supplied input from the query string (`location.search`) was reflected into the DOM using the `innerHTML` sink.  
2. Confirmed that no sanitization or encoding was applied before the content was rendered.  
3. Injected the payload `<img src=1 onerror=alert(1)>` into the search parameter.  
4. The injected payload caused the browser to execute the embedded JavaScript via the `onerror` event.  
5. The lab displayed **“Congratulations, you solved the lab!”**, confirming successful exploitation.

---

## Evidence

### 1) Lab Solved Confirmation
![Lab Solved Screenshot](/images/Dom%20XSS%20in%20inner%20HTML%20lab%20solved.jpg)

### 2) JavaScript Alert Execution
![JavaScript Alert Screenshot](/images/Dom%20XSS%20in%20inner%20HTML.jpg)

> The screenshots show both the successful JavaScript alert popup and the “Lab Solved” confirmation message from the PortSwigger Web Security Academy.

---

## Result
- Successfully triggered a **DOM XSS** vulnerability using the `innerHTML` sink.  
- JavaScript executed in the victim’s browser via the injected image payload.  
- Lab marked ✅ *Solved*.

---

## Root Cause
- The application uses `innerHTML` to insert untrusted data into the DOM without sanitization.  
- `innerHTML` interprets user input as HTML and executes embedded JavaScript code.  
- Lack of proper input handling or context-aware encoding led to arbitrary code execution.

---

## Mitigations
1. **Avoid using `innerHTML` for untrusted data.** Use `textContent` or `innerText` instead.  
2. **Sanitize input** using libraries such as DOMPurify before inserting it into the DOM.  
3. **Use safe templating frameworks** that automatically escape user input.  
4. **Implement Content Security Policy (CSP):** Limit execution of inline JavaScript and define trusted sources.  
5. **Validate and encode data** before rendering on the client side.

---

## References
- [PortSwigger Web Security Academy: DOM XSS in innerHTML](https://portswigger.net/web-security/cross-site-scripting/dom-based/innerhtml-sink)  
- [OWASP: DOM-Based Cross-Site Scripting (XSS)](https://owasp.org/www-community/attacks/DOM_Based_XSS)  
- [MDN: innerHTML Property](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML)  
- [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)

---
*End of report.*
