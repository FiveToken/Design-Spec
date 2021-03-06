# Private Key/Mnemonic Words Management

The private key / mnemonic words management plan is independently designed by FiveToken. Details as below: 

![privatekey](pk.png)

1. After the password derives the private key, the private key takes bitwise XOR to replace AES as the encryption solution.

   （Password is generated by user input with a low entropy state that can not be adopted by cryptographic applications. A common password contains six digits, ranging from 0 to 9, and provides only 0 to 99999 randomness. A common encryption algorithm, such as AES-256, requires a key length of 32 bytes and requires 0 to 2^256 randomness.）

   

2. The digest of the private key is generated using the message-digest algorithm and stored with the ciphertext [sk]_kek. When the SK is in use, after decrypting and recovering the SK, you need to calculate whether the digest of the SK is consistent with the recorded digest to determine the integrity of the ciphertext, that is, whether [sk]_kek is tampered with. In this scheme, the message digest algorithm is SHA-256, and the first 16 bytes are truncated as the last stored digest.

   The Message_Digest method provided in Java cryptography library Java. security includes several alternative algorithms: MD5, SHA--1, SHA-256, and SHA-512. You can set algorithm parameters to implement different digest algorithms.

   （The message digest algorithm acts like a fingerprint of a message, which can be compared to determine whether a message has been tampered with. Once the message is tampered with, the digest (aka. the fingerprint) will not match.）

3. How to generate salt：it is generated in SHA-256(pk_i, string= filWallet, pwd_i) mode and is not stored locally. The key is generated with the PWD entered by the user.  

Multiple private keys sk_i are stored locally. Each private key is protected by the password pwd_i set by the user. After being encrypted, each private key is stored locally in the following format:  

![salt](salt.png)

