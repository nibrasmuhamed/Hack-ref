# ffuf - fuzzer
fuzz faster you fool

## Basics

The Help page can be displayed using `ffuf -h` and it will be useful as we will use a lot of options.  

```bash
Fuzz Faster U Fool - v1.3.0-dev  
  
HTTP OPTIONS:  
  -H                  Header `"Name: Value"`, separated by colon. Multiple -H flags are accepted.  
  -X                  HTTP method to use  
  -b                  Cookie data `"NAME1=VALUE1; NAME2=VALUE2"` for copy as curl functionality.  
  -d                  POST data  
  -ignore-body        Do not fetch the response content. (default: false)  
  -r                  Follow redirects (default: false)  
  -recursion          Scan recursively. Only FUZZ keyword is supported, and URL (-u) has to end in it. (default: false)  
  -recursion-depth    Maximum recursion depth. (default: 0)  
  -recursion-strategy Recursion strategy: "default" for a redirect based, and "greedy" to recurse on all matches (default: default)  
  -replay-proxy       Replay matched requests using this proxy.  
  -timeout            HTTP request timeout in seconds. (default: 10)  
  -u                  Target URL  
  -x                  Proxy URL (SOCKS5 or HTTP). For example: http://127.0.0.1:8080 or socks5://127.0.0.1:8080  
  
GENERAL OPTIONS:  
  -V                  Show version information. (default: false)  
  -ac                 Automatically calibrate filtering options (default: false)  
  -acc                Custom auto-calibration string. Can be used multiple times. Implies -ac  
  -c                  Colorize output. (default: false)  
  -config             Load configuration from a file  
  -maxtime            Maximum running time in seconds for entire process. (default: 0)  
  -maxtime-job        Maximum running time in seconds per job. (default: 0)  
  -p                  Seconds of `delay` between requests, or a range of random delay. For example "0.1" or "0.1-2.0"  
  -rate               Rate of requests per second (default: 0)  
  -s                  Do not print additional information (silent mode) (default: false)  
  -sa                 Stop on all error cases. Implies -sf and -se. (default: false)  
  -se                 Stop on spurious errors (default: false)  
  -sf                 Stop when > 95% of responses return 403 Forbidden (default: false)  
  -t                  Number of concurrent threads. (default: 40)  
  -v                  Verbose output, printing full URL and redirect location (if any) with the results. (default: false)  
  
MATCHER OPTIONS:  
  -mc                 Match HTTP status codes, or "all" for everything. (default: 200,204,301,302,307,401,403,405)  
  -ml                 Match amount of lines in response  
  -mr                 Match regexp  
  -ms                 Match HTTP response size  
  -mw                 Match amount of words in response  
  
FILTER OPTIONS:  
  -fc                 Filter HTTP status codes from response. Comma separated list of codes and ranges  
  -fl                 Filter by amount of lines in response. Comma separated list of line counts and ranges  
  -fr                 Filter regexp  
  -fs                 Filter HTTP response size. Comma separated list of sizes and ranges  
  -fw                 Filter by amount of words in response. Comma separated list of word counts and ranges  
  
INPUT OPTIONS:  
  -D                  DirSearch wordlist compatibility mode. Used in conjunction with -e flag. (default: false)  
  -e                  Comma separated list of extensions. Extends FUZZ keyword.  
  -ic                 Ignore wordlist comments (default: false)  
  -input-cmd          Command producing the input. --input-num is required when using this input method. Overrides -w.  
  -input-num          Number of inputs to test. Used in conjunction with --input-cmd. (default: 100)  
  -input-shell        Shell to be used for running command  
  -mode               Multi-wordlist operation mode. Available modes: clusterbomb, pitchfork (default: clusterbomb)  
  -request            File containing the raw http request  
  -request-proto      Protocol to use along with raw request (default: https)  
  -w                  Wordlist file path and (optional) keyword separated by colon. eg. '/path/to/wordlist:KEYWORD'  
  
OUTPUT OPTIONS:  
  -debug-log          Write all of the internal logging to the specified file.  
  -o                  Write output to file  
  -od                 Directory path to store matched results to.  
  -of                 Output file format. Available formats: json, ejson, html, md, csv, ecsv (or, 'all' for all formats) (default: json)  
  -or                 Don't create the output file if we don't have results (default: false)  
``  

Deploy the machine.

At a minimum we're required to supply two options: `-u` to specify an URL and `-w` to specify a wordlist. The default keyword `FUZZ` is used to tell ffuf where the wordlist entries will be injected. We can append it to the end of the URL like so:

Command for Q1  

`ffuf -u http://10.10.40.30/FUZZ -w /usr/share/seclists/Discovery/Web-Content/big.txt`

