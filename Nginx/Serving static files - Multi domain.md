#nginx #web #dns #web-server 

<font color="#ffc000">Note</font>: Nginx will show 403 if it *can't find the index page* or have *config issue* or even *permission issue on index page*.

It's **not optimized** for the *application server* to **serve the medias or static file**. so what we do ? whe use object storages like<font color="#c0504d"> S3</font> or <font color="#c0504d">Minio</font> for storing files and just **keep the path to file in backend**, but <u>if we have css and html that are complied or want to be serve that's better to be renderd throw the web-serve</u>r.

## Nginx configuration example

path to file is `/etc/nginx/conf.d/default.conf`.

```nginx
# you can have multiple domain in here, if you want defien another server 
server {
    listen 80; # ipv4 setup
    listen [::]:80; # this is ipv6 setup
    server_name localhost; # your domain name here


	# this is the root directory
    location / {
        # your html folder here
        root /usr/share/nginx/html/main;
        # default html file here
        index  index.html; 
    }
}
```

Example: 
```nginx
server {
    listen 80;
    server_name main.com ns.main.com *.main.com;

    location / {
        root /usr/share/nginx/html/main;
        index index.html;
    }
}

server {
    listen 80;
    server_name secondary.com ns.secondary.com *.secondary.com;

    location / {
        root /usr/share/nginx/html/secondary;
        index index.html;
    }
}
```