# curl fundamentals


GET request. Make a GET request to the web server with path /ctf/get
```
curl http://10.10.123.227:8081/ctf/get
```

POST request. Make a POST request with the body "flag\_please" to /ctf/post
```
curl \-X POST \--data "flag\_please" http://10.10.123.227:8081/ctf/post
```

Get a cookie. Make a GET request to /ctf/getcookie and check the cookie the server gives you
```
curl --cookie-jar README.md http://10.10.123.227:8081/ctf/getcookie
```
```
curl -c - http://10.10.123.227:8081/ctf/getcookie
```

Set a cookie. Set a cookie with name "flagpls" and value "flagpls" in your devtools (or with curl!) and make a GET request to /ctf/sendcookie
```
curl \-v \--cookie 'flagpls=flagpls' http://10.10.123.227:8081/ctf/sendcookie
```

Changing User-agent using curl
```
curl -A "R" -L [http://10.10.23.158/](http://10.10.23.158/)`

```
```
curl "http://10.10.23.158" -H "User-Agent: R" -L
```
