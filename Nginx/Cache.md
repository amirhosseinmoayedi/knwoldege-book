#nginx #cache #proxy 
Nginx caches are stored **in disk**.

For caching **we need a directory** to store data, normally in *Linux* it is in `/var/cache/`
directory, <u>there are other tools that we can use to store cache</u>.

In **Nginx** we define a cache setting which specify the<u> directory to store data</u>, and some other configuration like:
* we define a **zone** with name (cache container name), which also specify the amount of *shared memory*.
* **inactive** is an option that will delete cache if data is not requested in that *time range*.
* **level** is an option that will specify the file structure.
* **max_size** is an option that will specify the maximum size of the cache.
    * if max size is _exceed_ it would delete the least recently used data.(**LRU**)

We can add HTTP header in <font color="#9bbb59">proxy state</font>, for that we use `add_header` key in<font color="#c0504d"> server section</font>.

The cache can be set for <font color="#00b0f0">multiple locations</font>.

[Nginx measurment units](http://nginx.org/en/docs/syntax.html#:~:text=Sizes%20can%20be%20specified%20in,using%20g%20or%20G%20suffixes.)

If we set only for the `root` or `/` it will aplied to all the **URLS**. 

Nginx uses  `vary` HTTP header for saving **multiple version of a single page** like: different language version of a single page. #http_header 
for this perpose Nginx make <font color="#92cddc">Cache Key</font> which will be used to determine what cache should it read from.

<font color="#ffff00">Note</font>: if cahce is set for <font color="#f79646">cookie</font> the <u>cache is per user and not per page</u>, we can disable it by *ignoring the vary header* which sometimes it will be eqall to `cookie`.

Example:
```nginx configuration
proxy_cache_path /var/cache/nginx # location of cache
                    keys_zone=NginxCache:20m # zone name and shared memory size in megabytes
                    inactive=60m
                    levels=1:2
                    max_size=10g;

upstream demo {
    server web:8000;
}

server {
    listen 80;

    location / {
        proxy_pass http://demo;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $host;
        
        # specify the cache
        proxy_cache NginxCache;
        # it tells the nginx to chache after 5 requests 
        proxy_cache_min_uses 5;
        # only GET request will be cached
        proxy_cache_methods GET;
        # 200 http status responses, valid for 10 minutes 
        proxy_cache_valid 200 10m;
        # 404 http status responses, valid for 5 minutes
        proxy_cache_valid 404 5m;
        # igonore this header for making cache key
		proxy_ignore_header Vary;
        # add header to show the cache status it has status of HIT or MISS
        add_header X-Proxy-Cache $upstream_cache_status; 
    }

    location /static/ {
        alias /home/app/staticfiles/;
    }

    location /p1 {
        proxy_pass http://demo;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $host;
	    
        # bypass the cache for this request, usage is that if the cache is defined globbaly 
        proxy_cache_bypass $http_cache_bypass;
        # disable the cache for requests to this location
        proxy_cache off; 
        add_header X-Proxy-Cache $upstream_cache_status;
    }

}

```

