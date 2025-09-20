# Lab Report: Insecure Direct Object References (IDOR)

## Overview
This lab from **PortSwigger Web Security Academy** explores an **Insecure Direct Object References (IDOR)** vulnerability.  
In this scenario, user chat logs are stored as files on the server and accessed via static URLs. The task is to retrieve a password for user **carlos** by manipulating access to these files, then use those credentials to log into their account.

**Lab Goal:**
- Find the password for user `carlos`  
- Log in as `carlos`

---

## Steps to Exploitation

1. **Access the Live Chat / Send a Message**  
   - Navigate to the “Live chat” section.  
   - Initiate or send a message to ensure a transcript is generated.

2. **View the Transcript & Analyze the Filename**  
   - Click “View transcript” for a chat session.  
   - Observe the URL / downloaded file: something like `/download-transcript/2.txt`. The number is incrementing per transcript. :contentReference[oaicite:0]{index=0}

3. **Modify the Transcript Filename**  
   - Using a proxy tool (e.g. Burp Suite), or via browser dev tools + HTTP history, locate the request for `2.txt`. :contentReference[oaicite:1]{index=1}  
   - Change the filename in the URL to `1.txt` (or another earlier number). :contentReference[oaicite:2]{index=2}

4. **Retrieve the Content**  
   - Send the modified request.  
   - Inspect the response; you’ll find a chat transcript containing the password for `carlos`. :contentReference[oaicite:3]{index=3}

5. **Log In as Carlos**  
   - Go to the login page.  
   - Use the discovered credentials to log in as `carlos`.  
   - Confirm that access is successful. :contentReference[oaicite:4]{index=4}

---

## Result
- Successfully obtained the password of user **carlos** from an earlier transcript (`1.txt`).  
- Logged in as `carlos`.  
- Lab status: ✅ *“Congratulations, you solved the lab!”*

---

## Root Cause
- The application uses file names (transcripts) that are predictable (incrementing numbers).  
- There is **no authorization check** to ensure that only the owner of a transcript can access a particular file.  
- Because of this, an attacker can guess or try other transcript names and access data (such as passwords) belonging to other users.  

---

## Mitigation Recommendations
1. **Implement strict access controls**  
   - Ensure that only the owner of a resource (e.g. transcript) can access it.  
   - Validate that the authenticated user matches the resource’s user before returning file contents.

2. **Avoid predictable file names / identifiers**  
   - Use randomized or non-guessable identifiers (e.g., UUIDs, hashes) if users need to reference or download resources.  

3. **Authentication + Authorization at every request**  
   - Even static resources (files) should go through authorization checks.  
   - The server should not rely solely on the URL to determine access.

4. **Least privilege & data minimization**  
   - Chat transcripts should avoid containing sensitive data like passwords.  
   - Passwords should *never* be included in documents or logs visible to users.

5. **Logging and monitoring**  
   - Track file‐access events, especially for downloads of transcripts.  
   - Alert on unusual access patterns (e.g. repeated requests for transcript files).

---

## Skills Demonstrated
- Use of HTTP proxies and intercepting tools (e.g. Burp Suite).  
- URL / parameter manipulation (IDOR exploitation).  
- Analyzing file naming patterns to infer resource access.  
- Ethical hacking methodology & systematic testing.  

---

## References
- [PortSwigger Lab: Insecure Direct Object References](https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references) 

---

## Screenshots
![Lab Solved](/images/IDOR-Lab1.jpg)  

![Password Found in Transcript](/images/IDOR-Lab2-PasswordFound.jpg)  