You could also use any custom keyword instead of `FUZZ`, you just need to define it like this `wordlist.txt:KEYWORD`.  

`ffuf -u http://10.10.40.30/NORAJ -w /usr/share/seclists/Discovery/Web-Content/big.txt:NORAJ`
```

At a minimum we're required to supply two options: `-u` to specify an URL and `-w` to specify a wordlist. The default keyword `FUZZ` is used to tell ffuf where the wordlist entries will be injected. We can append it to the end of the URL like so:

Command for Q1  

`ffuf -u http://10.10.40.30/FUZZ -w /usr/share/seclists/Discovery/Web-Content/big.txt`

You could also use any custom keyword instead of `FUZZ`, you just need to define it like this `wordlist.txt:KEYWORD`.  

`ffuf -u http://10.10.40.30/NORAJ -w /usr/share/seclists/Discovery/Web-Content/big.txt:NORAJ`


## Finding pages and directories

One approach you could take would be to start enumerating with a generic list of files such as raft-medium-files-lowercase.txt.

Command for Q1  

`ffuf -u http://10.10.40.30/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt`

However, using a large generic wordlist containing irrelevant file extensions is not very efficient.

Instead, we can usually assume `index.<extension>` is the default page on most websites so we can try common extensions for just the index page. With this method, we can usually determine what programming language or languages the site uses.

For example, we can append the extension after index.

```bash
head /usr/share/seclists/Discovery/Web-Content/web-extensions.txt                                                              
.asp  
.aspx  
.bat  
.c  
.cfm  
.cgi  
.css  
.com  
.dll  
.exe`  
```
Command for Q2

`ffuf -u http://10.10.40.30/indexFUZZ -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt`

Now that we know the extensions supported we can try a list of generic words (without of extension) and apply the extensions we know works (found from Q2) + some common ones like `.txt`.

We'll exclude the 4 letter extensions from this wordlist as it will result in many false positives.

Command for Q3

`ffuf -u http://10.10.40.30/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt -e .php,.txt`  

Directory names are not always dependent on the type of environment you're enumerating and is often a good starting point before attempting to fuzz for files. If we wanted to fuzz directories we only need to provide a wordlist.

Command for Q4

`ffuf -u http://10.10.40.30/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories-lowercase.txt`

## Filters

Remember the command we ran for Q1 of Task 3?

`$ ffuf -u http://MACHINE_IP/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt  
  
        /'___\  /'___\           /'___\  
       /\ \__/ /\ \__/  __  __  /\ \__/  
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\  
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/  
         \ \_\   \ \_\  \ \____/  \ \_\  
          \/_/    \/_/   \/___/    \/_/  
  
       v1.3.1-dev  
________________________________________________  
  
 :: Method           : GET  
 :: URL              : http://10.10.130.176/FUZZ  
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt  
 :: Follow redirects : false  
 :: Calibration      : false  
 :: Timeout          : 10  
 :: Threads          : 40  
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405  
________________________________________________  
  
favicon.ico             [Status: 200, Size: 1406, Words: 5, Lines: 2]  
.htaccess               [Status: 403, Size: 289, Words: 21, Lines: 11]  
logout.php              [Status: 302, Size: 0, Words: 1, Lines: 1]  
robots.txt              [Status: 200, Size: 26, Words: 3, Lines: 2]  
phpinfo.php             [Status: 302, Size: 0, Words: 1, Lines: 1]  
.                       [Status: 302, Size: 0, Words: 1, Lines: 1]  
php.ini                 [Status: 200, Size: 148, Words: 17, Lines: 5]  
about.php               [Status: 200, Size: 4840, Words: 331, Lines: 109]  
.html                   [Status: 403, Size: 285, Words: 21, Lines: 11]  
login.php               [Status: 200, Size: 1523, Words: 89, Lines: 77]  
.php                    [Status: 403, Size: 284, Words: 21, Lines: 11]  
setup.php               [Status: 200, Size: 4067, Words: 308, Lines: 123]  
.htpasswd               [Status: 403, Size: 289, Words: 21, Lines: 11]  
security.php            [Status: 302, Size: 0, Words: 1, Lines: 1]  
.htm                    [Status: 403, Size: 284, Words: 21, Lines: 11]  
.htpasswds              [Status: 403, Size: 290, Words: 21, Lines: 11]  
index.php               [Status: 302, Size: 0, Words: 1, Lines: 1]  
.htgroup                [Status: 403, Size: 288, Words: 21, Lines: 11]  
wp-forum.phps           [Status: 403, Size: 293, Words: 21, Lines: 11]  
.htaccess.bak           [Status: 403, Size: 293, Words: 21, Lines: 11]  
.htuser                 [Status: 403, Size: 287, Words: 21, Lines: 11]  
.ht                     [Status: 403, Size: 283, Words: 21, Lines: 11]  
.htc                    [Status: 403, Size: 284, Words: 21, Lines: 11]  
:: Progress: [16243/16243] :: Job [1/1] :: 1690 req/sec :: Duration: [0:00:13] :: Errors: 0 ::`  
  
