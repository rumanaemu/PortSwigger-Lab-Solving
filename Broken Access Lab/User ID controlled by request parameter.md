# Lab Report: User ID Controlled by Request Parameter, with Unpredictable User IDs

## Overview
This lab from **PortSwigger Web Security Academy** demonstrates an access control vulnerability 
where a user's account is accessed using a request parameter (`id`) in the URL.  
Even though user IDs are **unpredictable**, the application does not enforce proper 
authorization checks. As a result, an attacker can still access another user’s account 
by modifying the parameter.

**Lab Goal:**
- Access the account page of another user.

**Credentials provided:**
- Username: `wiener`
- Password: `peter`

---

## Steps to Exploitation

### 1. Login as Normal User
- Logged in using the provided credentials (`wiener:peter`).
- Navigated to the "My Account" page.
- Observed that the account page URL contained a parameter like:  

### 2. Identifying the Vulnerability
- The application relies on the request parameter `id` to determine which account to load.  
- IDs are not sequential or predictable (e.g., not `1,2,3`), but can still be guessed or 
obtained from other parts of the application (e.g., response data, hidden fields).

### 3. Attempting Account Takeover
- Modified the `id` parameter in the URL from:

## Result
- Successfully accessed the victim account (`carlos`).  
- Lab status changed to:  
✅ *"Congratulations, you solved the lab!"*

## Root Cause
The vulnerability exists because the application:
- Uses a **request parameter (`id`)** to control which account is displayed.  
- Does not verify on the **server-side** whether the logged-in user is authorized 
to view the requested account.  

Even with unpredictable IDs, the absence of **access control checks** allows attackers 
to view or manipulate other users’ accounts.  

---

## Mitigation Recommendations
1. **Enforce server-side authorization checks**  
 - Ensure that users can only access their own resources, regardless of ID manipulation.  

2. **Avoid relying on request parameters for access control**  
 - Use the logged-in session to determine which account data to return.  

3. **Implement proper Access Control**  
 - Apply the principle of least privilege.  
 - Protect all endpoints against Insecure Direct Object Reference (IDOR).  

4. **Monitor and test for IDOR vulnerabilities**  
 - Regular penetration testing and automated checks should be performed.  

---

## Skills Demonstrated
- Authentication and session testing  
- Parameter tampering  
- Broken access control (IDOR) exploitation  
- Web security testing methodology  

---

## References
- [PortSwigger: Insecure direct object references (IDOR)](https://portswigger.net/web-security/access-control/idor)

---

## Screenshots
![Lab Solved Screenshot](/images/images/Broken-Access-Lab2.jpg) 