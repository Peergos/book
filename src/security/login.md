# Login

Decentralised login is achieved using a capability based system. Your identity key pairs and root encryption key are derived from your password salted with your username and passed through the scrypt hashing function (with parameters 17, 8, 1, 96). By virtue of being decentralised, we cannot rate limit attempts to crack your password, so choosing a good passord is imperative. We recommend at least 14 random alphanumeric characters.

<img alt="Login key derivation" src="img/scrypt.svg" class="center" style="width: 50%;" />