We had a lot of output but not much was useful.  
For example, a 403 [HTTP status code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) indicates that we're forbidden to access the requested resource. Let's hide responses with 403 status codes for now. We can accomplish this by using filters.

By adding `-fc 403` (filter code) we'll hide from the output all 403 [HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).  
  
Command for Q1

`ffuf -u http://10.10.32.5/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -fc 403`  
  

Sometimes you might want to filter out multiple status codes such as 500, 302, 301, 401, etc. For instance, if you know you want to see 200 status code responses, you could use `-mc 200` (match code) instead of having a long list of filtered codes.

Command for Q2  

`ffuf -u http://10.10.32.5/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -mc 200`  

Sometimes it might be beneficial to see what requests the server doesn't handle by matching for HTTP 500 Internal Server Error response codes (`-mc 500`). Finding irregularities in behavior could help better understand how the web app works.  
  
There are other filters and matchers. For example, you could encounter entries with a 200 status code with a response size of zero. eg. `functions.php` or `inc/myfile.php`.  

`$ ffuf -u http://MACHINE_IP/config/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -fc 403  
...  
.                       [Status: 200, Size: 1165, Words: 76, Lines: 18]  
config.inc.php          [Status: 200, Size: 0, Words: 1, Lines: 1]  
:: Progress: [16243/16243] :: Job [1/1] :: 1732 req/sec :: Duration: [0:00:13] :: Errors: 0 ::`  

Unless we have a LFI (local file inclusion) this kind of files aren't interesting, so we can use `-fs 0` (filter size).

Here are all filters and matchers:  

`$ ffuf -h  
...  
MATCHER OPTIONS:  
  -mc                 Match HTTP status codes, or "all" for everything. (default: 200,204,301,302,307,401,403,405)  
  -ml                 Match amount of lines in response  
  -mr                 Match regexp  
  -ms                 Match HTTP response size  
  -mw                 Match amount of words in response  
  
FILTER OPTIONS:  
  -fc                 Filter HTTP status codes from response. Comma separated list of codes and ranges  
  -fl                 Filter by amount of lines in response. Comma separated list of line counts and ranges  
  -fr                 Filter regexp  
  -fs                 Filter HTTP response size. Comma separated list of sizes and ranges  
  -fw                 Filter by amount of words in response. Comma separated list of word counts and ranges  
...  
`

We often see there are false positives with files beginning with a dot (eg. `.htgroups`,`.php`, etc.). They throw a 403 Forbidden error, however those files don't actually exist. It's tempting to use `-fc 403` but this could hide valuable files we don't have access to yet. So instead we can use a regexp to match all files beginning with a dot.

Command for Q3

`ffuf -u http://10.10.32.5/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -fr '/\..*'`

## Fuzzing parameters

What would you do when you find a page or API endpoint but don't know which parameters are accepted? You fuzz!

Discovering a vulnerable parameter could lead to file inclusion, path disclosure, XSS, SQL injection, or even command injection. Since ffuf allows you to put the keyword anywhere we can use it to fuzz for parameters.

Command for Q1

