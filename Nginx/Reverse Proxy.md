#nginx #web-server #proxy #reverse-proxy 

<font color="#f79646">Upstream</font>: upstream **defines a cluster that you can proxy requests to**. It's commonly used for defining either a <u>web server cluster for load balancing</u>, or an <u>app server cluster for routing / load balancing</u>.

<font color="#ffff00">Note</font>: Role of **<font color="#00b0f0">Gunicorn</font>** is to work as **application server** that translate request to something that Django can understand.

<font color="#ffff00">Note</font>: when we use Nginx to server our request **all the request given to <font color="#00b0f0">application server</font> will have the ip address of nginx**, this case is not always true and <u>we can set header to pass the ip address of user to django</u>.

For serving static files to nginx we can create shared volume and bind static of Django to Nginx. 

Example of reverse-proxy upstream:
```nginx configuration
# some where we want to send this inforamtion, web in this case is name of container in compose
upstream django { # we named django ourself it could be anything 
    server web:8000;
} 

server {
    listen 80;

    location / {
        # if request come for this path then send it to upstream
        proxy_pass http://django;
        # will add X-Forwarded-For header and transefer ip of client to upstream
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
        # will add Host header and transefer host of client to upstream
        proxy_set_header Host $host; 
    }
	# this is url we defined in the django setting for static url
    location /static { 
	    # this is the path to the static files
        alias /var/www/static/; 
    }
}
```
Docker-compose:
```docker
version: '3.9'

services:
  web_server:
    image: nginx
    ports:
      - "80:80"
    volumes:
      - static:/var/www/static/
      - ./nginx/nginx_config/:/etc/nginx/conf.d/

  web:
    build:
      context: ./django/.
      dockerfile: Dockerfile
    hostname: web
    container_name: web
    restart: always
    #    ports:
    #      - "8000:8000"
    expose:
      - 8000
    volumes:
      - ./django/nginx_reverse_proxy/:/usr/src/app/
      - static:/usr/src/app/static/
    #    command: python3 manage.py runserver 0.0.0.0:8000
    command: gunicorn nginx_reverse_proxy.wsgi:application -b 0.0.0.0:8000
    env_file:
      - ./.env/dev.env

volumes:
  static:
```