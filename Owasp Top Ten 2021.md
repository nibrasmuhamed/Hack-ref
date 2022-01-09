
# Owasp Top Ten 2021

The Open Web Application Security Project® (OWASP) is a nonprofit foundation that works to improve the security of software. Through community-led open-source software projects, hundreds of local chapters worldwide, tens of thousands of members, and leading educational and training conferences, the OWASP Foundation is the source for developers and technologists to secure the web.

# 1. Broken Access Control

Access control enforces policy such that users cannot act outside of their intended permissions. Failures typically lead to unauthorized information disclosure, modification, or destruction of all data or performing a business function outside the user's limits. Common access control vulnerabilities include:

-   Violation of the principle of least privilege or deny by default, where access should only be granted for particular capabilities, roles, or users, but is available to anyone.
    
-   Bypassing access control checks by modifying the URL (parameter tampering or force browsing), internal application state, or the HTML page, or by using an attack tool modifying API requests.
    
-   Permitting viewing or editing someone else's account, by providing its unique identifier (insecure direct object references)
    
-   Accessing API with missing access controls for POST, PUT and DELETE.
    
-   Elevation of privilege. Acting as a user without being logged in or acting as an admin when logged in as a user.
    
-   Metadata manipulation, such as replaying or tampering with a JSON Web Token (JWT) access control token, or a cookie or hidden field manipulated to elevate privileges or abusing JWT invalidation.
    
-   CORS misconfiguration allows API access from unauthorized/untrusted origins.
    
-   Force browsing to authenticated pages as an unauthenticated user or to privileged pages as a standard user.


## Test Scenarios
Users In different levels:
	- normal user
	- admin
	- super user
	- audit
webapp.com/admin_info
webapp.com/acctinfo?acct=1234

 Totally depend on authentication and session managment
 - **authentication** is identifying persons that they are who they are saying.
 - **session managment** identifying which subsequent HTTP requests are being made by that same user.
 - **access control** detemines whether the user is allowed to carry out the acrion that they are attempting to perform.

### Broken access control from userside
- vertical access control 
- horizontal access control
- context-dependent access controls

#### vertical access control
web apps  have different previleged user like admin and user. admin have the previlege to delete user account. in short admin have more privileges than user. 

#### Horizontal access control
horizontal access controls is accessing another users data within our account. this mechanism access to resources to the users who are specifically allowed to access those resources.

#### Context-dependent access controls
context-dependent access control depend on user's interaction. for example a retail web site won't  allow users to modify contents after their payment.

#### Examples of broken access controls

Broken access control vulnerabilities exist when a user can in fact access some resource or perform some action that they are not supposed to be able to access.

#### Vertical privilege escalation

If a user can gain access to functionality that they are not permitted to access then this is vertical privilege escalation. For example, if a non-administrative user can in fact gain access to an admin page where they can delete user accounts, then this is vertical privilege escalation.


***#1*** lab - enumerating robots.txt and checking direct access to restricted areas for bots

***#2*** some webapps use url less predictable. eg insecureweb.com/administrator-panel-yb1234
there may be some situation where url leak for users.
checking source code.

***#3*** parameeter based access cotrol methods: some application determine user's privileges at login and store information in a user controlable location like cookies or query string parameeter. 
```python
insecure.com/login/home.jsp?admin=true
insecure.com/login/home.jsp?role=1
```
user can easily change those values. #tip True can be true

***#4*** looking at the response will give better idea's. we can literally change or add in json values. json values can be add by putting comma and key:value pair.
```
{ "email":"someone@somemail.com","roleid":2
}
```
#### Broken access control resulting from platform misconfiguration
***#5*** Some application blocks specific HTTP request methods to specific URL.
```
DENY, POST, /admin/deleteuser , manager
```
this will block post request to `/admin/deleteuser` from all users in manager group.
this can be bypassed using other HTTP methods.
for eg:
```http
POST / HTTP/1.1  
X-Original-URL: /admin/deleteUser
```
**tip** : ` X-Original-URL: /admin` & `X-Rewrite-URL: admin/delete?username=carlos`  if it dosen't work with GET request Try changing request to POST.
with upgrade http request from admin, we can change admin identifier (cookie) to normal users, changing the username to be upraded as admin and tranfor request to GET from POST will upgrade normal user to admin with user credential (cookie) ref:brokenaccesscontrol/curcuvilated.

#### IDOR Section
***#6*** horizontal privilege escalation: this usually happens when user id passes as get request in low level desinged application.
for eg: `GET /my-account?id=wiener HTTP/1.1` changing wiener to other username will change user privileges or leads to account takeover.
***#7*** user id in parameter but unpredictable by attacker
this scenario is same as #6 th one. instead of `GET /my-account?id=wiener HTTP/1.1` application used complex id like `GET /my-account?id=9483ebd3-1c5f-403b-bb99-6a655292ab7e HTTP/1.1`. This make much more complex but, literally we can aquire user id somewhere else within application. other user uploaded blog, with blog author we can find user id.

***#8*** In some application redirectes to login page if we don't have permission to access the data. certain scenarios, redirect response may contain sensitive information belonging to that user.
looking at the response will solve this.

##### Horizontal to vertical previlege escalation
***#9*** 