`$ ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?FUZZ=1' -c -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -fw 39  
$ ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?FUZZ=1' -c -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt -fw 39  
`

Now that we found a parameter accepting integer values we'll start fuzzing values.

At this point, we could generate a wordlist and save a file containing integers. To cut out a step we can use `-w -` which tells ffuf to read a wordlist from [stdout](https://www.gnu.org/software/libc/manual/html_node/Standard-Streams.html). This will allow us to generate a list of integers with a command of our choice then pipe the output to ffuf. Below is a list of 5 different ways to generate numbers 0 - 255.

Commands for Q2

`$ ruby -e '(0..255).each{|i| puts i}' | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33  
$ ruby -e 'puts (0..255).to_a' | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33  
$ for i in {0..255}; do echo $i; done | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33  
$ seq 0 255 | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33  
$ cook '[0-255]' | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33`  
  
We can also use ffuf for wordlist-based brute-force attacks, for example, trying passwords on an authentication page.

Command for Q3  
  
 `$ ffuf -u http://MACHINE_IP/sqli-labs/Less-11/ -c -w /usr/share/seclists/Passwords/Leaked-Databases/hak5.txt -X POST -d 'uname=Dummy&passwd=FUZZ&submit=Submit' -fs 1435 -H 'Content-Type: application/x-www-form-urlencoded'`

Here we have to use the POST method (specified with `-X`) and to give the POST data (with `-d`) where we include the `FUZZ` keyword in place of the password.

We also have to specify a custom header `-H 'Content-Type: application/x-www-form-urlencoded'` because ffuf doesn't set this content-type header automatically as curl does.




## Finding vhosts and subdomains

ffuf may not be as efficient as specialized tools when it comes to subdomain enumeration but it's possible to do.

Command for Q1

`$ ffuf -u http://FUZZ.tryhackme.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt`

Some subdomains might not be resolvable by the DNS server you're using and are only resolvable from within the target's local network by their private DNS servers. So some virtual hosts (vhosts) may exist with private subdomains so the previous command doesn't find them. To try finding private subdomains we'll have to use the Host HTTP header as these requests might be accepted by the web server.  
**Note**: [virtual hosts](https://httpd.apache.org/docs/2.4/en/vhosts/examples.html) (vhosts) is the name used by Apache httpd but for Nginx the right term is [Server Blocks](https://www.nginx.com/resources/wiki/start/topics/examples/server_blocks/).  

Compare the results you obtain with direct subdomain enumeration and with vhost enumeration:

Commands for Q2  

`$ ffuf -u http://FUZZ.google.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -fs 0  
$ ffuf -u http://google.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H 'Host: FUZZ.google.com' -fs 0`  

For example there is a subdomain aol.google.com you can't find it with direct subdomain enumeration (1st command) but that you can find with vhost enumeration (2nd command).

We can use drill or dig to find the IP address of the root domain:  

`$ drill google.com  
...  
google.com.     104     IN      A       216.58.204.110  
...`  

If you enter this domain directly in your web browser or with curl, you won't be able to resolve it:

`$ curl https://aol.google.com  
curl: (6) Could not resolve host: aol.google.com`  

But now if you add it in `/etc/hosts` or force the resolution with curl you'll be able to browse it:

`$ curl --resolve aol.google.com:443:216.58.198.206 https://aol.google.com  
<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="fr">  
...`  

Here the aol subdomain is not very interesting as it's an alias for www.google.com and other subdomains are redirecting aliases. However, you shouldn't discount this enumeration technique as it may lead to discovering content that wasn't meant to be accessed externally.
## Proxifying ffuf traffic

Whether it' for [network pivoting](https://blog.raw.pm/en/state-of-the-art-of-network-pivoting-in-2019/) or for using BurpSuite plugins you can send all the ffuf traffic through a web proxy (HTTP or SOCKS5).  

`$ ffuf -u http://10.10.102.40/ -c -w /usr/share/seclists/Discovery/Web-Content/common.txt -x http://127.0.0.1:8080`

It's also possible to send only matches to your proxy for replaying:

`$ ffuf -u http://FUZZ.tryhackme.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -replay-proxy http://127.0.0.1:8080`

This may be useful if you don't need all the traffic to traverse an upstream proxy and want to minimize resource usage or to avoid polluting your proxy history.
