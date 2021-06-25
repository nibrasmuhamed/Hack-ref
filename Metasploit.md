# Metasploit
Metasploit, an open-source pentesting framework, is a powerful tool utilized by security engineers around the world. Maintained by Rapid 7, Metasploit is a collection of not only thoroughly tested exploits but also auxiliary and post-exploitation tools.


## Metasploit core
metasploit written ruby
to start metasploit 
```
sudo msfconsole
```
`metasploit -h ` will print core commands
to start db for metasploit
```
msfdb init
```
to get basic idea about metasploit, run
```
help
?
```
to search anything on metasploit
```
search <keyword>
```
to use any modules shown on search result
```
use <id>
```
command used to display one or more information about modules is
```
info
```
Metasploit has a built-in netcat-like function where we can make a quick connection with a host simply to verify that we can 'talk' to it
```
connect
```
to configure host or port to any module, we use command
```
set
```
the ` ` command will set lhost or lport globally while focusing on single box
```
setg
```
command will save output to file as well as screen
```
spool
```
![[iYuiWvP.png]]
- Exploit modules are the most utilised modules which holds all exploits

- Auxiliary module is most commonly used in scanning and verification machines are exploitable, this isn't same as actual exploitation of course

- Post module will allow us to loot and pivot on exploited machine

- Commonly utilized in payload obfuscation, which module allows us to modify the 'appearance' of our exploit such that we may avoid signature detection? is encoder

- NOP module is used with buffer overflow and ROP attacks

- load command will load modules

Metasploit comes with a built-in way to run nmap and feed it's results directly into our database. We can run that now by using the command `db_nmap -sV MACHINE_IP`

Let's go ahead and see what information we have collected in the database. Try typing the command `hosts` into the msfconsole now.

How about something else from the database, try the command `services` now.

One last thing, try the command `vulns` now. This won't show much at the current moment, however, it's worth noting that Metasploit will keep track of discovered vulnerabilities. One of the many ways the database can be leveraged quickly and powerfully.


