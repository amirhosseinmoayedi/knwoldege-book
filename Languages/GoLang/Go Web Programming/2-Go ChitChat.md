#go #golang #web #book_summery 
## Request Response Overview
![server][../../../../_resources/Screenshot_2022-08-02_180639.png]
When the request reaches the server, a ***multiplexer*** will inspect the *URL* being requested and redirect the request to the correct handler.

Once the **request** reaches a **handler**, the handler will retrieve information from the request and *process* it accordingly.

You don’t set the expiry date so that the **cookie becomes a session cookie** and it’s automatically removed when the browser shuts down. You set *HttpOnly* to only allow HTTP or HTTPS to access the cookie.