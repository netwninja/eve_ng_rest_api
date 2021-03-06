# eve_ng_rest_api

This repo contains information about REST_API usage on EVE-NG.

For this DEMO we are using EVE_NG server running on ESXI6.5 and it's reachable via ip address 10.0.0.221.


**EVE_NG login or authentication**
----------------------------------

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
---------------

```
curljson -i -s -c /tmp/cookie -b /tmp/cookie -X POST -d '{"path":"/CCIE","name":"test","version":"1","author":"User1","description":"A new demo lab","body":"Lab usage and guide"}' -H 'Content-type: application/json' http://10.0.0.221/api/labs
HTTP/1.1 200 OK
Date: Tue, 12 Feb 2019 06:05:18 GMT
Server: Apache/2.4.18 (Ubuntu)
X-Powered-By: Unified Networking Lab API
Cache-Control: post-check=0, pre-check=0
Pragma: no-cache
Content-Length: 73
Content-Type: application/json

{
    "code": 200,
    "message": "Lab has been created (60019).",
    "status": "success"
}

```


Now next step is going to create a node inside this LAB. In order to create the nodes we would need the LAB uuid.

```
curljson -i -s -c /tmp/cookie -b /tmp/cookie -X GET -H 'Content-type: application/json' http://10.0.0.221/api/labs/CCIE/test.unl
HTTP/1.1 200 OK
Date: Tue, 12 Feb 2019 06:17:16 GMT
Server: Apache/2.4.18 (Ubuntu)
X-Powered-By: Unified Networking Lab API
Cache-Control: post-check=0, pre-check=0
Pragma: no-cache
Content-Length: 285
Content-Type: application/json

{
    "code": 200,
    "data": {
        "author": "User1",
        "body": "Lab usage and guide",
        "description": "A new demo lab",
        "filename": "test.unl",
        "id": "5156debe-86c5-480e-9b73-8885008cd2b9",
        "lock": 0,
        "name": "test",
        "scripttimeout": 600,
        "version": "1"
    },
    "message": "Lab has been loaded (60020).",
    "status": "success"
}

```

once we have the UUID then go ahead and use the curl command mentioned below.  This node has following attributes

* type: iol
* image used: L3-ADVENTERPRISE9-15.4.2T.bin
* cpu: 1
* ethernet modules: 2



```
curljson -i  -s -c /tmp/cookie -b /tmp/cookie -X POST -d '{"type":"iol","template":"iol","config":"Unconfigured","delay":0,"icon":"Router.png","image":"L3-ADVENTERPRISE9-15.4.2T.bin","name":"Test Router1","left":"35%","top":"25%","ram":"256","console":"telnet","cpu":1,"ethernet":2,"uuid":"5156debe-86c5-480e-9b73-8885008cd2b9"}' -H 'Content-type: application/json' http://10.0.0.221/api/labs/CCIE/test.unl/nodes
HTTP/1.1 201 Created
Date: Tue, 12 Feb 2019 06:19:46 GMT
Server: Apache/2.4.18 (Ubuntu)
X-Powered-By: Unified Networking Lab API
Cache-Control: post-check=0, pre-check=0
Pragma: no-cache
Content-Length: 87
Content-Type: application/json

{
    "code": 201,
    "data": {
        "id": 1
    },
    "message": "Lab has been saved (60023).",
    "status": "success"
}


```

**Start the node**
-------------------


```
curljson -i  -s -c /tmp/cookie -b /tmp/cookie -X GET -H 'Content-type: application/json' http://10.0.0.221/api/labs/CCIE/test.unl/nodes/start
HTTP/1.1 200 OK
Date: Tue, 12 Feb 2019 06:25:50 GMT
Server: Apache/2.4.18 (Ubuntu)
X-Powered-By: Unified Networking Lab API
Cache-Control: post-check=0, pre-check=0
Pragma: no-cache
Content-Length: 66
Content-Type: application/json

{
    "code": 200,
    "message": "Nodes started (80048).",
    "status": "success"
}
whoami@ubuntu1$


```

Next step is to get the console access url for the above mentioned node

```
curljson -i -s -c /tmp/cookie -b /tmp/cookie -X GET -H 'Content-type: application/json' http://10.0.0.221/api/labs/CCIE/test.unl/nodes
HTTP/1.1 200 OK
Date: Tue, 12 Feb 2019 06:39:03 GMT
Server: Apache/2.4.18 (Ubuntu)
X-Powered-By: Unified Networking Lab API
Cache-Control: post-check=0, pre-check=0
Pragma: no-cache
Content-Length: 461
Content-Type: application/json

{
    "code": 200,
    "data": {
        "1": {
            "config": 0,
            "config_list": [],
            "console": "telnet",
            "delay": 0,
            "ethernet": 2,
            "icon": "Router.png",
            "id": 1,
            "image": "L3-ADVENTERPRISE9-15.4.2T.bin",
            "left": 358,
            "name": "Node1",
            "nvram": 1024,
            "ram": 256,
            "serial": 0,
            "status": 2,
            "template": "iol",
            "top": 192,
            "type": "iol",
            "url": "/html5/#/client/MzI3NjkAYwBteXNxbA==?token=B7E3D1733CCBE3640DF29B5CD6CD668A91442433158875B9A851DD176159AF3B"
        }
    },
    "message": "Successfully listed nodes (60026).",
    "status": "success"
}

```
