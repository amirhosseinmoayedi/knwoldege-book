 #grpc #api #microservice

<font color="#ffc000">Local procedure call</font>: run or invoke code on **local machine**. 
<font color="#ffc000">RPC</font>: run code on **another machin**.

A ***communication framework*** is usually used for <font color="#92d050">backend</font> and <font color="#92d050">microservices</font>.

one of the reasons for using <font color="#00b0f0">GRPC</font> is that: if we have services that are written with different languages or runtimes the HTTP implementation would be different.

GRPC implements the connections with<font color="#00b0f0"> HTTP 2.0</font> for us.

**<font color="#ffff00">Protocol Buffers</font>**: it's like a contract for communication.
we can define the schema for the input and output with them.
protocol buffer will *generate code* for the respected language.
protocol buffers are *serialized and sent as binary* across the services as result they are faster than JSON.

<u>GRPC can be used with json too</u>.
the<font color="#00b0f0"> HTTP/2</font> and <font color="#00b0f0">binary encoding</font> made GRPC <u>5 time faster than json</u>.
![[Pasted image 20230125170411.png]]




