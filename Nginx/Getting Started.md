#web-server #nginx #proxy #reverse-proxy
**Web Servers** examples are:

- Apcahe
- Nginx
- Cloudflare


**Nginx** Is mostly used web sever, and it can do a <font color="#ffff00">lot more than being a web server</font>, such as **<font color="#00b0f0">caching</font>** , **<font color="#00b0f0"> reverse proxy </font>** , <font color="#00b0f0">load balancing</font> and <font color="#00b0f0">media streaming.</font>

![nginx-application-server](../../../_resources/424e5d323a2608d46211df68cd1dfb0f.png)

nginx default serving directory is `/usr/share/nginx/html`.

A <font color="#ffff00">forward proxy</font> is a server that sits between user devices and the internet.
A forward proxy is commonly used for: 
	1️. Protect clients
	2️. Avoid browsing restrictions
	3️. Block access to certain content
 
 A <font color="#ffff00">reverse proxy</font> is a server that accepts a request from the client, forwards the request to web servers, and returns the results to the client as if the proxy server had processed the request.
 A reverse proxy is good for:
	1️. Protect servers
	2.  Load balancing
	3️. Cache static contents
	4.  Encrypt and decrypt SSL communications
![[forward_proxy_vs_reverse_proxy-f.png]]


we use <font color="#31859b">ENV</font> in Nginx like this:
```Nginx
upstream demo {
    server web:8000;
}
  
server {
	#variable
	listen ${NGINX_PORT}$;

    location / {
	    # variable
		proxy_pass http://${NGINX_UPSTREAM};
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $host;
    }
  
    location /static/ {
        alias /home/app/staticfiles/;
    }
}
```
