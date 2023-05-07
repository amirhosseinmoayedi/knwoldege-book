#nginx #security #web-server 
<font color="#76923c">Nginx</font> needs `apche2-utils` for using **HTTP password**, install it on the os( image- container ), also need to **create directory to store the password file**.
for example:
```Bash
sudo apt install apache2-utils
mkdir -p /etc/pwd
# create user and pass for http login 
htpasswd -c /etc/pwd/.htpasswd user1
```
<u>it will create file called .htpasswd and store the user1 and password hash in it.</u>

Nginx config:
```Nginx
upstream demo {
    server web:8000;
}
  
server {
    listen 80;
    location / {
        proxy_pass http://demo;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $host;
    }

	location /admin {
		proxy_pass http://demo;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $host;
        # define a auth with a name
        auth_basic "Admin Panel";
        # we give it the file with user and password hash
        auth_basic_user_file /etc/pwd/.htpasswd;

	}
	
    location /static/ {
        alias /home/app/staticfiles/;
    }
}
```