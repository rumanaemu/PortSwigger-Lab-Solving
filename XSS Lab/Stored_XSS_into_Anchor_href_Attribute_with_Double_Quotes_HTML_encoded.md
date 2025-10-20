# Lab Report: Stored XSS into Anchor href Attribute with Double Quotes HTML-encoded

## Overview
This lab demonstrates a **Stored Cross-Site Scripting (XSS)** vulnerability that occurs when user-supplied input is embedded into an anchor (`<a>`) element’s **href** attribute. The application HTML-encodes double quotes but fails to validate the URL scheme, allowing the use of `javascript:` pseudo-protocol to execute arbitrary JavaScript.

**Lab Title:** Stored XSS into anchor href attribute with double quotes HTML-encoded  
**Goal:** Execute a stored XSS payload via a crafted URL in an anchor tag’s `href` attribute.

**Payload Used:**
```html
javascript:alert(1)
```

---

## Steps Performed
1. Identified a form for posting comments, which included a “website” field that was used to create an anchor link in the rendered comment section.  
2. Intercepted the form submission using **Burp Suite Repeater** and modified the `website` parameter value to include a JavaScript payload:  
   ```
   website=javascript:alert(1)
   ```
3. Submitted the modified request to the server.  
4. Observed that the payload was stored and reflected inside the anchor element as follows:  
   ```html
   <a href="javascript:alert(1)">...</a>
   ```
5. When visiting the blog page and clicking the link, the JavaScript payload executed successfully.  
6. The Web Security Academy confirmed the successful exploitation with **“Congratulations, you solved the lab!”**.

---

## Evidence

### 1) Payload Injection via Burp Suite
![Payload Injection](/images/Stored%20XSS%20into%20anchor%20href%20attribute%20with%20double%20quotes.jpg)

### 2) Lab Solved Confirmation
![Lab Solved Confirmation](/images/Stored%20XSS%20into%20anchor%20href%20attribute%20with%20double%20quotes%20%20lab%20solved.jpg)

> The screenshots confirm successful XSS payload injection through the anchor `href` attribute and completion of the lab.

---

## Result
- Successfully executed a **Stored XSS** payload through an anchor tag’s `href` attribute.  
- The payload `javascript:alert(1)` triggered a JavaScript popup when the link was clicked.  
- The lab was marked ✅ **Solved**.

---

## Root Cause
- The application failed to validate or sanitize URLs provided by users before embedding them inside anchor attributes.  
- Although double quotes were HTML-encoded, the `javascript:` scheme was still accepted, leading to arbitrary script execution when the link was followed.

---

## Mitigations
1. **URL Scheme Validation:** Only allow safe URL schemes such as `http://` and `https://`.  
2. **Input Sanitization:** Strip or block unsafe protocols like `javascript:`, `data:`, and `vbscript:`.  
3. **Output Encoding:** Encode data properly in HTML attributes to prevent injection.  
4. **Content Security Policy (CSP):** Enforce a strict CSP to block inline script execution.  
5. **Use Safe HTML Builders:** Prefer frameworks or templating engines that automatically escape user input.

---

## References
- [PortSwigger: Stored XSS into anchor href attribute with double quotes HTML-encoded](https://portswigger.net/web-security/cross-site-scripting/stored/lab-anchor-href-double-quotes-html-encoded)  
- [OWASP: Cross-Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/)  
- [MDN: URL Schemes and javascript: protocol](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs)

---
*End of report.*
