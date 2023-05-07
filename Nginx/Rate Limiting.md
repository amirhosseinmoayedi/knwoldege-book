#nginx #http #web-server #security
## <font color="#31859b">Rate limiting</font> is a technique used to control the **rate of incoming requests** to a server **to prevent overloading and maintain stability**, by limiting the number of requests that can be made within a **specified time period**.

<font color="#31859b">RateLimiting</font> can be <u>per user, IP, cookie and etc</u>.

The Alogorithm for storing in the zone is <font color="#e36c09">FIFO</font>( First in First out ) by default.

Config for rate limit can be for a *<u>server or just a path of the server</u>*.

By default the [[HTTP]] response will be `503` which is **Service Temorarily Unavailable** but  the corect form is `429` which is **Too Many Requests**. 

the `binary_remote_addr` is ratelimiting option <u>base on IP</u>.

The "<font color="#e36c09">burst</font>" option in the rate limit section defines t**he additional number of requests that can be made over the defined rate limit**. For example, if the rate limit is set to 1 request per second, the burst option can be set to 5, allowing for a total of 6 requests in a second.

we can delay the burst with `delay` and paramether or `nodely`, <font color="#e36c09">no dely is mostly better</font>, **by default re bursts are delayed** but with `delay` set to paramether we difine how many of the bursts will be delayed. 

<font color="#953734">zone</font> <u>will keep the data about base on what it's going to be limited,</u> like when is base on IP it will keep the info of IP addresses.

```Nginx
# type of limiting (babse on ip in this case) and zone:name_of_zone:shared_memorysize_to_store_in_zone rate=number per time
limit_req_zone $binary_remote_addr zone:limitbyaddr:10m rate=1r/s
# http status to show if user passed the limit
limit_req_status 429;

upstream demo {
    server web:8000;
}
  
server {
    listen 80;
    # match the ratelimit to the server section
    limit_req zone=limitbyaddr burst=10 delay=5;
    location / {
        proxy_pass http://demo;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $host;
    }
  
    location /static/ {
        alias /home/app/staticfiles/;
    }
}
```
