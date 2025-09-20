# Lab Report: OS Command Injection (Simple Case)

## Overview
This lab from **PortSwigger Web Security Academy** demonstrates a simple OS command injection vulnerability in a product stock checker endpoint. The application uses user-supplied input (`productId` and `storeId`) to build a shell command, then returns the raw output of that command. 

**Lab Goal:**  
- Inject a command (e.g. `whoami`) via the vulnerable parameter  
- Determine the name of the current system user executing the application 

---

## Steps to Exploitation

1. **Locate the vulnerable request**  
   - Used the stock-checker feature.  
   - Observed a request to `/product/stock` (or similar) with parameters `productId` and `storeId`. 

2. **Intercept the request**  
   - Sent the request through a proxy tool (e.g. Burp Suite).  
   - Confirmed that `storeId` is being passed into a shell command, because certain special characters cause shell-style errors. 

3. **Test for command injection**  
   - Injected `|whoami` (or `; whoami`, `&& whoami`) into the `storeId` parameter.   
   - Sent the modified request.  

4. **Observe the output**  
   - The response included the result of `whoami`, showing the system user of the process running the application.   

5. **Finish lab**  
   - Captured the necessary output (the username).  
   - Submitted or used it to validate that the vulnerability was exploited.  

---

## Result
- The command injection was successful.  
- The response showed the output of the `whoami` command.  
- Lab completed: ✅ *Simple OS command injection exploited* 

---

## Root Cause
- The application directly includes user-supplied input in the shell command without sanitization or escaping.  
- There is no whitelist validation or filtering to ensure only safe input (e.g. numeric values) is accepted.  
- The server trusts the input blindly, allowing execution of arbitrary OS commands.  

---

## Mitigation Recommendations
1. **Input validation & sanitization**  
   - Only allow expected input shape (e.g. numeric IDs).  
   - Reject any input containing metacharacters like `|`, `;`, `&&`, etc., or escape them properly.

2. **Use parameterized or safer APIs**  
   - Avoid passing user input directly into shell commands.  
   - If possible, use language APIs that avoid shell invocation.

3. **Least privilege execution**  
   - Run services with minimal permissions so that even if command injection occurs, the impact is limited.

4. **Logging & monitoring**  
   - Log suspicious input or errors.  
   - Monitor responses for signs of unexpected command output.

---

## Skills Demonstrated
- Use of HTTP intercepting proxy (Burp Suite or equivalent)  
- Crafting and injecting malicious payloads  
- Analysis of server responses to detect successful command execution  
- Understanding of OS command injection vulnerability  

---

## References
- [PortSwigger Lab: OS Command Injection, Simple Case](https://portswigger.net/web-security/os-command-injection/lab-simple)   
- Writing-ups & walkthroughs from community sources   

---

## Screenshots
![Injection Payload & Whoami Output](/images/Code-Injection-Lab1.jpg)  


