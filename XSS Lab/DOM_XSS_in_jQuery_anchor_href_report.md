# Lab Report: DOM XSS in jQuery Anchor `href` Attribute Sink using `location.search` Source

## Overview
This lab demonstrates a **DOM-based Cross-Site Scripting (XSS)** vulnerability in a web page that dynamically updates an anchor element’s `href` attribute using untrusted data from `location.search`.  
Because the input is not validated or sanitized, attackers can inject a `javascript:` URI to execute arbitrary code when the link is clicked or automatically processed by JavaScript.

**Lab Title:** DOM XSS in jQuery anchor `href` attribute sink using `location.search` source  
**Goal:** Exploit the vulnerable anchor attribute to execute JavaScript and trigger an alert.

**Payload Used:**
```javascript
javascript:alert(1)
```

---

## Steps Performed
1. Observed that the web page used **jQuery** to dynamically update the `href` attribute of a link based on the query string in `location.search`.  
2. Determined that no input sanitization was performed, allowing injection of a `javascript:` URL.  
3. Injected the payload `javascript:alert(1)` into the `returnPath` parameter.  
4. Upon clicking the link, the browser executed the injected JavaScript code, displaying an alert popup.  
5. The lab displayed **“Congratulations, you solved the lab!”**, confirming successful exploitation.

---

## Evidence

### 1) Lab Solved Confirmation
![Lab Solved Screenshot](/images/Dom%20XSS%20in%20jQuery%20anchor%20href.jpg)

> The screenshot shows the `javascript:alert(1)` payload in the URL and the Web Security Academy’s “Lab Solved” confirmation banner.

---

## Result
- Successfully achieved **DOM XSS** by injecting a `javascript:` payload into a dynamically generated anchor `href`.  
- JavaScript executed successfully when the link was activated.  
- Lab marked ✅ *Solved*.

---

## Root Cause
- jQuery used untrusted user input from `location.search` to set the value of the `href` attribute without sanitization.  
- Lack of validation allowed the insertion of a `javascript:` URI, which browsers interpret as executable JavaScript.  
- This resulted in arbitrary code execution in the browser context.

---

## Mitigations
1. **Avoid assigning untrusted input to href attributes.** Only allow safe URL schemes such as `https` and `mailto`.  
2. **Sanitize input:** Use a whitelist or validation function to block `javascript:` and other dangerous schemes.  
3. **Use `attr()` safely:** In jQuery, avoid setting attributes directly with user-controlled data.  
4. **Implement Content Security Policy (CSP):** Prevent execution of injected JavaScript by blocking inline scripts.  
5. **Framework-level protection:** Use modern frameworks that auto-sanitize and validate dynamic DOM manipulations.

---

## References
- [PortSwigger Web Security Academy: DOM XSS in jQuery anchor href attribute](https://portswigger.net/web-security/cross-site-scripting/dom-based/jquery-href-attribute-sink)  
- [OWASP: DOM-Based Cross-Site Scripting (XSS)](https://owasp.org/www-community/attacks/DOM_Based_XSS)  
- [MDN: `javascript:` URL Scheme](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs#javascript_uris)  
- [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)

---
*End of report.*
