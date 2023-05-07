#protcol #general #web #websocket

## what is <font color="#00b050">WebSocket</font> ?
The **WebSocket API** is an advanced technology that makes it possible to open a <u>two-way interactive communication session</u> between the user's browser and a server. With this API, you can send messages to a server and **receive event-driven responses without having to poll the server for a reply**.

**<font color="#00b050">WebSocket</font>** is a computer communications protocol, providing full-duplex connection.

<font color="#00b050">WebSocket</font> is distinct from [[HTTP]]. Both protocols are located at **layer 7** in the OSI model and **depend on TCP at layer 4**.

It compatible with **HTTP**, To achieve compatibility, the WebSocket handshake uses the <font color="#ffff00">HTTP Upgrade header</font> to <u>change from the HTTP protocol to the WebSocket protocol</u>.

### Important

* Unlike regular cross-domain HTTP requests, WebSocket requests are not restricted by the <font color="#ffff00">Same-origin policy</font> ([[CORS]].) Therefore **WebSocket servers must validate the "Origin" header against the expected origins during connection establishment**, to avoid Cross-Site WebSocket <font color="#ff0000">Hijacking</font> attacks , which might be possible when the connection is authenticated with Cookies or HTTP authentication. It is better to use tokens or similar protection mechanisms to authenticate the WebSocket connection when sensitive (private) data is being transferred over the WebSocket.
* They require careful management of resources and error handling, as a misbehaving client can cause issues with the server.

### Use case 
They are commonly used for real-time applications such as online games, chat applications, and push notifications.

