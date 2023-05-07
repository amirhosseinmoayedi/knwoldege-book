#web #http #protcol #http_header #https

> A method is considered safe if it doesn’t **change the state of the server**

> **A method is considered *idempotent*  if the state of the server doesn’t change the second time the method is called with the same data.
## HTTP <font color="#ffc000">V1 over TCP</font>:
it's closes and open connection per request.
( back in 1996 when index html was just text and it was web1.0)

<font color="#ffc000">Note</font>: **TCP is expensive an it's slow**.(for preparing the packet to send)
## HTTP<font color="#ffc000"> V1.1</font>: 
add a header called <font color="#92d050">keep-alive</font> for keeping connection open for the requests and after we are done we close connection, it also has <font color="#92d050">caching</font> and <font color="#92d050">streaming</font> with chuck transfer.

## HTTP <font color="#ffc000">V2</font>:
it introduce <font color="#92d050">compression</font>, <font color="#92d050">multiplexing</font>, <font color="#92d050">server push</font>(server push request) and is <u>secure by default</u>.

<font color="#ffff00">multiplexing</font>: **multiple request goes to a single channel**. 
<font color="#ffff00">Note</font>: **HTTP is statelees**
## HTTP <font color="#ffff00">V3</font>:
like http 2 but **instead of tcp it uses quick**(udp with some options).
*one of the problem that HTTP 3 solve is that because HTTP 2 is over tcp if single tcp request fail it would wait for it to be send and then answer all even if their job is done, HTTP 3 does not care and will solve this*.

![[Pasted image 20230304124000.png]]
## <font color="#953734">Status Codes</font>
|Code| Description|
| --- | --- |
| 429 Too many requests | response status code indicates the user has sent too many requests in a given amount of time ("rate limiting").|
| 503  Service Unavailable|  server error response code indicates that the server is not ready to handle the request. |
| 301 Moved Permanently | Indicates that the resource has been permanently moved to a new URL and any future requests should use the new URL. |
| 302 Found | Indicates that the resource is temporarily located at a different URL and should be requested again at its original location. |
| 303 See Other | Indicates that the resource can be found at a different URL, but the request method should not be changed. |
| 307 Temporary Redirect | Indicates that the resource is temporarily located at a different URL and should be requested again at its original location using the same method. |
| 308 Permanent Redirect | Indicates that the resource has been permanently moved to a new URL and any future requests should use the new URL using the same method. |
![[http_status_code.jpg]]
##  <font color="#e36c09">Headers</font>
| Header | Description |
| --- | --- |
| Vary | The **`Vary`** HTTP response header describes the parts of the request message aside from the method and URL **that influenced the content of the response it occurs in**.|

## CORS
<font color="#76923c">Cross-Origin Resource Sharing</font> is an HTTP **header** set in the request for <u>content that must be load from multiple sources like a Django backend and react front</u>.
![[cors_principle.png]]
```
Access-Control-Allow-Origin: <origin> | *
```

```
Access-Control-Allow-Origin: https://mozilla.org
Vary: Origin
```

In CORS we can set the *http method and headers, how long it will be cached* too.
```
Access-Control-Allow-Origin: https://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Max-Age: 86400
```

## <font color="#00b050">HTTPS</font>
![[Pasted image 20230304124115.png]]
Step 2 - The client sends a “client hello” to the server. The message contains a set of <font color="#00b050">necessary encryption algorithms</font> (cipher suites) and the <font color="#00b050">latest TLS version it can support</font>. The server responds with a “server hello” so the browser knows whether it can support the algorithms and TLS version.

The server then sends the <font color="#00b050">SSL certificate to the client</font>. The c<font color="#00b050">ertificate contains the public key, hostname, expiry dates, etc</font>. The client validates the certificate.

Step 3 - After validating the SSL certificate, the <font color="#00b050">client generates a session key and encrypts it using the public key</font>. The <font color="#00b050">server receives the encrypted session key and decrypts it with the private key</font>. 

Step 4 - Now that both the client and the server hold the same session key (<font color="#00b050">symmetric encryption</font>), the encrypted data is transmitted in a secure bi-directional channel.