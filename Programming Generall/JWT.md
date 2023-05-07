#general #security #json 
<font color="#ffff00">Authentication</font>: the process of **identifying**. 
<font color="#ffff00">Authorizaion</font>: specifying **access rights/privileges to resources**.

## <font color="#b2a2c7">JWT</font> (JSON Web Token)
It's a way of **authentication**, which it's got a <u>payload or claim that stores some data with token</u>, in this way <font color="#fac08f">server does not need to keep the list of tokens in a database</font> and it <font color="#fac08f">can be used on multiple server</font>( <font color="#e36c09">SSO</font> ) if the siging is the same on multiple server.

JWT tokens are placed in [[HTTP]] header `authorization`, they normally start with `Token` or `Bearer`.

when the request is made to the server two type of toekn is created:
1. <font color="#548dd4">Access</font> : the client use this to access he resources and have short expire time.
2. <font color="#548dd4">Refresh</font>: if the access token is expired this token is used to refresh that token without the need to signin again, it has longer expire time.

Cycle of Getting Token :
![gnome-shell-screenshot-914O30.png](../../../_resources/gnome-shell-screenshot-914O30.png)

the <font color="#fac08f">Token</font> is composed of three parts which are <u>seperated by dots</u>:
1. <font color="#c3d69b">HEADER</font>: the **algorithm for coding and decoding**.
2. <font color="#c3d69b">PAYLOAD</font>: **Data of tokens**, mostly information about user.
3. <font color="#c3d69b">VERIFY SIGNATURE</font>: to verify that the token **hasn't temperd** with.

Token Example:
```JWT
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```
Image of an example Token:
![[Pasted image 20230301132443.png]]

><font color="#b7dde8">IAT</font>: when the token is created (in the body)
   <font color="#b7dde8">EXP</font> or <font color="#b7dde8">EAT</font>: expire date for the token (in body)
   <font color="#92cddc">nbf</font> (not before): It indicates the point in time when the JWT becomes valid.
   <font color="#92cddc">iss</font>:  This claim indicates the identity of the party that issued the JWT
   <font color="#92cddc">aud</font>: This abbreviation stands for audience. It indicates for whom the token is intended.

<hr>
## <font color="#953734">Logout</font>
We <u>can not signout on the jwt</u>, in the best secanario we will **remove it from client side but it still works if someone has it**.
to prevent miss use we<u> better always set EXP or EAT for the token</u>.
we also want to have a <font color="#fac08f">blacklist</font> for all the tokens that are valid and the user logged out or changed password but yet to be expired, we can use [[Redis]] for this 

<hr>
> OAuth 2.0 and OpenID Connect use JWT to exchange information between parties.
## <font color="#76923c">Symetric</font> Signeture 
When a service generates a JWT, it also creates a signature. Traditionally, this signature is an <font color="#ffc000">HMAC</font>.

<u>When a service receives an inbound JWT, it needs to verify the integrity before using the embedded data</u>. To do so, the service <u>uses the same secret key to calculate the HMAC of the JWT</u>. If the resulting HMAC is the same as the signature in the token, the service knows that all three inputs to the HMAC function <u>were the same as before</u>.

### Limataion
symmetric signatures <font color="#ffff00">prevent the sharing of the JWT with another service</font>.To verify the JWT’s integrity, all services would need to have access to the same secret key.
<font color="#ffff00">Sharing the HMAC secret with a third-party service creates a significant vulnerability</font>. **Even sharing the secret between different services within a single architecture is not recommended**.

## <font color="#548dd4">ASymetric</font> Signeture
An asymmetric signature uses a <font color="#548dd4">public/private key</font> pair. Such a key pair possesses a unique property. A signature generated with a private key can be verified with the public key.

> OpenID Connect is one of the most common protocols that uses this signature scheme.

In the OpenID Connect scenario, Google uses their private key to sign the identity token. The application uses Google's public key to verify its integrity, before relying on the embedded data.

## Key Managment
Cryptographic <font color="#e36c09">keys used for signing and encryption need to be rotated frequently. Performing too many operations with a key opens the application up to cryptanalysis attacks</font>.

