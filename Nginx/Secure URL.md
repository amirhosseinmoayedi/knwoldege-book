#nginx #web-server #security 
## What is <font color="#e36c09">Secure link</font> ?
<u>Secure Link is a feature in Nginx that allows for URL-based authentication and authorization of file downloads</u>. It works by adding a **cryptographic signature to a URL**, which Nginx uses to verify the authenticity of the request before allowing access to the requested file. This provides an extra layer of security as the signature is generated using a secret key shared between Nginx and the client, and can only be verified by Nginx, ensuring that the URL can only be accessed by authorized clients.

The difference between `rewrite` and `redirect` in Nginx is as follows:
1.  `rewrite`: The `rewrite` directive in Nginx is used to change the URL of a request <u>internally within Nginx</u>, <u>without the client being aware of the change</u>. The client sees the original URL in their browser's address bar.
2.  `redirect`: The `redirect` directive in Nginx sends a response to the client with a new URL, <u>instructing the client to make a new request to the new URL</u>. This results in a change of the URL in the browser's address bar. The HTTP status code returned in the response can be specified, with `301` (Moved Permanently) being the most common.
In summary, `rewrite` is used for internal URL rewriting, while `redirect` is used for external URL redirection.

`internal` *means the path decleared are only accessible via the* `rewrite`.
Nginx Config:
```Nginx
server {
	listen 80;
	location /member {
		# secret key to hash and compare
		secure_link_secret 12324242;
		if ($secure_link = "") {
			return 403;
		}
	# rewrite with regex ^ which is used for match the start of string
		rewrite ^ /secure/$secure_link;
	}
	location /secure {
		internall ;
		root /home
	}
}
```

In summary, this configuration uses <font color="#e36c09">Secure Link</font> to control access to files in the `/home` directory. Requests to `/member` URLs are verified using a secure signature, and if the signature is valid, the URL is rewritten to `/secure/$secure_link`, allowing access to the requested file from the `/home` directory.
The `secure_link_secret` directive in this Nginx configuration is used to set the secret key for generating secure links.