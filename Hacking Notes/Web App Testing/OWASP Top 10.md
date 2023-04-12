# Broken Access Control
[A01 Broken Access Control - OWASP Top 10:2021](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)  
  
**Access Control Vulnerabilities include:**  
• Lack of least privilege or default deny policies  
• Bypassing access control via URL/application tampering  
• Insecure Direct Object References  
• API without controls for POST, PUT, DELETE  
• Elevation of privilege by acting as a user w/o logging in  
• Replaying/tampering with tokens  
• CORS misconfiguration allows API access from untrusted origins  
  
**Prevention**  
• Deny by default  
• Disable directory listing  
• No metadata in web roots  
• Log/alert on access control failures  
• Rate limit API access  
• Re-use access control mechanisms and minimize CORS usage  
• Stateful session identifiers should be invalidated on the server after user logs out

# Cryptographic Failures
Overview  
[https://owasp.org/Top10/A02_2021-Cryptographic_Failures](https://owasp.org/Top10/A02_2021-Cryptographic_Failures)  
  
Prevention  
• Identify and classify storage, processing and transmission of data and all relevant regulatory requirements  
• Don't store unnecessary data  
• Use updated algorithms and protocols  
• Use authenticated encryption where possible  
• Encrypt sensitive data at rest  
  
Example
# XSS Injection
Overview  

Prevention  
• Use safe API that avoids interpreter, provides parameterized interface or migrates to ORMs  
• Use positive server-side input validation  
• Escape special character using specific escape syntax for that interpreter  
• Use LIMIT and other SQL access controls  
Example  

XSS is injection attack to execute malicious scripts on victim machine  

Vulnerable if app has unsanitized input. JS, VBS, Flash, CSS  

Stored = Inserted into DB  
Reflected = payload is part of victims request (attacker must phish user)  
DOM-based = executed as part of JS (document.querySelector().innerHTML)  
  
  
  
[XSS Payloads](http://www.xss-payloads.com/)  
- Keylogging  
- Port scanning  
- Defacing/vandalizing  
-  
[List of useful payloads for appsec & pentest](https://github.com/swisskyrepo/PayloadsAllTheThings)
# Insecure Design
Overview  
  
Prevention  
• Establish a secure software development lifecycle (SDLC) with application security professionals  
• Integrate security language into user stories / requirements  
• Use threat models  
• Integrate plausibility checks and tests  
• Write unit/integration tests to validate critical flows resist the threat model  
• Limit resource consumption by user/service  
  
Example
# Security Misconfiguration
Overview  
  
Prevention  
• Establish standardized hardening process for environment. Dev, QA, prod should have identical configs with different creds in each environment  
• Remove or avoid installing unused features & frameworks  
• Review permissions & configurations after patching  
• Enable security headers  
• Establish automated process to verify effectiveness of configs in all environments  
• Segment architecture between components/tenants  
• Change default configurations, credentials and permissions  
  
Example
# Vulnerable / Outdated Components
Overview  
  
Prevention  
• Remove unused dependencies, components files and documentation  
• Continuously monitor NVD for new CVEs  
• Continuously inventory versions of software, hardware and dependencies (OWASP Dependency Check, retire.js)  
• Obtain components from official services over secure links. Preferably signed packages  
• Monitor for unmaintained components/libraries that do not offer security patches for older version.  
Example
# Identification & Authentication Failures
Overview  
  
Prevention  
• Implement Multi-Factor Authentication  
• Do not ship/deploy with default credentials, especially for administrative users  
• Implement weak password checks against list of common passwords  
• Align password length, complexity and rotation requirements with NIST 800-63b  
• Use standardized and useful (yet vague) error messaging to prevent enumeration attacks  
• Limit failed login attempts  
• Use server-side session manager with high entropy Session ID (such as time + HMAC). Invalidated on the server after logout, idle or absolute timeout.  
  
Example
# Software/Data Integrity Failures
Overview  
  
Prevention  
• Use digital signatures to verify integrity of software  
• Ensure dependencies are sourced from trusted author/repositories  
• Use internal software supply chain tools (OWASP Dependency Checker or Cyclone DX) to verify components are free of common vulnerabilities  
• Establish/ensure the existence of a process for secure code reviews and configuration changes  
• Ensurce proper segmentation, configuration and access control of CI/CD tooling  
• Don't send unsigned/unencrypted serialized data without integrity check or digital signature  
  
Example
# Security Logging & Monitoring Failures
Overview  
  
Prevention  
• Ensure login/access control and validation is logged with sufficient user context and held long enough for forensic examination  
• Logs generated in an easy to consume format for log managers  
• Encode log data correctly to preserve integrity  
• Audit high value transactions  
• Preserve integrity (use only append operations)  
• Consult NIST 800-61r2  
  
Example
# Server-Side Request Forgery
Overview  
  
Prevention  
Network Layer  
• Segment remote resource functionality to separate networks  
• Enforce deny-by-default firewall or NAC policies  
  
Application Layer  
• Sanitize and validate user input  
• Enforce URL schema, port and destination with positive allow list  
• Do not send raw responses to clients  
• Disable HTTP redirect (and/or use HTST)  
  
Example

