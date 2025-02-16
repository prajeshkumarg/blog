---
title: Security Byte 01
---

# Security Byte-01 cURL-ing your way on the internet

cURL - is probably one of the most handy tool to use on the linux terminal. A quick run of the man curl command brings up the manual page for curl. curl supports numerous protocols for URL, although this blog post will mainls focus on HTTP(S) requests to a server. The objective of this blog post is to serve as a cheat sheet sort of page with curl shortcuts and explanations to them. HTTP has various request methods but lets limit the discussion to GET and POST
<hr>

### GET Requests

curl by default performs HTTP-GET requests unless specified other wise
```bash
    #for the endpoint /endpoint name -> supports url params too
    curl http://test.com/endpoint
    curl http://test.com/endpointb?key=please

    #Sending cookies and other header values
    curl http://test.com/endpoint --cookie "key=please"
    curl -H 'Content-Type: key/please' 
            -H 'Accept-Language: key-please'http://test.com/endpoint

    #passing array as parameter
    curl http://test.com/endpoint?key[0]=key&key[1]=please

    #passing hash dict
    curl http://test.com/endpoint?key[please]=1

    #Path traversal using curl
    curl 'http://test.com/endpoint/../etc/passwd' --path-as-is

    #Checking reverse proxies
    curl 'http://test.com/endpoint;endpoint'
```
<hr>

### POST Request
For POST request we need to use ```-X POST``` flag and send the data using ```-d``` flag. We can also send multi-part form data(very useful when creating payload automations).

```bash
#Post req with params (we can remove the -d flag for the empty body)
curl -X POST http://test.com/endpoint -d "key=please"

#we can also use -d with GET requests

#for HTTP param pollution-> technique to check
curl -X GET 'http://test.com/endpoint?key=please&key=please' 
curl -X POST http://test.com/endpoint -d 'key=please&key=notplease'
#some times the client may read the first parameter and the server may read the second and thus we can insert malicious stuff in the second param
#POST req with getting params and POST body too
# We can even add XML to -d flag
curl -X POST http://test.com/endpoint?key=please -d 'key=please'

#Sending multipart data
curl -F key=please -X POST http://test.com/endpoint
#for files
curl -F filename=@file.log http://test.com/endpoint
#with traversal
curl http://test.com/endpoint -F "filename=@dummy.txt;filename=../../dummy.txt"
```
<hr>

### Sending Data in Different Formats
### XML
```bash 
curl -H "Content-Type: application/xml" -X POST http://test.com/endpoint -d '<key value="please"></key>'
```

### JSON
```bash 
curl -X POST http://test.com/endpoint
   -H 'Content-Type: application/json'
   -d '{"key": "please"}'
#to escape "
curl -X POST http://test.com/endpoint -H 'Content-Type: application/json' -d '{"key": "please\""}'
```

### YAML
```bash 
curl -X POST http://test.com/endpoint -H 'Content-Type: application/yaml' -d 'key: please'
```
<hr>

Other Misc cURL Usages

### Custom Request Headers
You can specify custom request headers using -H:
```bash
curl -X GET http://test.com/endpoint -H "User-Agent: CustomAgent/1.0"
```
### Handling Redirects
By default, cURL does not follow redirects. Use -L to enable this:
```bash
curl -L http://test.com/redirect
```
### Saving Responses to a File
```bash
curl -o output.html http://test.com/endpoint
```
### Viewing Response Headers
```bash
curl -I http://test.com/endpoint
```
### Sending Authenticated Requests
#### Basic Authentication
```bash
curl -u username:password http://test.com/endpoint
```
#### Token Authentication
```bash
curl -H "Authorization: Bearer YOUR_ACCESS_TOKEN" http://test.com/endpoint
```
<hr>
    
cURL is a powerful and flexible command-line tool for making HTTP(S) requests. Whether you need to test APIs, automate tasks, or perform security testing, cURL provides numerous options to customize requests. This cheat sheet should serve as a quick reference for common use cases.

<i>Stay tuned for more Security Bytes!</i>

