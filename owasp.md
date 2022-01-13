# broken access control.

this vulnerability arises when a specific user is autheticated and can do certain things which he is not authorised to do.


# cryptographic failures.
previosly know as sensitive data exposure. when a webapp expose sensitive data within webapp. for eg password, credit card number, health record, personal information and business secrets needs extra protection.
is any data transfered in clear text or in http, smtp, ftp. 
any weak or old cryptographic algorithm.

# injection.
this vulnerability allows hackers to execute arbitrary system commands on victim machine. this may lead to full server compromise.
 By doing this, they can override the original command to gain access to a system, obtain sensitive data, or even execute an entire takeover of the application server or system
 -   User-supplied data is not validated, filtered, or sanitized by the application.
    
-   Dynamic queries or non-parameterized calls without context-aware escaping are used directly in the interpreter.

# insecure design.
this focuses on risk related to design and architechtural flows. weakly designed application will be more vulnerable. owasp tells we need to move beyond shift left inthe coding space to precode activities that are critical for the principle of secure design.
-   Establish and use a secure development lifecycle with AppSec professionals to help evaluate and design security and privacy-related controls
    
-   Establish and use a library of secure design patterns or paved road ready to use components

# Security misconfiguration.
security misconfiguration usually occurs from human mistakes. for eg: not changing default credential. improperly configured permission.
-   Unnecessary features are enabled or installed (e.g., unnecessary ports, services, pages, accounts, or privileges).

# Vulnerable and outdated components.
using previosly titled using components with known vulnerability.  let's say that a company hasn't updated their version of WordPress for a few years, and using a tool such as wpscan, you find that it's version 4.6. Some quick research will reveal that WordPress 4.6 is vulnerable to an unauthenticated remote code execution(RCE) exploit, and even better you can find an exploit already made on [exploit-db](https://www.exploit-db.com/exploits/41962).

# authentication failures
Authentication and session management constitute core components of modern web applications. Authentication allows users to gain access to web applications by verifying their identities.

# software and data integrity failure
An example of this is where an application relies upon plugins, libraries, or modules from untrusted sources, repositories, and content delivery networks (CDNs). An insecure CI/CD pipeline can introduce the potential for unauthorized access, malicious code, or system compromise.

# security logging and monitoring failure
-   Auditable events, such as logins, failed logins, and high-value transactions, are not logged.
    
-   Warnings and errors generate no, inadequate, or unclear log messages.
    
-   Logs of applications and APIs are not monitored for suspicious activity.
    
-   Logs are only stored locally.

# ssrf
SSRF flaws occur whenever a web application is fetching a remote resource without validating the user-supplied URL. It allows an attacker to coerce the application to send a crafted request to an unexpected destination, even when protected by a firewall, VPN, or another type of network access control list (ACL).
