#nginx #web #web-server #http 

## What is [[HTTP]] <font color="#e36c09">Redirect</font> ?
**HTTP Redirect is a process of forwarding an HTTP request from one URL to another.** This happens when the server sends a response with a "302 Found" or "301 Moved Permanently" status code, along with the new URL location, to the client's browser. The browser then makes a new request to the new URL, effectively redirecting the user to a different page.

The **most commonly used HTTP status codes for redirection** are <font color="#31859b">301</font> Moved Permanently and <font color="#31859b">302</font> Found:
* The 301 Moved Permanently is typically used when the resource has been permanently moved to a new URL and any future requests should use the new URL.
* The 302 Found is typically used when the resource is temporarily located at a different URL and should be requested again at its original location.

Examples of Redirection:
1. Redirect all HTTP traffic to HTTPS
```nginx 
server {
    listen 80;
    server_name example.com;
    return 301 https://$host$request_uri;
}
```
2. Redirect a specific URL to another URL
```nginx
server {
    listen 80;
    server_name example.com;
    location /old-page {
        return 301 /new-page;
    }
}
```
3.  Redirect a domain to another domain
```nginx
server {
    listen 80;
    server_name old.example.com;
    return 301 $scheme://new.example.com$request_uri;
}
```