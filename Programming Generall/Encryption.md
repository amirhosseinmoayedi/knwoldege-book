#TLS #general #Encryption
## what is Encryption
Encryption is **a way of scrambling data so that only authorized parties can understand the information**. In technical terms, it is the process of <u>converting human-readable plaintext</u> to incomprehensible text, also known as <font color="#fac08f">ciphertext</font>.

It **helps <font color="#fac08f">protect private</font> information, sensitive data, and can enhance the security of communication between client apps and servers**.

we have two type of encyption:
1. Symmetric: 
		 we use a **single key for encrypting and decryption**.
1. Asymmetric:
		 use a key for Encryption and another key for decyption. others will get the encryption key only (public) not the private one
s
![[Pasted image 20230216151239.png]]

Symmetric examples:
- AES (Advance Encryption Standard)
- Twofish
- serpent
- DES (not secure)

ASymmetric Examples:.
- RSA( used in almost every where like tls)
- Diffie-Hellman
- ElGamal

| type | pros| cons |
| --- | --- | --- |
| asymmetric | public key can be shared | slow |
| asymmetric | designed for small data (ssh) |     |
| symmetric | Fast | Hard to transport shared key |
| symmetric | efficient for large data |     |