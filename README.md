# eve_ng_rest_api

This repo contains information about REST_API usage on EVE-NG.

For this DEMO we are using EVE_NG server running on ESXI6.5 and it's reachable via ip address 10.0.0.221.


**EVE_NG login or authentication**
===================================

```
curljson -i -s -b /tmp/cookie -c /tmp/cookie -X POST -d '{"username":"admin","password":"eve"}' http://10.0.0.221/api/auth/login
HTTP/1.1 200 OK
Date: Tue, 12 Feb 2019 05:59:11 GMT
Server: Apache/2.4.18 (Ubuntu)
X-Powered-By: Unified Networking Lab API
Cache-Control: post-check=0, pre-check=0
Pragma: no-cache
Set-Cookie: unetlab_session=8c28bcf6-1514-4543-be14-5823d0a859fe; domain=10.0.0.221; path=/api/; expires=Sat, 12-Feb-3600 05:59:11 UTC
Content-Length: 67
Content-Type: application/json

{
    "code": 200,
    "message": "User logged in (90013).",
    "status": "success"
}

```

**CREATE A LAB**
================

```
curljson -i -s -c /tmp/cookie -b /tmp/cookie -X POST -d '{"path":"/CCIE","name":"Gasood","version":"1","author":"User1","description":"A new demo lab","body":"Lab usage and guide"}' -H 'Content-type: application/json' http://10.0.0.221/api/labs
HTTP/1.1 412 Precondition Failed
Date: Tue, 12 Feb 2019 06:03:03 GMT
Server: Apache/2.4.18 (Ubuntu)
X-Powered-By: Unified Networking Lab API
Cache-Control: post-check=0, pre-check=0
Pragma: no-cache
Content-Length: 104
Content-Type: application/json

{
    "code": 412,
    "message": "User is not authenticated or session timed out (90001).",
    "status": "unauthorized"
}
whoami@ubuntu1$


```
