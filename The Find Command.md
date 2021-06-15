# the find command

#linuxcommand 
#find 


## find
- find / -> used for specifing from which directory to find 
- find -type -> used to specify type
	- `d` used for directories
	- `f` used for files
- find `-name` used to specify name of file
	- `-name user.txt` returns exact match
	- `-iname user.txt` is case sensitive
	- `-name "*exploit*"` will return every file whose name contains the word exploit
- find `-user` will return all files with in users ownership

- find `-size` will return files with specified size 
	- `-n ` used to search lesser sized than input
		- `find /home -size -30k` will return file less than 30kb
	- `+n` return greater
	- `n ` returns same size files
	- `c ` suffix used to specify bytes and `k ` used to specify kib
		- `+30c
		- `-30k
- find `-perm ` used to specify permission
	- `644` and `u=r` will only returns exact permission files
	- `-` will return atleast the permission you specify
			- `-444` will return all files that are readable for everyone who even has permission to write and execute
		- `/ ` will return files that are match any of permission that you've set
		- `/666` will return files that are readable and writable for atleast one of the owner, group or others
		- Find all files with write permission for the group "others", regardless of any other permissions, with extension ".sh" (use symbolic format)
			- `find / -type f -perm -o=w -name "*.sh"`
- find `-exec ` can execute command
	- `find -exec whoami \;`
- find `2 >/dev/null `
	- will not print those files which is not accessible for us 