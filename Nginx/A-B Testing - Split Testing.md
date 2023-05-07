#nginx #web #architecture #test #web-server 

# What is <font color="#e36c09">A/B Testing</font>?
A/B testing is a method of <u>comparing two versions of a web page or app to determine which one performs better.</u> It involves showing the two versions (A and B) **to similar users** at the same time and measuring the effect each version has **on a predetermined metric**, such as *click-through rate* or *conversion rate*. A/B testing is used to<u> optimize user experience and increase conversions</u>.

For this operation we make a **Unique ID** for a user that requested and will be always serverd what was served at the first time and *not jumping between the versions*. <font color="#4bacc6">IP</font> address is not a very good for this operation but it's depend on what are difference between two version.
![[Pasted image 20230128131700.png]]

The Nginx <font color="#76923c">SplitClient</font> module is a tool that allows for the splitting of traffic between different servers based on the client's IP address. It works by taking a list of IP addresses and then assigning each one to a specific server. This allows for <u>more efficient load balancing and 
better performance</u>.

Example for static site:
```Nginx
# the algorithm for spliting in this case IP Address, and the percentage (it is variable in this case)
split_client "${remote_addr}" $testvariant {
	50% "home/html/v1";
	# * mean everybody else
	* "home/html/v2";
}
server {
    listen 80;

    location / {
	    # used the variable in location of root
	    root $testvariant;
	    index index.html;
	}
}
```

we can change the `remote_addr` to `arg_token` which will create a unique token for the client and not use the IP address. 
```nginx
split_client "${arg_token}" $testvariant {
...
```
the url should be like this for it to work `baseURL?token=something` like
`localhost?token=2373323`

Example for Reverse proxy:
```Nginx
upstream demo1 {
    server web1:8000;
}

upstream demo2 {
    server web2:8080;
}

split_clients "${arg_token}" $variant {
    50% demo1;
    *   demo2;
}

server {
    listen 80;
    location / {
        proxy_pass http://$variant;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $host;
    }

    location /static/ {
        alias /home/app/staticfiles/;
    }
}
```

anoter example of spliting:
```nginx
http {
  upstream appServer {
    ip_hash; # to persist session for ip and not jummping
    server old.app.com weight=9; # 90% of requests will be delived to this serveer 
    server new.app.com;
  }
  server {
    listen 80;
    location / {
      proxy_pass http://appServer;
    }
  }
}
```
