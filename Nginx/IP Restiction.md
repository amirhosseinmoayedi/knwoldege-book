#nginx #web-server #security 
In <font color="#76923c">Nginx</font> we have two option:
1. `allow`: syntax is  `allow what_to_allow`.
2. `deny`: syntax is  `deny what_to_deny`.

<u>We can deny all with</u> `deny all`, **by default** is `allow all`.

The process starts from **top to bottom**, so if you <u>deny all</u> and after you allow some IP it will **deny all and allow on bottom will not overwite it.**

It <u>can be range of ip or subnet instead of single ip</u>, `192.168.0.0/24`.

Nginx Config:
```nginx
upstream demo {
    server web:8000;
}
  
server {
    listen 80;
    
	allow 127.0.0.1;
	deny all;

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