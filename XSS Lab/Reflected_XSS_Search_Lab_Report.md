# Lab Report: Reflected XSS (Screenshots Evidence)

## Overview
This report documents a successful **reflected cross-site scripting (XSS)** exploitation of the search functionality. The provided screenshots show the attack URL with the injected payload and the resulting evidence: a browser alert and the lab "Solved" confirmation.

## Goal
Trigger the browser's `alert` function by injecting JavaScript into the search parameter and having it reflected and executed by the application.

## Payload used (as provided)
The exact payload used in the attack (as provided) is shown below:

```
<script>alert('XSS')</script>
```

## Steps performed
1. Open the lab page and identify the search input reflected into the page.  
2. Inject the script payload into the `search` (or `q`) parameter: `?<parameter>=<script>alert('XSS')</script>`.  
3. Submit the request (via address bar or crafted GET request).  
4. Observe the JavaScript alert in the browser — confirms successful code execution.  
5. The lab marks as **Solved** (screenshot evidence included).

## Evidence (screenshots)
### 1) Lab solved banner
![Lab solved banner](/images/XSS%20lab%201%20solved.jpg)

### 2) JavaScript alert triggered by payload
![Alert popup showing XSS](/images/XSS%20lab%201.jpg)

> Both images were provided by the user and show the lab completion banner and the alert popup demonstrating successful reflected XSS.

## Result
- Reflected XSS confirmed: arbitrary JavaScript executed in the context of the vulnerable page.  
- Lab status: **Solved** (confirmation banner visible).

## Recommendations / Mitigations (brief)
- Apply contextual output encoding for all reflected user input.  
- Use `textContent` / equivalent APIs instead of rendering unencoded HTML.  
- Implement a strict Content Security Policy (CSP) to reduce the impact of XSS.  
- Sanitize and validate inputs where appropriate; prefer whitelisting acceptable values.

---
*End of report.*
