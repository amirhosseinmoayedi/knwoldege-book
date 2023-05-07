#general #security #oAUTH #oAUTH2
<font color="#ffff00">OAuth</font> is an <u>open standard for authorization,</u> which allows users to <u>grant access to their private resources</u> (such as their social media accounts or online banking information) to a <u>third-party application or service without sharing their credentials</u> (username and password).

Instead of sharing login information, **OAuth uses tokens to grant access to specific resources**. The authorization process involves several steps, including the user granting permission to the third-party application and the application receiving an access token, which it can use to access the user's resources.

OAuth uses **several security measures to ensure that user data is protected**. For example, access tokens are short-lived and must be renewed periodically to maintain access to the user's resources. The protocol also supports a variety of encryption methods to protect sensitive data during transmission.

## Tokens
There are typically two types of access tokens that are used in OAuth:
1. <font color="#95b3d7">Bearer Tokens</font>: These are the **most commonly used type of access token in OAuth**. Bearer tokens are <u>random strings of characters that represent authorization to access a specific set of resources</u>. When a third-party application is granted access to a user's resources, it receives a bearer token that it can use to access those resources until the token expires or is revoked. Bearer tokens are often sent in the Authorization header of HTTP requests.

2. <font color="#95b3d7">JSON Web Tokens</font> ([[JWT]]): These are a type of access token that uses a <u>digitally signed payload to verify the identity of the user and the application</u>. JWTs are typically larger than bearer tokens and **include information such as the user's identity, expiration time, and a signature that can be used to verify the token's authenticity**. JWTs are often used when more granular access control is needed or when additional user information is required by the third-party application.


The proccess of granting token and using it is called <u>OAUTH flow</u>.

<font color="#ffff00">OAuth 2.0</font> is a newer and improved version of OAuth that is designed to be simpler, more flexible, and more secure. It offers improved support for different use cases and provides more granular control over permissions and scopes.

![110.jpg](../../../_resources/110.jpg)